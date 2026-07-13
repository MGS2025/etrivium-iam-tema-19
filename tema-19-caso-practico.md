# Tema 19 — Casos Prácticos

> **Título oficial**: Lenguajes de interrogación de bases de datos. El estándar ANSI SQL. Procedimientos almacenados. Eventos y disparadores.
>
> **Formato**: 3 casos prácticos sobre supuestos reales del Ayuntamiento de Madrid. Cada caso suma **10 puntos**.
> **Nivel**: C1 — Técnico Auxiliar TIC, Ayuntamiento de Madrid

Los tres casos recorren el tema sobre el esquema `DISTRITO / CONTRIBUYENTE / TRIBUTO / EXPEDIENTE / FUNCIONARIO` (ver tema-19-contenido.md, «Convenciones»): el **Caso 1** trabaja **SQL declarativo** (joins, agregación, subconsultas) sobre los tributos; el **Caso 2**, un **procedimiento almacenado con cursor** sobre el censo de contribuyentes; y el **Caso 3**, un **disparador de auditoría** sobre la tramitación de expedientes.

---

## Caso 1 — Análisis de recaudación de tributos por distrito

### Enunciado

El área de Hacienda quiere analizar la recaudación de **TRIBUTO** liquidada por **CONTRIBUYENTE** y **DISTRITO**. Se pide escribir, en **SQL estándar**, las consultas necesarias para responder a cuatro preguntas de negocio sobre el esquema `DISTRITO(id_distrito, nombre_distrito)`, `CONTRIBUYENTE(dni, nombre, id_distrito)` y `TRIBUTO(id_tributo, tipo, importe, dni_contribuyente, fecha_liquidacion)`.

### Cuestiones

**Cuestión 1 — Agregación y filtrado de grupos (3 puntos).** Escriba la consulta que devuelve, para cada distrito, el número de tributos liquidados y el importe total recaudado, mostrando **solo los distritos** cuyo total recaudado supere los 100.000 €, ordenados de mayor a menor recaudación.

**Cuestión 2 — Outer join (2 puntos).** Escriba la consulta que devuelve **todos** los contribuyentes de un distrito, tengan o no algún tributo liquidado, junto con el importe de sus tributos (o vacío si no tiene ninguno).

**Cuestión 3 — Subconsulta correlacionada (3 puntos).** Escriba la consulta que devuelve el nombre de los contribuyentes cuyo **tributo de mayor importe** sea superior a la **media de importes de su propio distrito**.

**Cuestión 4 — Función de ventana (2 puntos).** Escriba la consulta que, para cada tributo, muestre su importe junto con el **puesto** que ocupa ese tributo dentro de los tributos del mismo contribuyente, ordenados de mayor a menor importe.

### Solución orientativa

- **C1**: agregación con `GROUP BY` y filtro de grupo con `HAVING` (§2.4). El orden de evaluación explica por qué la condición sobre el total va en `HAVING`, no en `WHERE`.

```sql
SELECT d.id_distrito, d.nombre_distrito, COUNT(*) AS num_tributos, SUM(t.importe) AS total_recaudado
FROM   DISTRITO d
JOIN   CONTRIBUYENTE c ON c.id_distrito = d.id_distrito
JOIN   TRIBUTO t ON t.dni_contribuyente = c.dni
GROUP BY d.id_distrito, d.nombre_distrito
HAVING SUM(t.importe) > 100000
ORDER BY total_recaudado DESC;
```

- **C2**: `LEFT JOIN` desde `CONTRIBUYENTE` hacia `TRIBUTO`, para no perder los contribuyentes sin tributos (§2.5).

```sql
SELECT c.nombre, t.importe
FROM   CONTRIBUYENTE c
LEFT JOIN TRIBUTO t ON t.dni_contribuyente = c.dni
WHERE  c.id_distrito = 3;
```

- **C3**: subconsulta **correlacionada** (referencia `c.id_distrito` de la fila externa) comparando el `MAX` del contribuyente frente al `AVG` de su distrito (§2.6).

```sql
SELECT c.nombre
FROM   CONTRIBUYENTE c
WHERE  (SELECT MAX(t.importe) FROM TRIBUTO t WHERE t.dni_contribuyente = c.dni)
     > (SELECT AVG(t2.importe) FROM TRIBUTO t2
        JOIN CONTRIBUYENTE c2 ON c2.dni = t2.dni_contribuyente
        WHERE c2.id_distrito = c.id_distrito);
```

- **C4**: función de ventana `RANK()` particionada por contribuyente (§2.8); no colapsa filas, a diferencia de un `GROUP BY`.

```sql
SELECT dni_contribuyente, importe,
       RANK() OVER (PARTITION BY dni_contribuyente ORDER BY importe DESC) AS puesto
FROM   TRIBUTO;
```

### Criterios de evaluación

| Criterio | Puntos |
|---|---|
| GROUP BY + HAVING correctos, con orden y agregación adecuados | 3 |
| LEFT JOIN correcto, conserva contribuyentes sin tributos | 2 |
| Subconsulta correlacionada correcta, referencia la fila externa | 3 |
| Función de ventana RANK() con PARTITION BY correcta | 2 |

---

## Caso 2 — Procedimiento almacenado de recálculo de bonificaciones del Padrón

### Enunciado

Se necesita un **procedimiento almacenado** que, para un distrito dado, recorra **todos** sus contribuyentes con tributos liquidados y aplique una bonificación del 10 % a los importes superiores a 500 €, devolviendo además el **número total de tributos bonificados**. La lógica no puede expresarse como una única sentencia `UPDATE` de conjunto porque, en una versión posterior, cada tributo llevará una regla de bonificación distinta según su tipo (se pide, por tanto, resolverlo con cursor).

### Cuestiones

**Cuestión 1 — Parámetros (2 puntos).** Defina la cabecera del procedimiento `bonificar_tributos_distrito` indicando el modo (IN/OUT) de cada parámetro: el distrito a procesar y el número de tributos bonificados.

**Cuestión 2 — Cursor (3 puntos).** Escriba, en pseudocódigo SQL genérico, el cursor que recorre los tributos superiores a 500 € de ese distrito, y su ciclo `DECLARE → OPEN → FETCH → CLOSE`.

**Cuestión 3 — Control de flujo (3 puntos).** Dentro del bucle del cursor, escriba la lógica que aplica la bonificación del 10 % y actualiza el contador de salida.

**Cuestión 4 — Excepciones (2 puntos).** ¿Qué bloque de manejo de errores añadiría, y qué haría si falla la actualización de un tributo concreto? Justifique si debería abortar todo el procedimiento o continuar con el resto.

### Solución orientativa

- **C1**: un parámetro `IN` (dato de entrada, no se modifica) y un parámetro `OUT` (el procedimiento lo asigna y se devuelve al llamador) (§3.3).

```
CREAR PROCEDIMIENTO bonificar_tributos_distrito(
    ENTRADA p_distrito : entero,
    SALIDA p_num_bonificados : entero
)
```

- **C2**: cursor explícito forward-only sobre los tributos superiores a 500 € del distrito (§3.4).

```
DECLARAR CURSOR c_tributos PARA
    SELECT t.id_tributo, t.importe
    FROM   TRIBUTO t JOIN CONTRIBUYENTE c ON c.dni = t.dni_contribuyente
    WHERE  c.id_distrito = p_distrito AND t.importe > 500

ABRIR c_tributos
BUCLE
    OBTENER SIGUIENTE DE c_tributos EN v_id_tributo, v_importe
    SALIR_SI_NO_HAY_MAS_FILAS
    -- (cuerpo en C3)
FIN_BUCLE
CERRAR c_tributos
```

- **C3**: dentro del bucle, se calcula el nuevo importe y se actualiza la fila y el contador (§3.5, estructura `SI`).

```
INICIO
    p_num_bonificados = 0
    v_nuevo_importe = v_importe * 0.90
    ACTUALIZAR TRIBUTO
    ESTABLECER importe = v_nuevo_importe
    DONDE id_tributo = v_id_tributo
    p_num_bonificados = p_num_bonificados + 1
FIN
```

- **C4**: un bloque `INICIO_BLOQUE_PROTEGIDO … EXCEPCION CUANDO … FIN_BLOQUE_PROTEGIDO` alrededor de la actualización de cada fila (§3.6). Como el procesamiento es **fila a fila** y el fallo de un tributo no debería impedir bonificar el resto, lo más razonable es **capturar la excepción dentro del bucle**, registrar el error (por ejemplo, insertándolo en una tabla de incidencias) y **continuar** con la siguiente iteración, en lugar de deshacer todo el procedimiento con un `ROLLBACK` global — salvo que el requisito de negocio exija que el lote sea todo-o-nada.

### Criterios de evaluación

| Criterio | Puntos |
|---|---|
| Cabecera con modos IN/OUT correctos | 2 |
| Cursor con ciclo DECLARE/OPEN/FETCH/CLOSE completo y condición correcta | 3 |
| Lógica de bonificación y actualización del contador correctas | 3 |
| Manejo de excepciones razonado (continuar vs abortar) | 2 |

---

## Caso 3 — Auditoría de cambios de estado en expedientes

### Enunciado

Un expediente pasa por los estados `ABIERTO → EN_TRAMITE → RESUELTO → CERRADO`. Se pide diseñar los **disparadores** necesarios para: (a) **impedir** que un expediente pase a `CERRADO` si no ha pasado antes por `RESUELTO`, y (b) **registrar** en una tabla `AUDITORIA_EXPEDIENTE` cada cambio de estado, con el estado anterior, el nuevo y el funcionario que lo realizó.

### Cuestiones

**Cuestión 1 — Clasificación (2 puntos).** ¿Qué tipo de disparador (BEFORE/AFTER, de fila/de sentencia) usaría para cada una de las dos necesidades (a) y (b)? Justifique la elección.

**Cuestión 2 — Validación (3 puntos).** Escriba, en pseudocódigo SQL genérico, el disparador que impide el cambio a `CERRADO` sin pasar por `RESUELTO`.

**Cuestión 3 — Auditoría (3 puntos).** Escriba el disparador que registra cada cambio de estado en `AUDITORIA_EXPEDIENTE(id_expediente, estado_anterior, estado_nuevo, id_funcionario, fecha)`.

**Cuestión 4 — Riesgos (2 puntos).** ¿Qué riesgo debe vigilarse si en el futuro se añade un tercer disparador sobre `EXPEDIENTE` que también modifica la propia tabla `EXPEDIENTE`? ¿Cómo lo mitigaría?

### Solución orientativa

- **C1**: (a) requiere un disparador **BEFORE UPDATE, de fila** (`FOR EACH ROW`), porque debe **validar y poder cancelar** la operación **antes** de que se aplique, y necesita el valor `NUEVO.estado` de cada fila concreta; (b) requiere un disparador **AFTER UPDATE, de fila**, porque debe registrar el cambio **una vez confirmado**, con acceso a `ANTIGUO.estado` y `NUEVO.estado` de cada fila (§4.2).

- **C2**: el disparador `BEFORE` valida la transición y lanza un error que cancela la operación si no se cumple (§3.6, §4.2).

```
CREAR DISPARADOR trg_valida_cierre_expediente
ANTES DE ACTUALIZAR EN EXPEDIENTE
PARA_CADA_FILA
INICIO
    SI NUEVO.estado = 'CERRADO' Y ANTIGUO.estado <> 'RESUELTO' ENTONCES
        LANZAR ERROR 'No se puede cerrar un expediente que no ha sido resuelto'
    FIN_SI
FIN
```

- **C3**: el disparador `AFTER` inserta la fila de auditoría solo cuando el estado ha cambiado realmente (§4.3, auditoría y trazabilidad).

```
CREAR DISPARADOR trg_auditoria_expediente
DESPUES DE ACTUALIZAR EN EXPEDIENTE
PARA_CADA_FILA
INICIO
    SI NUEVO.estado <> ANTIGUO.estado ENTONCES
        INSERTAR EN AUDITORIA_EXPEDIENTE (id_expediente, estado_anterior, estado_nuevo, id_funcionario, fecha)
        VALORES (NUEVO.id_expediente, ANTIGUO.estado, NUEVO.estado, USUARIO_ACTUAL(), FECHA_ACTUAL())
    FIN_SI
FIN
```

- **C4**: el riesgo es una **cadena de disparadores** (*trigger chain*): si el tercer disparador vuelve a modificar `EXPEDIENTE`, puede reactivar `trg_valida_cierre_expediente` y `trg_auditoria_expediente`, generando efectos difíciles de rastrear o incluso una **recursión no controlada** (§4.4). Se mitiga documentando explícitamente el orden y el propósito de cada disparador, evitando que un disparador AFTER vuelva a escribir sobre la misma tabla que lo activó salvo que sea estrictamente necesario, y probando el comportamiento con actualizaciones **masivas**, no solo de una fila.

### Criterios de evaluación

| Criterio | Puntos |
|---|---|
| Clasificación BEFORE/AFTER y de fila justificada para (a) y (b) | 2 |
| Disparador de validación correcto, cancela la operación indebida | 3 |
| Disparador de auditoría correcto, solo registra cambios reales | 3 |
| Identifica el riesgo de cadena de disparadores y propone mitigación | 2 |

---

*Los tres casos son orientativos y pensados para la autoevaluación; las soluciones muestran una vía correcta, no la única posible. Las consultas del Caso 1 usan SQL estándar; los procedimientos y disparadores de los Casos 2 y 3, pseudocódigo SQL genérico (decisión de Joan), independiente de cualquier motor concreto.*
