# Cuaderno de prácticas · Bases de Datos

**Módulo 0484 — Bases de Datos · 1.º DAW · IES Río Arba · Curso 2026-27**

Este cuaderno reúne **ejercicios extra de clase**: prácticas más largas que los ejercicios de cada apartado de los apuntes, pensadas para trabajarlas en el aula con el enunciado delante y la herramienta abierta. **No son evaluables**: no se entregan por ningún canal ni cuentan para la nota. Sirven para lo que sirven los apuntes cuando dicen «ejecuta, rompe y arregla»: para que el método te salga solo cuando llegue la actividad evaluativa y el examen.

Cómo funciona:

- Cada práctica lleva el código **CPu.n**: cuaderno de prácticas, unidad *u*, número *n*. CP2.1 es la primera práctica de la UD2.
- Se trabaja **en clase**, de forma individual o en pareja, según se indique ese día. Lo que no te dé tiempo a terminar en el aula lo acabas en casa y lo traes a la sesión de corrección.
- **La solución no está aquí.** Se corrige en clase y, después de corregirla, el docente la publica en el Classroom del módulo, con los ficheros de la herramienta para que los abras y los compares con los tuyos.
- Las prácticas de diagramas se hacen con **draw.io**, como las entregas de la unidad: la [guía rápida de draw.io](../recursos/guia-drawio.md) explica los gestos, la biblioteca de formas y el nombrado de los ficheros. Los diagramas en Chen siguen la convención completa de la UD2 (ver apartado 3.4 de la [UD2](ud2-modelo-entidad-relacion.md)): la pareja (mín, máx) escrita junto a una entidad dice cuántas ocurrencias de esa entidad se asocian con cada ocurrencia de la otra, y sobre cada rombo va la etiqueta de su tipo (1:1, 1:N o N:M).

El cuaderno crece durante el curso: solo aparecen las unidades que tienen prácticas extra, y cada unidad va ganando prácticas a medida que se trabajan en clase.

## UD2 · Modelo entidad-relación

### CP2.1 · Del enunciado al diagrama: la autoescuela comarcal

**Qué refuerza.** Los apartados 1 a 3 de la UD2: entidades y tipos de atributo (apartado 2.2), claves candidata y primaria con sus tres criterios (apartado 2.3), relaciones binarias y reflexivas con roles, atributos de relación (apartado 3.1), cardinalidades máxima y mínima con el método de fijar y preguntar (apartados 3.2 y 3.3) y la convención de parejas y etiqueta de tipo (apartado 3.4). No hace falta nada del apartado 4 en adelante: en este enunciado **no hay entidades débiles, ni jerarquías, ni agregaciones, ni relaciones ternarias**. Si en algún punto te parece que necesitas una, vuelve a leer la frase: tiene solución con lo visto hasta el apartado 3.

**Enunciado.**

1. Una autoescuela de la comarca, con sedes en Tauste y Ejea de los Caballeros, quiere una base de datos para gestionar su actividad.
2. De cada sede se guardan su código, la localidad y un teléfono.
3. De cada alumno se guardan su número de expediente, su DNI, su nombre, su dirección (calle, número, localidad y código postal), uno o varios teléfonos de contacto, su fecha de nacimiento y su edad, que se calcula a partir de ella; cada alumno se matricula en una sola sede, y una sede recién abierta puede no tener todavía alumnos.
4. Cada sede cuenta con su plantilla de profesores —todo profesor está en plantilla—, de los que se guardan su número de empleado, su nombre y su fecha de alta.
5. Cada profesor tiene asignado un vehículo fijo, del que se guardan la matrícula, la marca, el modelo y la fecha de la última ITV; ningún vehículo está asignado a más de un profesor, y la autoescuela mantiene vehículos de reserva sin profesor asignado.
6. Cada profesor tiene designado como mucho un sustituto para sus ausencias, que es otro profesor de la autoescuela; un mismo profesor puede ser el sustituto de varios compañeros o de ninguno, y los recién contratados aún no tienen sustituto.
7. Las clases prácticas se registran con un código, la fecha, la hora de inicio y la duración; cada clase la recibe un único alumno y la imparte un único profesor, y un alumno recién matriculado, o un profesor recién contratado, todavía no tiene clases.
8. Normalmente cada clase se da con el vehículo fijo del profesor, pero si ese vehículo está en el taller se usa uno de reserva, así que de cada clase hay que saber con qué vehículo se dio; un vehículo de reserva puede pasar meses sin usarse en ninguna clase.
9. Los exámenes los convoca la administración de tráfico: de cada convocatoria la autoescuela guarda un código, la fecha, el tipo (teórico o práctico) y el lugar, y solo registra una convocatoria cuando inscribe en ella al menos a un alumno.
10. Un alumno puede presentarse a varias convocatorias, a cada convocatoria se presentan varios alumnos, y hay alumnos que aún no se han presentado a ninguna; de cada presentación se guarda el resultado (apto o no apto).

**Tareas.**

- **(a) Entidades y atributos.** Lista las entidades del enunciado y, para cada una, sus atributos con su tipo (simple o compuesto; monovalorado o multivalorado; derivado o no). Elige la clave primaria de cada entidad. En la entidad que tiene **dos claves candidatas**, justifica la elección con los tres criterios del apartado 2.3 (siempre conocido, estable, mínimo) y di por qué descartas la otra.
- **(b) Relaciones y cardinalidades.** Para cada relación, escribe las dos frases de fijar y preguntar («dado UN …, ¿cuántos …?»), la pareja (mín, máx) que va junto a cada entidad y la etiqueta de tipo (1:1, 1:N o N:M). Junto a cada pareja, cita el número de la frase del enunciado que la justifica («frase 5») y cópiala. Si una relación es reflexiva, escribe sus dos roles; si tiene atributos propios, dilos y explica por qué no son de ninguna de las dos entidades.
- **(c) Diagrama en Chen.** Dibuja el modelo completo en notación de Chen con draw.io, con las formas de la biblioteca Entity Relation: rectángulo para la entidad, elipse para el atributo (doble para el multivalorado, discontinua para el derivado, con las partes colgando para el compuesto, nombre subrayado para la clave primaria) y rombo para la relación. Las parejas (mín, máx) van como etiquetas de las líneas, junto a su entidad; la etiqueta de tipo, sobre el rombo; los roles de la reflexiva, en sus líneas; el atributo de relación, colgando del rombo.
- **(d) El mismo modelo en pata de gallo.** Dibújalo también en pata de gallo con draw.io: una tabla por entidad con sus atributos y la clave primaria marcada como PK, líneas con los terminadores de pata de gallo en los **dos** extremos, y las **anotaciones textuales** de todo lo que la pata de gallo no dibuja, como mandan las notas de entrega de los apartados 2.2 y 3.4 de la UD2: «compuesto: …», «multivalorado», «derivado: no se almacena», los roles de la reflexiva como etiquetas de línea y el atributo de relación anotado en su línea.
- **(e) La pregunta al cliente.** Hay **una cardinalidad que el enunciado no fija**. Localízala, escribe la pregunta exacta que le harías a la autoescuela para resolverla y explica cómo cambia el diagrama según cuál sea la respuesta (qué pareja cambia y qué etiqueta de tipo cambia con ella). Mientras no tengas respuesta, dibuja la opción más sencilla y déjala señalada con una anotación.

**Cómo trabajar.** Lee el enunciado dos veces antes de dibujar: la primera para subrayar los sustantivos (candidatos a entidad o a atributo) y la segunda para subrayar los verbos (candidatos a relación) y las frases que hablan de cantidades («un único», «varios», «como mucho», «puede no tener», «al menos»). Cada pareja (mín, máx) que escribas tiene que apoyarse en una frase del enunciado; si no la encuentras, o la frase no existe —y entonces es la de la tarea (e)— o te la estás inventando. Nombra los ficheros como dice la guía de draw.io, con tu alias: `ud2-cp2-1-chen-<alias>.drawio` y `ud2-cp2-1-pata-de-gallo-<alias>.drawio`. Aunque las prácticas no se entregan, nómbralas así: es el mismo formato de las entregas evaluables y conviene tenerlo automatizado.
