# Bases de Datos — UD4: Consultas de datos con SQL (SELECT)

**Ciclo:** CFGS Desarrollo de Aplicaciones Web (DAW) — 1.º curso
**Módulo:** Bases de Datos (0484)
**Unidad didáctica:** 4 — Realización de consultas
**Curso:** 2026-2027

## Vinculación con RA y CE

| RA | CE trabajados en esta unidad | Apartado |
|----|------------------------------|----------|
| RA3 — Realiza consultas de datos interpretando su alcance | CE relativos a: identificación de herramientas y sentencias de consulta; consultas simples sobre una tabla; consultas con filtrado y ordenación; funciones del lenguaje sobre filas; funciones de agregado y agrupamiento; consultas sobre varias tablas (composiciones internas y externas); subconsultas | §1–§8 |

!!! warning "Pendiente de verificar"
    Sustituir por la redacción literal de los CE del módulo 0484 cuando la Orden ECD/843/2024 / Catálogo Modular de Aragón esté disponible en el Proyecto.

## Al terminar esta unidad sabrás...

- Recuperar exactamente los datos que necesitas de una tabla, ni uno más ni uno menos.
- Filtrar, ordenar y limitar resultados con criterio, incluyendo el manejo correcto de valores nulos.
- Transformar valores con funciones de cadena, numéricas y de fecha.
- Calcular totales, medias y recuentos agrupando filas, y filtrar sobre esos cálculos.
- Combinar datos de varias tablas con JOIN, entendiendo qué pasa con las filas que "no casan".
- Escribir subconsultas y decidir cuándo son mejores (o peores) que un JOIN.
- Leer una consulta ajena y explicar qué devuelve — que es lo que defenderás oralmente.

---

<div style="page-break-before: always;"></div>

## §0. Nuestro dominio de trabajo: TiendaDAW

Toda la unidad usa la misma base de datos: la de la tienda online que estáis construyendo en el proyecto del módulo. Antes de empezar, carga este script en tu SGBD para poder ejecutar y modificar **todos** los ejemplos de la unidad. En esta unidad no basta con leer: cada consulta hay que ejecutarla, romperla y arreglarla.

```sql
DROP TABLE IF EXISTS lineas_pedido, pedidos, productos, clientes;

CREATE TABLE clientes (
    id        INT PRIMARY KEY AUTO_INCREMENT,
    nombre    VARCHAR(100) NOT NULL,
    email     VARCHAR(150) NOT NULL UNIQUE,
    provincia VARCHAR(50),
    alta      DATE NOT NULL
);

CREATE TABLE productos (
    id        INT PRIMARY KEY AUTO_INCREMENT,
    nombre    VARCHAR(120) NOT NULL,
    categoria VARCHAR(50)  NOT NULL,
    precio    DECIMAL(8,2) NOT NULL,
    stock     INT NOT NULL DEFAULT 0
);

CREATE TABLE pedidos (
    id         INT PRIMARY KEY AUTO_INCREMENT,
    cliente_id INT NOT NULL,
    fecha      DATE NOT NULL,
    FOREIGN KEY (cliente_id) REFERENCES clientes(id)
);

CREATE TABLE lineas_pedido (
    pedido_id   INT NOT NULL,
    producto_id INT NOT NULL,
    cantidad    INT NOT NULL,
    PRIMARY KEY (pedido_id, producto_id),
    FOREIGN KEY (pedido_id)   REFERENCES pedidos(id),
    FOREIGN KEY (producto_id) REFERENCES productos(id)
);

INSERT INTO clientes (nombre, email, provincia, alta) VALUES
('Lucía Aranda',  'lucia@mail.com',  'Zaragoza', '2025-03-10'),
('Marc Ferrer',   'marc@mail.com',   'Huesca',   '2025-06-22'),
('Sara Osuna',    'sara@mail.com',   NULL,       '2026-01-15'),
('Raúl Domingo',  'raul@mail.com',   'Zaragoza', '2026-02-01'),
('Ana Peralta',   'ana@mail.com',    'Teruel',   '2026-05-30');

INSERT INTO productos (nombre, categoria, precio, stock) VALUES
('Teclado mecánico 60%',   'Periféricos', 79.90, 12),
('Ratón inalámbrico',      'Periféricos', 24.50, 40),
('Monitor 27" QHD',        'Monitores',  249.00,  5),
('Hub USB-C 7 puertos',    'Accesorios',  35.00,  0),
('Alfombrilla XL',         'Accesorios',   9.99, 60),
('Webcam 1080p',           'Periféricos', 45.00,  8);

INSERT INTO pedidos (cliente_id, fecha) VALUES
(1, '2026-04-02'),
(1, '2026-06-15'),
(2, '2026-05-20'),
(4, '2026-06-28');

INSERT INTO lineas_pedido (pedido_id, producto_id, cantidad) VALUES
(1, 1, 1), (1, 5, 2),
(2, 3, 1),
(3, 2, 1), (3, 5, 1), (3, 6, 1),
(4, 2, 2);
```

Relación entre tablas:

```
clientes 1───∞ pedidos 1───∞ lineas_pedido ∞───1 productos
```

Observa dos detalles del juego de datos que usaremos constantemente: **Sara no tiene provincia** (NULL) y **Sara y Ana no tienen ningún pedido**. Están puestos a propósito: son los casos límite donde las consultas mal escritas fallan en silencio.

---

<div style="page-break-before: always;"></div>

## §1. La consulta mínima: SELECT ... FROM

Una consulta SELECT no "busca": **describe el resultado que quieres** y el SGBD decide cómo obtenerlo. Esa es la idea clave del SQL como lenguaje declarativo, y la diferencia fundamental con los lenguajes imperativos que conoces (en Java o Python dices *cómo*; en SQL dices *qué*).

```sql
SELECT nombre, precio
FROM productos;
```

Devuelve dos columnas de todas las filas de `productos`. El resultado de un SELECT es siempre, a su vez, una tabla — este detalle parece menor ahora, pero es lo que hará posible las subconsultas de §8.

Reglas de estilo del módulo desde hoy:

1. **Nunca `SELECT *` en código que se entrega.** Pide las columnas que necesitas. `*` rompe cuando la tabla cambia, arrastra datos que no usas y, en la aplicación web que haréis en 2.º, significa enviar por la red columnas que nadie pidió.
2. Palabras clave en mayúsculas, identificadores en minúsculas. No es obligatorio para el SGBD; es obligatorio para que tu código se lea.

### Columnas calculadas y alias

Puedes derivar columnas nuevas con expresiones y renombrarlas con `AS`:

```sql
SELECT nombre,
       precio,
       ROUND(precio * 1.21, 2) AS precio_con_iva
FROM productos;
```

El alias solo existe en el resultado: no modifica la tabla ni se guarda en ningún sitio. Si el alias lleva espacios, necesita comillas invertidas en MySQL (`` AS `precio con IVA` ``), pero evítalo: los alias con espacios dan problemas al consumirlos desde código.

### Eliminar duplicados: DISTINCT

```sql
SELECT DISTINCT provincia
FROM clientes;
```

Devuelve cada provincia una sola vez (incluido el NULL de Sara, que cuenta como un valor más a efectos de DISTINCT). Importante: `DISTINCT` actúa sobre **la combinación completa de columnas seleccionadas**, no sobre la primera. `SELECT DISTINCT provincia, nombre` no eliminará casi nada, porque cada pareja provincia-nombre es distinta.

### Ejercicios del apartado

- **E1.** Muestra el nombre y la categoría de todos los productos.
- **E2.** Muestra el nombre de cada producto junto a una columna calculada `valor_almacen` con el valor total de su stock (precio × unidades), redondeado a 2 decimales.
- **E3.** Obtén la lista de categorías distintas que vende la tienda.
- **E4.** Ejecuta `SELECT DISTINCT categoria, precio FROM productos;` y explica por escrito, en dos líneas, por qué devuelve más filas que E3.

---

<div style="page-break-before: always;"></div>

## §2. Filtrar filas: WHERE

`WHERE` decide **qué filas** entran en el resultado. Se evalúa fila a fila: para cada fila, la condición da verdadero, falso o desconocido, y solo pasan las filas con verdadero.

```sql
-- Productos con stock, de menos de 50 €
SELECT nombre, precio, stock
FROM productos
WHERE stock > 0
  AND precio < 50;
```

Operadores que debes dominar:

```sql
WHERE provincia = 'Zaragoza'              -- igualdad (¡un solo =!)
WHERE precio <> 24.50                     -- distinto (también !=)
WHERE precio BETWEEN 10 AND 50            -- rango, extremos INCLUIDOS
WHERE provincia IN ('Zaragoza','Huesca','Teruel')
WHERE nombre LIKE 'Tecla%'                -- % = cualquier secuencia, _ = un carácter
WHERE provincia IS NULL                   -- NULL nunca se compara con =
```

### Combinar condiciones: AND, OR, NOT y los paréntesis

`AND` tiene más prioridad que `OR`, y esto produce errores lógicos clásicos. Compara:

```sql
-- ¿Qué quiere decir esto?
WHERE categoria = 'Periféricos' OR categoria = 'Accesorios' AND precio < 20
```

Sin paréntesis, se interpreta como `Periféricos OR (Accesorios AND precio < 20)`: devuelve TODOS los periféricos sin importar el precio. Si lo que querías era "productos de esas dos categorías que cuesten menos de 20 €":

```sql
WHERE (categoria = 'Periféricos' OR categoria = 'Accesorios')
  AND precio < 20
-- o mejor:
WHERE categoria IN ('Periféricos', 'Accesorios')
  AND precio < 20
```

Regla del módulo: en cuanto mezclas AND y OR, paréntesis obligatorios, aunque "no hagan falta".

### El caso NULL: lógica de tres valores

`NULL` no es cero ni cadena vacía: es "valor desconocido". Cualquier comparación con `NULL` da `NULL` (ni verdadero ni falso). Consecuencias prácticas:

- `WHERE provincia = NULL` no devuelve *nada nunca*, aunque Sara exista. La forma correcta es `IS NULL`.
- Más traicionero: `WHERE provincia <> 'Zaragoza'` **tampoco devuelve a Sara**, porque `NULL <> 'Zaragoza'` es desconocido, no verdadero. Si quieres "los que no son de Zaragoza, incluidos los de provincia desconocida", necesitas `WHERE provincia <> 'Zaragoza' OR provincia IS NULL`.

Este es el error silencioso más caro de la unidad: no da mensaje de error, simplemente devuelve menos filas de las que esperas. En la tienda real, esto es un cliente que no recibe el email de campaña y nadie sabe por qué.

### Ejercicios del apartado

- **E5.** Clientes dados de alta en 2026 (usa `BETWEEN` con fechas literales en formato `'AAAA-MM-DD'`).
- **E6.** Productos de la categoría Periféricos que cuesten entre 20 y 80 euros.
- **E7.** Clientes cuyo email NO termina en `@mail.com`.
- **E8.** Predice cuántas filas devuelve `SELECT nombre FROM clientes WHERE provincia <> 'Huesca';` con nuestros datos. Ejecútala, comprueba tu predicción y explica el resultado.
- **E9.** Escribe la consulta que devuelve los clientes que no son de Huesca, contando también a los de provincia desconocida.

---

<div style="page-break-before: always;"></div>

## §3. Ordenar y limitar: ORDER BY y LIMIT

Sin `ORDER BY`, el orden de las filas **no está garantizado** — que hoy salgan ordenadas por id es casualidad de implementación, no un contrato. Si el orden importa, se declara.

```sql
-- Los 3 productos más caros
SELECT nombre, precio
FROM productos
ORDER BY precio DESC
LIMIT 3;
```

`ORDER BY` admite varias claves, cada una con su sentido:

```sql
SELECT nombre, provincia, alta
FROM clientes
ORDER BY provincia ASC, alta DESC;
```

Ordena por provincia y, dentro de cada provincia, del cliente más reciente al más antiguo. Los NULL de ordenación van, en MySQL, antes que cualquier valor en ASC (Sara saldrá la primera).

`LIMIT n` corta el resultado a n filas; `LIMIT n OFFSET m` salta las m primeras y devuelve las n siguientes. Este segundo formato es la base de la **paginación** que implementaréis en la web ("mostrando resultados 21–40"):

```sql
-- Página 2 del catálogo, a 2 productos por página, ordenado por nombre
SELECT nombre, precio
FROM productos
ORDER BY nombre
LIMIT 2 OFFSET 2;
```

Aviso importante: `LIMIT` sin `ORDER BY` es una lotería — "dame 3 productos cualesquiera". Casi siempre que escribas LIMIT, debe haber un ORDER BY delante.

### Ejercicios del apartado

- **E10.** Los 2 clientes más antiguos de la tienda (nombre y fecha de alta).
- **E11.** Catálogo completo ordenado por categoría alfabéticamente y, dentro de cada categoría, del más barato al más caro.
- **E12.** La "página 3" de un listado de productos ordenado por precio ascendente, con páginas de 2 productos. ¿Qué productos salen con nuestros datos?
- **E13.** ¿Por qué `SELECT nombre FROM productos LIMIT 1` no es una forma fiable de obtener "el primer producto"? Responde en 2-3 líneas.

---

<div style="page-break-before: always;"></div>

## §4. Funciones sobre filas: cadenas, números y fechas

Además de operadores, SQL trae funciones que transforman el valor de cada fila. No colapsan filas (eso son los agregados de §5): entran n filas, salen n filas transformadas.

### Cadenas

```sql
SELECT UPPER(nombre)                    AS mayusculas,
       LENGTH(email)                    AS longitud_email,
       CONCAT(nombre, ' <', email, '>') AS destinatario,
       SUBSTRING(email, 1, 4)           AS inicio_email
FROM clientes;
```

`CONCAT` merece atención por su comportamiento con NULL: si cualquier argumento es NULL, el resultado entero es NULL. `CONCAT('Provincia: ', provincia)` devuelve NULL para Sara, no "Provincia: ". Para dar un valor por defecto existe `COALESCE`, que devuelve el primer argumento no nulo:

```sql
SELECT nombre,
       COALESCE(provincia, 'Sin provincia') AS provincia_mostrable
FROM clientes;
```

`COALESCE` es de las funciones más usadas en aplicaciones reales: es la traducción de "si no hay dato, muestra esto".

### Números

```sql
SELECT nombre,
       ROUND(precio * 1.21, 2) AS con_iva,     -- redondea
       FLOOR(precio)           AS truncado,    -- hacia abajo
       CEIL(precio)            AS techo        -- hacia arriba
FROM productos;
```

### Fechas

```sql
SELECT nombre,
       alta,
       YEAR(alta)                      AS anio_alta,
       DATEDIFF(CURDATE(), alta)       AS dias_como_cliente,
       DATE_ADD(alta, INTERVAL 1 YEAR) AS primer_aniversario
FROM clientes;
```

Las funciones de fecha varían entre SGBD más que ninguna otra parte del SQL (lo que aquí es `DATEDIFF`, en PostgreSQL es una resta de fechas). Concepto estable, sintaxis a consultar en el manual del SGBD que toque.

Un matiz de rendimiento que os pedirán explicar en 2.º: aplicar funciones a una columna **dentro del WHERE** (`WHERE YEAR(alta) = 2026`) impide al SGBD usar índices sobre esa columna. La versión eficiente es el rango: `WHERE alta BETWEEN '2026-01-01' AND '2026-12-31'`. De momento, quédate con la idea; la desarrollaremos en la UD de optimización.

### Ejercicios del apartado

- **E14.** Genera para cada cliente una columna `usuario` con la parte de su email anterior a la arroba, en mayúsculas (pista: `SUBSTRING_INDEX` o `SUBSTRING` + `LOCATE`).
- **E15.** Listado de clientes con su provincia, mostrando el texto `'(desconocida)'` cuando sea NULL.
- **E16.** Para cada pedido, su fecha y cuántos días han pasado desde entonces hasta hoy.
- **E17.** Reescribe `WHERE YEAR(alta) = 2025` como un rango con BETWEEN y explica en dos líneas por qué la segunda forma es preferible.

---

<div style="page-break-before: always;"></div>

## §5. Funciones de agregado: resumir muchas filas en una

Las funciones de agregado colapsan un conjunto de filas en un único valor:

```sql
SELECT COUNT(*)      AS total_productos,
       AVG(precio)   AS precio_medio,
       MIN(precio)   AS mas_barato,
       MAX(precio)   AS mas_caro,
       SUM(stock)    AS unidades_en_almacen
FROM productos;
```

Tres matices que distinguen un aprobado de un notable:

1. **`COUNT(*)` cuenta filas; `COUNT(columna)` cuenta filas donde esa columna no es NULL.** Con nuestros datos, `COUNT(*)` sobre clientes da 5 y `COUNT(provincia)` da 4. Son preguntas distintas: "¿cuántos clientes hay?" vs. "¿de cuántos clientes conocemos la provincia?".
2. **Los agregados ignoran los NULL** (salvo `COUNT(*)`). `AVG(provincia_...)` de una columna con nulos promedia solo los valores existentes — que puede ser lo que quieres o no, pero tienes que saberlo.
3. **`COUNT(DISTINCT columna)`** cuenta valores distintos: `COUNT(DISTINCT provincia)` da 3 (los NULL no cuentan aquí).

Y una regla sintáctica sin excepciones: **no se puede usar un agregado en el WHERE** (`WHERE COUNT(*) > 3` es error siempre). El WHERE se evalúa fila a fila, antes de que exista ningún recuento. Para filtrar sobre agregados está el HAVING del apartado siguiente.

### Ejercicios del apartado

- **E18.** ¿Cuánto dinero vale todo el stock de la tienda? (una sola cifra)
- **E19.** ¿Cuántos clientes tienen provincia registrada y cuántas provincias distintas hay? (una consulta, dos columnas)
- **E20.** Precio medio de los productos de la categoría Periféricos.
- **E21.** Sin ejecutarla: ¿qué error da `SELECT nombre FROM productos WHERE precio > AVG(precio);` y por qué? ¿Cómo se resuelve lo que intenta hacer? (adelanto de §8 — inténtalo)

---

<div style="page-break-before: always;"></div>

## §6. Agrupar: GROUP BY y HAVING

`GROUP BY` parte la tabla en grupos y aplica los agregados **a cada grupo**:

```sql
-- ¿Cuántos productos y qué precio medio tiene cada categoría?
SELECT categoria,
       COUNT(*)              AS num_productos,
       ROUND(AVG(precio), 2) AS precio_medio
FROM productos
GROUP BY categoria;
```

Visualiza lo que ocurre: el SGBD reparte las 6 filas en 3 montones (Periféricos, Monitores, Accesorios) y ejecuta los agregados dentro de cada montón. El resultado tiene una fila por grupo.

**Regla de oro:** en un SELECT con `GROUP BY`, cada columna que aparezca fuera de una función de agregado debe estar en el `GROUP BY`. Si escribes `SELECT categoria, nombre, COUNT(*) ... GROUP BY categoria`, la pregunta "¿qué nombre?" no tiene respuesta: en el montón "Periféricos" hay tres nombres. MySQL en modo estricto (`only_full_group_by`, el de nuestras máquinas) te dará error; otros modos devuelven un nombre arbitrario, que es peor porque parece funcionar.

### Filtrar grupos: HAVING

```sql
-- Categorías con al menos 2 productos
SELECT categoria, COUNT(*) AS num_productos
FROM productos
GROUP BY categoria
HAVING COUNT(*) >= 2;
```

La distinción que cae seguro en el examen: **`WHERE` filtra filas antes de agrupar; `HAVING` filtra grupos después de agrupar.** Pueden convivir, y el orden conceptual de evaluación es:

```
FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT
```

Ejemplo con los dos filtros, léelo siguiendo ese orden:

```sql
-- De los productos con stock, categorías cuyo precio medio supera 30 €
SELECT categoria, ROUND(AVG(precio), 2) AS precio_medio
FROM productos
WHERE stock > 0                  -- 1º: fuera los productos agotados (filas)
GROUP BY categoria               -- 2º: agrupo lo que queda
HAVING AVG(precio) > 30          -- 3º: fuera los grupos baratos
ORDER BY precio_medio DESC;      -- 4º: ordeno el resultado
```

Criterio de estilo: si un filtro no usa agregados, va en WHERE aunque HAVING "funcione". Es más eficiente (descarta filas antes de agrupar) y más claro.

### Ejercicios del apartado

- **E22.** Número de clientes por provincia, mostrando también el grupo de provincia desconocida.
- **E23.** Para cada pedido, cuántas líneas tiene y cuántas unidades totales suma (tabla `lineas_pedido`).
- **E24.** Categorías cuyo stock total supere las 20 unidades.
- **E25.** De los productos que cuestan menos de 100 €, precio máximo por categoría, pero solo de las categorías con más de un producto en ese rango. (Piensa qué va en WHERE y qué en HAVING antes de escribir.)
- **E26.** ¿Por qué `SELECT categoria, nombre, COUNT(*) FROM productos GROUP BY categoria;` es conceptualmente erróneo aunque algún SGBD lo ejecute? Explícalo en 3 líneas con el ejemplo del "montón".

---

<div style="page-break-before: always;"></div>

## §7. Consultas sobre varias tablas: JOIN

Aquí está el salto de nivel de la unidad. Los datos están repartidos entre tablas — eso lo decidisteis al normalizar en la UD3 — y `JOIN` los vuelve a reunir. **El JOIN es la clave ajena en acción**: si tu diagrama está bien hecho, los JOIN se escriben solos siguiendo las flechas.

### 7.1 INNER JOIN: solo lo que casa

```sql
-- Cada pedido con el nombre de su cliente
SELECT p.id, p.fecha, c.nombre
FROM pedidos p
INNER JOIN clientes c ON p.cliente_id = c.id;
```

Fíjate en los **alias de tabla** (`p`, `c`): obligatorios en este módulo en cuanto hay más de una tabla, porque `id` existe en ambas y sin prefijo la consulta es ambigua.

El mecanismo: para cada fila de `pedidos`, el SGBD busca las filas de `clientes` cuyo `id` coincide con su `cliente_id`, y produce una fila combinada por cada pareja. Las filas sin pareja **desaparecen**: Sara y Ana no salen porque no tienen pedidos. Con nuestros datos: 4 pedidos → 4 filas.

### 7.2 LEFT JOIN: conservar la tabla izquierda

```sql
-- TODOS los clientes, con sus pedidos si los tienen
SELECT c.nombre, p.id AS pedido, p.fecha
FROM clientes c
LEFT JOIN pedidos p ON p.cliente_id = c.id;
```

Ahora salen 6 filas: los 4 pedidos con su cliente, más Sara y Ana con `pedido` y `fecha` a **NULL**. El LEFT JOIN promete: "todas las filas de la tabla izquierda saldrán, casen o no". (RIGHT JOIN es lo mismo conservando la derecha; en la práctica casi todo el mundo escribe LEFT y ordena las tablas para ello.)

De esa promesa sale el patrón más útil del comercio real:

```sql
-- Clientes que NUNCA han comprado (para la campaña de reactivación)
SELECT c.nombre, c.email
FROM clientes c
LEFT JOIN pedidos p ON p.cliente_id = c.id
WHERE p.id IS NULL;
```

Léelo despacio: traigo a todos los clientes, engancho sus pedidos, y me quedo con las filas donde el enganche falló. Este patrón (**LEFT JOIN + IS NULL**) hay que saber explicarlo con tus palabras en la defensa; es de las preguntas favoritas.

Un matiz avanzado que distingue el sobresaliente: en un LEFT JOIN, **no es lo mismo poner una condición en el ON que en el WHERE**. `LEFT JOIN pedidos p ON p.cliente_id = c.id AND p.fecha >= '2026-06-01'` conserva a todos los clientes y solo engancha sus pedidos de junio; la misma condición en el WHERE (`WHERE p.fecha >= '2026-06-01'`) elimina las filas con fecha NULL... y de paso convierte tu LEFT JOIN en un INNER encubierto. Ejecuta ambas contra nuestros datos y compara.

### 7.3 CROSS JOIN y el producto cartesiano

`CROSS JOIN` (o `FROM a, b` sin condición de enlace) combina cada fila de una tabla con **todas** las de la otra: 5 clientes × 6 productos = 30 filas. Casi nunca es lo que quieres; casi siempre es el síntoma de un ON olvidado. Lo estudiamos para reconocerlo, no para usarlo.

### 7.4 Encadenar JOINs: la consulta de negocio completa

```sql
-- Detalle facturable: quién compró qué, cuándo y por cuánto
SELECT c.nombre                 AS cliente,
       p.fecha,
       pr.nombre                AS producto,
       lp.cantidad,
       lp.cantidad * pr.precio  AS importe_linea
FROM clientes c
INNER JOIN pedidos p        ON p.cliente_id  = c.id
INNER JOIN lineas_pedido lp ON lp.pedido_id  = p.id
INNER JOIN productos pr     ON pr.id         = lp.producto_id
ORDER BY p.fecha DESC;
```

Cuatro tablas, y cada `ON` sigue el camino de claves ajenas del esquema. Y los JOIN combinan con todo lo anterior — agregando sobre la consulta combinada obtienes los informes de verdad:

```sql
-- Facturación total por cliente (solo clientes con compras)
SELECT c.nombre,
       SUM(lp.cantidad * pr.precio) AS total_facturado
FROM clientes c
INNER JOIN pedidos p        ON p.cliente_id = c.id
INNER JOIN lineas_pedido lp ON lp.pedido_id = p.id
INNER JOIN productos pr     ON pr.id        = lp.producto_id
GROUP BY c.id, c.nombre
ORDER BY total_facturado DESC;
```

(Agrupamos por `c.id` además de por nombre: dos clientes podrían llamarse igual, y la clave primaria es lo que de verdad identifica al cliente.)

### Ejercicios del apartado

- **E27.** Listado de líneas de pedido "legible": fecha del pedido, nombre del producto y cantidad, ordenado por fecha.
- **E28.** Productos que no se han vendido nunca (nombre y stock). Usa el patrón de §7.2.
- **E29.** Número de pedidos por cliente, **incluyendo con 0 a los que nunca han comprado**. Pista: LEFT JOIN + un COUNT sobre la columna adecuada (piensa por qué `COUNT(*)` daría 1 en vez de 0 para Sara).
- **E30.** Facturación total por categoría de producto.
- **E31.** Ejecuta la consulta 7.4 cambiando el primer INNER por LEFT (y razona si los siguientes también deben cambiar). ¿Qué filas nuevas aparecen y con qué valores? Escribe la respuesta antes de ejecutar y comprueba.

---

<div style="page-break-before: always;"></div>

## §8. Subconsultas

Una subconsulta es un SELECT dentro de otro — recuerda §1: el resultado de un SELECT es una tabla, así que puede usarse donde se espera un valor, una lista o una tabla. Tres formas que debes reconocer:

```sql
-- (a) Subconsulta escalar: devuelve UN valor
SELECT nombre, precio
FROM productos
WHERE precio > (SELECT AVG(precio) FROM productos);
```

Esta es, por fin, la respuesta correcta al E21: como los agregados no pueden ir en el WHERE, se calcula la media en una subconsulta y se compara contra su resultado.

```sql
-- (b) Subconsulta de lista: con IN
SELECT nombre
FROM clientes
WHERE id IN (SELECT DISTINCT cliente_id
             FROM pedidos
             WHERE fecha >= '2026-06-01');
```

```sql
-- (c) Subconsulta correlada: con EXISTS
SELECT c.nombre
FROM clientes c
WHERE EXISTS (SELECT 1
              FROM pedidos p
              WHERE p.cliente_id = c.id);
```

La (c) se llama **correlada** porque la subconsulta menciona una columna de la consulta externa (`c.id`): conceptualmente se evalúa una vez por cada cliente, preguntando "¿existe algún pedido suyo?". `EXISTS` no mira qué devuelve la subconsulta, solo si devuelve algo — por eso se escribe `SELECT 1`.

Una cuarta forma, subconsulta en el FROM, trata el resultado de una consulta como una tabla temporal sobre la que seguir consultando:

```sql
-- ¿Cuál es la facturación media por pedido?
SELECT ROUND(AVG(t.importe), 2) AS ticket_medio
FROM (SELECT lp.pedido_id,
             SUM(lp.cantidad * pr.precio) AS importe
      FROM lineas_pedido lp
      INNER JOIN productos pr ON pr.id = lp.producto_id
      GROUP BY lp.pedido_id) AS t;
```

Dos niveles de agregación (primero sumar por pedido, luego promediar los pedidos) no caben en un solo SELECT: la subconsulta en FROM es la herramienta para encadenarlos. El alias `AS t` es obligatorio.

### ¿Subconsulta o JOIN?

Muchas consultas admiten ambas escrituras. Criterio práctico del módulo: si necesitas **columnas** de la otra tabla en el resultado, JOIN; si solo la usas para **filtrar**, la subconsulta suele leerse mejor. En la defensa se te puede pedir reescribir una en la otra forma, así que practica la traducción en los dos sentidos.

Cuidado con la trampa clásica de `NOT IN`: si la subconsulta puede devolver algún NULL, `NOT IN` no devuelve ninguna fila (otra vez la lógica de tres valores de §2). Para "los que no están", el patrón LEFT JOIN + IS NULL o `NOT EXISTS` son las formas seguras.

### Ejercicios del apartado

- **E32.** Productos más caros que el producto medio de su misma tienda (consulta (a) adaptada — ya la tienes casi hecha).
- **E33.** Con subconsulta e IN: clientes que han comprado algún producto de la categoría Periféricos.
- **E34.** Reescribe E33 con JOIN. ¿Necesitas `DISTINCT`? ¿Por qué?
- **E35.** Con NOT EXISTS: productos que no aparecen en ninguna línea de pedido. Compara mentalmente con tu solución de E28.
- **E36.** ¿Qué pedido tiene el importe más alto? Devuelve su id y su importe (necesitarás una subconsulta en FROM o comparar contra un MAX calculado en subconsulta).

---

<div style="page-break-before: always;"></div>

## Errores frecuentes y cómo depurarlos

**1. `ERROR 1052: Column 'id' in field list is ambiguous`**
Hay más de una tabla con esa columna y no has puesto prefijo. Solución: alias de tabla y prefijar *todas* las columnas en consultas multitabla.

**2. La consulta devuelve muchísimas más filas de las esperadas (producto cartesiano).**
Se te ha olvidado la condición `ON`, o has escrito `FROM a, b` sin WHERE de enlace. Diagnóstico: si el número de filas es `filas_a × filas_b`, es esto (§7.3).

**3. `ERROR 1055: ... nonaggregated column ... incompatible with sql_mode=only_full_group_by`**
Has puesto en el SELECT una columna que ni está agregada ni está en el GROUP BY. Repasa la regla de oro de §6.

**4. `WHERE x = NULL` (o `<> NULL`) devuelve 0 filas sin error.**
Lógica de tres valores: `IS NULL` / `IS NOT NULL`. Este no avisa; solo lo cazas revisando el resultado con espíritu crítico y con casos de prueba que incluyan nulos — por eso Sara está en el juego de datos.

**5. `ERROR 1111: Invalid use of group function`**
Has puesto un agregado en el WHERE (`WHERE COUNT(*) > 3`). Los agregados van en HAVING o en una subconsulta (§5, §8).

**6. `HAVING` usado como `WHERE`** (`HAVING precio > 10` sin agrupar).
Algunos SGBD lo toleran, pero es conceptualmente erróneo y penaliza en la corrección: los filtros de fila van en WHERE.

**7. Comillas dobles vs. simples.**
En SQL estándar las cadenas van con comillas **simples** (`'Zaragoza'`). MySQL es permisivo con las dobles; PostgreSQL (que usaréis en Entorno Servidor) las reserva para identificadores y te dará `column "Zaragoza" does not exist`.

---

<div style="page-break-before: always;"></div>

## La IA en esta unidad

Un asistente de IA escribe consultas SELECT correctas para casos sencillos con total soltura, y lo sabéis. Uso razonable en esta unidad: pedirle consultas alternativas a la tuya para comparar enfoques, o que te explique una consulta que no entiendes. Lo que es responsabilidad tuya, la use quien la use: (1) **verificar el resultado contra los datos** — ejecuta la consulta sobre el juego de datos de §0 y comprueba con casos que conozcas que devuelve lo que debe, especialmente con los NULL de Sara y con las filas sin pareja en los JOIN, que es donde más falla el código generado; (2) **poder explicar cada cláusula**. En la defensa de la práctica se te pedirá justificar por qué un JOIN es LEFT y no INNER, o reescribir una subconsulta como JOIN, con el guion cerrado. "Me lo dio la IA" no es una explicación; "lo verifiqué así" sí lo es.

---

<div style="page-break-before: always;"></div>

## Actividad evaluativa final

**Contexto:** la dirección de TiendaDAW te pide un pequeño cuadro de mando de ventas. Trabajas sobre el esquema y los datos de §0. **Tiempo estimado:** 2 horas. **Entrega:** un único script `.sql` con las consultas numeradas y comentadas, en tu repositorio de la práctica. Cada consulta debe ejecutar sin errores y devolver exactamente lo pedido — se corrige ejecutando.

**Calificación:** 10 ejercicios, **1 punto cada uno** (nota sobre 10). Cada ejercicio indica el CE que evalúa. La dificultad es creciente: los primeros consolidan, los últimos distinguen.

- **AE1.** *(CE: filtrado y ordenación)* "Buscador del catálogo": productos disponibles (con stock) cuyo nombre contenga una palabra dada (usa `'usb'` para la entrega), mostrando nombre, categoría y precio con IVA (21%, redondeado a 2 decimales), ordenados del más barato al más caro.
- **AE2.** *(CE: ordenación y limitación)* Los 3 productos más caros que están disponibles para la venta (nombre y precio).
- **AE3.** *(CE: funciones sobre filas)* Ficha de clientes para atención al cliente: nombre, email, provincia (mostrando `'(desconocida)'` si no consta) y años completos como cliente.
- **AE4.** *(CE: agregación y agrupamiento)* Resumen por pedido: id, número de líneas y unidades totales, pero solo de los pedidos con 2 o más líneas.
- **AE5.** *(CE: composiciones externas + agregación)* Informe de ventas por categoría: para cada categoría, unidades vendidas e importe facturado, ordenado por importe descendente. Las categorías sin ventas deben aparecer con 0 en ambas columnas — decide y justifica en un comentario el tipo de JOIN.
- **AE6.** *(CE: composiciones internas + agregación)* Ticket medio por cliente: para cada cliente con compras, número de pedidos y gasto medio por pedido (dos decimales). Cuidado: el gasto medio por pedido no es el gasto total entre líneas, sino entre pedidos.
- **AE7.** *(CE: composiciones externas + agrupamiento)* "Clientes dormidos": clientes cuyo último pedido es anterior al 1 de junio de 2026 **o** que no han comprado nunca, con nombre, email y fecha del último pedido (NULL si no existe). Una sola consulta.
- **AE8.** *(CE: subconsultas + agregación)* El producto estrella: nombre del producto con más unidades vendidas, resuelto **dos veces**: una con `ORDER BY ... LIMIT` y otra con subconsulta (sin LIMIT). Comenta en el script qué pasaría en cada versión si hubiera empate.
- **AE9.** *(CE: composiciones + agregación con DISTINCT)* Productos comprados por más de un cliente distinto (nombre del producto y número de clientes distintos que lo han comprado).
- **AE10.** *(CE: subconsultas, razonamiento)* Pregunta de razonamiento (respuesta en comentario, 5-8 líneas): un compañero propone `SELECT c.nombre FROM clientes c WHERE c.id NOT IN (SELECT p.cliente_id FROM pedidos p WHERE p.fecha >= '2026-06-01');` para obtener los clientes sin compras desde junio. Explica en qué situación de datos esta consulta dejaría de funcionar correctamente, por qué, y escribe la versión segura.

!!! warning "Pendiente de verificar"
    Al disponer de los CE literales de la Orden, sustituir las etiquetas de CE por su codificación oficial (p. ej. RA3.c).

---

<div style="page-break-before: always;"></div>

## Para ampliar

- Manual de referencia de MySQL 8 — capítulo *SELECT Statement* (dev.mysql.com).
- Documentación de PostgreSQL — *Queries* (postgresql.org/docs), útil porque es el SGBD de Entorno Servidor y es más estricto.
- Estándar de estilo SQL del módulo (repositorio de clase).
