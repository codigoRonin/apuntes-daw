# UD1. Arquitecturas y tecnologías de programación en servidor

**Módulo 0613 — Desarrollo Web en Entorno Servidor · 2.º DAW · IES Río Arba · Curso 2026-27 · 14 horas · 1.ª evaluación**

**Vinculación con el resultado de aprendizaje.** Esta unidad trabaja completo el **RA1**: *"Selecciona las arquitecturas y tecnologías de programación web en entorno servidor, analizando sus capacidades y características propias"* (RD 686/2010 en la redacción dada por el RD 405/2023). El criterio g) — herramientas y frameworks — es incorporación de 2023 y **evaluable**.

| CE | Qué exige (resumen) | Apartado donde se trabaja |
|---|---|---|
| RA1.a | Caracterizar y diferenciar los modelos de ejecución de código en el servidor y en el cliente web | Apartados 2 y 3 |
| RA1.b | Reconocer las ventajas de la generación dinámica de páginas | Apartado 2 |
| RA1.c | Identificar los mecanismos de ejecución de código en los servidores web | Apartados 1 y 4 |
| RA1.d | Reconocer las funcionalidades de los servidores de aplicaciones y su integración con los servidores web | Apartado 4 |
| RA1.e | Identificar y caracterizar los principales lenguajes y tecnologías de programación web en entorno servidor | Apartado 5 |
| RA1.f | Verificar los mecanismos de integración de los lenguajes de marcas con los lenguajes de programación en entorno servidor | Apartado 6 |
| RA1.g | Reconocer y evaluar las herramientas y frameworks de programación en entorno servidor | Apartado 7 |

**Al terminar esta unidad sabrás:** explicar qué pasa exactamente desde que escribes una dirección hasta que la página aparece; distinguir una página estática de una generada y decidir cuál conviene en cada caso; repartir con criterio el trabajo entre el cliente y el servidor; describir cómo ejecuta código un servidor y qué añade un servidor de aplicaciones; situarte en el panorama de lenguajes y tecnologías de servidor sin perderte; verificar con tus propias herramientas que el navegador siempre recibe lenguaje de marcas, lo genere quien lo genere; y evaluar un framework con criterios profesionales en vez de por moda.

**Entorno de trabajo de la unidad.** En esta unidad todavía no programas: **ejecutas y observas**. Necesitas el navegador con las herramientas de desarrollo abiertas (pestaña **Red**) y, para los experimentos del apartado 6, **PHP** instalado — en el aula ya lo está; en casa descárgalo de la web oficial (apartado 11) y comprueba la instalación escribiendo `php -v` en un terminal: su **servidor embebido de desarrollo** (`php -S`) es todo lo que estos experimentos necesitan. PHP aparece en esta unidad como **herramienta instrumental** para ejecutar y observar; su evaluación como lenguaje del módulo empieza en la UD2 — lo que aquí se evalúa es el RA1. Node.js, la otra pieza de la pila, se presentará en su unidad (los servicios REST). Los dos experimentos del apartado 6 están completos en estos apuntes y también en el repositorio de la unidad del aula de código. El caso hilo de la teoría es la **web de un club de montaña comarcal**: calendario de salidas, inscripciones y área de socios.

<div style="page-break-before: always;"></div>

## Apartado 1. El viaje de una petición: cliente, servidor y HTTP

Empecemos por deshacer una ambigüedad que arrastra todo el mundo: **"servidor" nombra dos cosas**. Es la **máquina** que está encendida esperando peticiones, y es el **programa** que corre en esa máquina y las atiende. En este módulo casi siempre hablaremos del programa — y de hecho tú vas a ejecutar servidores en tu propio portátil, que no por eso se convierte en un centro de datos.

El otro protagonista es el **cliente**: normalmente el navegador, aunque también puede serlo una app móvil o otro programa. Cliente y servidor hablan mediante **HTTP**, un protocolo de pregunta y respuesta tan simple en su esqueleto que cabe en un diagrama:

```
Navegador (cliente)                                Servidor
    │
    │ ── petición: GET /calendario ─────────▶  recibe la petición,
    │    (método + ruta + cabeceras)           decide qué responder,
    │                                          prepara la respuesta
    │ ◀── respuesta: 200 OK + HTML ─────────
    │    (código + cabeceras + cuerpo)
    ▼
  interpreta el HTML y pinta la página
```

De la **petición** te interesan ya tres piezas: el **método** (de momento, `GET` para pedir y `POST` para enviar datos; el repertorio completo llegará con los servicios REST en la UD7), la **ruta** (qué recurso se pide: `/calendario`, `/logo.png`) y las **cabeceras** (información auxiliar: qué formatos acepta el cliente, qué navegador es…).

De la **respuesta**, otras tres: el **código de estado**, que es la respuesta en una cifra — léelos como frases: `200` "aquí tienes", `301` "esto se mudó, ve a esta otra dirección", `403` "sé lo que pides pero tú no puedes verlo", `404` "eso no existe", `500` "he fallado yo, el servidor" —; las **cabeceras**, donde destaca `Content-Type`, que declara qué tipo de contenido viaja en el cuerpo (`text/html`, `image/png`…); y el **cuerpo**, el contenido en sí.

Todo esto no hay que creérselo: se mira. Abre las herramientas de desarrollo del navegador (F12), pestaña **Red**, recarga cualquier página y selecciona el primer elemento de la lista (el documento). Verás la petición y la respuesta reales, con su método, su código de estado y sus cabeceras. Esa pestaña va a ser tu mirilla durante todo el curso: cuando algo falle en tu aplicación, la primera pregunta profesional es siempre "¿qué se pidió y qué se respondió de verdad?".

**Ejercicios del apartado.**

- **E1.** Abre el sitio de apuntes del módulo con la pestaña Red activa y recarga. Anota, para el documento principal y para dos recursos más (una imagen o un CSS): método, código de estado y `Content-Type` de la respuesta. Indica cuál de los tres es el único que el navegador "lee" como página.
- **E2.** Empareja cada situación con el código de estado que devolvería el servidor (200, 301, 403, 404, 500) y justifica en una línea: (1) un socio pide `/calendario` y todo va bien; (2) alguien teclea `/calendaroi`; (3) un visitante sin sesión pide `/socios/listado-telefonos`; (4) la web cambió `/salidas` por `/calendario` hace un año y alguien usa un marcador antiguo; (5) el programa del servidor lanza una excepción al preparar el calendario.
- **E3.** Este es el resumen que muestra la pestaña Red de una petición al sitio del club: `Request Method: GET · Status Code: 200 · Content-Type: text/html; charset=utf-8`. Responde: ¿qué hizo el cliente, qué contestó el servidor y qué tipo de contenido viajó en el cuerpo? ¿Puede saberse desde aquí si esa página estaba escrita en un fichero o se generó al momento?

<div style="page-break-before: always;"></div>

## Apartado 2. Páginas estáticas y generación dinámica

Una **página estática** es un fichero que existe tal cual en el servidor: cuando llega la petición, el servidor lo localiza y lo envía sin tocarlo. El servidor trabaja de repartidor. Es el modelo con el que arrancó la web y sigue siendo imbatible en lo suyo: rapidez, simplicidad, facilidad de cachear y poquísima superficie de error — un fichero no tiene bugs de ejecución.

El problema aparece cuando el contenido **depende de algo**. El calendario de salidas del club cambia cada semana; el área de socios muestra a cada persona sus propias inscripciones; el formulario de inscripción tiene que comprobarse y guardarse. Con páginas estáticas, la única salida es que alguien edite el HTML a mano cada vez — y en el club, de hecho, lo hacía un voluntario: cada lunes abría el fichero, cambiaba las fechas y rezaba por no romper una etiqueta.

La **generación dinámica** invierte el modelo: la página **no existe hasta que alguien la pide**. Cuando llega la petición, un programa en el servidor consulta los datos del momento, fabrica el HTML y lo envía. Sus ventajas son exactamente los dolores del voluntario del lunes:

1. **Una única fuente de datos**: las salidas viven en un solo sitio (un fichero de datos, y pronto una base de datos) y todas las páginas que las muestran se fabrican desde ahí — se acabó el divergir.
2. **Contenido siempre actual**: la página refleja el estado en el instante de la petición, no en el de la última edición.
3. **Personalización**: la misma ruta puede responder distinto según quién pregunta — tu área de socio y la mía no son la misma página.
4. **Procesamiento de entrada**: los formularios dejan de ser decorativos; el servidor recibe, valida y actúa.
5. **Una plantilla, mil páginas**: la ficha de cada salida no se escribe cien veces; se escribe una plantilla y se rellena cien veces.

Nada es gratis: la generación dinámica añade piezas (un programa que puede fallar, datos que mantener), algo de latencia por petición y más superficie de ataque. Por eso la decisión profesional no es "todo dinámico", sino quirúrgica: **estático lo que no cambia; dinámico lo que depende de datos o de quién mira**. La página "quiénes somos" del club puede ser un fichero hasta el fin de los tiempos; el calendario, no.

**Ejercicios del apartado.**

- **E4.** Para cada elemento de la web del club, di si apostarías por estático o dinámico y nombra la **pista observable** que lo delata (¿cambia al recargar?, ¿depende de quién eres?, ¿procesa lo que envías?): (1) el logotipo; (2) el calendario de salidas; (3) la página de historia del club; (4) el buscador de rutas por dificultad; (5) tu lista personal de inscripciones; (6) las normas de seguridad en PDF.
- **E5.** Describe dos problemas concretos que el voluntario del lunes habrá sufrido seguro editando el calendario a mano, y para cada uno señala cuál de las cinco ventajas de la generación dinámica lo elimina.
- **E6.** El club estrena "galería de fotos por salida", donde los socios suben fotos tras cada ruta. Razona si la galería es estática, dinámica o una mezcla, separando qué parte es cada cosa.

<div style="page-break-before: always;"></div>

## Apartado 3. El reparto del trabajo: código en el cliente y código en el servidor

Ya sabes, de primero y de DWEC, que el navegador ejecuta código: JavaScript que reacciona a clics, valida campos, anima interfaces. Ese es el **modelo de ejecución en el cliente**: el código **viaja** con la página y se ejecuta en la máquina del usuario. Consecuencia inmediata, y capital: ese código es **público y manipulable**. Cualquiera puede leerlo, modificarlo o directamente saltárselo. Es perfecto para mejorar la experiencia; es inútil para imponer reglas.

El **modelo de ejecución en el servidor** es el espejo: el código **nunca viaja** — se ejecuta en el servidor y lo único que llega al cliente es su **resultado**, normalmente HTML. De ahí se derivan sus superpoderes: puede guardar secretos (credenciales de la base de datos, claves de servicios), puede tocar datos que el cliente jamás debe ver enteros, y sus decisiones **tienen autoridad**, porque nadie puede editarlas desde fuera. Por eso "Ver código fuente de la página" nunca te enseñará el programa del servidor: solo verás la respuesta que fabricó.

El reparto profesional del trabajo queda así:

| Tarea | ¿Dónde? | Por qué |
|---|---|---|
| Reaccionar al instante a la interfaz (desplegar un menú, avisar de un campo vacío al teclear) | Cliente | Necesita inmediatez y no toca datos de nadie |
| Guardar y consultar los datos reales | Servidor | Los datos y sus credenciales no pueden viajar |
| Decidir si un usuario puede hacer algo | Servidor | La decisión debe ser inmanipulable |
| Validar un formulario | **Ambos** | En el cliente por cortesía (aviso inmediato); en el servidor por autoridad (la única que cuenta) |

Grábate la fila de la validación: es la **regla de oro** que reaparecerá en la UD2 (formularios) y en la UD3 (autenticación). El ejemplo canónico del club: una salida tiene 20 plazas. Tu pantalla puede decir "quedan 3", pero esa cifra es una foto del pasado — quizá alguien se inscribió hace un segundo. Quien decide si de verdad queda plaza es el servidor, en el momento de procesar tu inscripción, mirando el dato real. El cliente propone; el servidor dispone.

**Ejercicios del apartado.**

- **E7.** Decide dónde debe ejecutarse cada tarea — cliente, servidor o ambos — y justifica las que marques como "ambos": (1) mostrar u ocultar la contraseña al pulsar el ojito; (2) comprobar que el correo de registro no está ya usado; (3) avisar de que la contraseña es demasiado corta mientras se teclea; (4) aplicar el descuento de socio al precio de la salida; (5) ordenar la tabla de rutas al pulsar una columna; (6) registrar la inscripción y descontar la plaza; (7) impedir que un menor se inscriba en una ruta de alta montaña; (8) recordar qué pestaña tenías abierta.
- **E8.** Un compañero valida la fecha de nacimiento solo con JavaScript en el navegador y asegura que "así nadie menor se inscribe". Explícale, paso a paso, cómo se lo saltaría alguien con las herramientas de desarrollo, y qué haría falta para que la regla fuera de verdad.
- **E9.** Con el ejemplo de las 20 plazas: ¿por qué el número que ves puede estar desactualizado sin que nadie haya programado mal nada? ¿Qué modelo de ejecución produce ese número y qué modelo tiene la última palabra? Redáctalo usando los dos términos del apartado.

<div style="page-break-before: always;"></div>

## Apartado 4. Mecanismos de ejecución en el servidor: servidores web y servidores de aplicaciones

¿Y cómo pasa exactamente un servidor de "repartir ficheros" a "ejecutar programas"? La historia de la web es la historia de esa pregunta, y sus respuestas siguen todas en producción hoy:

1. **CGI, el mecanismo pionero**: el servidor web, al recibir ciertas rutas, lanzaba un **programa externo nuevo por cada petición**, le pasaba los datos y devolvía lo que el programa imprimiera. Funcionaba — y arrancar un proceso entero por visita resultaba carísimo con tráfico real. Entenderlo explica todo lo que vino después.
2. **Intérprete embebido en el servidor web**: el lenguaje se incrusta como módulo dentro del propio servidor web, que interpreta el código sin arrancar procesos nuevos. Es el patrón clásico con el que PHP conquistó la web sobre Apache.
3. **Servidor de aplicaciones**: un proceso **persistente y especializado** en ejecutar tu aplicación, separado del servidor web, con el que se entiende. Además de ejecutar código, aporta servicios que ningún programa querría reinventar: gestión de procesos e hilos para atender muchas peticiones a la vez, reserva de conexiones a la base de datos, gestión de sesiones, mecanismos de despliegue y monitorización. Es la pieza natural del mundo Java y de la gran empresa.
4. **El runtime moderno**: en plataformas como Node.js, **tu programa es el servidor** — con unas líneas creas un proceso que escucha peticiones directamente (lo vas a hacer en el apartado 6). En producción, ese proceso no suele estar solo: delante se coloca un servidor web haciendo de **proxy inverso**, y un gestor de procesos cubre lo que aportaría un servidor de aplicaciones (reinicios, varios procesos, monitorización).

Lo que no cambia entre mecanismos es la **integración con el servidor web** y el reparto de papeles en un despliegue real:

```
              ┌───────────────────── máquina servidora ─────────────────────┐
 Navegador ─▶ │  Servidor web (p. ej. Nginx)                                │
              │    ├── /logo.png, /estilos.css ──▶ carpeta de estáticos     │
              │    └── /calendario, /socios/…  ──▶ proxy ──▶ App (Node)     │
              └─────────────────────────────────────────────────────────────┘
```

El servidor web despacha lo estático (lo hace mejor y más barato que nadie) y **delega** en la aplicación lo que hay que fabricar. Cada petición acaba en la pieza especializada en atenderla.

**Ejercicios del apartado.**

- **E10.** Empareja cada mecanismo (CGI · intérprete embebido · servidor de aplicaciones · runtime tipo Node) con su descripción de una línea y añade una **consecuencia práctica** de cada uno (coste, capacidad, complejidad…).
- **E11.** Con el despliegue del diagrama, indica qué pieza atiende finalmente cada petición y por qué: `GET /logo.png` · `GET /calendario` · `GET /socios/perfil` · `GET /estilos.css` · `POST /inscripcion`.
- **E12.** El club decide publicar su aplicación "a pelo": el proceso Node solo, sin servidor web delante ni gestor de procesos. Señala dos servicios de la lista del mecanismo 3 que echarán de menos y describe el problema concreto que sufrirán por cada uno.

<div style="page-break-before: always;"></div>

## Apartado 5. El panorama: lenguajes y tecnologías de servidor

Al servidor le da igual el lenguaje: cualquier lenguaje capaz de leer una petición y producir texto puede programar el lado servidor. Por eso el panorama es amplio — y por eso hace falta un mapa con **ejes** en vez de una lista de nombres: para cada tecnología pregúntate qué lenguaje usa, sobre qué plataforma corre, qué madurez y comunidad tiene, en qué tipo de proyectos brilla y qué demanda laboral genera.

Con esos ejes, las fichas esenciales:

| Tecnología | En una frase | Dónde brilla |
|---|---|---|
| **PHP** | El lenguaje que creció con la web; se embebe en HTML con naturalidad y corre en cualquier alojamiento barato | La web de contenidos: la mayor parte de los gestores de contenidos del mundo, con WordPress a la cabeza |
| **Java / Jakarta EE** | Plataforma veterana y robusta, tipado fuerte, servidores de aplicaciones maduros | Gran empresa, banca, sistemas que deben durar décadas |
| **Python** | Sintaxis clara y ecosistema gigante; en web, con frameworks como Django o Flask | Proyectos que conviven con datos, ciencia e IA; desarrollo rápido |
| **C# / ASP.NET Core** | La plataforma de Microsoft, moderna y multiplataforma | Ecosistemas corporativos Microsoft y videojuegos con servicios |
| **Ruby (on Rails)** | El framework que enseñó productividad y convenciones a todos los demás | Producto web ágil; su influencia se nota en todo el panorama |
| **JavaScript / Node.js** | El lenguaje del navegador, ejecutándose fuera de él: **un solo idioma en toda la pila** | Aplicaciones web y APIs, tiempo real, equipos full-stack |

Dos avisos de lectura. Primero: **no hay tecnología ganadora universal; hay contextos** — la pregunta profesional nunca es "¿cuál es la mejor?" sino "¿cuál encaja con este proyecto, este equipo y este mantenimiento?". Segundo: este panorama **caduca** — las posiciones relativas se mueven cada año, así que las afirmaciones sobre popularidad o versiones se comprueban en fuentes oficiales con fecha, no se recitan de memoria (ni tuya ni de nadie).

Este curso el lenguaje principal será **PHP** — con **JavaScript** de vuelta, en su papel, cuando lleguen los servicios —, y en el apartado 7 se argumenta la elección con los criterios de un profesional — no con fe.

**Ejercicios del apartado.**

- **E13.** Completa una ficha con las columnas *lenguaje · plataforma/entorno · dónde brilla · un ejemplo de proyecto típico* para tres tecnologías de la tabla (una de ellas, obligatoriamente, una que no sea PHP), usando solo la información del apartado.
- **E14.** Propón tecnología candidata y justifícala con los ejes para tres encargos: (1) la web corporativa con blog de una asesoría, que mantendrá una empresa local de hosting clásico; (2) la API que dará servicio a la app móvil de un gimnasio; (3) un panel interno que cruza datos de ventas con un modelo de IA ya existente en Python.
- **E15.** Verdadero o falso, con justificación de una o dos líneas; reescribe las falsas en una versión defendible: (1) "PHP está muerto"; (2) "Node es siempre más rápido que las demás opciones"; (3) "Para cada proyecto existe una tecnología objetivamente mejor"; (4) "Con Node, cliente y servidor pueden compartir lenguaje"; (5) "La popularidad de una tecnología es un dato fijo que basta aprenderse una vez".

<div style="page-break-before: always;"></div>

## Apartado 6. La integración con el lenguaje de marcas

Todo lo anterior converge en un hecho que conviene formular en voz alta: **el navegador solo entiende lenguaje de marcas** (con su CSS y su JavaScript). Haga lo que haga el servidor — consultar datos, aplicar reglas, calcular — su trabajo solo sirve si termina convertido en HTML que viaja en la respuesta. Los **mecanismos de integración** son justamente las formas en que el código de servidor produce ese HTML. Dos familias:

**Mecanismo 1 — el código construye el HTML.** El programa fabrica el texto de la respuesta: concatena, interpola, monta. Es el estilo natural de los servicios y de plataformas como Node, donde **tu programa es además el servidor** — lo verás en acción en la unidad de servicios REST; hoy te basta reconocerlo como la otra familia.

**Mecanismo 2 — el código se embebe en el HTML.** Se invierte el papel: el documento manda, y entre las marcas se insertan bloques de código que el servidor ejecuta al servir la página. Así nacieron JSP, ASP y — sobre todo — **PHP, el lenguaje principal de este curso, que trae este mecanismo de serie**: todo lo que va entre `<?php` y `?>` (o su atajo de impresión, `<?= … ?>`) se ejecuta en el servidor, y el navegador recibe únicamente el HTML resultante. Es el mecanismo de los dos experimentos de abajo — y el que usarás a diario desde la UD2.

Y ahora, a **verificar** — que es lo que pide el criterio, y lo que distingue saber de creer. Los dos experimentos (están también en el repositorio de la unidad) se sirven con el servidor embebido de PHP: abre un terminal en la carpeta de los archivos, lanza `php -S localhost:8000` y déjalo escuchando (se para con Ctrl+C); en esa misma terminal verás el registro de cada petición atendida.

```php
<?php // experimento-1-fijo.php — la respuesta es siempre la misma
$titulo = 'Club de montaña';
$proxima = 'Ibón de Estanés';
?>
<h1><?= $titulo ?></h1>
<p>Próxima salida: <?= $proxima ?>.</p>
```

```php
<?php // experimento-2-generado.php — la respuesta se fabrica en el momento
$salidas = ['Ibón de Estanés', 'Peña Oroel', 'Cañón de Añisclo'];
$ahora = date('d/m/Y H:i:s');
$proxima = $salidas[array_rand($salidas)];
?>
<h1>Club de montaña</h1>
<p>Página generada el <?= $ahora ?>.</p>
<p>Próxima salida sorteada: <?= $proxima ?>.</p>
```

Abre `http://localhost:8000/experimento-1-fijo.php` con la pestaña Red activa y mira la respuesta: `Content-Type: text/html`, cuerpo HTML — y en **Ver código fuente**, ni rastro de `<?php`: las etiquetas y las variables se quedaron en el servidor. Abre después el segundo y recarga varias veces: **cambia el contenido — la hora, la salida sorteada — pero no la naturaleza de lo que viaja**: sigue siendo HTML, indistinguible para el navegador de un fichero escrito a mano. Esa es la verificación que buscábamos: la integración funciona cuando el cliente **no puede saber, ni necesita saber**, si el HTML que recibió fue escrito o fabricado.

**Ejercicios del apartado.**

- **E16.** Ejecuta el experimento 1 y ábrelo desde el navegador. Captura de la pestaña Red el `Content-Type` y el cuerpo de la respuesta, compara el código fuente recibido con el archivo `experimento-1-fijo.php`, y explica en dos líneas qué viajó por la red y qué se quedó en el servidor sin viajar jamás.
- **E17.** Ejecuta el experimento 2 y recarga tres veces. Anota qué cambia entre recargas y qué permanece, y señala la línea exacta del código responsable de cada uno de los dos cambios.
- **E18.** Sobre el papel (sin ejecutar todavía): escribe la modificación del experimento 2 para que muestre las **tres** salidas del array como lista HTML (`<ul>` con sus `<li>`, con un bucle embebido entre las marcas), y explica en una frase por qué eso que acabas de escribir es, literalmente, "integración del lenguaje de marcas con el lenguaje de programación".

<div style="page-break-before: always;"></div>

## Apartado 7. Herramientas y frameworks: reconocer y evaluar

Los experimentos del apartado 6 funcionan, pero imagina escalar ese estilo a una aplicación entera: analizar cada ruta a mano, montar cada respuesta letra a letra, reinventar la gestión de formularios, de sesiones, de errores… Todo eso es idéntico en todas las aplicaciones web del mundo, y el software tiene una respuesta estándar para el trabajo idéntico: empaquetarlo.

Dos formas de empaquetarlo que no conviene confundir. Una **librería** es una caja de herramientas: tu código manda y la llama cuando quiere. Un **framework** es un chasis: trae la estructura de la aplicación ya decidida y **es él quien llama a tu código** en los puntos previstos — tú rellenas los huecos. Esa inversión de quién llama a quién es la frontera entre ambos.

Un framework web de servidor te da, de serie: **enrutado** (qué función atiende cada ruta y método), integración con **motores de plantillas**, gestión cómoda de **peticiones y respuestas**, un mecanismo de **middleware** para insertar pasos comunes (registro, autenticación…), protecciones de **seguridad** básicas y — nada menor — una **estructura común** que permite que un equipo entero, o el tú de dentro de seis meses, encuentre las cosas donde espera encontrarlas.

Cada ecosistema del apartado 5 tiene los suyos: **Express** y NestJS en Node; Laravel y Symfony en PHP; Spring Boot en Java; Django y Flask en Python; ASP.NET Core en C#; Rails en Ruby.

¿Y cómo se elige, si no es por moda? Con una **rúbrica de evaluación** — la misma que usarás en los ejercicios y en la actividad final:

| Criterio | Qué mirar | Dónde está la evidencia |
|---|---|---|
| Documentación | ¿Oficial, completa, con guía de inicio que funciona? | La web oficial del proyecto |
| Comunidad y adopción | ¿Se usa de verdad? ¿Hay respuestas cuando algo falla? | Actividad del repositorio, ecosistema de paquetes |
| Mantenimiento vivo | **Fecha de la última versión** y ritmo de publicación | La sección de versiones de la web o del repositorio oficial |
| Licencia | ¿Libre? ¿Permite uso comercial? | El archivo de licencia del proyecto |
| Curva y encaje | ¿Qué cuesta aprenderlo? ¿Encaja con lo que el equipo ya sabe? | La guía de inicio, probada |
| Ecosistema | ¿Existen piezas hechas para lo que necesitaré (sesiones, ORM…)? | El gestor de paquetes de la plataforma |

Apliquémosla, como ejemplo trabajado, a **Laravel** — el framework del cierre de nuestra pila: documentación oficial extensa, con guía de instalación y tutorial de primera aplicación; adopción dominante en el ecosistema PHP, del que es el framework de referencia; proyecto vivo, con calendario de versiones publicado en su web oficial; licencia MIT, sin restricciones para lo que haremos; curva exigente si se toma entero — por eso en este curso llega **al final y acotado a cuatro piezas** (rutas, plantillas Blade, Eloquent y validación), cuando ya domines a mano los mecanismos que él automatiza — con encaje directo en el PHP que para entonces traerás de serie; y un ecosistema **Composer/Packagist** con piezas para todo lo que una aplicación necesita, de la autenticación al ORM. (Express, el framework de la parte JavaScript de la pila, tendrá su propia argumentación en la unidad de servicios REST.) Fíjate en que dos casillas de la rúbrica (fechas de versiones, estado del repositorio) son **evidencia con caducidad**: no la des por sabida — se consulta en la fuente oficial cada vez, y eso harás en los ejercicios.

Completan el taller del curso las herramientas alrededor del framework: **Composer** (el gestor de dependencias de PHP, que llega con el tramo Laravel), **npm** (su equivalente en el ecosistema Node, que usarás en la unidad de servicios), las herramientas de **prueba de APIs** (las usarás desde la UD7) y las **DevTools** que ya manejas.

**Ejercicios del apartado.**

- **E19.** Clasifica cada pieza como *lenguaje · entorno de ejecución · framework · motor de plantillas · gestor de paquetes · herramienta*: PHP · el intérprete de PHP (con su servidor embebido `php -S`) · Laravel · Blade · Composer · la pestaña Red. Justifica las dos que te resulten más dudosas.
- **E20.** Elige un framework de **otra** pila (de la lista del apartado) y evalúalo con los seis criterios de la rúbrica, aportando al menos dos evidencias reales y fechadas tomadas de sus fuentes oficiales (por ejemplo, fecha de la última versión y licencia), citando de dónde las sacaste.
- **E21.** "¿Framework sí o no?": el club necesita (a) una página única para anunciar la marcha anual, que no cambiará en meses, y (b) su aplicación de inscripciones con socios, plazas y pagos. Argumenta para cada encargo si compensa usar un framework, nombrando qué aporta y qué cuesta en cada caso.

<div style="page-break-before: always;"></div>

## Apartado 8. Errores frecuentes

1. **"El servidor" entendido solo como una máquina.** Es también — y sobre todo, en este módulo — un programa. En tu portátil correrán servidores todo el curso, empezando por los experimentos del apartado 6.
2. **"JavaScript es cosa del navegador."** Lo era. Node.js ejecuta JavaScript fuera del navegador, con capacidades que allí no existen (ficheros, red, procesos). Mismo lenguaje, entorno distinto, poderes distintos.
3. **"Si la página se mueve, es dinámica."** Una animación o un menú desplegable son JavaScript **en el cliente** sobre una página que puede ser perfectamente estática. El criterio correcto es dónde se **fabrica el HTML**, no cuánto se mueve.
4. **"Con Ver código fuente veo el programa del servidor."** Nunca: solo ves la respuesta que fabricó. El código de servidor no viaja — exactamente por eso puede custodiar secretos y tomar decisiones con autoridad.
5. **`Failed to listen on localhost:8000 (reason: Address already in use)`** al lanzar `php -S`: el mensaje real que verás cuando otro proceso — casi siempre tu ejecución anterior, que no cerraste — sigue escuchando en el puerto 8000. Vuelve a esa terminal y páralo con Ctrl+C antes de lanzar de nuevo.
6. **"Me funciona en `http://localhost:8000`, así que ya está publicado."** `localhost` es tu propia máquina: nadie más llega ahí. Publicar de verdad exige un servidor accesible desde fuera — y el despliegue con todas sus piezas lo verás más adelante en el ciclo.

<div style="page-break-before: always;"></div>

## Apartado 9. La IA en esta unidad

Puedes usar un asistente de IA en esta unidad, con cabeza y con responsabilidad sobre el resultado — la misma regla que te aplicará cualquier empresa, y la que fija este módulo: **el uso se declara en el `DECISIONES.md` de cada entrega**.

- **Usos razonables aquí**: pedir explicaciones alternativas de un concepto que se resista (proxy inverso, inversión de control, la diferencia entre los mecanismos del apartado 4), generar más casos de clasificación estático/dinámico o cliente/servidor para practicar, o pedirle que te haga preguntas de repaso antes de la defensa.
- **Lo que debes verificar siempre**: todo lo del panorama que lleve fecha implícita — versiones, popularidad, "cuál es mejor" — porque caduca y porque mezcla opinión con dato; contrástalo con las fuentes oficiales del apartado 11 y con evidencias fechadas, como exige la rúbrica del apartado 7. Y cualquier clasificación que te dé hecha: los matices ("¿esto es dinámico o es JavaScript de cliente?") son exactamente donde más falla.
- **Lo que se te pedirá defender**: cada entrega evaluable de este módulo se defiende. En particular, la propuesta de pila de la AE10 la sostendrás con tus palabras y respondiendo a un "¿y si…?" — si la escribió un asistente y no la entiendes, la defensa lo revela en el primer minuto. Si está en tu entrega, es tuyo.

<div style="page-break-before: always;"></div>

## Apartado 10. Actividad evaluativa final

**Contexto — la academia de idiomas.** Una academia comarcal con **3 sedes**, unos **400 alumnos** y **25 cursos por trimestre** funciona hoy así: una web estática que hizo hace años un antiguo alumno (HTML editado a mano, y ya nadie se atreve a tocarlo), matrículas por teléfono y una hoja de cálculo por sede. Quieren: un **catálogo de cursos** siempre actualizado desde una única fuente, **matrícula en línea** con plazas limitadas por curso, un **área personal** donde cada alumno vea sus horarios y facturas, y un **boletín mensual**. El presupuesto es ajustado y la persona que mantendrá el sistema conoce PHP.

**Instrucciones.** 10 ejercicios, 1 punto cada uno; se responde **razonando sobre el contexto** (respuestas sin justificar no puntúan completas). **Tiempo estimado: 2 horas**, más la preparación de evidencias. **Entrega**: documento `ud1/actividad-evaluativa.md` en tu repositorio de la unidad del aula de código, con commits por bloques de ejercicios, las capturas en `ud1/evidencias/` y el `DECISIONES.md` actualizado. **Defensa individual de 4–5 minutos** según el calendario publicado.

- **AE1** `[RA1.a]` Clasifica dónde se ejecuta el código responsable de cada comportamiento de la futura web — cliente o servidor — y justifica en una línea: (1) el menú se despliega al pasar el ratón; (2) el catálogo muestra los cursos del trimestre en vigor; (3) al teclear el correo, avisa al momento de que falta la arroba; (4) el área personal muestra tus facturas y no las de otro; (5) el buscador filtra la tabla de cursos ya cargada, sin recargar; (6) al matricularte, se comprueba y descuenta la plaza.
- **AE2** `[RA1.b]` El catálogo se edita hoy a mano en HTML. Identifica **tres** problemas concretos que la academia ya sufre con esa práctica y, para cada uno, la ventaja de la generación dinámica que lo elimina.
- **AE3** `[RA1.a]` La matrícula tiene plazas limitadas. Explica dónde debe ejecutarse la decisión "queda plaza" y qué podría ocurrir si se tomara en el cliente; describe el reparto correcto de la validación entre cliente y servidor usando la regla de oro del apartado 3.
- **AE4** `[RA1.c]` Este es el registro de consola de un servidor de pruebas de la academia (formato: método, ruta, código de estado devuelto). Interpreta cada línea — qué pidió el cliente y cómo respondió el servidor:
  `GET / 200` · `GET /cursos 200` · `GET /estilos.css 200` · `POST /matricula 302` · `GET /matricula-ok 200` · `GET /admin 403`
- **AE5** `[RA1.c]` Describe, paso a paso y con el vocabulario de la unidad, el viaje completo de la petición "una alumna abre su horario en el área personal": desde que teclea la dirección hasta que la página se pinta, nombrando qué ocurre en el cliente, qué en el servidor y qué viaja en cada sentido.
- **AE6** `[RA1.d]` La aplicación se desplegará como aplicación PHP detrás de un servidor web. Dibuja o describe el despliegue, explica el papel de cada pieza (qué atiende el servidor web y qué delega en la aplicación) y nombra **dos** funcionalidades de la capa de servidor de aplicaciones o gestor de procesos que la academia agradecerá, con el problema que evita cada una.
- **AE7** `[RA1.e]` Elabora la ficha comparada de **dos** tecnologías candidatas para este proyecto — PHP y otra a tu elección del apartado 5 — aplicando los ejes del panorama **a este caso concreto** (equipo, presupuesto, tipo de aplicación), y cierra con una recomendación provisional.
- **AE8** `[RA1.f]` La lista de cursos se generará con una plantilla con huecos. Dado el fragmento y los datos siguientes, escribe el **HTML exacto** que recibiría el navegador y señala qué no puede saber el navegador sobre su procedencia (la sintaxis es la del apartado 6; basta con leerla):

  ```
  <ul>
    <?php foreach ($cursos as $curso) { ?>
      <li><?= $curso['nombre'] ?> — <?= $curso['plazas'] ?> plazas</li>
    <?php } ?>
  </ul>
  ```

  con `$cursos = [['nombre' => 'Inglés B1', 'plazas' => 12], ['nombre' => 'Francés A2', 'plazas' => 8]]`.
- **AE9** `[RA1.f]` Evidencia práctica: ejecuta el experimento 2 del apartado 6, captura de la pestaña Red el `Content-Type` y el cuerpo recibido tras dos recargas, y redacta en 3–4 líneas la verificación: qué mecanismo de integración queda demostrado y en qué se nota. Adjunta las capturas en `ud1/evidencias/`.
- **AE10** `[RA1.e, RA1.g]` **Informe final para la academia** (300–400 palabras): propuesta razonada de pila — lenguaje, framework y motor de plantillas — evaluando el framework elegido con al menos **cuatro** criterios de la rúbrica del apartado 7 y citando **dos evidencias reales y fechadas** tomadas de sus fuentes oficiales (por ejemplo, la fecha de la última versión y la licencia). Se defiende sin leer.

<div style="page-break-before: always;"></div>

## Apartado 11. Para ampliar

- [Primeros pasos en la programación de lado servidor (MDN, en español)](https://developer.mozilla.org/es/docs/Learn_web_development/Extensions/Server-side/First_steps) — el módulo introductorio de Mozilla: qué es el lado servidor, la visión cliente-servidor y una panorámica de frameworks; el complemento perfecto de los apartados 1 a 4.
- [Manual oficial de PHP (en español)](https://www.php.net/manual/es/) — la referencia del lenguaje principal del curso: sintaxis, tipos, estructuras de control, clases y objetos, sesiones… De su web oficial saldrá también tu instalación de casa; será tu documentación de cabecera desde la UD2 (las páginas aún sin traducir se muestran en inglés).
- [Documentación oficial de Laravel](https://laravel.com/docs) — la referencia del framework del cierre del curso (en inglés): de momento te basta reconocerla y hojear su guía de instalación; será tu guía cuando llegue el tramo Laravel.
