# Tema 19 — Índice

> **Título oficial**: Lenguajes de interrogación de bases de datos. El estándar ANSI SQL. Procedimientos almacenados. Eventos y disparadores.
>
> **Bloque**: Parte II — Técnico (Temas 11-40)
> **Nivel**: C1 — Técnico Auxiliar TIC, Ayuntamiento de Madrid

---

## Estructura del tema

1. **Lenguajes de interrogación de bases de datos**
   1.1. Definición en el modelo relacional
   1.2. Álgebra relacional y el cálculo relacional
   1.3. Tipología de lenguajes en un SGBD
   1.4. Lenguajes procedimentales y declarativos
   1.5. Utilización de lenguajes en aplicaciones

2. **El estándar ANSI SQL**
   2.1. Origen y evolución del estándar SQL
   2.2. Niveles de conformidad
   2.3. Sintaxis de una sentencia SQL
   2.4. Operaciones de agregación, agrupamiento y filtrado
   2.5. Tipologías de acoplamiento (Join)
   2.6. Subconsultas: correlacionadas, no correlacionadas y predicados cuantificados
   2.7. Expresiones de Tabla Comunes (CTE) y consultas recursivas
   2.8. Funciones de ventana analíticas
   2.9. Extensiones del estándar y extensiones procedimentales

3. **Procedimientos almacenados**
   3.1. Concepto y características
   3.2. Arquitectura y ciclo de vida de ejecución en el servidor
   3.3. Parámetros de entrada, salida y retorno
   3.4. Gestión de cursores y tipos de cursores
   3.5. Estructuras de control de flujo
   3.6. Gestión de excepciones y errores

4. **Eventos y disparadores**
   4.1. Definición, arquitectura orientada a eventos
   4.2. Clasificación de los disparadores
   4.3. Casos de uso de eventos y disparadores
   4.4. Riesgos, limitaciones y buenas prácticas

5. **Tendencias actuales en los lenguajes de interrogación (material complementario)**

---

## Conceptos clave para memorizar

| Concepto | Dato clave |
|---|---|
| Lenguaje de interrogación | Permite formular consultas sobre las relaciones (tablas) del modelo relacional de Codd (1970) |
| Álgebra relacional | Lenguaje **procedimental**: operadores (σ, π, ρ, ∪, −, ×, ⋈, ÷) que se componen paso a paso; dice **CÓMO** |
| Cálculo relacional | Lenguaje **declarativo**, basado en lógica de predicados; dice **QUÉ** se quiere, no el procedimiento |
| Completitud relacional (Codd) | Un lenguaje es relacionalmente completo si iguala en poder expresivo al álgebra/cálculo relacional |
| DDL / DML / DQL | Definición (CREATE/ALTER/DROP) / Manipulación (INSERT/UPDATE/DELETE) / Consulta (SELECT) |
| DCL / TCL | Control de datos: permisos (GRANT/REVOKE) / Control de transacciones (COMMIT/ROLLBACK/SAVEPOINT) |
| SEQUEL → SQL | Chamberlin y Boyce (IBM, 1974) sobre System R; renombrado SQL por conflicto de marca |
| SQL-86 / SQL-92 / SQL:1999 | Primer estándar ANSI (1986) / gran expansión — joins explícitos, subconsultas (1992) / recursividad y triggers (1999) |
| Niveles de conformidad SQL-92 | Entry, Intermediate, Full — sustituidos desde SQL:1999 por «core» + «features» opcionales |
| Orden lógico de evaluación | FROM → WHERE → GROUP BY → HAVING → SELECT → DISTINCT → ORDER BY → LIMIT |
| WHERE vs HAVING | WHERE filtra filas **antes** de agrupar; HAVING filtra grupos **después** de agregar |
| Subconsulta correlacionada | Referencia columnas de la consulta externa; se reevalúa por cada fila candidata |
| CTE recursiva | `WITH RECURSIVE`: miembro ancla `UNION ALL` miembro recursivo, con condición de parada |
| Función de ventana | `OVER (PARTITION BY … ORDER BY …)`: calcula sobre un grupo **sin colapsar** las filas (a diferencia de GROUP BY) |
| Procedimiento almacenado | Bloque de código precompilado y almacenado en el catálogo del SGBD, ejecutado en el servidor |
| Cursor | Puntero para recorrer fila a fila un resultado cuando no basta el procesamiento por conjuntos |
| Parámetros IN / OUT / INOUT | Solo entrada / solo salida / entrada y salida |
| Disparador (trigger) | Procedimiento especial que se ejecuta automáticamente ante un evento, sin invocación explícita |
| BEFORE / AFTER | Se dispara antes de aplicar el cambio (puede cancelarlo) / después de aplicarlo |
| De fila / de sentencia | Una ejecución por cada fila afectada (`FOR EACH ROW`) / una única ejecución por sentencia |
| INSTEAD OF | Sustituye la operación DML sobre una vista no actualizable, redirigiéndola a las tablas base |

---

*Tiempo estimado de estudio: 13-15 horas*
*Extensión del contenido: ~13.000-15.000 palabras · 12 diagramas SVG embebidos*
