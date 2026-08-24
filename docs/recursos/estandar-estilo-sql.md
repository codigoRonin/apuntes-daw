# Estándar de estilo SQL del módulo

En cualquier equipo profesional, el SQL se escribe con una guía de estilo común: el código se lee muchas más veces de las que se escribe, y un estilo uniforme hace posible leerlo, corregirlo y mantenerlo. Aquí igual: lo que entregas **se lee, se ejecuta y se defiende**. Ninguna de estas reglas es nueva — viven repartidas por las unidades; este documento las reúne para consultarlas de un vistazo.

## Apartado 1. El script de entrega

- Un único archivo `.sql` por actividad, que **ejecuta de arriba abajo sin errores**: se corrige ejecutando, y una sentencia que revienta a mitad deja sin corregir lo que venga detrás.
- Cabecera en comentario con tu **alias** (nunca tu nombre real: política de protección de datos del centro), la actividad y la fecha:

```sql
-- Alias: A07 · UD4 · Actividad evaluativa final
-- Fecha: 2026-11-14
```

- Cada ejercicio, precedido de su número en comentario (`-- AE3`). Las justificaciones que el enunciado pida van en comentario junto a su consulta, no en un documento aparte.
- Toda sentencia termina en `;`.

## Apartado 2. La escritura de la consulta

- **Palabras clave SQL en MAYÚSCULAS** (`SELECT`, `FROM`, `LEFT JOIN`, `GROUP BY`...); nombres de tablas y columnas en minúsculas.
- En cuanto la consulta crece, **una cláusula principal por línea** (`SELECT` / `FROM` / cada `JOIN` / `WHERE` / `GROUP BY` / `HAVING` / `ORDER BY` / `LIMIT`), con sangría para lo subordinado: condiciones del `ON`, subconsultas, ramas de un `UNION`.
- Fechas siempre en formato ISO entre comillas simples (`'2026-06-01'`); cadenas entre comillas simples.

## Apartado 3. Los nombres

- Identificadores en `snake_case`, **sin tildes ni eñes**; tablas en plural (`clientes`, `pedidos`). El nombre de una clave ajena debe hacer evidente a qué tabla apunta.
- **Alias de tabla obligatorios en cuanto hay más de una tabla**: cortos, consistentes dentro del script, y con **todas las columnas prefijadas** en consultas multitabla — sin prefijo, la consulta es ambigua para el gestor y para quien te corrige.
- Alias de columna sin espacios (`precio_con_iva`, no `` `precio con IVA` ``): los alias con espacios dan problemas al consumirlos desde código.

## Apartado 4. Lo que no se entrega

- **`SELECT *` en código entregado, nunca.** Pide las columnas que necesitas: `*` rompe cuando la tabla cambia y arrastra datos que nadie pidió.
- **Consultas sin probar.** Si no la has ejecutado sobre el juego de datos y comprobado el resultado, no va al script.
- En ejercicios de rendimiento: **funciones sobre la columna filtrada cuando existe la escritura sargable** (apartado 10 de la UD4) — el índice deja de servir y lo sabes demostrar con `EXPLAIN`.

## Apartado 5. Ejemplo de referencia

Una consulta escrita al estándar, sobre el juego de datos TiendaDAW de la UD4:

```sql
-- AE4 · Ticket medio por cliente (JOIN interno: solo clientes con compras)
SELECT c.nombre,
       COUNT(DISTINCT p.id)                 AS num_pedidos,
       ROUND(SUM(lp.cantidad * pr.precio)
             / COUNT(DISTINCT p.id), 2)     AS gasto_medio_pedido
FROM clientes c
    INNER JOIN pedidos p        ON p.cliente_id = c.id
    INNER JOIN lineas_pedido lp ON lp.pedido_id = p.id
    INNER JOIN productos pr     ON pr.id = lp.producto_id
GROUP BY c.id, c.nombre
ORDER BY gasto_medio_pedido DESC;
```

Fíjate: número de ejercicio y decisión justificada en el comentario, palabras clave en mayúsculas, una cláusula por línea con los `JOIN` sangrados, alias cortos y todas las columnas prefijadas, alias de resultado en `snake_case`, y punto y coma final.

## Apartado 6. Estatus de estas reglas

Las reglas de los apartados 1, 3 y 4 **las exigen los propios enunciados** de las actividades evaluables — forman parte del formato de entrega, y una entrega que no las cumple se corrige como el enunciado ya prevé. Las demás son la convención profesional del módulo: se espera su uso, y en la defensa oral se te podrá pedir justificar cualquier desviación. Si alguna vez este documento y un enunciado concreto entran en conflicto, **manda el enunciado**.
