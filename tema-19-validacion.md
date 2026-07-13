# Tema 19 — Checklist de Validación

> **Título oficial**: Lenguajes de interrogación de bases de datos. El estándar ANSI SQL. Procedimientos almacenados. Eventos y disparadores.
> **Versión**: v1.0 — Pendiente validación
> **Fecha**: 2026-07-13
> **Revisores**: María y Ana (eTrivium) · revisión técnica IAM (Jesús Cuadrado)
> **Instrucciones**: marcar cada ítem. Los cambios no se guardan en la web (imprimir o exportar a PDF si se desea fijarlos).

---

## 1. Cobertura del temario oficial

- [ ] **Lenguajes de interrogación de bases de datos**: definición en el modelo relacional, álgebra/cálculo relacional, tipología DDL/DML/DQL/DCL/TCL, procedimentales/declarativos, utilización en aplicaciones — §1.1-1.5
- [ ] **El estándar ANSI SQL**: origen, evolución, niveles de conformidad, sintaxis, agregación, joins, subconsultas, CTE recursivas, funciones de ventana, extensiones — §2.1-2.9
- [ ] **Procedimientos almacenados**: concepto, arquitectura y ciclo de vida, parámetros, cursores, control de flujo, excepciones — §3.1-3.6
- [ ] **Eventos y disparadores**: definición, clasificación (BEFORE/AFTER, fila/sentencia, INSTEAD OF, programados), casos de uso, riesgos y buenas prácticas — §4.1-4.4

## 2. Contenido teórico

- [ ] El nivel de profundidad (ampliado, como T18) es adecuado para C1 (¿hay que ampliar o recortar alguna sección?)
- [ ] Las definiciones de álgebra/cálculo relacional, DDL/DML/DQL/DCL/TCL y orden lógico de evaluación son correctas
- [ ] La decisión de usar **ANSI SQL puro** en los ejemplos de consulta es adecuada (¿o se prefiere un motor concreto para casos de uso reales del Ayuntamiento?)
- [ ] La decisión de usar **pseudocódigo SQL genérico** para procedimientos y disparadores es adecuada (¿o se prefiere el dialecto real de un motor, p. ej. si el Ayto. usa Oracle/PostgreSQL/SQL Server en producción?)
- [ ] La frontera con el Tema 17 (diseño lógico relacional), el Tema 18 (lenguajes de programación) y el Tema 20 (POO) está clara
- [ ] Los ejemplos Ayto Madrid (tributos, Padrón/censo, expedientes, distritos) son verosímiles

## 3. Fuentes

- [ ] Todas las afirmaciones técnicas están respaldadas por fuente Tier 1
- [ ] Las referencias inline se corresponden con `tema-19-fuentes.md`
- [ ] Atribuciones históricas correctas (Codd 1970, Chamberlin-Boyce 1974, SQL-86/92/1999/2003/2016/2023)

## 4. Test (60 preguntas)

- [ ] Cada pregunta tiene una sola respuesta correcta e inequívoca
- [ ] Los distractores (A/B/C) son plausibles
- [ ] La distribución de la opción correcta entre A/B/C está equilibrada (20/20/20)
- [ ] Las explicaciones y referencias de cada respuesta son correctas

## 5. Casos prácticos (3)

- [ ] Realistas y propios del Ayuntamiento (tributos, Padrón, expedientes)
- [ ] Soluciones orientativas técnicamente correctas (SQL estándar y pseudocódigo)
- [ ] La puntuación de cada caso suma 10 puntos

## 6. Diagramas (12 SVG)

- [ ] Cada diagrama es correcto y legible (también impreso en B/N)
- [ ] Accesibilidad: todos tienen `role="img"` y `aria-label`
- [ ] Sin desbordes de texto ni colisiones de estilo entre SVG (clases con sufijo único, QA de caja contenedora)

## 7. Referencias cruzadas a otros temas

- [ ] Validadas contra BOAM 10.032 (T3, T6, T15, T16, T17, T18, T20, T21, T23)
- [ ] Ninguna referencia cruzada cita un enunciado de tema incorrecto

## 8. Calidad editorial

- [ ] Ortografía verificada (tildes y ñ) — sin diacríticos perdidos
- [ ] Coherencia de versión (v1.0) en title, badges, banner y footer del `index.html`
- [ ] El `index.html` abre, navega entre las 8 pestañas y el motor de test funciona
- [ ] Las listas anidadas del Contenido se muestran con sus niveles (sin aplanar)
- [ ] Los bloques de código SQL y pseudocódigo se muestran correctamente formateados, sin markdown crudo

---

## Observaciones abiertas

_(Espacio para anotaciones de María, Ana y la revisión IAM.)_
