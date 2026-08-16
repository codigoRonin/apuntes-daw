# UD1. Sistemas de almacenamiento, SGBD y el valor del dato

**Módulo 0484 — Bases de Datos · 1.º DAW · IES Río Arba · Curso 2026-27 · 22 horas · 1.ª evaluación**

**Vinculación con el resultado de aprendizaje.** Esta unidad trabaja completo el **RA1**: *"Reconoce los elementos de las bases de datos analizando sus funciones y valorando la utilidad de los sistemas gestores"* (RD 686/2010 en la redacción dada por el RD 405/2023).

| CE | Qué exige (resumen) | Apartado donde se trabaja |
|---|---|---|
| RA1.a | Analizar los sistemas lógicos de almacenamiento y sus características | Apartado 1 |
| RA1.b | Identificar los tipos de bases de datos según el modelo de datos | Apartado 2 |
| RA1.c | Identificar los tipos de bases de datos según la ubicación de la información | Apartado 3 |
| RA1.d | Evaluar la utilidad de un sistema gestor de bases de datos | Apartado 4 |
| RA1.e | Reconocer la función de cada elemento de un SGBD | Apartado 4 |
| RA1.f | Clasificar los sistemas gestores de bases de datos | Apartado 4 |
| RA1.g | Reconocer la utilidad de las bases de datos distribuidas | Apartado 5 |
| RA1.h | Analizar las políticas de fragmentación de la información | Apartado 5 |
| RA1.i | Identificar la legislación vigente sobre protección de datos | Apartado 6 |
| RA1.j | Reconocer los conceptos de Big Data y de la inteligencia de negocios | Apartado 7 |

**Al terminar esta unidad sabrás:** explicar por qué guardar datos en ficheros sueltos acaba mal y qué problemas exactos resuelve una base de datos; distinguir los modelos de datos y elegir razonadamente entre ellos; decidir si una organización necesita datos centralizados o distribuidos, y cómo se reparten; describir qué hace por ti un sistema gestor y quién es quién dentro de él; aplicar las obligaciones básicas de la protección de datos a una base de datos concreta; y explicar qué son Big Data y la inteligencia de negocios sin caer en la palabrería.

**Entorno de trabajo de la unidad.** Esta unidad es conceptual: no necesitas instalar nada todavía. Trabajaremos con un caso hilo — la **biblioteca municipal** que ya conoces de la prueba inicial — y con pequeños ficheros de texto que se publicarán en Classroom para los ejercicios del apartado 1 (`prestamos.txt` y `prestamos.csv`, con las mismas anotaciones de préstamos del caso).

<div style="page-break-before: always;"></div>

## Apartado 1. Sistemas lógicos de almacenamiento: la vida antes de las bases de datos

Toda aplicación necesita que sus datos sobrevivan a un apagado. La forma más elemental de conseguirlo es un **fichero**: una secuencia de bytes con nombre, guardada en disco. Antes de valorar las bases de datos hay que entender qué dan de sí los ficheros — y dónde se rompen.

### Apartado 1.1. Ficheros planos

Un **fichero plano** guarda los datos como texto, registro tras registro, normalmente un registro por línea y los campos separados por un delimitador. El formato CSV (*comma-separated values*) es el ejemplo universal:

```text
carne;nombre;telefono;titulo;estante;fecha_prestamo
101;Marta García;600111222;El Quijote;3;2026-10-02
101;Marta García;600111222;Poesía completa;5;2026-10-02
102;Íker Sanz;600333444;El Quijote;3;2026-10-03
103;Rosa Pérez;;Historia de Aragón;2;2026-10-04
```

Para encontrar los préstamos de Rosa hay que **leer el fichero entero desde el principio** (acceso *secuencial*): con cuatro líneas es instantáneo; con cuarenta millones, no. Y observa los problemas que ya arrastra: el nombre y el teléfono de Marta se repiten en cada préstamo (**redundancia**), y si su teléfono cambia hay que corregirlo en todas partes o los datos se contradicen (**inconsistencia**) — exactamente lo que detectaste en la prueba inicial.

### Apartado 1.2. Ficheros indexados y de acceso directo

Dos mejoras clásicas intentan arreglar la lentitud del acceso secuencial:

- **Ficheros indexados**: junto al fichero de datos se mantiene un **índice** — una estructura aparte, ordenada, que asocia el valor de un campo (por ejemplo, `carne`) con la posición del registro en el fichero. Para buscar el 103 ya no se recorre todo: se consulta el índice (búsqueda rápida por estar ordenado) y se salta directamente a la posición. El precio: cada alta, baja o modificación debe **mantener el índice al día**.
- **Ficheros de acceso directo (hashing)**: la posición del registro se **calcula** a partir del valor de la clave mediante una función (de dispersión o *hash*). Acceso casi instantáneo por clave exacta; a cambio, mal soporte de recorridos ordenados y necesidad de gestionar colisiones (dos claves que caen en la misma posición).

!!! tip "Idea que reaparecerá todo el curso"
    Guárdate esta: **un índice acelera la lectura a costa de encarecer la escritura y ocupar espacio**. En la UD4 la retomarás con los índices de MySQL y los planes de ejecución, con millones de filas de por medio.

### Apartado 1.3. Los límites del enfoque por ficheros

Cuando cada aplicación gestiona sus propios ficheros aparecen, sistemáticamente, estos problemas — memorízalos como lista de síntomas, porque son la justificación histórica de las bases de datos:

1. **Redundancia e inconsistencia**: los mismos datos, repetidos y divergiendo.
2. **Dependencia entre datos y programas**: si cambia el formato del fichero, hay que retocar todos los programas que lo leen.
3. **Dificultad de acceso**: cada nueva pregunta ("¿qué libros se prestaron en octubre?") exige programar una rutina nueva.
4. **Problemas de integridad**: las reglas del negocio ("el carné debe existir", "la fecha de devolución no puede ser anterior a la de préstamo") viven repartidas por el código de cada programa, si es que viven en algún sitio.
5. **Problemas de concurrencia**: dos bibliotecarios registrando préstamos a la vez sobre el mismo fichero pueden pisarse los cambios.
6. **Problemas de seguridad**: o se ve el fichero entero o no se ve nada; dar acceso "solo a los títulos, no a los teléfonos" es artesanía.
7. **Recuperación ante fallos**: un corte de luz a mitad de escritura puede dejar el fichero corrupto, sin mecanismo estándar de vuelta atrás.

**Ejercicios del apartado.**

- **E1.** El fichero `prestamos.csv` de la unidad tiene los campos `carne;nombre;telefono;titulo;estante;fecha_prestamo`. Señala qué datos concretos están redundantes y describe una secuencia de dos operaciones de actualización que lo dejaría inconsistente.
- **E2.** La biblioteca quiere buscar préstamos por `carne` de forma habitual y, una vez al mes, listar todos los préstamos ordenados por fecha. Razona qué organización de fichero (secuencial, indexado, acceso directo) encaja mejor con cada necesidad y qué precio paga.
- **E3.** De los siete síntomas del apartado 1.3, identifica los **tres** que ya aparecieron (aunque en pequeño) en el ejercicio de la prueba inicial sobre el cuaderno de la biblioteca, citando el detalle concreto del caso que los muestra.

<div style="page-break-before: always;"></div>

## Apartado 2. Bases de datos: concepto y tipos según el modelo de datos

Una **base de datos** es un conjunto de datos **relacionados entre sí**, almacenados de forma **estructurada** y con la **mínima redundancia posible**, pensado para ser compartido por distintas aplicaciones y usuarios. La clave está en la palabra *estructurada*: los datos se organizan conforme a un **modelo de datos** — un conjunto de conceptos y reglas para describir la información y las operaciones posibles sobre ella.

Según el modelo de datos, distinguimos las familias principales:

| Modelo | Idea central | Situación actual |
|---|---|---|
| **Jerárquico** | Los datos forman un árbol: cada registro tiene un único padre (biblioteca → estantería → libro) | Histórico (años 60-70); sobrevive en sistemas heredados |
| **En red** | Como el jerárquico pero un registro puede tener varios padres (un libro pertenece a una estantería y a una colección) | Histórico; superado por el relacional |
| **Relacional** | Los datos se organizan en **tablas** (filas y columnas) relacionadas por valores; se consulta con SQL | El estándar dominante desde los años 80; el corazón de este módulo (UD2 a UD7) |
| **Orientado a objetos** | Los datos se guardan como objetos, con sus atributos y métodos, al estilo de la programación orientada a objetos | Nicho; su herencia práctica son las extensiones objeto-relacionales |
| **No relacionales (NoSQL)** | Familia diversa: documentos, clave-valor, columnares, grafos; sacrifican rigidez de esquema a cambio de flexibilidad y escalado | En expansión desde los 2000 para casos concretos; los estudiarás en la UD8 |

Del modelo relacional te llevas hoy solo la intuición (la desarrollarás durante siete unidades): la biblioteca serían tablas `lectores`, `libros` y `prestamos`, donde cada préstamo referencia el carné del lector y el libro — sin repetir el nombre de Marta en ningún sitio más que en su fila de `lectores`.

De las no relacionales, la intuición contraria: un préstamo podría guardarse como un **documento** autocontenido (`{lector: "Marta", titulo: "El Quijote", fecha: ...}`), flexible y rápido de leer entero, a cambio de reintroducir controladamente parte de la redundancia que el relacional elimina. Cuándo compensa cada cosa es, precisamente, una pregunta de profesional — y la respuesta nunca es "porque está de moda".

**Ejercicios del apartado.**

- **E4.** Clasifica según el modelo de datos: (a) un sistema que guarda cada pedido de una tienda como un documento JSON completo; (b) las tablas `alumnos`, `modulos` y `matriculas` de un instituto; (c) el árbol de carpetas y ficheros de tu ordenador; (d) una red social que almacena "quién sigue a quién" para recorrer conexiones.
- **E5.** La biblioteca duda entre modelo relacional y documental para su sistema de préstamos. Da un argumento sólido a favor de cada opción usando datos concretos del caso (lectores, libros, préstamos, devoluciones).
- **E6.** Explica por qué el modelo jerárquico representa mal esta situación real: "un mismo libro pertenece a la colección 'Clásicos' y a la colección 'Lecturas de Bachillerato'". ¿Qué dos modelos la representan sin forzarla?

<div style="page-break-before: always;"></div>

## Apartado 3. Tipos según la ubicación: centralizadas y distribuidas

El segundo criterio de clasificación no mira cómo se estructuran los datos, sino **dónde están**.

- **Base de datos centralizada**: todos los datos residen en un único sistema (un servidor, o un clúster que se comporta como uno), y todos los usuarios — locales o remotos — acceden a ese punto único. Es el caso normal: la biblioteca municipal tiene un servidor y las dos bibliotecarias trabajan contra él.
- **Base de datos distribuida**: los datos se reparten entre **varios nodos** conectados en red, en ubicaciones posiblemente distintas, pero se presentan al usuario como **una sola base de datos lógica**. Piensa en una red de bibliotecas comarcal: cada villa tiene su servidor con sus préstamos, y aun así puede consultarse "¿en qué biblioteca de la comarca hay un ejemplar libre de La colmena?" como si todo fuera una única base.

La diferencia decisiva no es técnica sino de **compromiso**: la centralizada es más simple de administrar y de mantener coherente; la distribuida acerca los datos a quien los usa (rendimiento), tolera mejor los fallos de un nodo (disponibilidad) y respeta realidades organizativas (cada sede gobierna lo suyo) — a cambio de una complejidad mucho mayor, que estudiamos en el apartado 5.

Un matiz de vocabulario que evita confusiones clásicas: **distribuir no es copiar**. Una copia de seguridad en otro edificio no convierte la base de datos en distribuida; y un servidor centralizado al que se accede desde muchos sitios por red sigue siendo centralizado. Lo que define la distribución es que **los datos mismos**, en su operación normal, viven repartidos entre nodos que cooperan.

**Ejercicios del apartado.**

- **E7.** Clasifica como centralizada o distribuida, justificando en una línea: (a) la base de datos de Secretaría del instituto, consultada desde 40 equipos del centro; (b) la red comarcal de bibliotecas con un nodo por villa; (c) un servidor único en la nube usado por clientes de tres países; (d) los datos de una cadena de tiendas donde cada tienda opera su nodo y la central consolida.
- **E8.** La biblioteca municipal funciona con base centralizada y le proponen "distribuirla" entre sus dos plantas del mismo edificio. Argumenta si la propuesta tiene sentido, distinguiendo qué beneficio real de la distribución aplicaría (o no) en este caso.
- **E9.** Da un ejemplo, distinto de los del texto, donde la distribución responda a una razón **organizativa** (quién gobierna qué datos) y no de rendimiento.

<div style="page-break-before: always;"></div>

## Apartado 4. El sistema gestor de bases de datos (SGBD)

### Apartado 4.1. Qué es y por qué compensa

Una base de datos no se toca directamente: entre los datos y quien los usa hay un **sistema gestor de bases de datos** (SGBD): el software que crea, gestiona y protege las bases de datos y que atiende todas las peticiones de acceso. MySQL — el gestor de este curso —, PostgreSQL, Oracle, SQL Server, SQLite o MongoDB son SGBD.

Su **utilidad** se entiende repasando la lista de síntomas del apartado 1.3, porque un SGBD ataca los siete de serie:

| Síntoma con ficheros | Qué aporta el SGBD |
|---|---|
| Redundancia e inconsistencia | Estructuras que permiten guardar cada dato una sola vez |
| Dependencia datos-programas | **Independencia de datos**: los programas piden datos por su significado, no por su posición en un fichero |
| Dificultad de acceso | Un **lenguaje de consulta** (SQL): la pregunta nueva es una consulta nueva, no un programa nuevo |
| Integridad | **Reglas declaradas** en la propia base (claves, restricciones) que el gestor hace cumplir a todo el mundo |
| Concurrencia | **Control de concurrencia**: cientos de usuarios simultáneos sin pisarse (lo verás a fondo en la UD6) |
| Seguridad | **Usuarios y privilegios**: quién ve y quién toca, dato a dato |
| Fallos | **Transacciones y recuperación**: operaciones todo-o-nada y vuelta a un estado coherente tras un fallo |

Un anticipo mínimo del lenguaje, en sintaxis MySQL, solo para que le pongas cara (la UD4 es entera para esto):

```sql
SELECT titulo, estante
FROM libros
WHERE genero = 'Novela';
```

Esa es la gracia de la independencia de datos: la petición dice **qué** se quiere, y el gestor decide **cómo** encontrarlo.

### Apartado 4.2. Funciones y componentes: quién hace qué

Las funciones clásicas de un SGBD se agrupan en tres, y conviene saber nombrarlas:

1. **Definición** de los datos: describir la estructura de la base (tablas, tipos, reglas) — el gestor mantiene esa descripción en el **diccionario de datos** (o catálogo), que es la base de datos *sobre* la base de datos.
2. **Manipulación**: insertar, consultar, modificar y borrar datos.
3. **Control**: seguridad (usuarios y privilegios), integridad, concurrencia y recuperación.

Por dentro, los **elementos** que cooperan para servir cada petición (RA1.e pide reconocer la función de cada uno):

| Elemento | Función |
|---|---|
| **Procesador de consultas** | Recibe la sentencia (p. ej. SQL), la analiza, la **optimiza** (elige el mejor camino: usar o no un índice, en qué orden combinar tablas) y la ejecuta |
| **Gestor de almacenamiento** | Traduce entre las estructuras lógicas (tablas) y los ficheros físicos en disco; gestiona la memoria intermedia (*buffer*) para minimizar accesos a disco |
| **Diccionario de datos** | Guarda la descripción de todas las estructuras: qué tablas existen, sus columnas, sus reglas, sus índices, sus usuarios |
| **Gestor de transacciones** | Garantiza que las operaciones agrupadas se ejecutan enteras o no se ejecutan, incluso ante fallos |
| **Control de concurrencia** | Coordina los accesos simultáneos para que el resultado sea correcto |
| **Gestor de recuperación** | Mantiene registros (*logs*) que permiten reconstruir un estado coherente tras una caída |

Y alrededor del gestor, los **actores humanos**: el **administrador (DBA)**, que instala, configura, respalda y da permisos; los **desarrolladores**, que construyen las aplicaciones que usan la base; y los **usuarios finales**, que ni saben que hay un SGBD debajo — la bibliotecaria usa "el programa de préstamos".

### Apartado 4.3. Clasificación de los SGBD

Los gestores se clasifican por varios ejes independientes (un mismo gestor se sitúa en todos a la vez):

- **Por el modelo de datos** que implementan: relacionales (MySQL, PostgreSQL, Oracle), documentales (MongoDB), clave-valor (Redis), de grafos (Neo4j)…
- **Por la licencia**: libres/de código abierto (MySQL Community, PostgreSQL, SQLite) frente a propietarios (Oracle Database, SQL Server).
- **Por el despliegue**: locales (en tus servidores) frente a servicios gestionados en la nube.
- **Por el ámbito**: **embebidos** dentro de la aplicación, sin servidor (SQLite en un móvil), frente a **cliente-servidor**, donde el gestor es un servicio al que se conectan muchas aplicaciones (MySQL).

!!! warning "Precisión que cae en examen"
    **La base de datos no es el SGBD.** La base de datos son los datos organizados (la de la biblioteca); el SGBD es el software que la gestiona (MySQL). Un mismo MySQL puede gestionar veinte bases de datos distintas. Decir "instalé una base de datos" cuando instalaste MySQL es como decir "instalé una novela" cuando instalaste un lector de libros.

**Ejercicios del apartado.**

- **E10.** La biblioteca sigue con sus CSV y te pide un informe: "¿qué tres problemas concretos nuestros desaparecerían con un SGBD?". Elige los tres síntomas más graves **de su caso** y explica, para cada uno, qué mecanismo del SGBD lo resuelve.
- **E11.** Traza el recorrido de la consulta del apartado 4.1 por los elementos internos del gestor: indica, en orden, qué hace cada elemento implicado desde que llega la sentencia hasta que vuelven las filas.
- **E12.** Sitúa MySQL, SQLite y MongoDB en los cuatro ejes de clasificación del apartado 4.3. Señala el eje en el que MySQL y SQLite difieren de forma más importante y qué consecuencia práctica tiene.
- **E13.** ¿Quién debe hacer cada tarea — DBA, desarrollador o usuario final?: (a) dar de alta a la nueva bibliotecaria con permiso de solo lectura sobre los teléfonos; (b) registrar la devolución de un libro; (c) cambiar la aplicación para que muestre también la fecha de reserva; (d) programar la copia de seguridad nocturna.

<div style="page-break-before: always;"></div>

## Apartado 5. Bases de datos distribuidas y fragmentación

### Apartado 5.1. Para qué distribuir

Retomamos la red comarcal de bibliotecas del apartado 3 y ponemos nombre a las utilidades de la distribución (RA1.g):

1. **Rendimiento por cercanía**: cada villa consulta sus propios préstamos en su nodo local, sin cruzar la red comarcal para el 95 % de su trabajo diario.
2. **Disponibilidad**: si el nodo de una villa cae, las demás siguen operando; el sistema degrada, no muere.
3. **Escalado**: crecer añadiendo nodos, en lugar de comprar un servidor cada vez más grande.
4. **Autonomía organizativa**: cada biblioteca gobierna sus datos y responde de ellos, sin renunciar a la vista de conjunto.

El ideal que persigue todo sistema distribuido es la **transparencia**: que el usuario formule "¿dónde hay un ejemplar libre de La colmena?" sin saber — ni tener que saber — en cuántos nodos vive la respuesta.

### Apartado 5.2. Políticas de fragmentación

Distribuir exige decidir **qué parte de los datos va a cada nodo**. Esa decisión es la **fragmentación**, y sus políticas son materia de análisis (RA1.h):

- **Fragmentación horizontal**: se reparten **filas**. La tabla comarcal `prestamos` se trocea por villa: el nodo de cada biblioteca guarda las filas de sus préstamos. Todas las villas tienen la misma estructura de tabla; difieren las filas. Es la política natural cuando el criterio de reparto coincide con el de uso ("cada villa trabaja con lo suyo").
- **Fragmentación vertical**: se reparten **columnas**. De la tabla `lectores`, los datos de contacto (teléfono, dirección) podrían residir solo en el nodo de la villa del lector, mientras un núcleo mínimo (carné, nombre) se comparte para las consultas comarcales. Exige que cada fragmento conserve la clave (el carné) para poder recomponer la fila completa.
- **Fragmentación mixta (híbrida)**: combinación de ambas — primero se trocea horizontalmente por villa y, dentro de cada villa, verticalmente por sensibilidad de los datos, por ejemplo.

Dos propiedades definen una fragmentación bien hecha: **completitud** (ningún dato se queda sin fragmento: la unión de los trozos reconstruye el total) y **disyunción** en la horizontal (una fila vive en un solo fragmento, salvo réplica deliberada).

La **replicación** es la pieza complementaria: mantener copias del mismo fragmento en varios nodos. Mejora la disponibilidad y las lecturas; complica las escrituras, porque las copias deben mantenerse de acuerdo. Fragmentar y replicar son decisiones independientes que se combinan: la red comarcal puede fragmentar los préstamos por villa **y además** replicar el catálogo completo de libros en todos los nodos, porque se lee muchísimo y cambia poco.

!!! tip "Regla mental para elegir política"
    Pregunta siempre: **¿cómo se van a consultar estos datos?** Si cada nodo pregunta casi siempre por "sus" filas → horizontal. Si distintos usos necesitan distintas columnas (y algunas son sensibles) → vertical. Si se lee mucho y en todas partes y cambia poco → réplica. La fragmentación se diseña desde las consultas, no desde la estética del reparto.

**Ejercicios del apartado.**

- **E14.** La red comarcal decide: préstamos fragmentados horizontalmente por villa; catálogo de libros replicado en todos los nodos; datos de contacto de lectores solo en el nodo de su villa. Justifica cada una de las tres decisiones con la utilidad o regla del apartado que la respalda.
- **E15.** Propón una fragmentación **vertical** razonable de la tabla `lectores(carne, nombre, telefono, direccion, email, fecha_nacimiento)` en dos fragmentos, indica qué columna debe estar en ambos y por qué, y comprueba la completitud.
- **E16.** Un diseño reparte `prestamos` así: nodo A, préstamos de 2025; nodo B, préstamos de 2026; nodo C, "préstamos de lectores de Tauste". Detecta el defecto de la política (pista: propiedades de una buena fragmentación) y corrígelo.
- **E17.** Señala una situación del caso comarcal en la que la replicación del catálogo **empeoraría** las cosas, y explica el mecanismo del empeoramiento.

<div style="page-break-before: always;"></div>

## Apartado 6. Protección de datos: la ley dentro de la base de datos

### Apartado 6.1. El marco legal vigente

En cuanto una base de datos guarda información de personas, deja de ser solo un problema técnico. La legislación vigente que debes identificar (RA1.i):

- El **Reglamento General de Protección de Datos** (RGPD, Reglamento (UE) 2016/679): norma europea, de aplicación directa en toda la Unión desde 2018.
- La **Ley Orgánica 3/2018, de Protección de Datos Personales y garantía de los derechos digitales** (LOPDGDD): la ley española que adapta y complementa el RGPD.
- La autoridad de control en España es la **Agencia Española de Protección de Datos (AEPD)**, que supervisa, atiende reclamaciones y sanciona.

**Dato personal** es toda información sobre una persona física identificada **o identificable** — y ese "identificable" es más amplio de lo que parece: el carné 101 es dato personal aunque no lleve nombre, porque la biblioteca puede saber quién es. Hay además **categorías especiales** (salud, ideología, religión, orientación sexual, datos biométricos…) con protección reforzada: prohibido tratarlas salvo excepciones tasadas.

### Apartado 6.2. Principios, roles y derechos que afectan al diseño

Los **principios** del RGPD que condicionan directamente cómo diseñas una base de datos:

1. **Licitud y finalidad**: los datos se recogen para fines determinados y legítimos, con una base jurídica (consentimiento, contrato, obligación legal…), y no se usan después para otra cosa incompatible.
2. **Minimización**: solo los datos **necesarios** para el fin. Si la biblioteca no necesita la fecha de nacimiento para prestar libros, esa columna sobra — y una columna que sobra es un riesgo que sobra.
3. **Exactitud**: datos correctos y actualizados (¿te suena? la inconsistencia del apartado 1 es, además de un defecto técnico, un incumplimiento).
4. **Limitación del plazo**: no se conservan indefinidamente; hay que prever el borrado o la anonimización.
5. **Integridad y confidencialidad**: medidas técnicas y organizativas adecuadas — control de acceso, cifrado cuando proceda, copias de seguridad. Los usuarios y privilegios del SGBD (apartado 4) son la herramienta técnica de este principio.

Los **roles**: el **responsable del tratamiento** decide fines y medios (el ayuntamiento, para la biblioteca); el **encargado del tratamiento** trata datos por cuenta del responsable (la empresa informática que aloja el sistema). Como futuro desarrollador trabajarás casi siempre en la posición de encargado o para un responsable: sus obligaciones te llegan por contrato.

Los **derechos** de las personas — los conocidos como ARSOPL: **acceso, rectificación, supresión ("olvido"), oposición, portabilidad y limitación** — tienen consecuencia técnica directa: tu diseño debe permitir localizar todos los datos de una persona, corregirlos, exportarlos y **borrarlos de verdad**. Un diseño donde "borrar a un lector" deja rastros repartidos e ilocalizables no es solo malo: es ilegal.

!!! info "Lo tienes delante desde la semana 1"
    El sistema de **alias** con el que trabajas en este módulo es minimización aplicada: las herramientas externas no necesitan tu nombre real para que aprendas, así que no lo tienen. Cuando diseñes tu base de datos del curso te pediremos el mismo reflejo: cada columna de datos personales, justificada o fuera.

**Ejercicios del apartado.**

- **E18.** Audita esta tabla de la biblioteca según el principio de minimización, sabiendo que su única finalidad es gestionar préstamos: `lectores(carne, nombre, telefono, direccion, dni, fecha_nacimiento, profesion, estado_salud_observaciones)`. Indica columna a columna: se queda / sobra / prohibida salvo excepción, con el motivo.
- **E19.** Un lector ejerce su derecho de supresión. Enumera qué debería poder hacer la biblioteca sobre sus datos y señala qué problema práctico plantea el historial de préstamos (pista: ¿hay obligaciones o fines que justifiquen conservar algo, y en qué forma?).
- **E20.** Identifica responsable y encargado del tratamiento en: "El ayuntamiento contrata a la empresa DataSoft el alojamiento y mantenimiento del sistema de préstamos". ¿Cuál de los dos decide cuántos años se conservan los históricos?
- **E21.** Relaciona cada principio del apartado 6.2 con un mecanismo concreto del SGBD o del diseño visto en esta unidad que ayuda a cumplirlo (una pareja por principio; hay varias respuestas válidas).

<div style="page-break-before: always;"></div>

## Apartado 7. Big Data e inteligencia de negocios: el valor del dato

### Apartado 7.1. Big Data

**Big Data** designa los escenarios donde los datos desbordan, por sus características, las herramientas tradicionales de gestión. Se caracterizan con las **V**:

- **Volumen**: cantidades que no caben o no rinden en un único servidor clásico.
- **Velocidad**: los datos llegan en flujo continuo y hay que ingerirlos (y a veces reaccionar) en tiempo casi real.
- **Variedad**: conviven datos estructurados (tablas), semiestructurados (JSON, registros de actividad) y no estructurados (texto, imagen).
- Se añaden a menudo **veracidad** (calidad y fiabilidad desigual — recuerda tu ejercicio de datos "raros") y **valor** (el dato solo importa si sostiene decisiones).

Ojo al matiz que separa al profesional del vendedor de humo: **Big Data no significa "muchos datos"** a secas. Cuarenta millones de filas bien indexadas en MySQL son un martes normal, no Big Data; el término aplica cuando volumen, velocidad o variedad **obligan a cambiar de herramientas** — procesamiento distribuido, bases no relacionales (UD8), flujos.

### Apartado 7.2. Inteligencia de negocios

La **inteligencia de negocios** (*Business Intelligence*, BI) es el conjunto de procesos y herramientas que transforman los datos operativos en **información para decidir**: indicadores, informes, cuadros de mando.

La distinción estructural que debes dominar es **operacional frente a analítico**:

| | Sistema operacional (OLTP) | Sistema analítico (BI) |
|---|---|---|
| Pregunta típica | "Registra este préstamo" | "¿Qué géneros crecen este año y en qué villas?" |
| Operaciones | Muchas, pequeñas, constantes (leer/escribir filas sueltas) | Pocas, grandes (resumir millones de filas) |
| Datos | El presente, al detalle | Histórico, a menudo agregado |
| Usuario | La bibliotecaria en mostrador | Dirección, coordinación |

Por eso las organizaciones separan ambos mundos: los datos operativos se **extraen, transforman y cargan** (proceso **ETL**) periódicamente en un **almacén de datos** (*data warehouse*) organizado para el análisis, del que beben los cuadros de mando. El ciclo completo — dato → integración → indicador → decisión — es lo que convierte el dato en activo: la biblioteca que sabe qué se lee y cuándo compra mejor, programa mejor y justifica su presupuesto.

Este CE (RA1.j) se queda aquí en el plano conceptual, y lo reconectaremos sin nota en la UD8 cuando veas las herramientas no relacionales que sostienen parte de estos escenarios.

**Ejercicios del apartado.**

- **E22.** Clasifica como escenario Big Data o no (justificando con las V): (a) los 40.000 préstamos anuales de la comarca; (b) las publicaciones, imágenes y reacciones por segundo de una red social global; (c) las lecturas cada 10 segundos de 5.000 sensores industriales durante 5 años.
- **E23.** Formula dos preguntas **operacionales** y dos **analíticas** sobre la biblioteca comarcal, y señala para cada una quién la haría y sobre qué sistema (OLTP o almacén de datos) debería responderse.
- **E24.** La dirección quiere el cuadro de mando "en directo contra la base de préstamos, que para eso está". Explica, con la tabla del apartado 7.2, los dos problemas de esa idea y qué propone en su lugar la arquitectura BI.

<div style="page-break-before: always;"></div>

## Apartado 8. Errores frecuentes

1. **Confundir base de datos con SGBD.** "He instalado una base de datos" → has instalado un gestor. En los ejercicios de clasificación, deja siempre claro de cuál de los dos hablas.
2. **Creer que distribuida = tener copias.** Una copia de seguridad remota o un acceso remoto no distribuyen nada. Distribución es reparto de los datos operativos entre nodos que cooperan.
3. **Confundir fragmentación con replicación.** Fragmentar es trocear (cada dato en su fragmento); replicar es copiar (el mismo dato en varios nodos). Se combinan, pero son decisiones distintas con costes opuestos en escritura.
4. **Fragmentaciones que se solapan o dejan huecos.** El error de E16: criterios de reparto que no son disjuntos ni completos. Comprueba siempre las dos propiedades.
5. **Pensar que el RGPD "no aplica" si no hay nombres.** Identificable basta: carnés, alias reidentificables, matrículas. Y "es para un trabajo de clase" no es una base jurídica.
6. **Usar "Big Data" para cualquier tabla grande.** Si MySQL con buenos índices lo maneja, no es Big Data; reserva el término para cuando las V obligan a cambiar de herramientas — te tomarán más en serio.

<div style="page-break-before: always;"></div>

## Apartado 9. La IA en esta unidad

Puedes usar un asistente de IA en esta unidad, con cabeza y responsabilidad sobre el resultado — la misma regla que te aplicará cualquier empresa.

- **Usos razonables aquí**: pedir explicaciones alternativas de un concepto que no termina de encajar (fragmentación, transparencia, OLTP frente a analítico), generar ejemplos adicionales de clasificación para practicar, o comprobar tu comprensión pidiéndole que te haga preguntas.
- **Lo que debes verificar siempre**: las afirmaciones legales del apartado 6 (normas y derechos: contrástalas con las fuentes oficiales del apartado 11 — los asistentes mezclan con facilidad marcos legales de países distintos) y cualquier clasificación que te dé hecha, porque los matices (¿es distribuida o solo remota?) son exactamente donde más fallan.
- **Lo que se te pedirá defender**: los ejercicios y la actividad evaluativa se defienden oralmente. "Me lo dijo la IA" no es una justificación: si está en tu entrega, es tuyo, y debes poder explicarlo con el caso concreto y responder a un "¿y si…?". Entregar lo que no entiendes es la forma más rápida de que una buena nota escrita se convierta en una mala nota defendida.

<div style="page-break-before: always;"></div>

## Apartado 10. Actividad evaluativa final

**Contexto — la empresa del curso.** La empresa comarcal de **mantenimiento de parques renovables** gestiona 15 parques (eólicos y fotovoltaicos), unos 450 activos (aerogeneradores, inversores), 5.000 sensores que envían lecturas periódicas (5 millones acumuladas), 60 técnicos en cuadrillas, 30.000 órdenes de trabajo preventivas y correctivas con sus partes de intervención, y un almacén de repuestos. Hoy todo se gestiona con hojas de cálculo por parque y un fichero CSV mensual que la oficina central consolida a mano.

**Instrucciones.** 10 ejercicios, 1 punto cada uno; se responde razonando sobre el contexto anterior (respuestas sin justificar no puntúan completas). **Tiempo estimado: 2 horas.** Entrega: documento en la tarea de Classroom de la unidad. La actividad se defiende oralmente por muestreo.

- **AE1** `[RA1.a]` La oficina central consolida a mano un CSV mensual por parque. Identifica **tres** de los síntomas del almacenamiento por ficheros presentes en esa práctica, citando para cada uno el detalle del contexto que lo evidencia.
- **AE2** `[RA1.b]` La empresa se plantea el modelo de datos para dos conjuntos distintos: (1) técnicos, órdenes y repuestos; (2) el flujo masivo de lecturas de sensores. Propón modelo para cada uno y justifica por qué no tiene por qué ser el mismo.
- **AE3** `[RA1.c]` Clasifica la situación actual (hojas por parque + consolidación mensual central) según la ubicación de la información, y clasifica también la situación objetivo si se implanta un sistema único en la sede central al que acceden los 15 parques por red. Justifica ambas.
- **AE4** `[RA1.d]` Redacta, para la gerencia (no técnica), el argumento de utilidad de implantar un SGBD: elige los **tres** problemas actuales más caros para el negocio y explica qué mecanismo del gestor resuelve cada uno.
- **AE5** `[RA1.e]` El jefe de mantenimiento lanza la consulta "órdenes correctivas abiertas con más de 7 días en parques de la zona norte". Describe el recorrido de esa petición por los elementos del SGBD, indicando la función que cumple cada elemento implicado.
- **AE6** `[RA1.f]` La empresa compara MySQL, SQLite y un servicio de base de datos gestionado en la nube para su sistema central. Sitúa las tres opciones en los ejes de clasificación y descarta razonadamente una de ellas para este caso.
- **AE7** `[RA1.g]` La conectividad de algunos parques de montaña es inestable. Argumenta qué utilidades de una base de datos distribuida (nodo por parque + consolidación) aplican a este caso concreto, y señala una que **no** aplique apenas y por qué.
- **AE8** `[RA1.h]` Diseña la política de fragmentación del sistema distribuido: decide para `lecturas`, `ordenes_trabajo`, el catálogo de `repuestos` y los datos de `tecnicos` entre fragmentación horizontal, vertical, mixta o replicación, justifica cada decisión desde las consultas previsibles y comprueba completitud y disyunción donde proceda.
- **AE9** `[RA1.i]` Audita la protección de datos del diseño: identifica qué tablas contienen datos personales, aplica el principio de minimización a `tecnicos(dni, nombre, telefono, cualificaciones, salario, estado_salud_reconocimiento)` columna a columna, y asigna los roles de responsable y encargado sabiendo que la empresa contrata el alojamiento a un proveedor externo.
- **AE10** `[RA1.j]` Las 5 millones de lecturas crecen sin parar y dirección quiere un cuadro de mando de disponibilidad por parque. Razona: (1) si el escenario de las lecturas es Big Data y con qué V; (2) por qué el cuadro de mando no debe atacar directamente el sistema operacional, y qué arquitectura propone la inteligencia de negocios.

<div style="page-break-before: always;"></div>

## Apartado 11. Para ampliar

- Manual de referencia de MySQL (capítulos introductorios sobre qué es MySQL y su arquitectura): https://dev.mysql.com/doc/
- Texto consolidado del RGPD — Reglamento (UE) 2016/679: https://eur-lex.europa.eu/eli/reg/2016/679
- Agencia Española de Protección de Datos — guías para responsables y ciudadanía: https://www.aepd.es/
- Documentación de MongoDB (introducción a bases de datos documentales, como anticipo de la UD8): https://www.mongodb.com/docs/
