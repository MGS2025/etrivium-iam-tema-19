# Tema 19 — Test de Autoevaluación

> **Título**: Lenguajes de interrogación de bases de datos. El estándar ANSI SQL. Procedimientos almacenados. Eventos y disparadores.
> **Formato**: 60 preguntas tipo test A/B/C (formato oficial oposición)
> **Nivel**: C1 — Técnico Auxiliar TIC, Ayuntamiento de Madrid
> **Versión**: v1.0 — Pendiente validación
> **Fecha**: 2026-07-13
> **Fuentes**: ver tema-19-fuentes.md

---

## Instrucciones

- Cada pregunta tiene **3 opciones** (A, B, C). Solo una es correcta.
- Penalización en examen real: respuesta incorrecta descuenta **1/3** del valor de una correcta.
- Tiempo orientativo: 1 minuto por pregunta.
- Distribución: Modelo relacional, álgebra y cálculo (P1-P12), Tipología de lenguajes y utilización en apps (P13-P20), Estándar SQL: origen y sintaxis (P21-P28), Agregación y joins (P29-P36), Subconsultas, CTE y ventanas (P37-P44), Extensiones (P45-P46), Procedimientos almacenados (P47-P52), Eventos y disparadores (P53-P58), Tendencias (P59-P60).

---

### Pregunta 1

**¿Qué produce, en el modelo relacional, un lenguaje de interrogación al operar sobre una o más relaciones?**

A) Otra relación (propiedad de cierre)
B) Un fichero binario sin estructura
C) Un objeto en memoria fuera del modelo relacional

<details><summary>Respuesta</summary>

**Correcta: A) Otra relación (propiedad de cierre)** El resultado de cualquier operación sobre relaciones es, a su vez, una relación, lo que permite componer consultas dentro de otras consultas.

*Referencia: §1.1 [CODD70]*
</details>

---

### Pregunta 2

**En el vocabulario del modelo relacional, ¿qué es una tupla?**

A) El nombre de una columna
B) Una fila de la relación
C) El esquema completo de la tabla

<details><summary>Respuesta</summary>

**Correcta: B) Una fila de la relación** La relación es la tabla completa; el atributo es la columna; la tupla es cada fila individual.

*Referencia: §1.1 [CODD70]*
</details>

---

### Pregunta 3

**¿Qué demostró Codd sobre el álgebra relacional y el cálculo relacional seguro (safe)?**

A) Que el álgebra es más potente que el cálculo
B) Que el cálculo relacional no es formalizable matemáticamente
C) Que ambos son expresivamente equivalentes (completitud relacional)

<details><summary>Respuesta</summary>

**Correcta: C) Que ambos son expresivamente equivalentes (completitud relacional)** Todo lo expresable en álgebra relacional lo es en cálculo relacional seguro, y viceversa; esta equivalencia fundamenta el criterio de completitud relacional para evaluar otros lenguajes de consulta.

*Referencia: §1.2 [CODD70; DATE-REL]*
</details>

---

### Pregunta 4

**El operador de selección (σ) del álgebra relacional actúa sobre:**

A) Filas, extrayendo las que cumplen una condición
B) Columnas, eliminando duplicados
C) Dos relaciones, combinándolas por producto cartesiano

<details><summary>Respuesta</summary>

**Correcta: A) Filas, extrayendo las que cumplen una condición** La proyección (π) es la que opera sobre columnas; el producto cartesiano (×) combina dos relaciones completas.

*Referencia: §1.2 [DATE-INTRO, cap. 6]*
</details>

---

### Pregunta 5

**¿Qué requisito deben cumplir dos relaciones para poder aplicarles unión, diferencia o intersección?**

A) Tener el mismo número de filas
B) Ser compatibles por unión (mismo esquema de atributos)
C) Compartir una clave primaria

<details><summary>Respuesta</summary>

**Correcta: B) Ser compatibles por unión (mismo esquema de atributos)** Los operadores binarios de conjuntos exigen que ambas relaciones tengan el mismo número y tipo de atributos; no importa el número de filas.

*Referencia: §1.2 [GMUW, cap. 2]*
</details>

---

### Pregunta 6

**La reunión (join, ⋈) del álgebra relacional se define formalmente como:**

A) Un operador unario sobre una sola relación
B) Sinónimo exacto de la unión (∪)
C) Un producto cartesiano seguido de una selección por la condición de reunión

<details><summary>Respuesta</summary>

**Correcta: C) Un producto cartesiano seguido de una selección por la condición de reunión** El join es un operador derivado: se puede expresar combinando el producto cartesiano (×) y la selección (σ).

*Referencia: §1.2 [DATE-INTRO, cap. 6]*
</details>

---

### Pregunta 7

**¿Qué tipo de consulta resuelve típicamente el operador de división (÷) del álgebra relacional?**

A) «Qué X están relacionados con TODOS los Y»
B) «Cuántas filas tiene la tabla»
C) «Ordenar las filas por un atributo»

<details><summary>Respuesta</summary>

**Correcta: A) «Qué X están relacionados con TODOS los Y»** Es el operador derivado propio de consultas de universalidad, como «contribuyentes que tienen liquidados todos los tipos de tributo».

*Referencia: §1.2 [DATE-INTRO, cap. 6]*
</details>

---

### Pregunta 8

**En el cálculo relacional de dominios, las variables representan:**

A) Tuplas completas de una relación
B) Valores de un dominio (una columna concreta)
C) Nombres de tablas del esquema

<details><summary>Respuesta</summary>

**Correcta: B) Valores de un dominio (una columna concreta)** Frente al cálculo de tuplas (variables = tuplas completas), el cálculo de dominios usa variables que representan valores de una columna; es la base conceptual de QBE.

*Referencia: §1.2 [CODD70]*
</details>

---

### Pregunta 9

**¿Cuál de las siguientes afirmaciones sobre álgebra y cálculo relacional es correcta?**

A) El álgebra es declarativa y el cálculo procedimental
B) Solo el álgebra relacional tiene fundamento matemático formal
C) El álgebra es procedimental (dice CÓMO) y el cálculo es declarativo (dice QUÉ)

<details><summary>Respuesta</summary>

**Correcta: C) El álgebra es procedimental (dice CÓMO) y el cálculo es declarativo (dice QUÉ)** Es la distinción fundamental entre ambos lenguajes equivalentes.

*Referencia: §1.2 [DATE-REL, cap. 6]*
</details>

---

### Pregunta 10

**SQL, respecto al álgebra y el cálculo relacional, se describe como:**

A) Declarativo en su sintaxis, pero traducido internamente a operadores algebraicos por el optimizador
B) Puramente procedimental, sin relación con el cálculo relacional
C) Un lenguaje ajeno al modelo relacional de Codd

<details><summary>Respuesta</summary>

**Correcta: A) Declarativo en su sintaxis, pero traducido internamente a operadores algebraicos por el optimizador** SQL se parece al cálculo relacional en superficie, pero el motor lo ejecuta como un árbol de operadores algebraicos.

*Referencia: §1.4 [DATE-REL, cap. 6]*
</details>

---

### Pregunta 11

**La propiedad de cierre (closure) del álgebra relacional permite:**

A) Ejecutar consultas sin necesidad de optimizador
B) Componer operadores anidando el resultado de uno como entrada de otro
C) Prescindir del esquema de las relaciones

<details><summary>Respuesta</summary>

**Correcta: B) Componer operadores anidando el resultado de uno como entrada de otro** Como cada operación produce otra relación, su resultado puede alimentar directamente a la siguiente operación.

*Referencia: §1.2 [DATE-INTRO, cap. 6]*
</details>

---

### Pregunta 12

**¿Quién formuló por primera vez el modelo relacional en el que se apoyan el álgebra y el cálculo relacional?**

A) Chamberlin y Boyce, en el artículo de SEQUEL
B) Melton y Simon, en su guía del estándar SQL
C) E. F. Codd, en su artículo de 1970 sobre bases de datos relacionales

<details><summary>Respuesta</summary>

**Correcta: C) E. F. Codd, en su artículo de 1970 sobre bases de datos relacionales** *A Relational Model of Data for Large Shared Data Banks* (CACM, 1970) es el artículo fundacional del modelo relacional.

*Referencia: §1.1 [CODD70]*
</details>

---

### Pregunta 13

**¿Qué sublenguaje se encarga de crear, modificar y eliminar la estructura del esquema (tablas, índices, vistas)?**

A) DDL
B) DML
C) DCL

<details><summary>Respuesta</summary>

**Correcta: A) DDL** El Lenguaje de Definición de Datos opera sobre el catálogo del SGBD con `CREATE`, `ALTER`, `DROP` y `TRUNCATE`.

*Referencia: §1.3 [SILBERSCHATZ, cap. 3]*
</details>

---

### Pregunta 14

**MERGE, INSERT, UPDATE y DELETE pertenecen al sublenguaje:**

A) DDL
B) DML
C) TCL

<details><summary>Respuesta</summary>

**Correcta: B) DML** Son las sentencias que modifican el contenido de las tablas sin alterar su estructura.

*Referencia: §1.3 [ELMASRI, cap. 6]*
</details>

---

### Pregunta 15

**Según el estándar ISO/IEC 9075, la sentencia SELECT se clasifica formalmente dentro de:**

A) DCL
B) DDL
C) DML (la separación DQL es una convención didáctica)

<details><summary>Respuesta</summary>

**Correcta: C) DML (la separación DQL es una convención didáctica)** El estándar no separa formalmente DQL; es una distinción muy usada en manuales y oposiciones por la naturaleza claramente distinta del SELECT (no modifica datos) frente al resto del DML.

*Referencia: §1.3 [ISO9075]*
</details>

---

### Pregunta 16

**GRANT y REVOKE son sentencias del sublenguaje:**

A) DCL
B) TCL
C) DQL

<details><summary>Respuesta</summary>

**Correcta: A) DCL** El Lenguaje de Control de Datos gestiona permisos: `GRANT` concede privilegios y `REVOKE` los retira.

*Referencia: §1.3 [SILBERSCHATZ, cap. 3]*
</details>

---

### Pregunta 17

**COMMIT, ROLLBACK y SAVEPOINT pertenecen a:**

A) DDL
B) TCL
C) DML

<details><summary>Respuesta</summary>

**Correcta: B) TCL** El Lenguaje de Control de Transacciones gestiona el ciclo de vida de la transacción y sustenta las propiedades ACID.

*Referencia: §1.3 [ACID83]*
</details>

---

### Pregunta 18

**¿Cuál de estas afirmaciones distingue correctamente lo procedimental de lo declarativo?**

A) Lo declarativo especifica el procedimiento paso a paso
B) Ambos enfoques son equivalentes y no se distinguen en la práctica
C) Lo procedimental dice CÓMO obtener el resultado; lo declarativo dice QUÉ se quiere, y el sistema decide cómo

<details><summary>Respuesta</summary>

**Correcta: C) Lo procedimental dice CÓMO obtener el resultado; lo declarativo dice QUÉ se quiere, y el sistema decide cómo** Es la distinción central de §1.4, aplicable tanto al álgebra/cálculo como a SQL frente a un lenguaje de programación clásico.

*Referencia: §1.4 [MELTON, cap. 1]*
</details>

---

### Pregunta 19

**El acceso a datos mediante ODBC/JDBC tiene como objetivo principal:**

A) Ofrecer una interfaz común de acceso independiente del SGBD concreto, mediante un driver específico
B) Sustituir completamente al SQL por un lenguaje de objetos
C) Eliminar la necesidad de un servidor de bases de datos

<details><summary>Respuesta</summary>

**Correcta: A) Ofrecer una interfaz común de acceso independiente del SGBD concreto, mediante un driver específico** ODBC (C) y JDBC (Java) siguen el mismo patrón: driver del motor + interfaz común para la aplicación cliente.

*Referencia: §1.5 [SILBERSCHATZ, cap. 10]*
</details>

---

### Pregunta 20

**En el contexto del acceso a datos desde aplicaciones, un ORM (Object-Relational Mapping):**

A) Sustituye el modelo relacional por un modelo de grafos
B) Traduce automáticamente entre objetos del lenguaje y filas de tablas relacionales, generando el SQL correspondiente
C) Es un sinónimo de SQL embebido estático

<details><summary>Respuesta</summary>

**Correcta: B) Traduce automáticamente entre objetos del lenguaje y filas de tablas relacionales, generando el SQL correspondiente** Facilita la productividad, aunque puede generar SQL subóptimo si se usa sin entender la traducción resultante.

*Referencia: §1.5 [SILBERSCHATZ, cap. 10]*
</details>

---

### Pregunta 21

**SQL nace originalmente con el nombre de:**

A) QUEL
B) System R
C) SEQUEL, diseñado por Chamberlin y Boyce sobre el prototipo System R

<details><summary>Respuesta</summary>

**Correcta: C) SEQUEL, diseñado por Chamberlin y Boyce sobre el prototipo System R** El nombre se abrevió después a SQL por un conflicto de marca registrada.

*Referencia: §2.1 [CHAMBERLIN74]*
</details>

---

### Pregunta 22

**El primer estándar ANSI de SQL se publicó en:**

A) 1986 (SQL-86), adoptado por ISO en 1987
B) 1974, con la publicación de SEQUEL
C) 1999, con SQL3

<details><summary>Respuesta</summary>

**Correcta: A) 1986 (SQL-86), adoptado por ISO en 1987** Es el primer estándar formal, tras años de dialectos comerciales dispares.

*Referencia: §2.1 [MELTON, cap. 1]*
</details>

---

### Pregunta 23

**La edición del estándar SQL que introdujo la sintaxis moderna de JOIN explícito y las subconsultas tal como se usan hoy fue:**

A) SQL-86
B) SQL-92 (SQL2)
C) SQL:2003

<details><summary>Respuesta</summary>

**Correcta: B) SQL-92 (SQL2)** Fue la revisión más citada del estándar por su gran expansión de la sintaxis de consulta.

*Referencia: §2.1 [MELTON, cap. 1]*
</details>

---

### Pregunta 24

**¿Qué aportó SQL:1999 (SQL3) al estándar?**

A) Las funciones de ventana analíticas
B) El soporte nativo de JSON
C) La recursividad (CTE recursivas) y los disparadores

<details><summary>Respuesta</summary>

**Correcta: C) La recursividad (CTE recursivas) y los disparadores** Dos de los cuatro bloques de este mismo Tema 19 se incorporaron formalmente en esta edición.

*Referencia: §2.1 [EISENBERG99]*
</details>

---

### Pregunta 25

**Desde SQL:1999, el esquema de niveles de conformidad de SQL-92 (Entry/Intermediate/Full) se sustituyó por:**

A) Un núcleo («core») obligatorio más «features» opcionales identificadas por código
B) Un único nivel obligatorio para todos los SGBD
C) La certificación ISO 25010 de calidad del software

<details><summary>Respuesta</summary>

**Correcta: A) Un núcleo («core») obligatorio más «features» opcionales identificadas por código** Sustituye el esquema «todo o nada» de tres niveles por una declaración más granular de conformidad.

*Referencia: §2.2 [MELTON, cap. 2]*
</details>

---

### Pregunta 26

**¿Qué son PL/SQL, T-SQL y PL/pgSQL?**

A) Ediciones sucesivas del estándar ISO/IEC 9075
B) Dialectos procedimentales propios de cada motor (Oracle, SQL Server, PostgreSQL), no estándar
C) Sinónimos del lenguaje SQL/PSM, idénticos entre sí

<details><summary>Respuesta</summary>

**Correcta: B) Dialectos procedimentales propios de cada motor (Oracle, SQL Server, PostgreSQL), no estándar** Se inspiran en la referencia SQL/PSM del estándar, pero cada uno tiene su propia sintaxis.

*Referencia: §2.9 [MELTON-PSM]*
</details>

---

### Pregunta 27

**El orden LÓGICO en que un motor SQL evalúa las cláusulas de un SELECT es:**

A) SELECT → FROM → WHERE → GROUP BY
B) WHERE → SELECT → FROM → ORDER BY
C) FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY

<details><summary>Respuesta</summary>

**Correcta: C) FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY** Este orden lógico no coincide con el orden en que se escribe la sentencia.

*Referencia: §2.3 [SILBERSCHATZ, cap. 3]*
</details>

---

### Pregunta 28

**¿Por qué, en la mayoría de motores, un alias definido en el SELECT no puede usarse en la cláusula WHERE de la misma consulta?**

A) Porque WHERE se evalúa antes de que el motor calcule las expresiones del SELECT, según el orden lógico de evaluación
B) Porque los alias no existen en el estándar SQL
C) Porque WHERE solo admite nombres de tabla, nunca de columna

<details><summary>Respuesta</summary>

**Correcta: A) Porque WHERE se evalúa antes de que el motor calcule las expresiones del SELECT, según el orden lógico de evaluación** Por la misma razón, sí puede usarse en ORDER BY, que se evalúa después.

*Referencia: §2.3 [SILBERSCHATZ, cap. 3]*
</details>

---

### Pregunta 29

**¿Qué diferencia principal existe entre WHERE y HAVING?**

A) Son sinónimos intercambiables en cualquier posición
B) WHERE filtra filas antes de agrupar; HAVING filtra grupos después de la agregación
C) HAVING se ejecuta siempre antes que WHERE

<details><summary>Respuesta</summary>

**Correcta: B) WHERE filtra filas antes de agrupar; HAVING filtra grupos después de la agregación** Es una de las distinciones básicas del bloque SQL.

*Referencia: §2.4 [SILBERSCHATZ, cap. 3]*
</details>

---

### Pregunta 30

**¿Cuál de estas funciones NO es una función de agregación estándar de GROUP BY?**

A) SUM
B) AVG
C) ROW_NUMBER (es una función de ventana, no de agregación de GROUP BY)

<details><summary>Respuesta</summary>

**Correcta: C) ROW_NUMBER (es una función de ventana, no de agregación de GROUP BY)** `ROW_NUMBER()` requiere la cláusula `OVER` y no colapsa filas, a diferencia de `SUM`/`AVG`/`COUNT`/`MIN`/`MAX`.

*Referencia: §2.4-2.8 [ISO9075; ZEMKE03]*
</details>

---

### Pregunta 31

**Toda columna del SELECT que no forme parte de una función de agregación debe aparecer, por regla general, en:**

A) La cláusula GROUP BY
B) La cláusula ORDER BY únicamente
C) La cláusula WHERE

<details><summary>Respuesta</summary>

**Correcta: A) La cláusula GROUP BY** Es la regla de dependencia funcional del agrupamiento: toda columna «suelta» del SELECT debe estar entre las columnas de agrupación.

*Referencia: §2.4 [SILBERSCHATZ, cap. 3]*
</details>

---

### Pregunta 32

**Un INNER JOIN devuelve:**

A) Todas las filas de la tabla izquierda, coincidan o no
B) Solo las filas que coinciden en ambas tablas según la condición de reunión
C) El producto cartesiano completo sin condición

<details><summary>Respuesta</summary>

**Correcta: B) Solo las filas que coinciden en ambas tablas según la condición de reunión** Es el join más restrictivo de los tres «outer» y no produce valores NULL por ausencia de coincidencia.

*Referencia: §2.5 [ELMASRI, cap. 7]*
</details>

---

### Pregunta 33

**¿Qué join es la forma estándar de responder a «todos los contribuyentes, tengan o no tributos liquidados»?**

A) INNER JOIN
B) CROSS JOIN
C) LEFT [OUTER] JOIN

<details><summary>Respuesta</summary>

**Correcta: C) LEFT [OUTER] JOIN** Devuelve todas las filas de la tabla izquierda y, si no hay coincidencia en la derecha, sus columnas aparecen como NULL.

*Referencia: §2.5 [ELMASRI, cap. 7]*
</details>

---

### Pregunta 34

**Un SELF JOIN es:**

A) Una tabla unida consigo misma, usando alias distintos para cada «copia» lógica
B) Un join que solo puede hacerse con la cláusula NATURAL JOIN
C) Sinónimo de CROSS JOIN

<details><summary>Respuesta</summary>

**Correcta: A) Una tabla unida consigo misma, usando alias distintos para cada «copia» lógica** Es imprescindible dar alias distintos a las dos referencias de la misma tabla para poder distinguirlas en la consulta.

*Referencia: §2.5 [ELMASRI, cap. 7]*
</details>

---

### Pregunta 35

**El NATURAL JOIN se desaconseja en la práctica profesional principalmente porque:**

A) No existe en el estándar SQL
B) Es una reunión implícita por columnas homónimas, frágil ante cambios de esquema
C) Solo funciona con tablas vacías

<details><summary>Respuesta</summary>

**Correcta: B) Es una reunión implícita por columnas homónimas, frágil ante cambios de esquema** Un cambio de nombre de columna, o añadir una columna homónima nueva, altera silenciosamente el resultado.

*Referencia: §2.5 [ELMASRI, cap. 7]*
</details>

---

### Pregunta 36

**En un FULL OUTER JOIN entre CONTRIBUYENTE y TRIBUTO, una fila de CONTRIBUYENTE sin tributos asociados aparecerá:**

A) Excluida del resultado
B) Duplicada
C) Con las columnas de TRIBUTO a NULL

<details><summary>Respuesta</summary>

**Correcta: C) Con las columnas de TRIBUTO a NULL** El FULL OUTER JOIN conserva todas las filas de ambas tablas, coincidan o no, rellenando con NULL el lado que falte.

*Referencia: §2.5 [ELMASRI, cap. 7]*
</details>

---

### Pregunta 37

**Una subconsulta no correlacionada se caracteriza porque:**

A) Es independiente de la fila de la consulta externa y el motor puede ejecutarla una sola vez
B) Debe reevaluarse obligatoriamente por cada fila de la consulta externa
C) No puede usarse con el predicado IN

<details><summary>Respuesta</summary>

**Correcta: A) Es independiente de la fila de la consulta externa y el motor puede ejecutarla una sola vez** No referencia ninguna columna de la consulta que la contiene.

*Referencia: §2.6 [DATE-INTRO, cap. 8]*
</details>

---

### Pregunta 38

**Una subconsulta correlacionada:**

A) Nunca referencia columnas de la consulta externa
B) Referencia una columna de la consulta externa y su resultado depende de la fila actual
C) Solo puede aparecer en la cláusula FROM

<details><summary>Respuesta</summary>

**Correcta: B) Referencia una columna de la consulta externa y su resultado depende de la fila actual** Es el rasgo que la distingue de la no correlacionada; el optimizador puede reescribirla como join en muchos casos.

*Referencia: §2.6 [SILBERSCHATZ, cap. 3]*
</details>

---

### Pregunta 39

**¿Qué predicado comprueba si la subconsulta devuelve al menos una fila, sin importar su valor?**

A) IN
B) ALL
C) EXISTS

<details><summary>Respuesta</summary>

**Correcta: C) EXISTS** Comprueba solo la existencia de filas, no su contenido, y suele ser más eficiente que IN porque puede detenerse en la primera coincidencia.

*Referencia: §2.6 [DATE-INTRO, cap. 8]*
</details>

---

### Pregunta 40

**¿Qué riesgo clásico presenta NOT IN cuando la subconsulta puede devolver un valor NULL?**

A) Puede no devolver ninguna fila, por la lógica trivaluada de SQL
B) Provoca siempre un error de sintaxis
C) Se comporta exactamente igual que NOT EXISTS en todos los casos

<details><summary>Respuesta</summary>

**Correcta: A) Puede no devolver ninguna fila, por la lógica trivaluada de SQL** NOT EXISTS no tiene ese problema y suele ser la alternativa más segura.

*Referencia: §2.6 [SILBERSCHATZ, cap. 3]*
</details>

---

### Pregunta 41

**Una CTE (Common Table Expression), definida con WITH, es:**

A) Una tabla física permanente creada en el catálogo
B) Un conjunto de resultados nombrado y temporal, que existe solo durante la ejecución de la sentencia
C) Un tipo de índice sobre columnas calculadas

<details><summary>Respuesta</summary>

**Correcta: B) Un conjunto de resultados nombrado y temporal, que existe solo durante la ejecución de la sentencia** Mejora la legibilidad frente a subconsultas anidadas y permite referenciarse varias veces en la misma consulta.

*Referencia: §2.7 [EISENBERG99]*
</details>

---

### Pregunta 42

**Toda CTE recursiva (WITH RECURSIVE) necesita obligatoriamente:**

A) Un ORDER BY en el miembro ancla
B) Usar UNION en lugar de UNION ALL para evitar bucles
C) Un miembro ancla sin referencia a sí mismo y un miembro recursivo unido con UNION ALL

<details><summary>Respuesta</summary>

**Correcta: C) Un miembro ancla sin referencia a sí mismo y un miembro recursivo unido con UNION ALL** Sin una condición de reunión que reduzca el conjunto en cada paso, la recursión no terminaría.

*Referencia: §2.7 [ISO9075; EISENBERG99]*
</details>

---

### Pregunta 43

**Las funciones de ventana (OVER, PARTITION BY) se diferencian de GROUP BY en que:**

A) Conservan todas las filas originales, añadiendo una columna calculada sobre la partición, sin colapsar el resultado
B) Siempre reducen el número de filas del resultado a una por grupo
C) No pueden usar funciones de agregación como SUM o AVG

<details><summary>Respuesta</summary>

**Correcta: A) Conservan todas las filas originales, añadiendo una columna calculada sobre la partición, sin colapsar el resultado** Es la diferencia clave frente a GROUP BY, que sí colapsa las filas en una por grupo.

*Referencia: §2.8 [ZEMKE03]*
</details>

---

### Pregunta 44

**¿Qué función de ventana asigna un número secuencial único dentro de la partición, sin dejar huecos ni empates?**

A) RANK()
B) ROW_NUMBER()
C) NTILE()

<details><summary>Respuesta</summary>

**Correcta: B) ROW_NUMBER()** RANK() sí puede dejar huecos tras un empate (1,2,2,4); DENSE_RANK() no deja huecos pero sí permite empates (1,2,2,3); ROW_NUMBER() nunca empata.

*Referencia: §2.8 [ZEMKE03]*
</details>

---

### Pregunta 45

**SQL/PSM (ISO/IEC 9075-4) es:**

A) Un motor de bases de datos comercial
B) La sentencia estándar para crear vistas actualizables
C) La referencia normativa para procedimientos almacenados, que cada motor implementa con su propio dialecto (PL/SQL, T-SQL, PL/pgSQL)

<details><summary>Respuesta</summary>

**Correcta: C) La referencia normativa para procedimientos almacenados, que cada motor implementa con su propio dialecto (PL/SQL, T-SQL, PL/pgSQL)** Fija los conceptos comunes (variables, cursores, control de flujo, excepciones), pero la sintaxis concreta difiere por motor.

*Referencia: §2.9 [MELTON-PSM]*
</details>

---

### Pregunta 46

**El soporte nativo de JSON en SQL (funciones como JSON_VALUE, JSON_TABLE) se incorporó formalmente en:**

A) SQL:2016
B) SQL-92
C) SQL:1999

<details><summary>Respuesta</summary>

**Correcta: A) SQL:2016** Es uno de los hitos que acercan SQL a los datos semiestructurados típicos de NoSQL.

*Referencia: §2.9 [MELTON, cap. 12]*
</details>

---

### Pregunta 47

**Un procedimiento almacenado se diferencia de una función almacenada en que:**

A) La función nunca puede tener parámetros de entrada
B) La función siempre devuelve un único valor mediante RETURN y suele poder usarse dentro de una expresión SQL; el procedimiento se invoca como sentencia independiente
C) Un procedimiento no puede ejecutar sentencias DML

<details><summary>Respuesta</summary>

**Correcta: B) La función siempre devuelve un único valor mediante RETURN y suele poder usarse dentro de una expresión SQL; el procedimiento se invoca como sentencia independiente** Un procedimiento puede devolver cero, uno o varios valores mediante parámetros OUT/INOUT.

*Referencia: §3.1 [MELTON-PSM]*
</details>

---

### Pregunta 48

**¿Cuál es la principal ventaja de rendimiento de un procedimiento almacenado frente a enviar SQL dinámico repetidamente?**

A) Consume menos espacio en disco
B) No necesita permisos de ejecución
C) Reutiliza el plan de ejecución cacheado tras la primera invocación, evitando repetir el análisis y la optimización

<details><summary>Respuesta</summary>

**Correcta: C) Reutiliza el plan de ejecución cacheado tras la primera invocación, evitando repetir el análisis y la optimización** El coste de analizar y optimizar se paga una vez, no en cada llamada.

*Referencia: §3.2 [GMUW, cap. 8]*
</details>

---

### Pregunta 49

**Un parámetro de modo OUT en un procedimiento almacenado:**

A) Recibe el valor que el procedimiento le asigna y se devuelve al llamador al terminar; su valor de entrada se ignora
B) Solo puede leerse dentro del procedimiento, nunca modificarse
C) Es el modo por defecto de todo parámetro si no se indica lo contrario

<details><summary>Respuesta</summary>

**Correcta: A) Recibe el valor que el procedimiento le asigna y se devuelve al llamador al terminar; su valor de entrada se ignora** El modo por defecto es IN, no OUT.

*Referencia: §3.3 [MELTON-PSM; MSSQL-DOC]*
</details>

---

### Pregunta 50

**El ciclo habitual de un cursor explícito sigue la secuencia:**

A) OPEN → DECLARE → CLOSE → FETCH
B) DECLARE → OPEN → FETCH (en bucle) → CLOSE
C) FETCH → DECLARE → OPEN → CLOSE

<details><summary>Respuesta</summary>

**Correcta: B) DECLARE → OPEN → FETCH (en bucle) → CLOSE** Olvidar el CLOSE mantiene recursos y bloqueos ocupados durante la sesión.

*Referencia: §3.4 [SILBERSCHATZ, cap. 5]*
</details>

---

### Pregunta 51

**Un cursor scrollable, frente a uno forward-only, se caracteriza por:**

A) No poder recorrer ninguna fila del resultado
B) Ser siempre implícito, nunca declarado por el programador
C) Poder moverse hacia adelante y hacia atrás, e incluso saltar a una fila concreta

<details><summary>Respuesta</summary>

**Correcta: C) Poder moverse hacia adelante y hacia atrás, e incluso saltar a una fila concreta** Consume más recursos que el forward-only, que solo avanza.

*Referencia: §3.4 [ORACLE-DOC]*
</details>

---

### Pregunta 52

**En la gestión de excepciones de un procedimiento almacenado, el código SQLSTATE:**

A) Es un código de cinco caracteres normalizado por el estándar SQL para identificar el tipo de error
B) Es exclusivo de un único motor comercial y no está estandarizado
C) Sustituye por completo a los bloques de manejo de errores del procedimiento

<details><summary>Respuesta</summary>

**Correcta: A) Es un código de cinco caracteres normalizado por el estándar SQL para identificar el tipo de error** Muchos motores añaden además su propio código numérico (SQLCODE) específico del producto.

*Referencia: §3.6 [MELTON-PSM]*
</details>

---

### Pregunta 53

**Un disparador (trigger) se diferencia de un procedimiento almacenado normal en que:**

A) Siempre requiere parámetros de entrada explícitos
B) Se ejecuta automáticamente en respuesta a un evento, sin que nadie lo invoque directamente por su nombre
C) No puede acceder a los valores de la fila afectada

<details><summary>Respuesta</summary>

**Correcta: B) Se ejecuta automáticamente en respuesta a un evento, sin que nadie lo invoque directamente por su nombre** Un disparador no admite parámetros al estilo de un procedimiento; recibe el contexto del evento (OLD/NEW).

*Referencia: §4.1 [SILBERSCHATZ, cap. 5]*
</details>

---

### Pregunta 54

**Un disparador BEFORE, a diferencia de uno AFTER:**

A) Solo puede usarse sobre vistas
B) Se ejecuta siempre en una transacción distinta
C) Se ejecuta antes de aplicar el cambio y puede validar, modificar o incluso cancelar la operación

<details><summary>Respuesta</summary>

**Correcta: C) Se ejecuta antes de aplicar el cambio y puede validar, modificar o incluso cancelar la operación** AFTER, en cambio, se ejecuta cuando el cambio ya está aplicado, útil para acciones derivadas como la auditoría.

*Referencia: §4.2 [ISO9075]*
</details>

---

### Pregunta 55

**Un disparador definido FOR EACH ROW:**

A) Se ejecuta una vez por cada fila afectada por la sentencia, con acceso a los valores OLD y NEW
B) Se ejecuta una única vez por sentencia, sin importar el número de filas afectadas
C) Solo puede combinarse con eventos programados por tiempo

<details><summary>Respuesta</summary>

**Correcta: A) Se ejecuta una vez por cada fila afectada por la sentencia, con acceso a los valores OLD y NEW** FOR EACH STATEMENT es el que se ejecuta una única vez por sentencia, sin acceso directo a cada fila.

*Referencia: §4.2 [ELMASRI, cap. 5]*
</details>

---

### Pregunta 56

**Un disparador INSTEAD OF es imprescindible cuando:**

A) La tabla base tiene una clave primaria compuesta
B) Se necesita sustituir una operación DML sobre una vista no directamente actualizable (por ejemplo, con JOIN)
C) Se quiere programar una tarea de mantenimiento periódico por tiempo

<details><summary>Respuesta</summary>

**Correcta: B) Se necesita sustituir una operación DML sobre una vista no directamente actualizable (por ejemplo, con JOIN)** El disparador INSTEAD OF traduce la operación sobre la vista a las operaciones equivalentes sobre las tablas base.

*Referencia: §4.2 [MYSQL-DOC]*
</details>

---

### Pregunta 57

**Los eventos programados (scheduled events / jobs) se disparan:**

A) Por una sentencia INSERT sobre la tabla asociada
B) Por una sentencia DELETE exclusivamente
C) Por tiempo, no por una operación DML, y se usan típicamente para mantenimiento periódico

<details><summary>Respuesta</summary>

**Correcta: C) Por tiempo, no por una operación DML, y se usan típicamente para mantenimiento periódico** Ejemplos: Event Scheduler de MySQL, SQL Server Agent, DBMS_SCHEDULER de Oracle, pg_cron de PostgreSQL.

*Referencia: §4.2 [MYSQL-DOC; MSSQL-DOC; ORACLE-DOC; PSQL-DOC]*
</details>

---

### Pregunta 58

**¿Cuál es un riesgo característico de encadenar varios disparadores entre sí (trigger chain)?**

A) Efectos en cascada difíciles de depurar, incluyendo el riesgo de recursión no controlada
B) Ninguno: los disparadores encadenados siempre mejoran el rendimiento
C) El estándar SQL prohíbe expresamente más de un disparador por tabla

<details><summary>Respuesta</summary>

**Correcta: A) Efectos en cascada difíciles de depurar, incluyendo el riesgo de recursión no controlada** Es uno de los motivos por los que se recomienda usar disparadores con moderación y preferir restricciones declarativas cuando sea posible.

*Referencia: §4.4 [SILBERSCHATZ, cap. 5]*
</details>

---

### Pregunta 59

**SQL/PGQ, incorporado en SQL:2023, permite:**

A) Sustituir por completo el modelo relacional por un modelo de grafos
B) Expresar consultas de grafos de propiedades directamente sobre tablas relacionales
C) Eliminar la necesidad de índices en el SGBD

<details><summary>Respuesta</summary>

**Correcta: B) Expresar consultas de grafos de propiedades directamente sobre tablas relacionales** No sustituye el modelo relacional: añade una sintaxis declarativa de grafos sobre las mismas tablas.

*Referencia: §5 [MELTON, cap. 12]*
</details>

---

### Pregunta 60

**Las tendencias actuales del lenguaje SQL (JSON nativo, SQL/PGQ, funciones analíticas avanzadas):**

A) Sustituyen los fundamentos del modelo relacional y el núcleo declarativo del SELECT
B) Solo son relevantes para sistemas NoSQL, no para SGBD relacionales
C) Amplían el estándar SQL sin sustituir sus fundamentos: el modelo relacional y el núcleo declarativo siguen siendo la base

<details><summary>Respuesta</summary>

**Correcta: C) Amplían el estándar SQL sin sustituir sus fundamentos: el modelo relacional y el núcleo declarativo siguen siendo la base** El Tema 19 completo —desde el modelo relacional hasta los disparadores— sigue vigente como fundamento de estas extensiones más recientes.

*Referencia: §5 [MELTON, cap. 12]*
</details>

---

*Fin del banco de 60 preguntas. Distribución de la opción correcta verificada en QA: ver tema-19-validacion.md.*
