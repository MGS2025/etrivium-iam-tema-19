# Tema 19 — Fuentes

> **Título oficial**: Lenguajes de interrogación de bases de datos. El estándar ANSI SQL. Procedimientos almacenados. Eventos y disparadores.
>
> **Criterio**: todo dato del contenido cita un **ID** inline (p. ej. `[DATE-REL, cap. 3]`). Tier 1 = obras canónicas y normas del modelo relacional/SQL; Tier 2 = documentación oficial de motores (para ilustrar extensiones procedimentales sin atar el tema a ninguno, según decisión de Joan de usar **ANSI SQL puro** en los ejemplos); Tier 3 = material de apoyo y marco del puesto, no citado como contenido técnico.

---

## Tier 1 — Canónicas y normativas

| ID | Referencia |
|---|---|
| `[CODD70]` | Codd, E. F. (1970). *A Relational Model of Data for Large Shared Data Banks*. Communications of the ACM, 13(6). Artículo fundacional del modelo relacional, base del álgebra y el cálculo relacional. |
| `[CHAMBERLIN74]` | Chamberlin, D. D.; Boyce, R. F. (1974). *SEQUEL: A Structured English Query Language*. Proc. ACM SIGFIDET. Origen de SQL sobre el prototipo System R de IBM. |
| `[ISO9075]` | ISO/IEC 9075:2023 *Information technology — Database languages — SQL* (partes 1-Framework, 2-Foundation, 4-PSM, 14-JSON). Norma vigente del estándar SQL. |
| `[DATE-REL]` | Date, C. J. *SQL and Relational Theory* (3.ª ed.). O'Reilly. Fundamento lógico de SQL frente al álgebra y el cálculo relacional; distingue el estándar de sus implementaciones. |
| `[DATE-INTRO]` | Date, C. J. *An Introduction to Database Systems* (8.ª ed.). Addison-Wesley. Obra de referencia sobre el modelo relacional, álgebra/cálculo y lenguajes de consulta. |
| `[SILBERSCHATZ]` | Silberschatz, A.; Korth, H. F.; Sudarshan, S. *Database System Concepts* (7.ª ed.). McGraw-Hill. SQL, procedimientos almacenados, disparadores, transacciones. |
| `[ELMASRI]` | Elmasri, R.; Navathe, S. B. *Fundamentals of Database Systems* (7.ª ed.). Pearson. Álgebra y cálculo relacional, SQL avanzado, triggers. |
| `[GMUW]` | Garcia-Molina, H.; Ullman, J. D.; Widom, J. *Database Systems: The Complete Book* (2.ª ed.). Pearson. Álgebra relacional formal, optimización de consultas, procedimientos almacenados. |
| `[MELTON]` | Melton, J.; Simon, A. R. *Understanding the New SQL: A Complete Guide*. Morgan Kaufmann. Evolución histórica del estándar SQL edición a edición. |
| `[MELTON-PSM]` | Melton, J. (2002). *Understanding SQL's Stored Procedures: A Complete Guide to SQL/PSM*. Morgan Kaufmann. Referencia del estándar SQL/PSM (ISO/IEC 9075-4) para procedimientos almacenados. |
| `[EISENBERG99]` | Eisenberg, A.; Melton, J. (1999). *SQL:1999, formerly known as SQL3*. ACM SIGMOD Record 28(1). Introducción de recursividad (CTE), triggers y tipos definidos por el usuario en el estándar. |
| `[ZEMKE03]` | Zemke, F. et al. (2003). *Introducing OLAP Functions in SQL:1999*. ACM SIGMOD Record. Origen normativo de las funciones de ventana analíticas. |
| `[ACID83]` | Härder, T.; Reuter, A. (1983). *Principles of Transaction-Oriented Database Recovery*. ACM Computing Surveys. Propiedades ACID que fundamentan TCL (COMMIT/ROLLBACK). |

## Tier 2 — Documentación de motores (ilustración de extensiones procedimentales)

| ID | Referencia |
|---|---|
| `[PSQL-DOC]` | PostgreSQL Global Development Group. *PostgreSQL Documentation* — capítulos de PL/pgSQL, disparadores y funciones de ventana. postgresql.org/docs |
| `[ORACLE-DOC]` | Oracle Corporation. *Oracle Database PL/SQL Language Reference*. Cursores, excepciones, paquetes. docs.oracle.com |
| `[MSSQL-DOC]` | Microsoft. *Transact-SQL (T-SQL) Reference*. Procedimientos almacenados, TRY/CATCH, SQL Server Agent. learn.microsoft.com |
| `[MYSQL-DOC]` | Oracle Corporation. *MySQL 8.0 Reference Manual* — Stored Program Objects, Triggers, Event Scheduler. dev.mysql.com/doc |

## Tier 3 — Marco de calidad y del puesto (contexto, no contenido técnico)

| ID | Referencia |
|---|---|
| `[ISO25010]` | ISO/IEC 25010:2011 *Systems and software Quality Requirements and Evaluation (SQuaRE)* — mantenibilidad, fiabilidad aplicadas a lógica en el SGBD. |
| `[ENS]` | Real Decreto 311/2022, Esquema Nacional de Seguridad — trazabilidad y auditoría, relevante para disparadores de auditoría. |
| `[BOAM10032]` | BOAM 10.032 (23-dic-2025). Bases específicas TIC C1 Ayto. Madrid — temario oficial. |

---

*Las referencias Tier 1 fijan el fundamento teórico (modelo relacional, álgebra/cálculo, estándar ISO/IEC 9075) y son la base de todo el contenido; Tier 2 se cita solo en los callouts de [REFERENCIA CRUZADA]/contexto para señalar cómo cada motor real nombra sus extensiones procedimentales, sin que ningún ejemplo del cuerpo del tema dependa de un motor concreto — los ejemplos de código son **ANSI SQL estándar**, y los de procedimientos/disparadores, **pseudocódigo SQL genérico** (decisión de Joan); Tier 3 enmarca la calidad y la seguridad aplicables en el Ayuntamiento de Madrid.*
