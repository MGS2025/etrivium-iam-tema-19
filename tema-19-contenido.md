# Tema 19 — Contenido Teórico

> **Título oficial**: Lenguajes de interrogación de bases de datos. El estándar ANSI SQL. Procedimientos almacenados. Eventos y disparadores.
>
> **Bloque**: Parte II — Técnico
> **Nivel**: C1 — Técnico Auxiliar TIC, Ayuntamiento de Madrid
> **Versión**: v1.0 — Pendiente validación
> **Fecha generación**: 2026-07-13
> **Fuentes**: Ver tema-19-fuentes.md · **Diagramas**: Ver tema-19-diagramas.md · **Cambios**: Ver tema-19-changelog.md
>
> *Extensión: ~13.500 palabras · 12 diagramas SVG embebidos · 4 tipos de callout transversales*

---

## Convenciones del documento

Este tema incluye cuatro tipos de **cajas callout** para facilitar el estudio:

> **[DATO CLAVE EXAMEN]** Información de alta densidad memorística, con alta probabilidad de aparecer en el test oficial.

> **[EJERCICIO RESUELTO]** Problema + solución paso a paso (una consulta, una traza, el diseño de un procedimiento).

> **[EJEMPLO AYTO MADRID]** Aplicación real de la teoría al entorno municipal (Padrón, tributos, expedientes, licencias).

> **[REFERENCIA CRUZADA]** Enlace conceptual a otros temas del temario oficial.

Los ejemplos de **consultas** se escriben en **SQL estándar (ANSI SQL / ISO-IEC 9075)**, sin extensiones propietarias, tal y como corresponde a un tema centrado en el **estándar** (decisión de Joan). Los ejemplos de **procedimientos almacenados y disparadores** se escriben en **pseudocódigo SQL genérico** (`CREATE PROCEDURE … INICIO … FIN`, `SI … ENTONCES … FIN_SI`), porque el propio estándar (SQL/PSM, ISO/IEC 9075-4) apenas se implementa tal cual en la práctica: cada motor tiene su dialecto (PL/SQL de Oracle, T-SQL de SQL Server, PL/pgSQL de PostgreSQL). Cuando conviene situar una particularidad real se nombra el motor entre corchetes de referencia (`[ORACLE-DOC]`, `[MSSQL-DOC]`, `[PSQL-DOC]`, `[MYSQL-DOC]`) sin atar el tema a ninguno. Las fuentes se citan con etiquetas breves tipo `[DATE-REL]` o `[SILBERSCHATZ, cap. 8]`; el registro completo está en `tema-19-fuentes.md`.

**Esquema de ejemplo usado en todo el tema** (contexto Ayuntamiento de Madrid, simplificado):

```sql
DISTRITO       (id_distrito, nombre_distrito)
CONTRIBUYENTE  (dni, nombre, id_distrito)
TRIBUTO        (id_tributo, tipo, importe, dni_contribuyente, fecha_liquidacion)
EXPEDIENTE     (id_expediente, tipo, estado, dni_contribuyente, id_tramitador, fecha_apertura)
FUNCIONARIO    (id_funcionario, nombre, id_departamento)
```

---

## 1. Lenguajes de interrogación de bases de datos

### 1.1. Definición en el modelo relacional

Un **lenguaje de interrogación de bases de datos** (*query language*) es la notación formal que permite **formular consultas** —preguntas— sobre los datos almacenados en una base de datos y obtener como resultado los datos que satisfacen esa consulta [DATE-INTRO, cap. 1]. En el **modelo relacional** propuesto por E. F. Codd en 1970, los datos se organizan en **relaciones** (tablas), cada una formada por un conjunto de **tuplas** (filas) sobre un esquema fijo de **atributos** (columnas) [CODD70]. Un lenguaje de interrogación relacional opera, por tanto, **sobre relaciones y produce relaciones**: es la propiedad de **cierre** (*closure*) que hace posible componer consultas dentro de otras consultas.

Codd definió formalmente dos lenguajes equivalentes para interrogar el modelo relacional: el **álgebra relacional**, de naturaleza **procedimental**, y el **cálculo relacional**, de naturaleza **declarativa** [CODD70; DATE-REL, cap. 6]. Ambos son el fundamento teórico sobre el que se construyó después **SQL**, el lenguaje comercial que domina hoy la interrogación de bases de datos relacionales.

> **[DATO CLAVE EXAMEN]** Un lenguaje de interrogación relacional se dice **relacionalmente completo** cuando tiene, como mínimo, el mismo poder expresivo que el álgebra relacional (o el cálculo relacional seguro, que son equivalentes). Es la **prueba de Codd** para valorar si un lenguaje de consulta es suficientemente potente [CODD70; DATE-REL, cap. 6].

> **[REFERENCIA CRUZADA]** El **Tema 15** trata los SGBD relacionales, orientados a objetos y NoSQL como sistemas; el **Tema 16**, el **modelo conceptual** de datos (entidad-relación) que se traduce al modelo relacional; el **Tema 17**, el **diseño lógico** (normalización) que produce el esquema de tablas sobre el que este Tema 19 formula las consultas.

### 1.2. Álgebra relacional y el cálculo relacional

**Álgebra relacional.** Es un lenguaje **procedimental**: una consulta se expresa como una **secuencia de operaciones** sobre relaciones, cada una de las cuales produce una nueva relación que puede alimentar a la siguiente operación [DATE-INTRO, cap. 6; GMUW, cap. 2]. Sus operadores fundamentales, definidos por Codd, se dividen en:

- **Operadores unarios** (una sola relación de entrada):
  - **Selección** (σ): extrae las **filas** que cumplen una condición. `σ_{importe>100}(TRIBUTO)`.
  - **Proyección** (π): extrae **columnas**, eliminando duplicados. `π_{nombre,importe}(TRIBUTO)`.
  - **Renombrado** (ρ): cambia el nombre de una relación o de sus atributos, necesario para combinar una relación consigo misma.
- **Operadores binarios de conjuntos** (dos relaciones **compatibles por unión**, es decir, con el mismo esquema): **unión** (∪), **diferencia** (−) e **intersección** (∩, derivable de las dos anteriores).
- **Producto cartesiano** (×): combina **cada** tupla de una relación con **cada** tupla de otra. Es la base sobre la que se define el **join**.
- **Operadores derivados**, expresables a partir de los anteriores pero fundamentales en la práctica: la **reunión** o **join** (⋈, un producto cartesiano seguido de una selección por la condición de reunión) y la **división** (÷, resuelve consultas del tipo «qué X están relacionados con TODOS los Y»).

> **[DATO CLAVE EXAMEN]** El álgebra relacional es **procedimental**: el orden en que se escriben y componen los operadores importa y determina cómo se calcula el resultado. El **cálculo relacional** es **declarativo**: solo se especifica **qué** propiedades debe cumplir el resultado, sin indicar el procedimiento. **SQL hereda ambos rasgos**: en su sintaxis es declarativo (se parece al cálculo relacional), pero internamente el optimizador del SGBD lo traduce a un **plan de ejecución** expresado en términos algebraicos [DATE-REL, cap. 6].

**Cálculo relacional.** Se basa en la **lógica de predicados de primer orden**: una consulta se expresa como `{ t | P(t) }`, es decir, «el conjunto de tuplas *t* tales que se cumple el predicado *P*» [CODD70]. Existen dos variantes:

- **Cálculo relacional de tuplas**: las variables representan **tuplas completas** (`t ∈ TRIBUTO`). Es la base conceptual más cercana a SQL.
- **Cálculo relacional de dominios**: las variables representan **valores de un dominio** (una columna concreta), no tuplas completas. Es la base conceptual del lenguaje **QBE** (*Query by Example*).

Codd demostró la **equivalencia expresiva** entre el álgebra relacional y el cálculo relacional **seguro** (*safe*, es decir, restringido a dominios finitos para evitar resultados infinitos): todo lo expresable en uno es expresable en el otro. Esta equivalencia es el fundamento del criterio de **completitud relacional** citado arriba [CODD70; DATE-REL, cap. 6].

> **[EJERCICIO RESUELTO]** Sobre el esquema `CONTRIBUYENTE(dni, nombre, id_distrito)` y `TRIBUTO(id_tributo, importe, dni_contribuyente)`, expresa en álgebra relacional «nombre de los contribuyentes del distrito 3 con algún tributo de importe superior a 500 €».
> 1. Filtra distrito: `R1 = σ_{id_distrito=3}(CONTRIBUYENTE)`.
> 2. Filtra tributos: `R2 = σ_{importe>500}(TRIBUTO)`.
> 3. Reúne por DNI: `R3 = R1 ⋈_{dni=dni_contribuyente} R2`.
> 4. Proyecta el resultado: `π_{nombre}(R3)`.
> En cálculo relacional de tuplas, la misma consulta se expresaría de forma declarativa: `{ t.nombre | t ∈ CONTRIBUYENTE ∧ t.id_distrito = 3 ∧ (∃ u ∈ TRIBUTO)(u.dni_contribuyente = t.dni ∧ u.importe > 500) }`. Obsérvese que el álgebra **indica el procedimiento** (primero esto, luego aquello) y el cálculo **solo describe la condición**.

### 1.3. Tipología de lenguajes en un SGBD

Dentro de un SGBD, el lenguaje de interrogación se subdivide, por **función**, en varios sublenguajes que en conjunto conforman **SQL** [SILBERSCHATZ, cap. 3; ELMASRI, cap. 6]:

| Sublenguaje | Siglas | Función | Sentencias típicas |
|---|---|---|---|
| Lenguaje de Definición de Datos | **DDL** | Define y modifica la **estructura** (esquema): tablas, vistas, índices, restricciones | `CREATE`, `ALTER`, `DROP`, `TRUNCATE` |
| Lenguaje de Manipulación de Datos | **DML** | **Modifica el contenido** de las tablas | `INSERT`, `UPDATE`, `DELETE`, `MERGE` |
| Lenguaje de Consulta de Datos | **DQL** | **Recupera** datos sin modificarlos | `SELECT` |
| Lenguaje de Control de Datos | **DCL** | Gestiona **permisos** y seguridad de acceso | `GRANT`, `REVOKE` |
| Lenguaje de Control de Transacciones | **TCL** | Gestiona el **ciclo de vida de las transacciones** | `COMMIT`, `ROLLBACK`, `SAVEPOINT`, `SET TRANSACTION` |

> **[DATO CLAVE EXAMEN]** El estándar ISO/IEC 9075 no separa formalmente DQL del DML: `SELECT` se clasifica dentro del DML en la norma [ISO9075]. La distinción DDL/DML/**DQL**/DCL/TCL es una convención **didáctica**, muy usada en oposiciones y manuales, que separa el `SELECT` (consulta, no modifica datos) del resto de DML (que sí modifica) por su naturaleza claramente distinta. Es importante saber reconocer ambas clasificaciones.

**DDL (Data Definition Language).** Opera sobre el **catálogo o diccionario de datos** del SGBD: crea y elimina objetos (`CREATE TABLE`, `DROP INDEX`), y modifica la estructura de los ya existentes (`ALTER TABLE … ADD COLUMN`). En la mayoría de motores, las sentencias DDL provocan un **commit implícito**: no se pueden deshacer con `ROLLBACK` como una operación DML normal.

**DML (Data Manipulation Language).** Modifica el **contenido** de las tablas sin alterar su estructura: `INSERT` añade filas, `UPDATE` modifica valores de filas existentes, `DELETE` elimina filas, y `MERGE` (SQL:2003) combina inserción y actualización en una sola sentencia («upsert») según una condición de coincidencia.

**DQL (Data Query Language).** Se reduce, en la práctica, a la sentencia `SELECT` y su enorme riqueza de cláusulas (§2.3-2.8 de este tema).

**DCL (Data Control Language).** `GRANT` concede privilegios (`SELECT`, `INSERT`, `EXECUTE`…) sobre un objeto a un usuario o rol; `REVOKE` los retira. Es el mecanismo declarativo de **control de acceso discrecional** (DAC) dentro del SGBD.

**TCL (Transaction Control Language).** Gestiona la atomicidad de un conjunto de operaciones: `COMMIT` confirma de forma permanente los cambios de la transacción; `ROLLBACK` los deshace; `SAVEPOINT` marca un punto intermedio al que se puede retroceder sin deshacer toda la transacción. Estas sentencias son el mecanismo con el que el SGBD garantiza las propiedades **ACID** (Atomicidad, Consistencia, Aislamiento, Durabilidad) [ACID83].

> **[EJEMPLO AYTO MADRID]** Dar de alta un nuevo tipo de tasa municipal en el sistema implica los cinco sublenguajes: **DDL** para crear la tabla `TASA_NUEVA` si no existe; **DML** para insertar sus registros iniciales; **DQL** para comprobar que se ha cargado correctamente; **DCL** para conceder al perfil «gestor tributario» permiso de `SELECT` e `INSERT` sobre la tabla, pero no de `DROP`; y **TCL** para que la carga inicial de datos se confirme como una única unidad atómica (`COMMIT`) o se deshaga entera si algo falla a mitad (`ROLLBACK`).

### 1.4. Lenguajes procedimentales y declarativos

Ya introducida la distinción en §1.2 para álgebra/cálculo, conviene generalizarla porque es una de las preguntas más recurrentes del bloque [DATE-REL, cap. 6; MELTON, cap. 1]:

- **Procedimental**: el usuario especifica **el procedimiento**, es decir, la secuencia de pasos que hay que ejecutar para obtener el resultado (**CÓMO**). El álgebra relacional es procedimental.
- **Declarativo**: el usuario especifica **las propiedades del resultado deseado** (**QUÉ**), y es el sistema —normalmente a través de un **optimizador de consultas**— quien decide el procedimiento concreto (el **plan de ejecución**) para obtenerlo de la forma más eficiente. El cálculo relacional, y con él **SQL**, son declarativos.

**SQL es un lenguaje de cuarta generación (4GL) declarativo**: un `SELECT` describe el resultado que se quiere (qué filas, qué columnas, bajo qué condiciones), pero no indica en qué orden física se recorren las tablas ni qué algoritmo de *join* se usa; eso lo decide el **optimizador** en función de estadísticas, índices y coste estimado. Dos consultas SQL equivalentes desde el punto de vista lógico pueden ejecutarse con planes de ejecución muy distintos.

> **[DATO CLAVE EXAMEN]** SQL es **declarativo en su sintaxis** pero **procedimental en su ejecución interna**: el motor SQL traduce internamente cada consulta a una secuencia de operadores algebraicos (selección, proyección, reunión…) organizados en un **árbol de ejecución**, que es lo que realmente procesa el motor de bases de datos [DATE-REL, cap. 6].

### 1.5. Utilización de lenguajes en aplicaciones

Una aplicación rara vez interroga la base de datos «a mano»: necesita un **mecanismo de acceso a datos** que combine el lenguaje de interrogación con el lenguaje de programación de la aplicación [SILBERSCHATZ, cap. 10]:

- **SQL embebido** (*embedded SQL*): sentencias SQL insertadas directamente en el código fuente de un lenguaje anfitrión (C, COBOL), precompiladas antes de la compilación normal. El estándar lo recoge en ISO/IEC 9075-2 (embedded SQL) y en SQL/PSM (ISO/IEC 9075-4) para el código almacenado en el propio servidor (§3).
- **APIs de acceso a datos estandarizadas**: interfaces que exponen un conjunto común de funciones para conectar, enviar SQL y recuperar resultados, independientemente del SGBD concreto: **ODBC** (*Open Database Connectivity*, estándar de Microsoft/X-Open sobre C) y **JDBC** (*Java Database Connectivity*, su equivalente en el ecosistema Java). Ambas siguen el mismo patrón: driver específico del motor + interfaz común para la aplicación.
- **SQL dinámico frente a SQL estático**: el SQL **estático** se conoce y valida en tiempo de compilación (SQL embebido clásico); el SQL **dinámico** se construye y envía como cadena de texto en tiempo de ejecución (`EXECUTE IMMEDIATE`, o simplemente concatenando la sentencia en la aplicación), lo que da más flexibilidad pero exige extremar la prevención de **inyección SQL** mediante **sentencias parametrizadas** (*prepared statements*).
- **ORM** (*Object-Relational Mapping*): capa de abstracción que traduce automáticamente entre **objetos** del lenguaje de programación y **filas** de tablas relacionales, generando el SQL correspondiente (Hibernate/JPA en Java, Entity Framework en .NET). Facilita la productividad, pero puede generar consultas subóptimas si se usa sin entender el SQL resultante.
- **Procedimientos almacenados** (§3) como interfaz de acceso: la aplicación llama a un procedimiento por nombre en lugar de enviar SQL directamente, encapsulando la lógica de acceso en el servidor.

> **[REFERENCIA CRUZADA]** El **Tema 21** (arquitectura Java EE) desarrolla en detalle **JDBC** y las capas de persistencia de las aplicaciones empresariales; el **Tema 23** trata los lenguajes de programación web que a menudo consumen datos vía API en lugar de SQL directo.

---

## 2. El estándar ANSI SQL

### 2.1. Origen y evolución del estándar SQL

SQL nace como **SEQUEL** (*Structured English Query Language*), diseñado por Donald D. Chamberlin y Raymond F. Boyce en IBM (1974) para interrogar **System R**, el primer prototipo de SGBD relacional construido para validar en la práctica el modelo teórico que Codd había publicado en 1970 [CHAMBERLIN74; CODD70]. El nombre se abrevió a **SQL** por un conflicto de marca registrada previa con «SEQUEL».

Ante la proliferación de dialectos comerciales, **ANSI** (American National Standards Institute) publicó el primer estándar en **1986** (conocido como **SQL-86** o **SQL1**), adoptado por **ISO** un año después como ISO 9075:1987 [MELTON, cap. 1]. Desde entonces el estándar se ha revisado periódicamente:

| Edición | Año | Aportación principal |
|---|---|---|
| **SQL-86 / SQL1** | 1986 | Primer estándar ANSI; sintaxis básica de DDL, DML y DQL |
| **SQL-89** | 1989 | Revisión menor: integridad referencial (claves foráneas) |
| **SQL-92 / SQL2** | 1992 | Gran expansión: sintaxis de `JOIN` explícita, subconsultas, niveles de conformidad (§2.2) |
| **SQL:1999 / SQL3** | 1999 | **Recursividad** (CTE recursivas), **disparadores**, tipos definidos por el usuario, orientación a objetos [EISENBERG99] |
| **SQL:2003** | 2003 | **Funciones de ventana analíticas** [ZEMKE03], columnas de identidad autonuméricas, soporte de XML |
| **SQL:2006** | 2006 | Ampliación de la integración con **XML** (`XQuery`) |
| **SQL:2008** | 2008 | Sentencia `MERGE` (*upsert*), `TRUNCATE TABLE` |
| **SQL:2011** | 2011 | Datos **temporales** (tablas con periodo de validez, *temporal tables*) |
| **SQL:2016** | 2016 | Soporte nativo de **JSON**, funciones de reconocimiento de patrones de fila |
| **SQL:2019** | 2019 | *Multidimensional arrays*, polimorfismo de tablas |
| **SQL:2023** | 2023 | **SQL/PGQ**: consultas de **grafos de propiedades** sobre el modelo relacional |

> **[DATO CLAVE EXAMEN]** Tres hitos imprescindibles para el examen: **SQL-86**, primer estándar ANSI/ISO; **SQL-92**, la revisión más citada porque introdujo la sintaxis moderna de `JOIN` y las subconsultas tal como se usan hoy; y **SQL:1999**, que trajo la **recursividad** y los **disparadores** —dos de los cuatro bloques de este mismo Tema 19— al estándar [MELTON, cap. 1; EISENBERG99].

> **[REFERENCIA CRUZADA]** El origen de **System R** y la validación práctica del modelo relacional de Codd conectan con el **Tema 15** (SGBD relacionales: características y componentes) y con el **Tema 17** (diseño lógico relacional), ambos previos a este tema en el temario.

### 2.2. Niveles de conformidad

Ningún SGBD comercial implementa el 100 % del estándar SQL, ni todos implementan el mismo subconjunto: por ello el propio estándar define **niveles de conformidad** [MELTON, cap. 2]:

- **SQL-92** fue el primero en formalizar tres niveles: **Entry** (mínimo, próximo a SQL-89), **Intermediate** (subconjunto amplio, incluye ya `JOIN` explícito y tipos de fecha/hora) y **Full** (todas las características de la norma).
- Desde **SQL:1999**, el esquema de niveles único se sustituyó por un modelo de **«core» obligatorio + «features» opcionales**, identificadas con códigos (p. ej. `F031` para columnas de identidad, `T131` para CTE recursivas): un motor puede declarar conformidad con el *core* y con determinadas *features* concretas, sin exigir el «todo o nada» del esquema anterior.

Como consecuencia práctica, cada SGBD añade **extensiones propietarias** no estándar (funciones, tipos de datos, sintaxis procedimental — §2.9), lo que da lugar a **dialectos**: T-SQL (Microsoft SQL Server), PL/SQL (Oracle), PL/pgSQL (PostgreSQL), el dialecto de MySQL, etc. El **núcleo del estándar** (DQL, DDL, DML básico) es, sin embargo, muy portable entre motores.

> **[DATO CLAVE EXAMEN]** No existe un SGBD 100 % conforme con SQL. La utilidad práctica del estándar no es que todos los motores lo implementen íntegro, sino que fija un **núcleo común** que hace el conocimiento de SQL **transferible** entre productos, y un vocabulario compartido para describir extensiones.

### 2.3. Sintaxis de una sentencia SQL

La sentencia `SELECT` es la más completa del estándar. Su forma general, en orden de **escritura**, es:

```sql
SELECT [DISTINCT] lista_columnas
FROM   tabla_1 [JOIN tabla_2 ON condicion] ...
WHERE  condicion_de_fila
GROUP BY columnas_de_agrupacion
HAVING condicion_de_grupo
ORDER BY columnas [ASC|DESC]
[LIMIT n | FETCH FIRST n ROWS ONLY]
```

Sin embargo, el **orden en que se escribe** una consulta **no** coincide con el **orden lógico en que el motor la evalúa** [SILBERSCHATZ, cap. 3; ELMASRI, cap. 6]:

```
1. FROM      → identifica y combina las tablas de origen
2. JOIN/ON   → aplica las condiciones de reunión
3. WHERE     → filtra filas individuales
4. GROUP BY  → agrupa las filas restantes
5. HAVING    → filtra grupos ya agregados
6. SELECT    → calcula la lista de columnas/expresiones de salida
7. DISTINCT  → elimina filas duplicadas del resultado
8. ORDER BY  → ordena el resultado final
9. LIMIT/OFFSET → recorta el número de filas devueltas
```

> **[DATO CLAVE EXAMEN]** El **orden lógico de evaluación** es una de las preguntas más frecuentes del bloque SQL: `FROM → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT`. Explica, por ejemplo, por qué un **alias** definido en `SELECT` no puede usarse en `WHERE` en la mayoría de motores (el `WHERE` se evalúa **antes** de que exista ese alias), pero sí puede usarse en `ORDER BY` (que se evalúa **después**) [SILBERSCHATZ, cap. 3].

### 2.4. Operaciones de agregación, agrupamiento y filtrado

Las **funciones de agregación** resumen un conjunto de filas en un único valor: `COUNT` (número de filas), `SUM` (suma), `AVG` (media), `MIN` y `MAX` [ISO9075]. `GROUP BY` divide el resultado en **grupos** según el valor de una o varias columnas, y aplica la función de agregación **a cada grupo por separado**.

```sql
SELECT id_distrito, COUNT(*) AS num_tributos, SUM(importe) AS total_recaudado
FROM   TRIBUTO t JOIN CONTRIBUYENTE c ON t.dni_contribuyente = c.dni
GROUP BY id_distrito
HAVING SUM(importe) > 100000
ORDER BY total_recaudado DESC;
```

> **[DATO CLAVE EXAMEN]** La distinción **WHERE frente a HAVING** es de las más preguntadas del temario: **`WHERE`** filtra **filas individuales**, **antes** de que existan los grupos, y **no puede** contener funciones de agregación; **`HAVING`** filtra **grupos ya agregados**, **después** de `GROUP BY`, y es la cláusula correcta para condiciones como `SUM(importe) > 100000`. Toda columna del `SELECT` que no esté en una función de agregación debe figurar en el `GROUP BY` (regla de **dependencia funcional del `GROUP BY`**).

> **[EJEMPLO AYTO MADRID]** «Distritos donde se ha recaudado más de 100.000 € en tributos» exige `HAVING`, porque la condición depende de un **total agregado por distrito**; «tributos liquidados después del 1 de enero» exige `WHERE`, porque la condición se evalúa **fila a fila** antes de agrupar nada.

### 2.5. Tipologías de acoplamiento (Join)

El **join** (reunión) combina filas de dos o más tablas según una condición de relación entre ellas, y es la materialización práctica del operador algebraico de reunión (§1.2) [ELMASRI, cap. 7]:

| Tipo de join | Qué devuelve |
|---|---|
| **INNER JOIN** (o *equi-join*/*theta-join* si la condición no es de igualdad) | Solo las filas que **coinciden** en ambas tablas según la condición |
| **LEFT [OUTER] JOIN** | Todas las filas de la tabla **izquierda**, y las de la derecha que coincidan; `NULL` donde no hay coincidencia |
| **RIGHT [OUTER] JOIN** | Todas las filas de la tabla **derecha**, y las de la izquierda que coincidan; `NULL` donde no hay coincidencia |
| **FULL [OUTER] JOIN** | Todas las filas de **ambas** tablas, coincidan o no; `NULL` en el lado que falte |
| **CROSS JOIN** | El **producto cartesiano** completo: cada fila de una tabla combinada con cada fila de la otra, sin condición |
| **SELF JOIN** | Una tabla unida **consigo misma**, usando alias distintos para cada «copia» lógica |
| **NATURAL JOIN** | Reunión **implícita** por las columnas que comparten el mismo nombre en ambas tablas (desaconsejado: frágil ante cambios de esquema) |

```sql
-- INNER JOIN: solo contribuyentes con tributos liquidados
SELECT c.nombre, t.importe
FROM   CONTRIBUYENTE c
INNER JOIN TRIBUTO t ON c.dni = t.dni_contribuyente;

-- LEFT JOIN: todos los contribuyentes, tengan o no tributos
SELECT c.nombre, t.importe
FROM   CONTRIBUYENTE c
LEFT JOIN TRIBUTO t ON c.dni = t.dni_contribuyente;

-- SELF JOIN: funcionarios que tramitan expedientes del mismo distrito que su compañero
SELECT f1.nombre, f2.nombre
FROM   FUNCIONARIO f1
JOIN   FUNCIONARIO f2 ON f1.id_departamento = f2.id_departamento AND f1.id_funcionario < f2.id_funcionario;
```

> **[DATO CLAVE EXAMEN]** El `LEFT JOIN` es el más preguntado en supuestos prácticos: es la forma estándar de responder a «todos los X, **tengan o no** relación con Y» (todos los contribuyentes, tengan o no tributos liquidados). Cuando la fila de la derecha no existe, sus columnas aparecen como `NULL`, y ese `NULL` es precisamente lo que permite **detectar la ausencia** con `WHERE t.importe IS NULL`.

> **[REFERENCIA CRUZADA]** El **operador de división** del álgebra relacional (§1.2) resuelve en SQL consultas del tipo «contribuyentes que tienen liquidado **todos** los tipos de tributo», que se implementan típicamente combinando `GROUP BY`/`HAVING COUNT(...) = (SELECT COUNT(*) FROM ...)` o con predicados cuantificados (§2.6).

### 2.6. Subconsultas: correlacionadas, no correlacionadas y predicados cuantificados

Una **subconsulta** es un `SELECT` anidado dentro de otra sentencia SQL (en el `WHERE`, el `FROM`, el `SELECT` o incluso el `HAVING`) [DATE-INTRO, cap. 8; SILBERSCHATZ, cap. 3]. Según su relación con la consulta externa:

- **Subconsulta no correlacionada**: es **independiente** de la fila que se esté evaluando en la consulta externa. El motor la puede ejecutar **una sola vez** y reutilizar su resultado.
- **Subconsulta correlacionada**: **referencia una columna de la consulta externa**, por lo que su resultado depende de la fila actual y, en principio, debe **reevaluarse por cada fila candidata** de la consulta externa (aunque el optimizador puede reescribirla como un join en muchos casos).

```sql
-- No correlacionada: el subselect no depende de CONTRIBUYENTE
SELECT nombre
FROM   CONTRIBUYENTE
WHERE  dni IN (SELECT dni_contribuyente FROM TRIBUTO WHERE importe > 500);

-- Correlacionada: el subselect referencia c.dni de la consulta externa
SELECT c.nombre
FROM   CONTRIBUYENTE c
WHERE  EXISTS (SELECT 1 FROM TRIBUTO t WHERE t.dni_contribuyente = c.dni AND t.importe > 500);
```

**Predicados cuantificados**, la forma en que SQL expresa la lógica de existencia y universalidad del cálculo relacional (§1.2):

| Predicado | Significado |
|---|---|
| `IN` / `NOT IN` | El valor pertenece / no pertenece al conjunto devuelto por la subconsulta |
| `EXISTS` / `NOT EXISTS` | La subconsulta devuelve / no devuelve **al menos una fila** (no importa el valor, solo la existencia) |
| `ANY` / `SOME` (sinónimos) | La comparación es cierta para **al menos un** valor devuelto por la subconsulta |
| `ALL` | La comparación es cierta para **todos** los valores devueltos por la subconsulta |

> **[DATO CLAVE EXAMEN]** `EXISTS` suele ser más eficiente que `IN` con subconsultas grandes porque el motor puede **detenerse en la primera coincidencia** (no necesita construir el conjunto completo). `NOT IN`, además, tiene una trampa clásica de examen: si la subconsulta devuelve **algún `NULL`**, `NOT IN` puede no devolver ninguna fila (por la lógica trivaluada de SQL), mientras que `NOT EXISTS` no tiene ese problema.

> **[EJERCICIO RESUELTO]** «Contribuyentes cuyo tributo más alto es mayor que **todos** los tributos del distrito 1» se puede resolver con `ALL`: `SELECT dni_contribuyente FROM TRIBUTO WHERE importe > ALL (SELECT importe FROM TRIBUTO t2 JOIN CONTRIBUYENTE c2 ON t2.dni_contribuyente = c2.dni WHERE c2.id_distrito = 1)`. Es equivalente a comparar con el `MAX()` de esa subconsulta, pero `ALL` generaliza el patrón a cualquier operador de comparación.

### 2.7. Expresiones de Tabla Comunes (CTE) y consultas recursivas

Una **CTE** (*Common Table Expression*, introducida en SQL:1999) es un conjunto de resultados **nombrado y temporal**, definido con la cláusula `WITH`, que existe solo durante la ejecución de la sentencia que lo usa [EISENBERG99]. Mejora la legibilidad frente a subconsultas anidadas y permite **referenciarse varias veces** dentro de la misma consulta.

```sql
WITH tributos_altos AS (
    SELECT dni_contribuyente, SUM(importe) AS total
    FROM   TRIBUTO
    GROUP BY dni_contribuyente
    HAVING SUM(importe) > 1000
)
SELECT c.nombre, ta.total
FROM   CONTRIBUYENTE c
JOIN   tributos_altos ta ON c.dni = ta.dni_contribuyente;
```

Una **CTE recursiva** (`WITH RECURSIVE` en el estándar) se define en dos partes unidas por `UNION ALL` [ISO9075; EISENBERG99]:

1. **Miembro ancla** (*anchor member*): la consulta base, que no se referencia a sí misma; produce el conjunto inicial de filas.
2. **Miembro recursivo**: referencia a **la propia CTE**, y se ejecuta repetidamente **sobre el resultado del paso anterior** hasta que deja de producir filas nuevas —esa ausencia de filas nuevas es la **condición de parada implícita**.

```sql
WITH RECURSIVE organigrama AS (
    -- Miembro ancla: el funcionario raíz (sin superior)
    SELECT id_funcionario, nombre, id_superior, 1 AS nivel
    FROM   FUNCIONARIO
    WHERE  id_superior IS NULL

    UNION ALL

    -- Miembro recursivo: cada subordinado del nivel anterior
    SELECT f.id_funcionario, f.nombre, f.id_superior, o.nivel + 1
    FROM   FUNCIONARIO f
    JOIN   organigrama o ON f.id_superior = o.id_funcionario
)
SELECT * FROM organigrama ORDER BY nivel;
```

> **[DATO CLAVE EXAMEN]** Toda CTE recursiva necesita: (1) un **miembro ancla** sin referencia a sí misma, (2) `UNION ALL` (no `UNION`, que eliminaría duplicados de forma costosa e incluso podría impedir terminar en algunos casos) y (3) un **miembro recursivo** cuya condición de reunión avance hacia la parada. Sin una relación que reduzca el conjunto en cada paso, la recursión no termina.

> **[EJEMPLO AYTO MADRID]** Una CTE recursiva es la forma natural de resolver jerarquías administrativas del Ayuntamiento: la estructura de **Áreas de Gobierno → Direcciones Generales → Subdirecciones** (Tema 3), o un árbol de **expedientes padre-hijo** cuando un expediente se desglosa en sub-expedientes.

### 2.8. Funciones de ventana analíticas

Las **funciones de ventana** (*window functions*, incorporadas en SQL:2003) calculan un valor **para cada fila** en relación con un **grupo de filas relacionado** (su «ventana»), **sin colapsar** las filas del resultado como hace `GROUP BY` [ZEMKE03; SILBERSCHATZ, cap. 5]. Se identifican por la cláusula `OVER (...)`:

```sql
SELECT dni_contribuyente, importe,
       ROW_NUMBER() OVER (PARTITION BY dni_contribuyente ORDER BY importe DESC) AS orden,
       RANK()       OVER (PARTITION BY dni_contribuyente ORDER BY importe DESC) AS puesto,
       SUM(importe) OVER (PARTITION BY dni_contribuyente) AS total_contribuyente
FROM   TRIBUTO;
```

Funciones de ventana más habituales:

| Función | Qué calcula |
|---|---|
| `ROW_NUMBER()` | Un número **secuencial único** dentro de la partición, sin empates |
| `RANK()` | Posición dentro de la partición; **salta números** tras un empate (1, 2, 2, 4…) |
| `DENSE_RANK()` | Posición dentro de la partición; **no salta números** tras un empate (1, 2, 2, 3…) |
| `NTILE(n)` | Reparte las filas de la partición en **n grupos** de tamaño lo más igual posible |
| `LAG(col, n)` | Valor de la columna en la fila **n posiciones anterior** dentro de la partición |
| `LEAD(col, n)` | Valor de la columna en la fila **n posiciones posterior** dentro de la partición |
| `SUM()/AVG()/COUNT()… OVER (...)` | Funciones de agregación aplicadas **como ventana**, sin colapsar filas |

> **[DATO CLAVE EXAMEN]** Diferencia clave con `GROUP BY`: `GROUP BY` **reduce** el número de filas del resultado (una por grupo); una función de ventana **conserva todas las filas originales** y añade una columna calculada sobre su partición. `PARTITION BY` divide en grupos (como `GROUP BY`, pero sin colapsar); el `ORDER BY` **dentro** del `OVER (...)` define el orden en que se calcula la función dentro de cada partición (imprescindible para `ROW_NUMBER`, `RANK`, `LAG`/`LEAD`).

> **[EJEMPLO AYTO MADRID]** «Para cada contribuyente, mostrar cada tributo junto con el porcentaje que representa sobre el total que ese contribuyente paga» es un caso típico de ventana: `importe / SUM(importe) OVER (PARTITION BY dni_contribuyente)`, calculado fila a fila sin perder el detalle de cada tributo individual (que sí se perdería con un `GROUP BY`).

### 2.9. Extensiones del estándar y extensiones procedimentales

El **núcleo** del estándar SQL (DDL, DML, DQL básicos) es muy portable, pero cada motor añade **extensiones propias**, especialmente en dos frentes [MELTON, cap. 1]:

- **Extensiones procedimentales**: el estándar define una sintaxis de referencia, **SQL/PSM** (*Persistent Stored Modules*, ISO/IEC 9075-4), para escribir procedimientos almacenados y disparadores con estructuras de control (§3), pero en la práctica **cada motor implementa su propio dialecto procedimental**, inspirado en SQL/PSM pero no idéntico a él: **PL/SQL** en Oracle `[ORACLE-DOC]`, **T-SQL** (*Transact-SQL*) en Microsoft SQL Server `[MSSQL-DOC]`, **PL/pgSQL** en PostgreSQL `[PSQL-DOC]`, y el dialecto propio de MySQL `[MYSQL-DOC]`.
- **Extensiones funcionales**: soporte de **JSON** (funciones como `JSON_VALUE`, `JSON_TABLE`, normalizadas en SQL:2016), tipos y funciones **geoespaciales** (no forman parte del núcleo del estándar, se rigen por especificaciones del *Open Geospatial Consortium*), y, desde SQL:2023, consultas de **grafos de propiedades** (SQL/PGQ).

> **[DATO CLAVE EXAMEN]** Distinguir con precisión: el **estándar SQL** define el núcleo declarativo (DDL/DML/DQL/DCL/TCL) y una referencia procedimental (SQL/PSM); los **dialectos procedimentales reales** (PL/SQL, T-SQL, PL/pgSQL) son **extensiones no estándar**, cada una con su propia sintaxis, aunque comparten conceptos comunes (variables, cursores, control de flujo, excepciones) que sí están normalizados a nivel conceptual en SQL/PSM [MELTON-PSM].

---

## 3. Procedimientos almacenados

### 3.1. Concepto y características

Un **procedimiento almacenado** (*stored procedure*) es un bloque de código —combinación de sentencias SQL y lógica procedimental (condicionales, bucles, variables)— que se **escribe una vez**, se **compila y almacena en el catálogo del SGBD**, y se **ejecuta en el servidor** bajo demanda, invocado por su nombre [SILBERSCHATZ, cap. 5; MELTON-PSM]. Sus características principales:

- **Reutilización**: se define una vez y lo invocan muchas aplicaciones o usuarios distintos, evitando duplicar la misma lógica SQL en cada cliente.
- **Reducción de tráfico de red**: la aplicación envía **una sola llamada** (nombre + parámetros) en lugar de todo el texto SQL, especialmente relevante cuando la lógica implica varias sentencias encadenadas.
- **Rendimiento**: al estar precompilado y con su **plan de ejecución cacheado** (§3.2), evita repetir el análisis y la optimización en cada llamada.
- **Seguridad y encapsulación**: se puede conceder permiso de **ejecución** (`EXECUTE`) sobre el procedimiento sin dar acceso directo (`SELECT`/`UPDATE`) a las tablas subyacentes, encapsulando la lógica de negocio en la capa de datos.
- **Consistencia**: centraliza reglas de negocio que, de repetirse en cada aplicación cliente, podrían divergir con el tiempo.

Una **función almacenada** (*stored function*) se diferencia del procedimiento en que **siempre devuelve un único valor** mediante `RETURN` y, en la mayoría de motores, puede **usarse dentro de una expresión SQL** (por ejemplo, en un `SELECT`); un procedimiento se **invoca** como sentencia independiente (`CALL`) y puede devolver **cero, uno o varios** valores a través de parámetros de salida (§3.3).

> **[REFERENCIA CRUZADA]** Los **procedimientos, funciones y parámetros** en un lenguaje de programación de propósito general se estudian en el **Tema 18**; este Tema 19 aplica los mismos conceptos —modularidad, parámetros, ámbito— al contexto específico del **servidor de bases de datos**.

### 3.2. Arquitectura y ciclo de vida de ejecución en el servidor

El ciclo de vida de un procedimiento almacenado atraviesa varias fases [GMUW, cap. 8; ORACLE-DOC]:

1. **Creación** (`CREATE PROCEDURE`): el SGBD **analiza** (parsea) el código, comprueba su corrección sintáctica y las referencias a objetos existentes, y lo **almacena** en el **catálogo o diccionario de datos** junto con el resto de metadatos del esquema.
2. **Primera invocación**: en la primera llamada (o, en algunos motores, ya en la creación), el SGBD genera un **plan de ejecución** optimizado para las sentencias SQL internas del procedimiento y lo **cachea** en una zona de memoria compartida (*procedure cache* / *shared pool*).
3. **Invocaciones posteriores**: reutilizan el plan **cacheado**, evitando repetir el análisis y la optimización — la principal ventaja de rendimiento frente a enviar SQL dinámico repetidamente.
4. **Invalidación y recompilación**: el plan se invalida (y se recalcula en la siguiente llamada) cuando cambian las **estadísticas** de las tablas implicadas, se modifican objetos referenciados (por ejemplo, se añade un índice) o el propio código del procedimiento se **altera** (`ALTER PROCEDURE`).
5. **Eliminación** (`DROP PROCEDURE`): borra el procedimiento del catálogo; cualquier objeto que dependiera de él (otro procedimiento, un disparador) queda inválido si no se gestiona la dependencia.

> **[DATO CLAVE EXAMEN]** La ventaja de rendimiento de un procedimiento almacenado frente a enviar la misma consulta repetidamente como SQL dinámico se debe, sobre todo, a la **reutilización del plan de ejecución cacheado**: el coste de analizar y optimizar se paga **una vez**, no en cada llamada.

### 3.3. Parámetros de entrada, salida y retorno

Un procedimiento se comunica con quien lo invoca a través de **parámetros**, cuyo modo determina el sentido del flujo de datos [MELTON-PSM; MSSQL-DOC]:

| Modo | Sentido | Comportamiento |
|---|---|---|
| **IN** | Entrada | El llamador pasa un valor; el procedimiento **solo lo lee**, no puede modificarlo de forma visible al exterior. Es el modo **por defecto**. |
| **OUT** | Salida | El procedimiento **asigna** un valor a este parámetro, que se devuelve al llamador al terminar; su valor de entrada se ignora. |
| **INOUT** (o **IN OUT**) | Entrada y salida | El llamador pasa un valor inicial, el procedimiento puede **leerlo y modificarlo**, y el valor final se devuelve al llamador. |

```
CREAR PROCEDIMIENTO liquidar_tributo(
    ENTRADA dni_contribuyente : cadena,
    ENTRADA importe_base : decimal,
    SALIDA importe_liquidado : decimal,
    ENTRADA_SALIDA num_liquidaciones : entero
)
INICIO
    importe_liquidado = importe_base * 1.0   -- lógica de cálculo
    num_liquidaciones = num_liquidaciones + 1
    INSERTAR EN TRIBUTO (dni_contribuyente, importe) VALORES (dni_contribuyente, importe_liquidado)
FIN
```

Una **función**, a diferencia del procedimiento, siempre incluye una cláusula `RETURN` (o `RETURNS` en su declaración) que fija el **tipo del valor único** que devuelve, y esa `RETURN` **termina la ejecución** de la función en el punto en que se alcanza.

> **[DATO CLAVE EXAMEN]** Regla mnemotécnica: **IN se lee, OUT se escribe, INOUT se lee y se escribe**. Un procedimiento puede tener **varios** parámetros `OUT`/`INOUT` (varios «resultados» de salida); una función solo tiene **un único** valor de retorno vía `RETURN`, aunque puede aceptar tantos parámetros `IN` como necesite.

### 3.4. Gestión de cursores y tipos de cursores

SQL trabaja de forma nativa por **conjuntos** (*set-based*): una sentencia `SELECT` devuelve todas sus filas a la vez. Cuando la lógica procedimental necesita procesar ese resultado **fila a fila** —por ejemplo, para aplicar una regla de negocio distinta a cada fila—, se usa un **cursor**: un puntero que permite recorrer el conjunto de resultados de una consulta de forma controlada [SILBERSCHATZ, cap. 5; ORACLE-DOC].

**Según su gestión:**

- **Cursor implícito**: el propio SGBD lo crea y gestiona automáticamente para sentencias DML simples (un `UPDATE` o `DELETE` sin bucle explícito), sin que el programador lo declare.
- **Cursor explícito**: el programador lo **declara** para un `SELECT` concreto y controla su ciclo de vida paso a paso.

**Según su capacidad de desplazamiento:**

- **Forward-only** (o *unidireccional*): solo puede avanzar hacia adelante, fila a fila; es el más ligero y el más usado.
- **Scrollable** (bidireccional): puede moverse hacia adelante y hacia atrás, e incluso saltar a una fila concreta; consume más recursos.

**Según su sensibilidad a cambios concurrentes:**

- **Sensible**: refleja los cambios que otras transacciones hacen sobre los datos mientras el cursor está abierto.
- **Insensible**: trabaja sobre una **instantánea** (*snapshot*) tomada al abrir el cursor, ajena a cambios posteriores de otras transacciones.

El **ciclo de vida** de un cursor explícito sigue siempre cuatro pasos:

```
DECLARAR CURSOR c_tributos PARA
    SELECT dni_contribuyente, importe FROM TRIBUTO WHERE importe > 1000

ABRIR c_tributos
BUCLE
    OBTENER SIGUIENTE DE c_tributos EN v_dni, v_importe
    SALIR_SI_NO_HAY_MAS_FILAS
    -- procesar la fila v_dni, v_importe
FIN_BUCLE
CERRAR c_tributos
```

> **[DATO CLAVE EXAMEN]** Ciclo del cursor explícito: **DECLARE → OPEN → FETCH (en bucle) → CLOSE**. Un cursor abierto y no cerrado consume recursos del servidor (memoria, bloqueos) mientras dura la sesión; es una **mala práctica** habitual olvidar el `CLOSE`. Siempre que la lógica se pueda expresar como una operación de **conjunto** (un `UPDATE` con `WHERE`, por ejemplo), es preferible a recorrer fila a fila con un cursor, por rendimiento.

> **[EJEMPLO AYTO MADRID]** Recalcular la bonificación de cada expediente de un lote, aplicando una regla distinta según el historial de cada contribuyente, es un caso legítimo de cursor: la regla no se puede expresar como una única sentencia `UPDATE` de conjunto porque depende de una lógica condicional compleja fila a fila.

### 3.5. Estructuras de control de flujo

Los lenguajes procedimentales de los SGBD ofrecen, sobre la base de SQL/PSM, las mismas categorías de control de flujo que cualquier lenguaje de programación (→ **Tema 18**), adaptadas al contexto de la base de datos [MELTON-PSM]:

- **Condicionales**: `SI … ENTONCES … SINO … FIN_SI` (equivalente a `IF … THEN … ELSE`) y `SEGUN_CASO … CUANDO … FIN_SEGUN` (equivalente a `CASE`), para decidir entre varias ramas según el valor de una expresión.
- **Bucles**: `MIENTRAS … FIN_MIENTRAS` (equivalente a `WHILE`, condición evaluada antes de cada iteración), `REPETIR … HASTA` (equivalente a `REPEAT`, condición evaluada después), y bucles `PARA` sobre un cursor (una iteración por cada fila del resultado, encapsulando internamente el `FETCH`).
- **Sentencias de salto**: `SALIR`/`LEAVE` (rompe el bucle actual) e `ITERAR`/`CONTINUE` (salta directamente a la siguiente iteración).

```
CREAR PROCEDIMIENTO clasificar_tributo(ENTRADA importe : decimal, SALIDA categoria : cadena)
INICIO
    SEGUN_CASO
        CUANDO importe < 100 ENTONCES categoria = 'BAJO'
        CUANDO importe < 1000 ENTONCES categoria = 'MEDIO'
        SINO categoria = 'ALTO'
    FIN_SEGUN
FIN
```

> **[REFERENCIA CRUZADA]** Las estructuras `SI/SEGUN_CASO` y `MIENTRAS/REPETIR` son formalmente las mismas que las **instrucciones condicionales**, **bucles y recursividad** del **Tema 18** (Böhm-Jacopini: secuencia, selección, iteración); aquí se aplican dentro de un procedimiento que vive **en el servidor** en lugar de en una aplicación cliente.

### 3.6. Gestión de excepciones y errores

Igual que ocurre con la sintaxis procedimental en general, el estándar SQL/PSM define un modelo conceptual de manejo de errores que cada motor implementa con su propia sintaxis, pero con la misma idea de fondo [MELTON-PSM; MSSQL-DOC]: separar el **código normal** del **código de tratamiento de errores**, en bloques del tipo `INICIO … EXCEPCION CUANDO … FIN` (o `TRY/CATCH` en la terminología de otros lenguajes).

- **Códigos de error estandarizados**: el estándar SQL define `SQLSTATE`, un código de cinco caracteres normalizado (p. ej. `'02000'` indica «no se encontraron datos»); muchos motores añaden además su propio código numérico (`SQLCODE`) específico del producto.
- **Excepciones definidas por el usuario**: además de los errores del sistema, el propio código del procedimiento puede **lanzar** (`LANZAR`/`RAISE`/`THROW`) un error de negocio propio, con un mensaje descriptivo, cuando detecta una condición inválida (por ejemplo, un importe negativo).
- **Propagación y transacciones**: si una excepción no se captura dentro del procedimiento, se **propaga** hacia quien lo invocó; si la excepción ocurre dentro de una transacción no confirmada, buena parte de las plataformas exigen o recomiendan un `ROLLBACK` explícito en el manejador para no dejar la transacción a medias.

```
CREAR PROCEDIMIENTO liquidar_tributo(ENTRADA importe : decimal)
INICIO
    SI importe < 0 ENTONCES
        LANZAR ERROR 'El importe de un tributo no puede ser negativo'
    FIN_SI

    INICIO_BLOQUE_PROTEGIDO
        INSERTAR EN TRIBUTO (importe) VALORES (importe)
    EXCEPCION CUANDO ERROR_DE_INTEGRIDAD ENTONCES
        DESHACER
        LANZAR ERROR 'No se pudo liquidar el tributo: violación de integridad'
    FIN_BLOQUE_PROTEGIDO
FIN
```

> **[DATO CLAVE EXAMEN]** Un manejador de excepciones bien diseñado debe: (1) capturar el error **más específico** posible antes que uno genérico, (2) decidir explícitamente si hace `ROLLBACK` (deshacer) o puede continuar, y (3) **no silenciar** el error sin registrarlo — un `CATCH` vacío que «traga» la excepción es una de las peores prácticas de programación de bases de datos, porque oculta fallos que después son muy difíciles de depurar.

---

## 4. Eventos y disparadores

### 4.1. Definición, arquitectura orientada a eventos

Un **disparador** o **trigger** es un tipo especial de procedimiento almacenado que **no se invoca explícitamente** por su nombre, sino que el SGBD lo **ejecuta automáticamente** («se dispara») en respuesta a un **evento** que ocurre sobre una tabla o vista: una operación DML (`INSERT`, `UPDATE`, `DELETE`) o, en algunos motores, un evento del propio sistema o del calendario [SILBERSCHATZ, cap. 5; ELMASRI, cap. 5]. Es la aplicación, dentro del SGBD, de una **arquitectura orientada a eventos**: en lugar de que la aplicación cliente tenga que acordarse de ejecutar una acción derivada cada vez que modifica un dato, esa acción queda **garantizada por el propio motor**, con independencia de qué aplicación o usuario haya originado el cambio.

> **[DATO CLAVE EXAMEN]** La diferencia esencial entre un **procedimiento almacenado** y un **disparador**: el procedimiento se ejecuta cuando **alguien lo llama** explícitamente (`CALL`); el disparador se ejecuta cuando **ocurre el evento** para el que está definido, sin que nadie lo invoque directamente. Un disparador **no admite parámetros** de entrada al estilo de un procedimiento: recibe implícitamente el contexto del evento (los valores `OLD`/`NEW` de la fila afectada).

### 4.2. Clasificación de los disparadores

Los disparadores se clasifican por tres criterios independientes, que se combinan entre sí [ISO9075; MYSQL-DOC]:

#### BEFORE y AFTER

- **BEFORE**: se ejecuta **antes** de que el SGBD aplique físicamente el cambio. Permite **validar** o **modificar** los valores que se van a escribir (por ejemplo, normalizar un texto a mayúsculas, calcular un valor derivado) e incluso **cancelar** la operación si la lógica del disparador detecta un error.
- **AFTER**: se ejecuta **después** de que el cambio ya se ha aplicado. Es la elección natural para acciones **derivadas** que necesitan que el dato ya exista tal cual quedó escrito, como registrar una entrada de auditoría.

#### De fila y de sentencia

- **De fila** (`FOR EACH ROW`): se ejecuta **una vez por cada fila** afectada por la sentencia que lo dispara, con acceso a los valores `OLD` (el valor antes del cambio; no existe en `INSERT`) y `NEW` (el valor después del cambio; no existe en `DELETE`).
- **De sentencia** (`FOR EACH STATEMENT`): se ejecuta **una única vez** por sentencia, con independencia de cuántas filas haya afectado (incluso si afecta a cero filas o a diez mil), sin acceso directo a los valores individuales de cada fila.

#### INSTEAD OF sobre vistas

Un disparador **`INSTEAD OF`** **sustituye por completo** la operación DML sobre una **vista**, en lugar de ejecutarse antes o después de ella. Es imprescindible cuando la vista **no es directamente actualizable** —por ejemplo, una vista que combina varias tablas con un `JOIN`—: el disparador `INSTEAD OF` contiene la lógica que traduce el `INSERT`/`UPDATE`/`DELETE` sobre la vista en las operaciones equivalentes sobre las **tablas base** subyacentes.

#### Eventos programados

Además de los disparadores ligados a DML, la mayoría de motores ofrecen **eventos programados** (*scheduled events* / *jobs*), que se disparan por **tiempo** en lugar de por una modificación de datos: el *Event Scheduler* de MySQL `[MYSQL-DOC]`, el *SQL Server Agent* `[MSSQL-DOC]`, `DBMS_SCHEDULER` de Oracle `[ORACLE-DOC]` o `pg_cron` en PostgreSQL `[PSQL-DOC]` son ejemplos de esta capacidad, típicamente usada para tareas de mantenimiento periódico (purgas, recálculo de agregados, generación de informes nocturnos).

> **[DATO CLAVE EXAMEN]** Combinaciones posibles: `BEFORE`/`AFTER` × `FOR EACH ROW`/`FOR EACH STATEMENT` da **cuatro** tipos de disparador DML; `INSTEAD OF` es un caso aparte, exclusivo de vistas no actualizables directamente, y los eventos programados son un cuarto tipo, disparado por **tiempo** y no por DML.

> **[EJEMPLO AYTO MADRID]** Una vista `VISTA_EXPEDIENTES_COMPLETA` que combina `EXPEDIENTE`, `CONTRIBUYENTE` y `FUNCIONARIO` con varios `JOIN` no admite `UPDATE` directo en la mayoría de motores; un disparador `INSTEAD OF UPDATE` sobre esa vista traduce la actualización recibida a los `UPDATE` correctos sobre `EXPEDIENTE` (y, si procede, sobre `CONTRIBUYENTE`), de forma transparente para quien consulta la vista.

### 4.3. Casos de uso de eventos y disparadores

#### Auditoría y trazabilidad

El caso de uso más habitual: un disparador `AFTER INSERT/UPDATE/DELETE` de fila que escribe en una **tabla de auditoría** quién hizo el cambio, cuándo, y qué valores tenía antes y después (`OLD`/`NEW`), sin que la aplicación cliente tenga que implementar esa lógica en cada punto donde modifica el dato.

```
CREAR DISPARADOR trg_auditoria_tributo
DESPUES DE ACTUALIZAR EN TRIBUTO
PARA_CADA_FILA
INICIO
    INSERTAR EN AUDITORIA_TRIBUTO (id_tributo, importe_anterior, importe_nuevo, usuario, fecha)
    VALORES (ANTIGUO.id_tributo, ANTIGUO.importe, NUEVO.importe, USUARIO_ACTUAL(), FECHA_ACTUAL())
FIN
```

#### Mantenimiento de la integridad

Reglas de negocio **complejas o cruzadas entre tablas** que las restricciones declarativas (`CHECK`, clave foránea) no pueden expresar por sí solas —por ejemplo, «un expediente no puede pasar al estado `CERRADO` si tiene tareas pendientes en otra tabla»— se implementan con un disparador `BEFORE` que valida la condición y, si no se cumple, cancela la operación lanzando un error.

#### Automatización de procesos

Mantenimiento de **columnas derivadas o desnormalizadas** que dependen de otras tablas: actualizar un contador de expedientes abiertos en la tabla `DISTRITO` cada vez que se inserta un `EXPEDIENTE`, sincronizar un campo `fecha_ultima_modificacion` automáticamente en cada `UPDATE`, o generar una notificación (insertar en una cola de mensajes) cuando cambia el estado de un expediente a un valor crítico.

> **[REFERENCIA CRUZADA]** La **auditoría y trazabilidad** conecta con el **Esquema Nacional de Seguridad** [ENS], que exige registro y trazabilidad de las operaciones sobre datos sensibles; y con el **Tema 6** (Ley 19/2013 de transparencia), en la medida en que la trazabilidad de las modificaciones sustenta la rendición de cuentas de la actuación administrativa.

### 4.4. Riesgos, limitaciones y buenas prácticas

Los disparadores son una herramienta potente, pero conllevan riesgos bien documentados que justifican usarlos con moderación [SILBERSCHATZ, cap. 5; GMUW, cap. 8]:

- **Lógica oculta y difícil de rastrear**: a diferencia de una sentencia SQL explícita en la aplicación, un disparador se ejecuta de forma **implícita**; quien lee el código de la aplicación puede no darse cuenta de que una simple `UPDATE` desencadena efectos adicionales, lo que dificulta el mantenimiento y la depuración.
- **Efectos en cascada («trigger chain»)**: un disparador puede provocar una operación DML sobre otra tabla que, a su vez, dispare **otro** trigger, y así sucesivamente. Estas cadenas son difíciles de razonar y depurar, y en el peor caso pueden derivar en **recursión no controlada** (un trigger que, directa o indirectamente, vuelve a modificar la tabla que lo disparó).
- **Impacto en el rendimiento**: cada disparador añade **sobrecarga** a la operación DML que lo activa; en tablas de alto volumen de escritura, disparadores mal optimizados (por ejemplo, uno de fila que ejecuta una subconsulta costosa) pueden degradar notablemente el rendimiento.
- **Orden de ejecución no siempre garantizado**: cuando varios disparadores del mismo tipo (por ejemplo, dos `AFTER INSERT`) actúan sobre la misma tabla, el estándar no garantiza un orden determinado salvo que el motor ofrezca un mecanismo explícito para fijarlo (cláusulas del tipo `FOLLOWS`/`PRECEDES` en algunos motores).

**Buenas prácticas** ampliamente aceptadas: documentar claramente la existencia y el propósito de cada disparador (idealmente en un catálogo accesible al equipo, no solo en el propio código), mantener su lógica **mínima y centrada** en una sola responsabilidad, evitar cascadas profundas de disparadores encadenados, preferir restricciones declarativas (`CHECK`, clave foránea, `UNIQUE`) siempre que basten para expresar la regla, y probar explícitamente el comportamiento ante operaciones **masivas** (miles de filas en una sola sentencia), no solo ante cambios de una fila.

> **[DATO CLAVE EXAMEN]** Los disparadores deben reservarse para lo que **no se puede expresar de forma declarativa**: si una regla se puede implementar con una restricción `CHECK` o una clave foránea, esa opción es preferible a un disparador, porque el optimizador la puede aprovechar mejor y el comportamiento es más predecible y menos propenso a efectos ocultos [ISO25010].

---

## 5. Tendencias actuales en los lenguajes de interrogación

> **Material complementario.** El enunciado oficial de este tema no nombra este apartado. Se mantiene porque esta materia envejece deprisa y conviene conocer su estado actual, pero lo exigible es lo que enumera el título del tema.

El estándar SQL sigue evolucionando para responder a necesidades que no existían cuando se formalizó el modelo relacional clásico, sin que esto reste vigencia a los fundamentos de este tema [MELTON, cap. 12]:

- **Convergencia SQL / NoSQL**: el soporte nativo de **JSON** en el estándar (SQL:2016, funciones `JSON_VALUE`, `JSON_TABLE`) permite almacenar y consultar datos semiestructurados **dentro** de un motor relacional, reduciendo la necesidad de elegir entre un SGBD relacional y uno documental para un mismo proyecto (→ **Tema 15**, SGBD NoSQL).
- **Consultas de grafos sobre el modelo relacional**: **SQL/PGQ** (SQL:2023) incorpora una sintaxis declarativa para expresar consultas de **grafos de propiedades** (nodos y relaciones) directamente sobre tablas relacionales, sin necesidad de migrar a un motor de grafos especializado.
- **Motores analíticos (OLAP) sobre grandes volúmenes**: la extensión de las funciones de ventana analíticas (§2.8) y de agregaciones avanzadas (`ROLLUP`, `CUBE`, `GROUPING SETS`) responde a la creciente necesidad de análisis multidimensional directamente en SQL, sin exportar los datos a herramientas externas.
- **Generación de SQL desde la capa de aplicación**: el uso de **ORM** y *query builders* (§1.5) sigue creciendo en aplicaciones modernas, lo que hace todavía más relevante entender el SQL que esas herramientas generan «por debajo», para poder diagnosticar problemas de rendimiento que la capa de abstracción no siempre deja ver.
- **Consultas continuas sobre flujos de datos**: en arquitecturas orientadas a eventos a gran escala, han surgido variantes de SQL para expresar consultas **continuas** sobre flujos de datos en movimiento (*streaming SQL*), en lugar de sobre datos ya almacenados, aplicando conceptos como ventanas temporales que recuerdan, conceptualmente, a las funciones de ventana de §2.8.

> **[DATO CLAVE EXAMEN]** Estas tendencias **amplían** el estándar SQL sin sustituir sus fundamentos: el modelo relacional (§1), el núcleo declarativo del `SELECT` (§2.3-2.8) y la lógica de procedimientos/disparadores (§3-4) siguen siendo la base sobre la que se construyen todas estas extensiones más recientes.

---

*Fin del contenido teórico del Tema 19. Continúa en tema-19-diagramas.md (12 diagramas SVG), tema-19-test.md (60 preguntas) y tema-19-caso-practico.md (3 casos Ayuntamiento de Madrid).*
