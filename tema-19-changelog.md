# Tema 19 — Changelog

> **Título oficial**: Lenguajes de interrogación de bases de datos. El estándar ANSI SQL. Procedimientos almacenados. Eventos y disparadores.

---

## v1.2 — 2026-09-06 — Marcado del apartado complementario

**Estado**: pendiente de validación por el IAM.

**Motivo**: criterio de literalidad del título fijado por el IAM (Jesús Cuadrado, 02-09-2026).

### Alcance

- El apartado final que **el enunciado oficial del tema no nombra** queda marcado como **material complementario**, en el índice y al principio del propio apartado, con la advertencia de que lo exigible es lo que enumera el título.
- **Sin cambios de contenido**: el apartado se mantiene íntegro.

---

## v1.1 — 2026-09-06 — Ficha de extensión y tiempo de estudio

**Estado**: sin cambios de contenido. Solo se añade información sobre el propio tema.

**Motivo**: petición del IAM (Jesús Cuadrado, 02-09-2026) al validar el Tema 30. Acepta la extensión de los temas «compuestos» a condición de que se informe de «su extensión en palabras y tiempo estimado de estudio». Al revisarlo se vio que ese dato solo aparecía en 16 de los 40 temas, y que faltaba justo en los más largos.

### Alcance

- Ficha bajo la cabecera del tema, y al final de la pestaña Índice donde esa pestaña existe:
  - **Extensión**: ~8.800 palabras · 12 diagramas · 60 preguntas de test
  - **Tiempo estimado de estudio**: 10-12 horas (primera vuelta completa, sin contar repasos)
- La cifra de palabras de la tabla de entregables se sincroniza con la ficha, para que el tema no muestre dos recuentos distintos.
- Las horas salen de una fórmula común a los 40 temas, para que sean comparables entre sí: contenido a 1.500 palabras/hora (ritmo de estudio activo), diagramas a una hora por cada cinco y test a dos minutos por pregunta. Se publica como intervalo de dos horas.
- Generado con `_tools-qa/ficha_estudio.py`, idempotente y reejecutable tras cualquier regeneración con `build_tNN.py`.

---

## v1.0 — 2026-07-13 — Primera versión

**Estado**: pendiente de validación por María y Ana, y de revisión técnica del IAM (Jesús Cuadrado).

**Motivo**: desarrollo del Tema 19, dentro de la serie de temas técnicos generados desde cero (tras T13-T18), replicando la estructura y el formato de los Temas 1, 11, 17 y 18 ya consolidados, con pestaña Índice y listas anidadas correctas desde el inicio.

### Alcance de la v1.0

| Entregable | Cantidad |
|---|---|
| Contenido teórico | ~8.350 palabras · 5 secciones (4 del esqueleto oficial + Tendencias) con 27 epígrafes |
| Diagramas SVG inline | 12 (accesibles con `role`/`aria-label`, clases con sufijo único anti-colisión) |
| Banco de preguntas tipo test | 60 preguntas A/B/C con explicación y referencia, balanceadas **20/20/20** |
| Casos prácticos | 3 (recaudación de tributos / SQL declarativo; recálculo de bonificaciones del Padrón / procedimiento con cursor; auditoría de expedientes / disparadores) · 10 puntos cada uno |
| Fuentes Tier 1 | 13 referencias canónicas (Codd, Chamberlin-Boyce, ISO/IEC 9075, Date, Silberschatz, Elmasri, Garcia-Molina/Ullman/Widom, Melton, Eisenberg, Zemke, Härder-Reuter) |

### Decisiones de generación

1. **Sin material de cliente**: solo el esqueleto `Test_Prompting/temas junio/19.md`. Desarrollado desde fuentes canónicas del modelo relacional y el estándar SQL, todas referenciadas.
2. **Estructura fiel al esqueleto oficial**: cuatro secciones H2 (Lenguajes de interrogación; El estándar ANSI SQL; Procedimientos almacenados; Eventos y disparadores), ampliadas con una quinta sección «Tendencias actuales» (decisión de Joan, mismo criterio que T18).
3. **Ejemplos SQL en ANSI SQL puro** (decisión de Joan): coherente con que el propio enunciado oficial cita «el estándar ANSI SQL»; sin extensiones propietarias de ningún motor.
4. **Ejemplos de procedimientos y disparadores en pseudocódigo SQL genérico** (decisión de Joan): el estándar SQL/PSM apenas se implementa tal cual en la práctica — cada motor tiene su dialecto (PL/SQL, T-SQL, PL/pgSQL) — así que se sigue el mismo criterio que T18 con los lenguajes de programación: pseudocódigo neutro, con notas Tier 2 a los dialectos reales cuando aporta contexto.
5. **Profundidad ampliada** (decisión de Joan, «como T18»): álgebra y cálculo relacional con equivalencia de Codd, orden lógico de evaluación de un SELECT, distinción WHERE/HAVING, siete tipos de JOIN, predicados cuantificados con la trampa de `NOT IN` + `NULL`, CTE recursivas, funciones de ventana (ROW_NUMBER/RANK/DENSE_RANK/LAG/LEAD), arquitectura de caché de planes de un procedimiento, tipos de cursor (forward-only/scrollable, sensible/insensible), y las cuatro combinaciones de disparadores DML más INSTEAD OF y eventos programados.
6. **Sección «Tendencias actuales» con marco duradero** (mismo criterio que T18): JSON nativo (SQL:2016), SQL/PGQ — grafos de propiedades (SQL:2023), OLAP/ROLLUP/CUBE, ORM y *query builders*, *streaming SQL* — sin números de versión de producto que caduquen, subrayando que amplían el estándar sin sustituir sus fundamentos.
7. **Contexto Ayuntamiento de Madrid** en casos y ejemplos (tributos/IBI, Padrón, expedientes, distritos, organigrama de Áreas de Gobierno).
8. **Frontera con temas vecinos** cuidada: SGBD relacionales/NoSQL como sistemas al T15; modelo conceptual E/R al T16; diseño lógico y normalización al T17; procedimientos/funciones como concepto general de programación al T18; POO al T20; JDBC y Java EE al T21.
9. **Referencias cruzadas validadas contra BOAM 10.032**: T3 (organigrama, Áreas de Gobierno), T6 (transparencia/ENS aplicado a auditoría), T15, T16, T17, T18, T20, T21, T23. Todas comprobadas.
10. **Anti-colisión de SVG**: las clases CSS de cada diagrama llevan **sufijo numérico único** (`.t1`…`.t12`), evitando el bug sistémico de estilos que leakean entre los 12 SVG embebidos en la misma página (lección de T5).

### Pendientes para QA / próxima iteración

- Validación de profundidad por María/Ana/IAM (¿alguna sección a ampliar o recortar?).
- Confirmación de si, además del ANSI SQL puro, interesa una nota comparativa del motor real en producción en el Ayuntamiento (si lo hay).
- Verificación ortográfica con corrector es_ES (cuidado con falsos positivos por términos técnicos en inglés: join, trigger, cursor, cache, callback, timestamp…).

### Origen

Generado el 2026-07-13 en el flujo de trabajo de eTrivium, replicando el patrón de los Temas 1 (v2.1), 11 (v3.2), 17 (v1.0) y 18 (v1.0). `build_t19.py` y `_build_css.txt` persistidos en el repo.
