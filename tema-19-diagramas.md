# Tema 19 — Catálogo de Diagramas

> **Título oficial**: Lenguajes de interrogación de bases de datos. El estándar ANSI SQL. Procedimientos almacenados. Eventos y disparadores.
>
> **Versión**: v1.0
> **Fecha**: 2026-07-13
> **Autor**: ETRIVIUM
> **Formato**: SVG inline (zero-dependencias, escalable, imprimible, accesible con role/aria-label)
> **Paleta**: Ayuntamiento de Madrid #0055a0 (primario) + #d13c3c (alertas) + #2d8659 (ventajas) + #e89822 (callouts)
> **Nota técnica**: las clases CSS de cada SVG llevan sufijo numérico único (`.t1`, `.h1`…) para evitar colisiones de estilos entre los 12 diagramas embebidos en la misma página.

---

## Índice de diagramas

| ID | Título | Sección | Tipo | Formato |
|---|---|---|---|---|
| D1 | Anatomía de una relación | §1.1 | Bloques | 640×300 |
| D2 | Álgebra relacional frente a cálculo relacional | §1.2 | Comparativa | 680×300 |
| D3 | Operadores del álgebra relacional | §1.2 | Cheat sheet | 660×340 |
| D4 | Tipología de lenguajes en un SGBD | §1.3 | Bloques | 680×340 |
| D5 | Evolución del estándar SQL | §2.1 | Línea de tiempo | 680×320 |
| D6 | Orden lógico de evaluación de un SELECT | §2.3 | Flujo | 660×360 |
| D7 | Tipos de JOIN | §2.5 | Diagramas de Venn | 680×340 |
| D8 | Subconsulta correlacionada frente a no correlacionada | §2.6 | Comparativa | 680×320 |
| D9 | CTE recursiva: ancla y miembro recursivo | §2.7 | Ciclo | 640×340 |
| D10 | Ciclo de vida de un procedimiento almacenado | §3.2 | Flujo | 680×320 |
| D11 | Ciclo de vida de un cursor explícito | §3.4 | Ciclo | 640×320 |
| D12 | Clasificación de disparadores | §4.2 | Matriz | 680×360 |

---

## D1 · Anatomía de una relación

**Sección**: §1.1 — Definición en el modelo relacional
**Propósito**: Fijar el vocabulario base (relación, tupla, atributo) sobre una tabla de ejemplo.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 640 300" role="img" aria-label="Anatomía de una relación: la tabla es la relación, cada fila es una tupla, cada columna es un atributo, sobre el ejemplo TRIBUTO">
  <style>.t1{font:700 12px system-ui,sans-serif;fill:#fff}.s1{font:11px system-ui,sans-serif;fill:#123}.l1{font:11px system-ui,sans-serif;fill:#444}.h1{font:700 13px system-ui,sans-serif;fill:#0055a0}</style>
  <text x="320" y="24" text-anchor="middle" class="h1">RELACIÓN = TABLA · TUPLA = FILA · ATRIBUTO = COLUMNA</text>
  <rect x="120" y="42" width="120" height="30" fill="#0055a0"/><text x="180" y="62" text-anchor="middle" class="t1">id_tributo</text>
  <rect x="240" y="42" width="120" height="30" fill="#0055a0"/><text x="300" y="62" text-anchor="middle" class="t1">tipo</text>
  <rect x="360" y="42" width="120" height="30" fill="#0055a0"/><text x="420" y="62" text-anchor="middle" class="t1">importe</text>
  <rect x="120" y="72" width="360" height="28" fill="#eef4fa"/><text x="180" y="91" text-anchor="middle" class="s1">T-001</text><text x="300" y="91" text-anchor="middle" class="s1">IBI</text><text x="420" y="91" text-anchor="middle" class="s1">312,40</text>
  <rect x="120" y="100" width="360" height="28" fill="#fff"/><text x="180" y="119" text-anchor="middle" class="s1">T-002</text><text x="300" y="119" text-anchor="middle" class="s1">TASA</text><text x="420" y="119" text-anchor="middle" class="s1">58,00</text>
  <rect x="120" y="128" width="360" height="28" fill="#eef4fa"/><text x="180" y="147" text-anchor="middle" class="s1">T-003</text><text x="300" y="147" text-anchor="middle" class="s1">IBI</text><text x="420" y="147" text-anchor="middle" class="s1">401,10</text>
  <rect x="120" y="42" width="360" height="114" fill="none" stroke="#0055a0" stroke-width="1.5"/>
  <text x="60" y="182" class="l1">↑ La tabla completa es la RELACIÓN</text>
  <text x="490" y="90" class="l1">← una fila es una TUPLA</text>
  <text x="300" y="230" text-anchor="middle" class="l1">Cada columna (id_tributo, tipo, importe) es un ATRIBUTO;</text>
  <text x="300" y="248" text-anchor="middle" class="l1">el conjunto de nombres y tipos de columnas es el ESQUEMA de la relación.</text>
  <text x="630" y="292" text-anchor="end" style="font:11px system-ui;fill:#666">[Fuente: CODD70]</text>
</svg>
```

---

## D2 · Álgebra relacional frente a cálculo relacional

**Sección**: §1.2 — Álgebra relacional y el cálculo relacional
**Propósito**: Contrastar el enfoque procedimental (CÓMO) con el declarativo (QUÉ).

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 300" role="img" aria-label="Comparación entre álgebra relacional, que es procedimental y describe cómo obtener el resultado paso a paso, y cálculo relacional, que es declarativo y describe qué debe cumplir el resultado">
  <style>.t2{font:700 13px system-ui,sans-serif;fill:#fff}.s2{font:11px system-ui,sans-serif;fill:#fff}.l2{font:11px system-ui,sans-serif;fill:#444}.h2{font:700 13px system-ui,sans-serif;fill:#0055a0}</style>
  <text x="340" y="24" text-anchor="middle" class="h2">Dos lenguajes equivalentes para interrogar el modelo relacional</text>
  <rect x="40" y="44" width="280" height="160" rx="8" fill="#0055a0"/>
  <text x="180" y="72" text-anchor="middle" class="t2">ÁLGEBRA RELACIONAL</text>
  <text x="180" y="96" text-anchor="middle" class="s2">PROCEDIMENTAL — dice CÓMO</text>
  <text x="180" y="120" text-anchor="middle" class="s2">Secuencia de operadores:</text>
  <text x="180" y="138" text-anchor="middle" class="s2">σ selección · π proyección</text>
  <text x="180" y="156" text-anchor="middle" class="s2">ρ renombrado · ⋈ reunión</text>
  <text x="180" y="180" text-anchor="middle" class="s2">El orden de aplicación importa</text>
  <rect x="360" y="44" width="280" height="160" rx="8" fill="#2d8659"/>
  <text x="500" y="72" text-anchor="middle" class="t2">CÁLCULO RELACIONAL</text>
  <text x="500" y="96" text-anchor="middle" class="s2">DECLARATIVO — dice QUÉ</text>
  <text x="500" y="120" text-anchor="middle" class="s2">Lógica de predicados:</text>
  <text x="500" y="138" text-anchor="middle" class="s2">{ t | P(t) }</text>
  <text x="500" y="156" text-anchor="middle" class="s2">de tuplas o de dominios</text>
  <text x="500" y="180" text-anchor="middle" class="s2">No se indica el procedimiento</text>
  <text x="340" y="228" text-anchor="middle" class="l2">Codd demostró su EQUIVALENCIA EXPRESIVA (cálculo seguro) → «completitud relacional»</text>
  <text x="340" y="250" text-anchor="middle" style="font:700 12px system-ui;fill:#e89822">SQL: sintaxis declarativa (como el cálculo) + ejecución interna algebraica</text>
  <text x="670" y="292" text-anchor="end" style="font:11px system-ui;fill:#666">[Fuente: CODD70; DATE-REL]</text>
</svg>
```

---

## D3 · Operadores del álgebra relacional

**Sección**: §1.2 — Operadores unarios, binarios y derivados
**Propósito**: Cheat sheet de los 8 operadores fundamentales.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 660 340" role="img" aria-label="Tabla de los operadores del álgebra relacional: selección, proyección, renombrado como unarios; unión, diferencia, producto cartesiano como binarios de conjunto; reunión y división como derivados">
  <style>.t3{font:700 12px system-ui,sans-serif;fill:#fff}.s3{font:11px system-ui,sans-serif;fill:#123}.l3{font:11px system-ui,sans-serif;fill:#444}.h3{font:700 13px system-ui,sans-serif;fill:#0055a0}</style>
  <text x="330" y="22" text-anchor="middle" class="h3">Operadores del álgebra relacional</text>
  <rect x="30" y="36" width="190" height="26" fill="#0055a0"/><text x="125" y="54" text-anchor="middle" class="t3">UNARIOS</text>
  <rect x="230" y="36" width="190" height="26" fill="#2d8659"/><text x="325" y="54" text-anchor="middle" class="t3">BINARIOS (conjuntos)</text>
  <rect x="430" y="36" width="200" height="26" fill="#e89822"/><text x="530" y="54" text-anchor="middle" class="t3">DERIVADOS</text>
  <rect x="30" y="66" width="190" height="30" fill="#eef4fa"/><text x="40" y="86" class="s3">σ Selección (filas)</text>
  <rect x="30" y="98" width="190" height="30" fill="#fff"/><text x="40" y="118" class="s3">π Proyección (columnas)</text>
  <rect x="30" y="130" width="190" height="30" fill="#eef4fa"/><text x="40" y="150" class="s3">ρ Renombrado</text>
  <rect x="230" y="66" width="190" height="30" fill="#e7f3ec"/><text x="240" y="86" class="s3">∪ Unión</text>
  <rect x="230" y="98" width="190" height="30" fill="#fff"/><text x="240" y="118" class="s3">− Diferencia</text>
  <rect x="230" y="130" width="190" height="30" fill="#e7f3ec"/><text x="240" y="150" class="s3">× Producto cartesiano</text>
  <rect x="430" y="66" width="200" height="30" fill="#fbe9cf"/><text x="440" y="86" class="s3">⋈ Reunión (join)</text>
  <rect x="430" y="98" width="200" height="30" fill="#fff"/><text x="440" y="118" class="s3">÷ División</text>
  <rect x="430" y="130" width="200" height="30" fill="#fbe9cf"/><text x="440" y="150" class="s3">∩ Intersección</text>
  <text x="330" y="182" text-anchor="middle" class="l3">Unarios: una relación de entrada · Binarios: dos relaciones COMPATIBLES POR UNIÓN</text>
  <text x="330" y="200" text-anchor="middle" class="l3">Derivados: se expresan a partir de los anteriores, pero son fundamentales en la práctica</text>
  <rect x="80" y="222" width="500" height="90" rx="6" fill="#fdf3e3" stroke="#e89822"/>
  <text x="330" y="244" text-anchor="middle" style="font:700 12px system-ui;fill:#8a5a00">Cierre relacional</text>
  <text x="330" y="264" text-anchor="middle" class="l3">Toda operación del álgebra toma relación(es) como entrada</text>
  <text x="330" y="282" text-anchor="middle" class="l3">y produce OTRA RELACIÓN como salida → permite componer</text>
  <text x="330" y="300" text-anchor="middle" class="l3">operadores anidando el resultado de uno como entrada de otro</text>
  <text x="650" y="332" text-anchor="end" style="font:11px system-ui;fill:#666">[Fuente: DATE-INTRO, cap. 6]</text>
</svg>
```

---

## D4 · Tipología de lenguajes en un SGBD

**Sección**: §1.3 — DDL, DML, DQL, DCL, TCL
**Propósito**: Situar los cinco sublenguajes y su función.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 340" role="img" aria-label="Cinco sublenguajes de un SGBD: DDL define la estructura, DML manipula datos, DQL consulta datos, DCL controla permisos, TCL controla transacciones">
  <style>.t4{font:700 12px system-ui,sans-serif;fill:#fff}.s4{font:10.5px system-ui,sans-serif;fill:#fff}.l4{font:11px system-ui,sans-serif;fill:#444}.h4{font:700 13px system-ui,sans-serif;fill:#0055a0}</style>
  <text x="340" y="22" text-anchor="middle" class="h4">Los cinco sublenguajes que componen SQL</text>
  <rect x="20" y="42" width="120" height="86" rx="6" fill="#0055a0"/><text x="80" y="64" text-anchor="middle" class="t4">DDL</text><text x="80" y="82" text-anchor="middle" class="s4">Define estructura</text><text x="80" y="98" text-anchor="middle" class="s4">CREATE ALTER</text><text x="80" y="114" text-anchor="middle" class="s4">DROP TRUNCATE</text>
  <rect x="150" y="42" width="120" height="86" rx="6" fill="#2d8659"/><text x="210" y="64" text-anchor="middle" class="t4">DML</text><text x="210" y="82" text-anchor="middle" class="s4">Manipula datos</text><text x="210" y="98" text-anchor="middle" class="s4">INSERT UPDATE</text><text x="210" y="114" text-anchor="middle" class="s4">DELETE MERGE</text>
  <rect x="280" y="42" width="120" height="86" rx="6" fill="#3778b5"/><text x="340" y="64" text-anchor="middle" class="t4">DQL</text><text x="340" y="82" text-anchor="middle" class="s4">Consulta datos</text><text x="340" y="98" text-anchor="middle" class="s4">SELECT</text><text x="340" y="114" text-anchor="middle" class="s4">(subconjunto DML)</text>
  <rect x="410" y="42" width="120" height="86" rx="6" fill="#e89822"/><text x="470" y="64" text-anchor="middle" class="t4">DCL</text><text x="470" y="82" text-anchor="middle" class="s4">Controla permisos</text><text x="470" y="98" text-anchor="middle" class="s4">GRANT</text><text x="470" y="114" text-anchor="middle" class="s4">REVOKE</text>
  <rect x="540" y="42" width="120" height="86" rx="6" fill="#d13c3c"/><text x="600" y="64" text-anchor="middle" class="t4">TCL</text><text x="600" y="82" text-anchor="middle" class="s4">Control transacciones</text><text x="600" y="98" text-anchor="middle" class="s4">COMMIT ROLLBACK</text><text x="600" y="114" text-anchor="middle" class="s4">SAVEPOINT</text>
  <rect x="60" y="150" width="560" height="90" rx="6" fill="#eef4fa" stroke="#0055a0"/>
  <text x="340" y="172" text-anchor="middle" style="font:700 12px system-ui;fill:#0055a0">Ejemplo: alta de una nueva tasa municipal</text>
  <text x="340" y="194" text-anchor="middle" class="l4">DDL crea la tabla · DML inserta los registros · DQL comprueba la carga</text>
  <text x="340" y="212" text-anchor="middle" class="l4">DCL concede permiso al perfil gestor · TCL confirma la carga como unidad atómica</text>
  <text x="340" y="266" text-anchor="middle" style="font:700 12px system-ui;fill:#d13c3c">El estándar ISO/IEC 9075 clasifica SELECT dentro de DML — DQL es convención didáctica</text>
  <text x="670" y="330" text-anchor="end" style="font:11px system-ui;fill:#666">[Fuente: SILBERSCHATZ, cap. 3]</text>
</svg>
```

---

## D5 · Evolución del estándar SQL

**Sección**: §2.1 — Origen y evolución del estándar SQL
**Propósito**: Línea de tiempo de SEQUEL a SQL:2023, con el hito de cada edición.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 320" role="img" aria-label="Línea de tiempo del estándar SQL desde SEQUEL de 1974 hasta SQL 2023, pasando por SQL-86, SQL-92, SQL 1999 con recursividad y disparadores, SQL 2003 con funciones de ventana">
  <style>.t5{font:700 11px system-ui,sans-serif;fill:#fff}.s5{font:10px system-ui,sans-serif;fill:#fff}.l5{font:11px system-ui,sans-serif;fill:#444}.h5{font:700 13px system-ui,sans-serif;fill:#0055a0}</style>
  <text x="340" y="22" text-anchor="middle" class="h5">De SEQUEL (1974) al estándar SQL actual</text>
  <line x1="40" y1="60" x2="640" y2="60" stroke="#0055a0" stroke-width="3"/>
  <circle cx="60" cy="60" r="6" fill="#888"/><text x="60" y="42" text-anchor="middle" class="l5">1974</text><text x="60" y="80" text-anchor="middle" class="l5">SEQUEL</text>
  <circle cx="160" cy="60" r="7" fill="#0055a0"/><text x="160" y="42" text-anchor="middle" class="l5">1986</text><text x="160" y="80" text-anchor="middle" style="font:700 11px system-ui;fill:#0055a0">SQL-86</text>
  <circle cx="260" cy="60" r="7" fill="#0055a0"/><text x="260" y="42" text-anchor="middle" class="l5">1992</text><text x="260" y="80" text-anchor="middle" style="font:700 11px system-ui;fill:#0055a0">SQL-92</text>
  <circle cx="360" cy="60" r="7" fill="#0055a0"/><text x="360" y="42" text-anchor="middle" class="l5">1999</text><text x="360" y="80" text-anchor="middle" style="font:700 11px system-ui;fill:#0055a0">SQL:1999</text>
  <circle cx="460" cy="60" r="6" fill="#888"/><text x="460" y="42" text-anchor="middle" class="l5">2003</text><text x="460" y="80" text-anchor="middle" class="l5">SQL:2003</text>
  <circle cx="550" cy="60" r="6" fill="#888"/><text x="550" y="42" text-anchor="middle" class="l5">2016</text><text x="550" y="80" text-anchor="middle" class="l5">SQL:2016</text>
  <circle cx="620" cy="60" r="6" fill="#888"/><text x="620" y="42" text-anchor="middle" class="l5">2023</text><text x="620" y="80" text-anchor="middle" class="l5">SQL:2023</text>
  <rect x="20" y="100" width="240" height="46" rx="5" fill="#0055a0"/><text x="140" y="120" text-anchor="middle" class="t5">SQL-86 (SQL1)</text><text x="140" y="136" text-anchor="middle" class="s5">Primer estándar ANSI/ISO</text>
  <rect x="270" y="100" width="200" height="46" rx="5" fill="#3778b5"/><text x="370" y="120" text-anchor="middle" class="t5">SQL-92 (SQL2)</text><text x="370" y="136" text-anchor="middle" class="s5">JOIN explícito · subconsultas</text>
  <rect x="480" y="100" width="180" height="46" rx="5" fill="#d13c3c"/><text x="570" y="120" text-anchor="middle" class="t5">SQL:1999</text><text x="570" y="136" text-anchor="middle" class="s5">Recursividad · DISPARADORES</text>
  <rect x="60" y="160" width="200" height="40" rx="5" fill="#2d8659"/><text x="160" y="184" text-anchor="middle" style="font:700 11px system-ui;fill:#fff">SQL:2003 — Ventanas OVER</text>
  <rect x="280" y="160" width="180" height="40" rx="5" fill="#e89822"/><text x="370" y="184" text-anchor="middle" style="font:700 11px system-ui;fill:#fff">SQL:2016 — JSON nativo</text>
  <rect x="480" y="160" width="180" height="40" rx="5" fill="#888"/><text x="570" y="184" text-anchor="middle" style="font:700 11px system-ui;fill:#fff">SQL:2023 — SQL/PGQ (grafos)</text>
  <text x="340" y="230" text-anchor="middle" class="l5">Los tres hitos resaltados en azul/rojo: SQL-86, SQL-92 y SQL:1999</text>
  <text x="340" y="250" text-anchor="middle" class="l5">Cada motor comercial implementa un subconjunto del estándar más extensiones propias</text>
  <text x="670" y="312" text-anchor="end" style="font:11px system-ui;fill:#666">[Fuente: MELTON, cap. 1; CHAMBERLIN74]</text>
</svg>
```

---

## D6 · Orden lógico de evaluación de un SELECT

**Sección**: §2.3 — Sintaxis de una sentencia SQL
**Propósito**: Contrastar el orden de escritura con el orden real en que el motor evalúa las cláusulas.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 660 370" role="img" aria-label="Orden lógico de evaluación de un SELECT: FROM, JOIN, WHERE, GROUP BY, HAVING, SELECT, DISTINCT, ORDER BY, LIMIT, distinto del orden en que se escribe la sentencia">
  <style>.t6{font:700 12px system-ui,sans-serif;fill:#fff}.l6{font:10.5px system-ui,sans-serif;fill:#444}.h6{font:700 13px system-ui,sans-serif;fill:#0055a0}</style>
  <text x="330" y="22" text-anchor="middle" class="h6">Orden de escritura ≠ orden de evaluación</text>
  <text x="330" y="42" text-anchor="middle" style="font:700 11px system-ui;fill:#e89822">▼ El motor evalúa en este orden, de arriba abajo ▼</text>
  <g>
  <rect x="40" y="56" width="580" height="30" rx="4" fill="#0055a0"/><text x="330" y="76" text-anchor="middle" class="t6">1. FROM — identifica y combina las tablas de origen</text>
  <rect x="40" y="90" width="580" height="30" rx="4" fill="#3778b5"/><text x="330" y="110" text-anchor="middle" class="t6">2. JOIN / ON — aplica las condiciones de reunión</text>
  <rect x="40" y="124" width="580" height="30" rx="4" fill="#2d8659"/><text x="330" y="144" text-anchor="middle" class="t6">3. WHERE — filtra filas individuales</text>
  <rect x="40" y="158" width="580" height="30" rx="4" fill="#3f9970"/><text x="330" y="178" text-anchor="middle" class="t6">4. GROUP BY — agrupa las filas restantes</text>
  <rect x="40" y="192" width="580" height="30" rx="4" fill="#e89822"/><text x="330" y="212" text-anchor="middle" class="t6">5. HAVING — filtra grupos ya agregados</text>
  <rect x="40" y="226" width="580" height="30" rx="4" fill="#c98a1f"/><text x="330" y="246" text-anchor="middle" class="t6">6. SELECT — calcula columnas/expresiones de salida</text>
  <rect x="40" y="260" width="580" height="30" rx="4" fill="#8a6fb0"/><text x="330" y="280" text-anchor="middle" class="t6">7. DISTINCT — elimina filas duplicadas</text>
  <rect x="40" y="294" width="580" height="30" rx="4" fill="#d13c3c"/><text x="330" y="314" text-anchor="middle" class="t6">8. ORDER BY / LIMIT — ordena y recorta el resultado final</text>
  </g>
  <text x="330" y="342" text-anchor="middle" style="font:700 11px system-ui;fill:#0055a0">Por eso un alias del SELECT no sirve en WHERE, pero sí en ORDER BY</text>
  <text x="650" y="364" text-anchor="end" style="font:11px system-ui;fill:#666">[Fuente: SILBERSCHATZ, cap. 3]</text>
</svg>
```

---

## D7 · Tipos de JOIN

**Sección**: §2.5 — Tipologías de acoplamiento (Join)
**Propósito**: Representar con diagramas de Venn INNER, LEFT, RIGHT, FULL y CROSS.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 340" role="img" aria-label="Diagramas de Venn de los tipos de join: INNER JOIN solo la intersección, LEFT JOIN todo el círculo izquierdo, RIGHT JOIN todo el círculo derecho, FULL JOIN ambos círculos completos">
  <style>.t7{font:700 11px system-ui,sans-serif;fill:#123}.l7{font:10.5px system-ui,sans-serif;fill:#444}.h7{font:700 13px system-ui,sans-serif;fill:#0055a0}</style>
  <text x="340" y="22" text-anchor="middle" class="h7">Tipos de JOIN (A = tabla izquierda, B = tabla derecha)</text>
  <g transform="translate(25,50)">
    <circle cx="45" cy="60" r="45" fill="#0055a0" opacity="0.35"/><circle cx="85" cy="60" r="45" fill="#2d8659" opacity="0.35"/>
    <circle cx="65" cy="60" r="20" fill="#e89822"/>
    <text x="65" y="130" text-anchor="middle" class="t7">INNER JOIN</text><text x="65" y="146" text-anchor="middle" class="l7">solo coincidencias</text>
  </g>
  <g transform="translate(190,50)">
    <circle cx="45" cy="60" r="45" fill="#0055a0"/><circle cx="85" cy="60" r="45" fill="#2d8659" opacity="0.35"/>
    <text x="65" y="130" text-anchor="middle" class="t7">LEFT JOIN</text><text x="65" y="146" text-anchor="middle" class="l7">todo A + coincid. de B</text>
  </g>
  <g transform="translate(355,50)">
    <circle cx="45" cy="60" r="45" fill="#0055a0" opacity="0.35"/><circle cx="85" cy="60" r="45" fill="#2d8659"/>
    <text x="65" y="130" text-anchor="middle" class="t7">RIGHT JOIN</text><text x="65" y="146" text-anchor="middle" class="l7">todo B + coincid. de A</text>
  </g>
  <g transform="translate(520,50)">
    <circle cx="45" cy="60" r="45" fill="#0055a0"/><circle cx="85" cy="60" r="45" fill="#2d8659"/>
    <text x="65" y="130" text-anchor="middle" class="t7">FULL JOIN</text><text x="65" y="146" text-anchor="middle" class="l7">todo A + todo B</text>
  </g>
  <rect x="60" y="220" width="560" height="80" rx="6" fill="#eef4fa" stroke="#0055a0"/>
  <text x="340" y="242" text-anchor="middle" style="font:700 12px system-ui;fill:#0055a0">CROSS JOIN · SELF JOIN · NATURAL JOIN</text>
  <text x="340" y="264" text-anchor="middle" class="l7">CROSS = producto cartesiano completo, sin condición · SELF = una tabla unida consigo misma</text>
  <text x="340" y="282" text-anchor="middle" class="l7">NATURAL = reunión implícita por columnas homónimas — desaconsejado, frágil ante cambios de esquema</text>
  <text x="670" y="330" text-anchor="end" style="font:11px system-ui;fill:#666">[Fuente: ELMASRI, cap. 7]</text>
</svg>
```

---

## D8 · Subconsulta correlacionada frente a no correlacionada

**Sección**: §2.6 — Subconsultas y predicados cuantificados
**Propósito**: Mostrar la dependencia (o no) de la subconsulta respecto a la fila externa.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 320" role="img" aria-label="Comparación entre subconsulta no correlacionada que se ejecuta una sola vez de forma independiente y subconsulta correlacionada que referencia la fila externa y se reevalúa por cada fila candidata">
  <style>.t8{font:700 12px system-ui,sans-serif;fill:#fff}.s8{font:11px system-ui,sans-serif;fill:#fff}.l8{font:10.5px system-ui,sans-serif;fill:#444}.h8{font:700 13px system-ui,sans-serif;fill:#0055a0}</style>
  <text x="340" y="22" text-anchor="middle" class="h8">Subconsultas: dependencia respecto a la consulta externa</text>
  <rect x="30" y="42" width="300" height="120" rx="8" fill="#0055a0"/>
  <text x="180" y="66" text-anchor="middle" class="t8">NO CORRELACIONADA</text>
  <text x="180" y="88" text-anchor="middle" class="s8">Independiente de la fila externa</text>
  <text x="180" y="108" text-anchor="middle" class="s8">Se ejecuta UNA vez</text>
  <text x="180" y="128" text-anchor="middle" class="s8">... WHERE dni IN</text>
  <text x="180" y="146" text-anchor="middle" class="s8">(SELECT dni FROM TRIBUTO ...)</text>
  <rect x="350" y="42" width="300" height="120" rx="8" fill="#d13c3c"/>
  <text x="500" y="66" text-anchor="middle" class="t8">CORRELACIONADA</text>
  <text x="500" y="88" text-anchor="middle" class="s8">Referencia una columna externa</text>
  <text x="500" y="108" text-anchor="middle" class="s8">Se reevalúa por cada fila candidata</text>
  <text x="500" y="128" text-anchor="middle" class="s8">... WHERE EXISTS</text>
  <text x="500" y="146" text-anchor="middle" class="s8">(SELECT 1 ... WHERE t.dni=c.dni)</text>
  <rect x="60" y="186" width="560" height="100" rx="6" fill="#fdf3e3" stroke="#e89822"/>
  <text x="340" y="208" text-anchor="middle" style="font:700 12px system-ui;fill:#8a5a00">Predicados cuantificados</text>
  <text x="340" y="230" text-anchor="middle" class="l8">IN/NOT IN · EXISTS/NOT EXISTS · ANY/SOME · ALL</text>
  <text x="340" y="250" text-anchor="middle" class="l8">EXISTS suele ser más eficiente que IN (se detiene en la 1ª coincidencia)</text>
  <text x="340" y="268" text-anchor="middle" style="font:700 11px system-ui;fill:#d13c3c">NOT IN + NULL en el conjunto → puede no devolver filas (trampa clásica)</text>
  <text x="670" y="312" text-anchor="end" style="font:11px system-ui;fill:#666">[Fuente: DATE-INTRO, cap. 8]</text>
</svg>
```

---

## D9 · CTE recursiva: ancla y miembro recursivo

**Sección**: §2.7 — Expresiones de Tabla Comunes y consultas recursivas
**Propósito**: Visualizar el bucle lógico ancla → recursivo → parada.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 640 346" role="img" aria-label="Ciclo de una CTE recursiva: el miembro ancla produce el conjunto inicial, el miembro recursivo se une con UNION ALL y se reevalúa sobre el resultado anterior hasta que no produce filas nuevas">
  <style>.t9{font:700 12px system-ui,sans-serif;fill:#fff}.s9{font:10.5px system-ui,sans-serif;fill:#fff}.l9{font:11px system-ui,sans-serif;fill:#444}.h9{font:700 13px system-ui,sans-serif;fill:#0055a0}</style>
  <text x="320" y="22" text-anchor="middle" class="h9">WITH RECURSIVE: ancla + miembro recursivo</text>
  <rect x="230" y="40" width="180" height="46" rx="6" fill="#0055a0"/><text x="320" y="60" text-anchor="middle" class="t9">MIEMBRO ANCLA</text><text x="320" y="78" text-anchor="middle" class="s9">consulta base (nivel 1)</text>
  <path d="M320 86 L320 116" stroke="#888" stroke-width="2" marker-end="url(#a9)"/>
  <text x="345" y="106" class="l9">UNION ALL</text>
  <rect x="200" y="118" width="240" height="46" rx="6" fill="#2d8659"/><text x="320" y="138" text-anchor="middle" class="t9">MIEMBRO RECURSIVO</text><text x="320" y="156" text-anchor="middle" class="s9">referencia la propia CTE</text>
  <path d="M460 216 L500 216 L500 141 L444 141" stroke="#0055a0" stroke-width="2" fill="none" marker-end="url(#a9)"/><text x="470" y="210" class="l9">SÍ</text>
  <text x="506" y="170" class="l9">se repite sobre</text>
  <text x="506" y="184" class="l9">el resultado previo</text>
  <path d="M320 164 L320 194" stroke="#888" stroke-width="2" marker-end="url(#a9)"/>
  <rect x="180" y="196" width="280" height="40" rx="6" fill="#e89822"/><text x="320" y="221" text-anchor="middle" class="t9">¿produce filas nuevas?</text>
  <path d="M320 236 L320 260" stroke="#d13c3c" stroke-width="2" marker-end="url(#a9)"/>
  <text x="345" y="252" class="l9">NO</text>
  <rect x="230" y="262" width="180" height="40" rx="6" fill="#d13c3c"/><text x="320" y="287" text-anchor="middle" class="t9">PARADA (implícita)</text>
  <defs><marker id="a9" markerWidth="8" markerHeight="8" refX="4" refY="4" orient="auto"><path d="M0 0 L8 4 L0 8 z" fill="#888"/></marker></defs>
  <text x="60" y="322" class="l9">Sin una condición de reunión que reduzca el conjunto, la recursión no termina</text>
  <text x="630" y="340" text-anchor="end" style="font:11px system-ui;fill:#666">[Fuente: EISENBERG99]</text>
</svg>
```

---

## D10 · Ciclo de vida de un procedimiento almacenado

**Sección**: §3.2 — Arquitectura y ciclo de vida de ejecución en el servidor
**Propósito**: Mostrar las fases desde CREATE hasta la reutilización del plan cacheado.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 320" role="img" aria-label="Ciclo de vida de un procedimiento almacenado: creación y análisis, almacenamiento en el catálogo, primera ejecución con generación de plan, ejecuciones posteriores reutilizando el plan cacheado, invalidación y recompilación">
  <style>.t10{font:700 11px system-ui,sans-serif;fill:#fff}.s10{font:10px system-ui,sans-serif;fill:#fff}.l10{font:11px system-ui,sans-serif;fill:#444}.h10{font:700 13px system-ui,sans-serif;fill:#0055a0}</style>
  <text x="340" y="22" text-anchor="middle" class="h10">CREATE PROCEDURE → catálogo → caché de planes</text>
  <rect x="20" y="42" width="140" height="60" rx="6" fill="#0055a0"/><text x="90" y="66" text-anchor="middle" class="t10">1. CREACIÓN</text><text x="90" y="82" text-anchor="middle" class="s10">análisis y</text><text x="90" y="94" text-anchor="middle" class="s10">almacenamiento</text>
  <path d="M160 72 L188 72" stroke="#888" stroke-width="2" marker-end="url(#a10)"/>
  <rect x="190" y="42" width="140" height="60" rx="6" fill="#3778b5"/><text x="260" y="66" text-anchor="middle" class="t10">2. CATÁLOGO</text><text x="260" y="82" text-anchor="middle" class="s10">diccionario de</text><text x="260" y="94" text-anchor="middle" class="s10">datos del SGBD</text>
  <path d="M330 72 L358 72" stroke="#888" stroke-width="2" marker-end="url(#a10)"/>
  <rect x="360" y="42" width="150" height="60" rx="6" fill="#2d8659"/><text x="435" y="62" text-anchor="middle" class="t10">3. 1ª EJECUCIÓN</text><text x="435" y="78" text-anchor="middle" class="s10">genera plan y</text><text x="435" y="92" text-anchor="middle" class="s10">lo cachea</text>
  <path d="M510 72 L547 72" stroke="#888" stroke-width="2" marker-end="url(#a10)"/>
  <rect x="555" y="42" width="105" height="60" rx="6" fill="#e89822"/><text x="607" y="66" text-anchor="middle" class="t10">4. SIGUIENTES</text><text x="607" y="82" text-anchor="middle" class="s10">reutiliza</text><text x="607" y="94" text-anchor="middle" class="s10">plan cacheado</text>
  <path d="M607 102 C 607 170 435 170 435 112" stroke="#2d8659" stroke-width="2" fill="none" marker-end="url(#a10)"/>
  <text x="340" y="178" text-anchor="middle" style="font:700 11px system-ui;fill:#2d8659">Rendimiento: se evita repetir análisis y optimización</text>
  <text x="340" y="194" text-anchor="middle" style="font:700 11px system-ui;fill:#2d8659">en cada llamada</text>
  <rect x="120" y="216" width="440" height="70" rx="6" fill="#fdecec" stroke="#d13c3c"/>
  <text x="340" y="238" text-anchor="middle" style="font:700 12px system-ui;fill:#8a1f1f">5. Invalidación y recompilación</text>
  <text x="340" y="258" text-anchor="middle" class="l10">Cambian estadísticas, se altera un objeto referenciado, o ALTER PROCEDURE</text>
  <text x="340" y="274" text-anchor="middle" class="l10">→ el plan se recalcula en la siguiente llamada</text>
  <defs><marker id="a10" markerWidth="8" markerHeight="8" refX="4" refY="4" orient="auto"><path d="M0 0 L8 4 L0 8 z" fill="#888"/></marker></defs>
  <text x="670" y="312" text-anchor="end" style="font:11px system-ui;fill:#666">[Fuente: GMUW, cap. 8]</text>
</svg>
```

---

## D11 · Ciclo de vida de un cursor explícito

**Sección**: §3.4 — Gestión de cursores y tipos de cursores
**Propósito**: Fijar la secuencia DECLARE → OPEN → FETCH → CLOSE.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 640 320" role="img" aria-label="Ciclo de vida de un cursor explícito: declarar el cursor sobre un SELECT, abrirlo, recorrer las filas con fetch en bucle hasta agotar el resultado, y cerrarlo"><style>.t11{font:700 12px system-ui,sans-serif;fill:#fff}.s11{font:10.5px system-ui,sans-serif;fill:#fff}.l11{font:11px system-ui,sans-serif;fill:#444}.h11{font:700 13px system-ui,sans-serif;fill:#0055a0}</style>
  <text x="320" y="22" text-anchor="middle" class="h11">DECLARE → OPEN → FETCH (bucle) → CLOSE</text>
  <rect x="30" y="44" width="130" height="50" rx="6" fill="#0055a0"/><text x="95" y="64" text-anchor="middle" class="t11">DECLARE</text><text x="95" y="80" text-anchor="middle" class="s11">define el SELECT</text>
  <path d="M160 69 L188 69" stroke="#888" stroke-width="2" marker-end="url(#a11)"/>
  <rect x="190" y="44" width="110" height="50" rx="6" fill="#3778b5"/><text x="245" y="64" text-anchor="middle" class="t11">OPEN</text><text x="245" y="80" text-anchor="middle" class="s11">ejecuta la consulta</text>
  <path d="M300 69 L328 69" stroke="#888" stroke-width="2" marker-end="url(#a11)"/>
  <rect x="330" y="44" width="150" height="50" rx="6" fill="#2d8659"/><text x="405" y="64" text-anchor="middle" class="t11">FETCH</text><text x="405" y="80" text-anchor="middle" class="s11">obtiene la fila siguiente</text>
  <path d="M405 94 C 405 140 405 140 405 94" stroke="none"/>
  <path d="M480 69 L495 69 L495 130 L405 130 L405 100" stroke="#e89822" stroke-width="2" fill="none" marker-end="url(#a11)"/>
  <text x="450" y="150" text-anchor="middle" style="font:700 11px system-ui;fill:#e89822">¿quedan filas? → repetir FETCH</text>
  <path d="M405 160 L405 188" stroke="#888" stroke-width="2" marker-end="url(#a11)"/>
  <text x="440" y="180" class="l11">no</text>
  <rect x="330" y="190" width="150" height="50" rx="6" fill="#d13c3c"/><text x="405" y="212" text-anchor="middle" class="t11">CLOSE</text><text x="405" y="228" text-anchor="middle" class="s11">libera el cursor</text>
  <rect x="60" y="256" width="520" height="46" rx="6" fill="#fdecec" stroke="#d13c3c"/>
  <text x="320" y="284" text-anchor="middle" style="font:11px system-ui;fill:#8a1f1f">Olvidar el CLOSE mantiene recursos y bloqueos ocupados durante toda la sesión</text>
  <defs><marker id="a11" markerWidth="8" markerHeight="8" refX="4" refY="4" orient="auto"><path d="M0 0 L8 4 L0 8 z" fill="#888"/></marker></defs>
  <text x="630" y="316" text-anchor="end" style="font:11px system-ui;fill:#666">[Fuente: SILBERSCHATZ, cap. 5]</text>
</svg>
```

---

## D12 · Clasificación de disparadores

**Sección**: §4.2 — Clasificación de los disparadores
**Propósito**: Matriz BEFORE/AFTER × fila/sentencia, más INSTEAD OF y eventos programados.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 680 360" role="img" aria-label="Matriz de clasificación de disparadores: BEFORE y AFTER cruzados con de fila y de sentencia, más INSTEAD OF sobre vistas y eventos programados por tiempo">
  <style>.t12{font:700 11px system-ui,sans-serif;fill:#fff}.s12{font:10px system-ui,sans-serif;fill:#fff}.l12{font:11px system-ui,sans-serif;fill:#444}.h12{font:700 13px system-ui,sans-serif;fill:#0055a0}</style>
  <text x="340" y="20" text-anchor="middle" class="h12">Cuatro combinaciones DML + dos tipos especiales</text>
  <rect x="180" y="34" width="180" height="26" fill="#0055a0"/><text x="270" y="52" text-anchor="middle" class="t12">DE FILA</text>
  <rect x="360" y="34" width="180" height="26" fill="#2d8659"/><text x="450" y="52" text-anchor="middle" class="t12">DE SENTENCIA</text>
  <rect x="20" y="60" width="160" height="26" fill="#e89822"/><text x="100" y="78" text-anchor="middle" class="t12">BEFORE</text>
  <rect x="20" y="86" width="160" height="26" fill="#c98a1f"/><text x="100" y="104" text-anchor="middle" class="t12">AFTER</text>
  <rect x="180" y="60" width="180" height="52" fill="#fbe9cf" stroke="#fff"/><text x="270" y="81" text-anchor="middle" style="font:10.5px system-ui;fill:#123">valida/modifica antes,</text>
  <text x="270" y="97" text-anchor="middle" style="font:10.5px system-ui;fill:#123">1 ejecución por fila (OLD/NEW)</text>
  <rect x="360" y="60" width="180" height="52" fill="#e3f1e9" stroke="#fff"/><text x="450" y="81" text-anchor="middle" style="font:10.5px system-ui;fill:#123">valida antes del lote,</text>
  <text x="450" y="97" text-anchor="middle" style="font:10.5px system-ui;fill:#123">1 ejecución por sentencia</text>
  <rect x="20" y="130" width="300" height="70" rx="6" fill="#0055a0"/>
  <text x="170" y="152" text-anchor="middle" class="t12">INSTEAD OF (sobre vistas)</text>
  <text x="170" y="172" text-anchor="middle" class="s12">sustituye el DML cuando la vista</text>
  <text x="170" y="186" text-anchor="middle" class="s12">no es directamente actualizable</text>
  <rect x="340" y="130" width="300" height="70" rx="6" fill="#d13c3c"/>
  <text x="490" y="152" text-anchor="middle" class="t12">EVENTOS PROGRAMADOS</text>
  <text x="490" y="172" text-anchor="middle" class="s12">se disparan por TIEMPO, no por DML</text>
  <text x="490" y="186" text-anchor="middle" class="s12">mantenimiento periódico, purgas</text>
  <rect x="60" y="216" width="560" height="120" rx="6" fill="#eef4fa" stroke="#0055a0"/>
  <text x="340" y="238" text-anchor="middle" style="font:700 12px system-ui;fill:#0055a0">Tres casos de uso principales</text>
  <text x="340" y="260" text-anchor="middle" class="l12">Auditoría y trazabilidad (AFTER + tabla de log con OLD/NEW)</text>
  <text x="340" y="280" text-anchor="middle" class="l12">Mantenimiento de integridad (BEFORE + validación cruzada entre tablas)</text>
  <text x="340" y="300" text-anchor="middle" class="l12">Automatización (contadores derivados, timestamps, notificaciones)</text>
  <text x="340" y="322" text-anchor="middle" style="font:700 11px system-ui;fill:#d13c3c">Usar con moderación: efectos ocultos, cascadas, coste en rendimiento</text>
  <text x="670" y="352" text-anchor="end" style="font:11px system-ui;fill:#666">[Fuente: ELMASRI, cap. 5; MYSQL-DOC]</text>
</svg>
```

---

*Los 12 diagramas usan la misma paleta y convenciones de accesibilidad que el resto de la serie técnica (T11-T18). Ver QA de caja contenedora en tema-19-validacion.md.*
