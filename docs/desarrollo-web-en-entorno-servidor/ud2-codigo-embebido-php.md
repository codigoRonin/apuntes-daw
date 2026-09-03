# UD2. Código embebido en lenguajes de marcas: sintaxis, estructuras, funciones y formularios

**Módulo 0613 — Desarrollo Web en Entorno Servidor · 2.º DAW · IES Río Arba · Curso 2026-27 · 36 horas · 1.ª evaluación**

**Vinculación con los resultados de aprendizaje.** Esta unidad trabaja completos dos RA (RD 686/2010 en la redacción dada por el RD 405/2023). **RA2**: *"Escribe sentencias ejecutables por un servidor web reconociendo y aplicando procedimientos de integración del código en lenguajes de marcas"*. **RA3**: *"Escribe bloques de sentencias embebidos en lenguajes de marcas, seleccionando y utilizando las estructuras de programación"*. Fíjate en el verbo de los dos enunciados: **escribe**. La UD1 fue de entender y verificar; esta es de programar.

| CE | Qué exige (resumen) | Apartado donde se trabaja |
|---|---|---|
| RA2.a | Reconocer los mecanismos de generación de páginas a partir de marcas con código embebido | Apartados 1 y 8 |
| RA2.b | Identificar las principales tecnologías asociadas | Apartado 1 |
| RA2.c | Utilizar etiquetas para la inclusión de código en el lenguaje de marcas | Apartados 1 y 8 |
| RA2.d | Reconocer la sintaxis del lenguaje de programación | Apartados 2, 3, 6 y 7 |
| RA2.e | Escribir sentencias simples y comprobar sus efectos en el documento resultante | Apartados 1, 2 y 8 |
| RA2.f | Utilizar directivas para modificar el comportamiento predeterminado | Apartado 3 |
| RA2.g | Utilizar los distintos tipos de variables y operadores del lenguaje | Apartados 2 y 7 |
| RA2.h | Identificar los ámbitos de utilización de las variables | Apartado 6 |
| RA3.a | Utilizar mecanismos de decisión en la creación de bloques de sentencias | Apartados 4 y 8 |
| RA3.b | Utilizar bucles y verificar su funcionamiento | Apartados 5 y 8 |
| RA3.c | Utilizar matrices (arrays) para almacenar y recuperar conjuntos de datos | Apartados 5, 7 y 8 |
| RA3.d | Crear y utilizar funciones | Apartado 6 |
| RA3.e | Utilizar formularios web para interactuar con el usuario | Apartados 9 y 10 |
| RA3.f | Emplear métodos para recuperar la información introducida en el formulario | Apartados 9 y 10 |
| RA3.g | Añadir comentarios al código | Apartado 1 (y todo el código de la unidad) |

**Al terminar esta unidad sabrás:** montar y usar tu entorno PHP de desarrollo con su servidor embebido y con el comprobador de sintaxis como primera herramienta de trabajo; alternar con soltura entre el lenguaje de marcas y el código embebido usando las etiquetas correctas; escribir sentencias con los tipos, variables y operadores de PHP sin que te traicionen las diferencias con Java; activar la directiva que endurece los tipos y trabajar con el intérprete en modo desarrollo; construir bloques con decisiones, bucles, arrays y funciones — y saber en qué ámbito vive cada variable; declarar tus primeros tipos compuestos con clases y objetos; fabricar páginas completas con plantillas embebidas legibles, con su escape de salida y sus parciales; y cerrar el círculo de la unidad: recibir un formulario, recuperar sus datos y validarlos **en el servidor, por autoridad**, exactamente como anunció la regla de oro de la UD1.

**Entorno de trabajo de la unidad.** Aquí ya programas: necesitas **PHP instalado** (en el aula ya lo está; en casa, la instalación quedó resuelta en la UD1) y un terminal siempre a mano con tres comandos: `php -v` (comprobar la instalación), **`php -l fichero.php`** (comprobar la sintaxis de un fichero **antes** de ejecutarlo — desde hoy es tu primer reflejo profesional) y `php -S localhost:8000` (el servidor embebido de desarrollo que ya usaste en los experimentos de la UD1). Trabaja dentro de la carpeta `ud2/` de tu repositorio de la unidad en el aula de código, con tu **alias** en todas las herramientas externas, como fija la política de protección de datos del centro. El caso hilo sigue siendo la **web del club de montaña comarcal** de la UD1 — para esta unidad, un club ficticio de aula, el *Club de Montaña Os Ibones* —; su calendario de otoño vive en el fichero de datos **`datos/salidas.php`**, que está **íntegro en el repositorio de la unidad** (aquí los apuntes trabajan con una muestra de cinco salidas). Dos fronteras de alcance, para que nadie se pierda: en esta unidad **no hay memoria entre peticiones** — nada de cookies ni sesiones: el mantenimiento del estado es territorio de la UD3 (RA4) — y **no hay base de datos** — los datos viven en arrays; el acceso a datos llega en la UD6 (RA6). Y una nota de propagación: la **programación orientada a objetos** aparece aquí como *sintaxis del lenguaje* — tipos compuestos, al servicio de RA2.d y RA2.g —; su uso arquitectónico (separación de capas, patrones de diseño) se evalúa en la UD5 (RA5).

**Muestra del fichero de datos** (cabecera y cinco filas; «fichero íntegro en el repositorio de la unidad»):

```php
<?php
// datos/salidas.php — calendario de otoño del Club de Montaña Os Ibones (datos de aula, ficticios)
// Este fichero DEVUELVE el array: cárgalo con  $salidas = require __DIR__ . '/datos/salidas.php';
return [
    ['nombre' => 'Ibón de Estanés',     'fecha' => '2026-10-04', 'dificultad' => 'media', 'plazas' => 20, 'inscritos' => 17],
    ['nombre' => 'Peña Oroel',          'fecha' => '2026-10-11', 'dificultad' => 'baja',  'plazas' => 25, 'inscritos' => 25],
    ['nombre' => 'Cañón de Añisclo',    'fecha' => '2026-10-18', 'dificultad' => 'alta',  'plazas' => 15, 'inscritos' => 9],
    ['nombre' => 'San Juan de la Peña', 'fecha' => '2026-10-25', 'dificultad' => 'baja',  'plazas' => 30, 'inscritos' => 12],
    ['nombre' => 'Ibones de Anayet',    'fecha' => '2026-11-08', 'dificultad' => 'alta',  'plazas' => 12, 'inscritos' => 12],
    // … (siete filas más en el fichero íntegro, hasta 12)
];
```

<div style="page-break-before: always;"></div>

## Apartado 1. Tu banco de trabajo: el intérprete, las etiquetas y los comentarios

En la UD1 verificaste que el mecanismo de **código embebido** existe: escribiste marcas con huecos, el servidor las ejecutó y al navegador solo llegó HTML. Ahora toca dominar la mecánica. Las **tecnologías asociadas** que la hacen posible son las que ya tienes delante: el **intérprete de PHP** (el programa que entiende el lenguaje), su **servidor embebido de desarrollo** (`php -S`, suficiente para todo este tramo del curso; en producción el intérprete trabaja detrás de un servidor web, como viste en el apartado 4 de la UD1), tu **editor**, el **navegador con su pestaña Red** y el **manual oficial del lenguaje** — la referencia de cabecera que estrenamos en el apartado 14.

La pieza central del mecanismo son las **etiquetas de inclusión de código**. Cuando el intérprete procesa un fichero, busca `<?php` y `?>`: lo que queda **dentro** se ejecuta como PHP; lo que queda **fuera** se envía tal cual, sin mirarlo siquiera. Esa indiferencia hacia lo de fuera es exactamente lo que permite embeber PHP en cualquier documento de marcas. Hay una segunda etiqueta de apertura que vas a usar constantemente: **`<?= expresión ?>`**, el atajo de "abre, imprime esta expresión y cierra" — equivale a `<?php echo expresión ?>`. Estas dos son las únicas etiquetas del curso: existe una forma corta `<?` que **no** usamos, porque depende de la configuración de cada servidor y el código profesional no puede depender de eso.

Tu primer fichero de la unidad, `hola.php`, es PHP puro — y estrena una convención profesional: cuando un fichero **solo** contiene PHP, la etiqueta de cierre `?>` **se omite**. No es capricho: cualquier espacio o salto de línea que quedara detrás del cierre se enviaría como salida, y esa salida fantasma provoca averías que verás con nombre y apellidos en el apartado 10.

```php
<?php
// hola.php — primer fichero de la unidad (PHP puro: sin etiqueta de cierre)
$club = 'Club de Montaña Os Ibones';
echo 'Portal del ', $club, "\n";
echo 'Temporada de otoño abierta', "\n";
```

Antes de ejecutarlo, el reflejo nuevo: **comprobar la sintaxis**.

```
$ php -l hola.php
No syntax errors detected in hola.php
```

Y ahora sí, ejecútalo por los dos caminos que conocerás todo el curso: en el terminal (`php hola.php`), donde la salida aparece tal cual, y servido (`php -S localhost:8000` y el navegador en `http://localhost:8000/hola.php`), donde esa misma salida viaja como cuerpo de una respuesta HTTP. Salida en terminal, letra a letra:

```
Portal del Club de Montaña Os Ibones
Temporada de otoño abierta
```

Fíjate en tres detalles de sintaxis que ya trabajan para ti: cada **sentencia termina en punto y coma** (olvidarlo es el error número uno del apartado 11); `echo` admite varias piezas separadas por comas; y `"\n"` es un salto de línea — que solo funciona entre **comillas dobles**, matiz que el apartado 2 explota a fondo.

El tercer protagonista del apartado son los **comentarios** — y en un lenguaje embebido tienen un matiz que no existía en primero. PHP admite tres formas: `// comentario de línea`, `# comentario de línea` (menos habitual) y `/* comentario de bloque */`. La convención de la casa: `//` para lo cotidiano y `/* */` para bloques o para desactivar código temporalmente. Pero atención al matiz: un comentario **PHP** vive dentro de las etiquetas y **se queda en el servidor**; un comentario **HTML** (`<!-- ... -->`) vive fuera y **viaja al navegador**, donde cualquiera lo lee con "Ver código fuente". Es el reparto cliente/servidor de la UD1 aplicado a la cosa más pequeña posible:

```php
<?php // saludo.php — dos comentarios con dos destinos muy distintos
$temporada = 'otoño';
// Este comentario PHP jamás sale del servidor.
?>
<!-- Este comentario HTML viaja al navegador con la respuesta. -->
<p>Temporada de <?= $temporada ?> en el club.</p>
```

Sírvelo con `php -S localhost:8000`, ábrelo y mira el código fuente recibido. Esto — exactamente esto, byte a byte — es lo que llegó al cliente:

```html
<!-- Este comentario HTML viaja al navegador con la respuesta. -->
<p>Temporada de otoño en el club.</p>
```

El comentario PHP, la variable y las etiquetas desaparecieron: se ejecutaron. El comentario HTML llegó entero. Moraleja profesional doble: comenta tu código PHP con libertad — es documentación privada del servidor — y no dejes jamás en comentarios HTML nada que no deba leer el mundo.

**Ejercicios del apartado.**

- **E1.** Escribe `agenda.php` (PHP puro, sin etiqueta de cierre) que guarde en variables el nombre del club y el número de salidas de la temporada (usa 12) e imprima dos líneas: una de bienvenida con el nombre y otra con el número. Pásale `php -l`, ejecútalo en terminal y pega su salida como comentario al final del propio fichero.
- **E2.** Provoca y documenta: quita un punto y coma de `agenda.php`, ejecuta `php -l` y copia el mensaje completo. Después rompe una etiqueta de apertura (`<?ph`) y repite. Explica en una línea qué detectó el comprobador en cada caso y por qué el segundo error es de naturaleza distinta (pista: ¿qué es ese texto para el intérprete si la etiqueta no abre?).
- **E3.** Sirve `saludo.php`, captura el código fuente recibido y márcalo: qué líneas del fichero original viajaron tal cual, cuáles se transformaron y cuáles no viajaron. Termina respondiendo: ¿qué criterio de evaluación de la UD1 acabas de re-verificar sin querer, y con qué mecanismo nuevo de esta unidad?

<div style="page-break-before: always;"></div>

## Apartado 2. Variables, tipos y operadores: PHP para gente que viene de Java

Traes de primero (0485, Programación) un año entero de POO con Java. Buena noticia: casi todo se transfiere — las estructuras, los operadores, la lógica. La letra pequeña: PHP toma tres decisiones de diseño distintas, y quien no las conoce se pasa la unidad peleando con fantasmas. Empecemos por la tabla de transferencia, que conviene tener a mano las dos primeras semanas:

| En Java (0485) | En PHP | El matiz que importa |
|---|---|---|
| `int plazas = 20;` | `$plazas = 20;` | Toda variable empieza por `$`; **no se declara el tipo**: lo determina el valor |
| `String nombre = "Oroel";` | `$nombre = 'Oroel';` | Sin declaración; comillas simples y dobles existen y **no** son equivalentes |
| `System.out.println(x);` | `echo $x, "\n";` | `echo` es construcción del lenguaje, no método |
| `x + " metros"` | `$x . ' metros'` | Concatenar es **`.`** (el `+` en PHP siempre es suma aritmética) |
| `x == y` / `equals()` | `$x == $y` (con conversión) / `$x === $y` (estricta) | La comparación segura por defecto es **`===`** |
| `final double IVA = 0.21;` | `const IVA = 0.21;` | Constantes sin `$`, por convención en MAYÚSCULAS |
| `// y /* */` | `// , # y /* */` | Idénticos, más la variante `#` |
| Tipado estático en compilación | **Tipado dinámico** + declaraciones de tipo opcionales | El endurecimiento llega en el apartado 3 con `strict_types` |

Los **tipos escalares** de PHP son cuatro — `int`, `float`, `string`, `bool` — más el valor especial `null` ("ausencia de valor"). La herramienta para radiografiarlos es `var_dump()`, que imprime **tipo y valor** y va a ser tu lupa de depuración toda la unidad:

```php
<?php
// tipos.php — radiografía de los escalares
$plazas    = 20;            // int
$precio    = 12.5;          // float
$nombre    = 'Peña Oroel';  // string
$federada  = true;          // bool
$fecha_baja = null;         // null: aún no hay valor
var_dump($plazas, $precio, $nombre, $federada, $fecha_baja);
```

```
int(20)
float(12.5)
string(11) "Peña Oroel"
bool(true)
NULL
```

¿`string(11)` en un nombre de diez letras? Cuenta otra vez: `var_dump` mide **bytes**, y la `ñ` ocupa dos en la codificación UTF-8 con la que trabajamos. Guárdate el dato: cuando midas longitudes de texto real en español usarás `mb_strlen()` ("mb" de *multibyte*), que cuenta caracteres — `strlen('Peña Oroel')` da 11, `mb_strlen('Peña Oroel')` da 10.

Sobre los **strings**, la decisión de diseño más rentable de aprender hoy: las **comillas simples** son literales (lo que escribes es lo que hay) y las **comillas dobles interpolan** — sustituyen variables por su valor y entienden secuencias como `\n`. Para expresiones con acceso a arrays u objetos dentro de una cadena doble, las llaves `{}` delimitan sin ambigüedad:

```php
<?php
$nombre = 'Ibón de Estanés';
$plazas = 20;
$inscritos = 17;
echo 'Con simples: $nombre tiene $plazas plazas', "\n";
echo "Con dobles: $nombre tiene $plazas plazas\n";
$salida = ['nombre' => 'Peña Oroel', 'plazas' => 25];
echo "Con llaves: {$salida['nombre']} ofrece {$salida['plazas']} plazas\n";
echo 'Concatenando: quedan ' . ($plazas - $inscritos) . " plazas en {$nombre}\n";
```

```
Con simples: $nombre tiene $plazas plazas
Con dobles: Ibón de Estanés tiene 20 plazas
Con llaves: Peña Oroel ofrece 25 plazas
Concatenando: quedan 3 plazas en Ibón de Estanés
```

Los **operadores** aritméticos (`+ - * / %`), lógicos (`&& || !`) y de asignación compuesta (`+=`, `.=`) funcionan como esperas de Java. La divergencia crítica está en la **comparación**, porque PHP hace **conversión de tipos** ("type juggling") cuando comparas con `==`: convierte los operandos para poder compararlos, y eso produce igualdades que te sorprenderán. La versión estricta `===` compara **valor y tipo**, sin conversiones. Compruébalo — y quédate con la regla de la casa: **comparar siempre con `===` y `!==`**, y reservar `==` para cuando la conversión sea exactamente lo que quieres y lo dejes comentado:

```php
<?php
var_dump(3 == '3');     // string convertida a número antes de comparar
var_dump(3 === '3');    // tipos distintos: falso sin mirar más
var_dump('1' == '01');  // dos strings numéricas: se comparan como números
var_dump('1' === '01'); // como cadenas son distintas
var_dump(0 == 'hola');  // en el PHP actual, una cadena no numérica NO es 0
```

```
bool(true)
bool(false)
bool(true)
bool(false)
bool(false)
```

La última línea trae aviso para navegantes: en versiones antiguas del lenguaje esa comparación daba `true`, y media internet de apuntes viejos sigue contándolo — otra razón para verificar contra el manual oficial y contra tu propio intérprete, nunca contra la memoria de un foro.

Dos operadores más completan tu caja de esta unidad, y los dos existen por la web: el **fusión de null** `??` devuelve el operando izquierdo si existe y no es `null`, y el derecho en caso contrario — es la forma canónica de dar valores por defecto a datos que pueden no venir (lo explotarás con los formularios del apartado 9) —; y el **ternario** `condición ? si : no`, con su forma corta `?:`. Por último, las **constantes**: `const IVA_REDUCIDO = 0.10;` — sin `$`, en mayúsculas, para valores que son de la aplicación y no del momento.

**Ejercicios del apartado.**

- **E4.** Sin ejecutar, predice la salida exacta de este fragmento; después ejecútalo, compara y explica cada discrepancia con el concepto que la causa (interpolación, concatenación, bytes):

```php
<?php
$pico = 'Moncayo';
$altitud = 2314;
echo 'Subida al $pico', "\n";
echo "Subida al $pico ({$altitud} m)\n";
echo strlen('Añisclo'), ' / ', mb_strlen('Añisclo'), "\n";
```

- **E5.** Predice el resultado (`true`/`false`) de cada comparación y verifica después con `var_dump`: (1) `'10' == '1e1'`; (2) `'10' === '1e1'`; (3) `0.1 + 0.2 == 0.3`; (4) `5 == true`; (5) `'' == null`; (6) `'' === null`. Para la (3), busca en el manual (apartado 14, "Los tipos") por qué los decimales se comportan así y propón la comparación correcta para dinero.
- **E6.** Escribe `precio.php`: con `const PRECIO_SOCIO = 8.5;` y `const PRECIO_NO_SOCIO = 14.0;`, y variables `$es_socio` (bool) y `$acompanantes` (int), calcula e imprime con una sola sentencia `echo` — usando el ternario y la interpolación con llaves — una línea como `Total para 3 personas: 25,50 €` (formato español con `number_format($total, 2, ',', '.')`).
- **E7.** El fragmento `echo 'Quedan ' + $plazas - $inscritos + ' plazas';` compila pero no hace lo que su autor quería. Ejecuta, observa (salida y avisos), explica qué hizo PHP con cada `+` y reescríbelo dos veces: con concatenación y con interpolación.

<div style="page-break-before: always;"></div>

## Apartado 3. Directivas: cambiar las reglas del juego antes de jugar

Una **directiva** es una instrucción que no calcula nada: **modifica el comportamiento predeterminado** del intérprete para el código que la sigue. Es exactamente lo que enuncia el criterio RA2.f — y en PHP tiene un representante estrella que vas a escribir en la primera línea de cada fichero del curso.

El comportamiento predeterminado de PHP con los tipos es la **coerción**: si una función declara que espera un `int` y le llega la string `'12'`, el intérprete la convierte y sigue. Cómodo — y peligroso en la web, donde (lo verás en el apartado 9) **todo lo que llega de un formulario llega como string**. La directiva `declare(strict_types=1)` desactiva esa mano blanda **para el fichero donde se escribe**: con ella, un tipo que no encaja no se convierte — falla, alto y claro, señalando la línea exacta. Compara el mismo programa con la directiva apagada y encendida:

```php
<?php
// coercion.php — comportamiento predeterminado: PHP convierte y sigue
function precio_total(int $personas, float $tarifa): float {
    return $personas * $tarifa;
}
var_dump(precio_total('3', '8.5')); // strings numéricas: coerción silenciosa
```

```
float(25.5)
```

```php
<?php
declare(strict_types=1);
// estricto.php — la directiva cambia las reglas: sin conversiones silenciosas
function precio_total(int $personas, float $tarifa): float {
    return $personas * $tarifa;
}
var_dump(precio_total('3', '8.5'));
```

```
PHP Fatal error:  Uncaught TypeError: precio_total(): Argument #1 ($personas) must be of type int, string given, called in /ruta/estricto.php on line 7 and defined in /ruta/estricto.php:4
Stack trace:
#0 /ruta/estricto.php(7): precio_total()
#1 {main}
  thrown in /ruta/estricto.php on line 4
```

El error no es un castigo: es **información en el momento exacto**. Sin la directiva, la string equivocada viaja por tu programa disfrazada de número y explota lejos de su origen (o peor: no explota y calcula mal). Con ella, el problema se declara en la frontera. Según la configuración del intérprete, la traza puede mostrar u ocultar los argumentos de cada llamada; la primera línea del mensaje es la que importa. Regla de la casa desde hoy: **todo fichero de lógica del curso empieza por `declare(strict_types=1);`** — tiene que ser la primera sentencia del fichero —; en las plantillas del apartado 8, que apenas llevan lógica, puede omitirse.

La directiva convive con una segunda palanca del comportamiento predeterminado: la **configuración del intérprete**. El fichero `php.ini` (y las opciones `-d` de la línea de comandos) gobiernan, entre muchas otras cosas, qué hace PHP con los errores: en el aula trabajamos en **modo desarrollo** — `display_errors` activado y `error_reporting` al máximo — para que cada aviso se vea en el acto; en producción esa misma configuración se invierte (los errores se registran, no se muestran: enseñarlos revela interioridades del servidor, como el `500` de la UD1 ya insinuaba). Puedes consultar la configuración activa con `php --ini` y, puntualmente, forzarla al lanzar el servidor: `php -d display_errors=1 -S localhost:8000`. No cambies nada más de momento; basta con que sepas **dónde** viven las reglas del juego y **quién** puede cambiarlas.

**Ejercicios del apartado.**

- **E8.** Escribe `edad.php` con la función `function puede_alta_montania(int $edad): bool { return $edad >= 18; }` y pruébala con `var_dump` pasando `18` y `'18'`. Ejecuta el fichero sin directiva y con `declare(strict_types=1)`, pega las dos salidas y redacta en dos líneas qué comportamiento predeterminado modificó la directiva.
- **E9.** Con `estricto.php` delante: identifica en el mensaje del `TypeError` las **cuatro** informaciones que te da (qué función, qué argumento, qué tipo esperaba, desde qué línea se llamó) y explica por qué "fallar en la frontera" es preferible a "convertir y seguir" en una aplicación que cobra inscripciones. Cierra con la razón por la que la directiva será obligatoria en el procesado de formularios (apartado 10) precisamente porque *todo llega como string*.
- **E10.** Arqueología de configuración: ejecuta `php --ini` y localiza el fichero de configuración cargado. Busca en él (solo lectura) `display_errors` y `error_reporting`, anota sus valores y responde: ¿tu máquina está configurada como entorno de desarrollo o de producción? ¿Qué argumento `-d` usarías para forzar el modo desarrollo al lanzar el servidor embebido sin tocar el fichero?

<div style="page-break-before: always;"></div>

## Apartado 4. Decisiones: if, match y el arte de elegir rama

Programar es, en buena medida, elegir. Traes de Java el `if`/`else if`/`else` y el `switch`, y en PHP los primeros son idénticos salvo por la palabra pegada `elseif`. La novedad que sí cambia tu forma de escribir es **`match`**, la construcción moderna que casi siempre sustituye con ventaja al viejo `switch`.

Primero el `if`, resolviendo un problema real del club: etiquetar cada salida según cuántas plazas le quedan. Fíjate en que las ramas se evalúan **en orden** y gana la primera que se cumple — de ahí que el orden de las condiciones no sea decorativo:

```php
<?php
declare(strict_types=1);
// plazas.php — decidir la etiqueta de una salida
$nombre = 'Ibón de Estanés';
$plazas = 20;
$inscritos = 17;
$libres = $plazas - $inscritos;

if ($libres <= 0) {
    $etiqueta = 'COMPLETA';
} elseif ($libres <= 3) {
    $etiqueta = 'Últimas plazas';
} else {
    $etiqueta = 'Plazas disponibles';
}
echo "$nombre: $etiqueta ($libres libres)\n";
```

```
Ibón de Estanés: Últimas plazas (3 libres)
```

Ahora `match`, y con él una idea que conviene interiorizar: `match` es una **expresión** — *produce un valor* que puedes asignar —, mientras que `if` y `switch` son **sentencias** que *ejecutan acciones*. Esa diferencia hace el código más legible cuando lo que quieres es traducir un valor de entrada en un valor de salida:

```php
<?php
declare(strict_types=1);
// dificultad.php — match: la decisión como expresión
$dificultad = 'media';
$icono = match ($dificultad) {
    'baja'  => 'Paseo — para todos los públicos',
    'media' => 'Montaña — hace falta fondo',
    'alta'  => 'Alta montaña — solo con experiencia',
};
echo $icono, "\n";
```

```
Montaña — hace falta fondo
```

`match` trae de serie dos regalos que el `switch` no da. El primero: compara con **`===`** (estricta), así que se acabaron las sorpresas de conversión del apartado 2. El segundo, y capital: **es exhaustivo**. Si el valor no encaja en ningún brazo y no hay `default`, no sigue como si nada — **falla**:

```
PHP Fatal error:  Uncaught UnhandledMatchError: Unhandled match case '...' in /ruta/dificultad_err.php:5
Stack trace:
#0 {main}
  thrown in /ruta/dificultad_err.php on line 5
```

Esa dureza es una virtud: el día que alguien añada una dificultad nueva al calendario sin actualizar el `match`, el programa te avisa en vez de esconder el caso. (Nota de reproducibilidad: que el valor no cubierto salga recortado como `'...'` o completo — `'extrema'` — no depende del intérprete, sino de la directiva `zend.exception_string_param_max_len`. El `php.ini` de producción que instalan Debian y Ubuntu la fija a 0 y por eso en el aula verás `'...'`; con el valor de fábrica de PHP (15) verías `'extrema'`. El mensaje y la clase del error, `UnhandledMatchError`, son idénticos en ambos casos, que es lo que importa.) Para cubrir el resto con red, agrupa casos con comas y cierra con `default`:

```php
<?php
declare(strict_types=1);
// aviso.php — agrupación de casos y brazo default
$dificultad = 'extrema';
$aviso = match ($dificultad) {
    'baja', 'media' => 'Apta para el grupo general',
    'alta'          => 'Requiere acreditar experiencia',
    default         => 'Dificultad sin catalogar: consulta en secretaría',
};
echo $aviso, "\n";
```

```
Dificultad sin catalogar: consulta en secretaría
```

Un último apunte que la web te va a exigir enseguida: en una condición, PHP evalúa la **veracidad** de un valor cuando no es estrictamente booleano, y sus reglas tienen trampas. Son "falsos" (falsy): `false`, `0`, `0.0`, `''` (cadena vacía), `'0'` (¡la cadena con el carácter cero!), `[]` (array vacío) y `null`. Todo lo demás es "verdadero". La string `'0'` como valor falso es fuente inagotable de bugs con datos de formulario — quédatela:

```php
<?php
var_dump((bool) '0');
var_dump((bool) '');
var_dump((bool) 'false');
var_dump((bool) 0.0);
var_dump((bool) []);
```

```
bool(false)
bool(false)
bool(true)
bool(false)
bool(false)
```

La tercera línea es la moraleja: la cadena `'false'` es **verdadera** (tiene contenido), aunque su texto diga lo contrario. Por eso a los datos que llegan de la web no se les pregunta "¿eres verdad?" sino cosas concretas: "¿estás vacío?", "¿eres un número válido?" — herramientas del apartado 10.

**Ejercicios del apartado.**

- **E11.** Este `if` tiene un fallo de orden: la etiqueta "COMPLETA" nunca se imprimirá. Ejecútalo con `$libres` a 0, a 3 y a 12, explica por qué la segunda rama es inalcanzable y reordena las condiciones para arreglarlo.

```php
<?php
$libres = 0; // prueba también con 3 y con 12
if ($libres < 5) {
    echo "Últimas plazas\n";
} elseif ($libres <= 0) {
    echo "COMPLETA\n";
} else {
    echo "Plazas disponibles\n";
}
```

- **E12.** Reescribe la cadena `if`/`elseif`/`else` de `plazas.php` como un `match (true)` (la forma de `match` que evalúa condiciones booleanas en vez de igualdades). Ejecuta ambos con los mismos datos y confirma salida idéntica; después explica en una línea cuándo prefieres `match (true)` y cuándo el `if` clásico.
- **E13.** El club clasifica a quien se inscribe por edad: menor de 14 → "infantil"; de 14 a 17 → "juvenil (con permiso)"; 18 o más → "adulto". Escríbelo con `match (true)` y prueba los límites exactos (13, 14, 17, 18). Después provoca a propósito un `UnhandledMatchError` quitando una rama y pega el mensaje: ¿qué te protege esa exhaustividad?

<div style="page-break-before: always;"></div>

## Apartado 5. Bucles y arrays: recorrer el calendario del club

Los datos de la web casi nunca vienen de uno en uno: vienen en **colecciones**. El calendario del club es una lista de salidas; cada salida, un conjunto de campos con nombre. Las dos herramientas de este apartado — **arrays** para guardar la colección y **bucles** para recorrerla — son, juntas, el motor de casi todo lo que hará tu servidor.

PHP tiene un único tipo `array` que hace dos trabajos. Como **array indexado**, guarda una secuencia con claves numéricas automáticas desde 0 (la lista de toda la vida). Como **array asociativo**, guarda pares **clave → valor** con claves de texto (el "diccionario" o "mapa" de otros lenguajes). La web los usa mezclados constantemente: una **lista de salidas**, donde cada salida es un **mapa de campos**.

```php
<?php
declare(strict_types=1);
// arrays.php — indexado y asociativo
$dificultades = ['baja', 'media', 'alta'];
echo $dificultades[0], "\n";
echo count($dificultades), " niveles\n";
$dificultades[] = 'expedición';
$salida = ['nombre' => 'Peña Oroel', 'plazas' => 25, 'inscritos' => 25];
echo $salida['nombre'], ': ', $salida['plazas'] - $salida['inscritos'], " libres\n";
var_dump($salida['fecha'] ?? 'sin fecha');
```

```
baja
3 niveles
Peña Oroel: 0 libres
string(9) "sin fecha"
```

Dos cosas que registrar de ahí. `$dificultades[] = 'expedición'` **añade al final** sin que tengas que calcular el índice — el idioma de PHP para "empujar" a una lista. Y la última línea estrena en su hábitat natural el operador `??` del apartado 2: `$salida['fecha']` no existe, así que en vez de un aviso de clave indefinida obtienes el valor por defecto. Esta pareja clave-inexistente + `??` reaparecerá idéntica con los datos de formulario del apartado 9, donde un campo que no llega es exactamente una clave que no existe.

El bucle que domina el desarrollo web es **`foreach`**, hecho a medida para recorrer arrays. Vamos ya con la muestra de cinco salidas del fichero de datos de la unidad:

```php
<?php
declare(strict_types=1);
// calendario.php — la muestra de cinco salidas del fichero de datos
$salidas = [
    ['nombre' => 'Ibón de Estanés',     'fecha' => '2026-10-04', 'dificultad' => 'media', 'plazas' => 20, 'inscritos' => 17],
    ['nombre' => 'Peña Oroel',          'fecha' => '2026-10-11', 'dificultad' => 'baja',  'plazas' => 25, 'inscritos' => 25],
    ['nombre' => 'Cañón de Añisclo',    'fecha' => '2026-10-18', 'dificultad' => 'alta',  'plazas' => 15, 'inscritos' => 9],
    ['nombre' => 'San Juan de la Peña', 'fecha' => '2026-10-25', 'dificultad' => 'baja',  'plazas' => 30, 'inscritos' => 12],
    ['nombre' => 'Ibones de Anayet',    'fecha' => '2026-11-08', 'dificultad' => 'alta',  'plazas' => 12, 'inscritos' => 12],
];
foreach ($salidas as $salida) {
    $libres = $salida['plazas'] - $salida['inscritos'];
    echo $salida['fecha'], '  ', $salida['nombre'], ' — ', $libres, " libres\n";
}
```

```
2026-10-04  Ibón de Estanés — 3 libres
2026-10-11  Peña Oroel — 0 libres
2026-10-18  Cañón de Añisclo — 6 libres
2026-10-25  San Juan de la Peña — 18 libres
2026-11-08  Ibones de Anayet — 0 libres
```

`foreach` también te da la clave, con la forma `foreach ($array as $clave => $valor)`. Es justo lo que necesitas para **contar por categorías** — un patrón que aparece en todos los paneles de administración del mundo. Aquí, cuántas salidas hay de cada dificultad, usando un array asociativo como marcador:

```php
$recuento = ['baja' => 0, 'media' => 0, 'alta' => 0];
foreach ($salidas as $salida) {
    $recuento[$salida['dificultad']]++;
}
foreach ($recuento as $dificultad => $total) {
    echo $dificultad, ': ', $total, "\n";
}
```

```
baja: 2
media: 1
alta: 2
```

Los otros bucles — `while`, `do-while`, `for` — siguen en tu caja para cuando **no** recorres una colección entera, sino que iteras mientras se cumpla algo o un número fijo de veces. `while` para "hasta encontrar el primero que cumple"; `for` cuando el índice numérico importa:

```php
$i = 0;
while ($salidas[$i]['inscritos'] < $salidas[$i]['plazas']) {
    $i++;
}
echo 'Primera completa: ', $salidas[$i]['nombre'], "\n";

for ($n = 1; $n <= count($salidas); $n++) {
    echo $n, '. ', $salidas[$n - 1]['nombre'], "\n";
}
```

```
Primera completa: Peña Oroel
1. Ibón de Estanés
2. Peña Oroel
3. Cañón de Añisclo
4. San Juan de la Peña
5. Ibones de Anayet
```

Completa el apartado un puñado de **funciones de array** que te ahorrarán bucles enteros y que la web usa a diario: `count()` (cuántos), `in_array($aguja, $pajar, true)` (¿está?, con el tercer argumento `true` para comparación estricta — úsalo siempre), `array_keys()` / `array_values()` (las claves / los valores), `implode(', ', $array)` (pegar en una cadena) y su inverso `explode()`. Dos ejemplos que resuelven preguntas reales sobre el calendario:

```php
$niveles_del_calendario = [];
foreach ($salidas as $salida) {
    if (!in_array($salida['dificultad'], $niveles_del_calendario, true)) {
        $niveles_del_calendario[] = $salida['dificultad'];
    }
}
echo 'Niveles ofertados: ', implode(', ', $niveles_del_calendario), "\n";
echo 'Campos de una salida: ', implode(', ', array_keys($salidas[0])), "\n";
```

```
Niveles ofertados: media, baja, alta
Campos de una salida: nombre, fecha, dificultad, plazas, inscritos
```

**Ejercicios del apartado.**

- **E14.** Sobre la muestra de cinco salidas: con un `foreach`, calcula e imprime el **total de plazas libres** de todo el calendario y, en la misma pasada, cuenta cuántas salidas están **completas** (`libres == 0`). Salida esperable en dos líneas.
- **E15.** Construye con un bucle un array asociativo `$por_dificultad` donde cada clave (`'baja'`, `'media'`, `'alta'`) apunte a **la lista de nombres** de las salidas de esa dificultad (una lista dentro de un mapa). Imprímelo con `print_r` y explica en una línea qué estructura acabas de fabricar (array de arrays).
- **E16.** Usa `for` con `count($salidas)` para numerar las salidas del 1 al 5 en la salida (como en el ejemplo), pero invirtiendo el orden (de la última a la primera). Después explica por qué `foreach` no habría sido cómodo aquí y `for` sí.
- **E17.** Toma la cadena `'baja;media;alta;baja;alta'`. Con `explode` conviértela en array, cuenta con un bucle cuántas veces aparece cada nivel (array asociativo de recuento) y vuelve a unir con `implode` solo los niveles que aparecen más de una vez, separados por " y ". Salida esperable: `baja y alta`.

<div style="page-break-before: always;"></div>

## Apartado 6. Funciones y ámbitos: dónde vive cada variable

Hasta aquí has escrito programas que se leen de arriba abajo. Funciona para un ejemplo; no escala. La herramienta para **no repetirte** y para **dar nombre a una idea** es la **función**: un bloque con nombre, que recibe datos (parámetros), hace algo y **devuelve** un resultado. En la web las escribirás a centenares.

La sintaxis en PHP añade a lo que traes de Java una virtud enorme: **declaración de tipos** en parámetros y en el valor de retorno. Con `declare(strict_types=1)` en cabecera (apartado 3), esos tipos se hacen cumplir — la función se protege sola de recibir basura:

```php
<?php
declare(strict_types=1);
// funciones.php — función tipada, con valor por defecto
function plazas_libres(int $plazas, int $inscritos): int {
    return $plazas - $inscritos;
}
function etiqueta_plazas(int $libres, int $umbral = 3): string {
    if ($libres <= 0)       return 'COMPLETA';
    if ($libres <= $umbral) return 'Últimas plazas';
    return 'Plazas disponibles';
}
$libres = plazas_libres(20, 17);
echo $libres, ' → ', etiqueta_plazas($libres), "\n";
echo '12 → ', etiqueta_plazas(12), "\n";
echo '12 (umbral 15) → ', etiqueta_plazas(12, 15), "\n";
```

```
3 → Últimas plazas
12 → Plazas disponibles
12 (umbral 15) → Últimas plazas
```

Fíjate en `etiqueta_plazas`: el parámetro `$umbral = 3` es un **valor por defecto** — si no lo pasas, vale 3; si lo pasas, manda el tuyo. Es el mismo patrón que ya viste en funciones del lenguaje. PHP suma otra comodidad muy legible, los **argumentos con nombre**, y el tipo `?string` que significa "string **o** `null`" — el idioma para "este dato puede no venir", que es el pan de cada día en la web:

```php
<?php
declare(strict_types=1);
// nombrados.php — argumentos con nombre y tipo que admite null
function resumen(string $nombre, ?string $fecha = null): string {
    return $nombre . ' (' . ($fecha ?? 'fecha por confirmar') . ')';
}
echo resumen('Moncayo'), "\n";
echo resumen(nombre: 'Anayet', fecha: '2026-11-08'), "\n";
```

```
Moncayo (fecha por confirmar)
Anayet (2026-11-08)
```

Y ahora la otra mitad del apartado, la que el criterio RA2.h señala expresamente: el **ámbito** de las variables. La regla de PHP es tajante y **distinta de la de muchos lenguajes**: una variable creada dentro de una función es **local** —vive y muere en esa llamada— y una variable de fuera **no** es visible dentro, aunque se llame igual. No hay "acceso automático al de fuera". Compruébalo:

```php
<?php
declare(strict_types=1);
// ambito.php — la variable de dentro no es la de fuera
$total = 100;
function suma_una(int $n): int {
    $total = $n + 1;   // ESTE $total es local, distinto del de fuera
    return $total;
}
echo suma_una(5), "\n";
echo $total, "\n";     // el de fuera sigue intacto
```

```
6
100
```

El `$total` de dentro y el de fuera son **dos variables distintas** que comparten nombre por casualidad. Modificar el de dentro no toca el de fuera: por eso la segunda línea sigue siendo 100. Esta frontera es una defensa, no un estorbo — impide que una función corrompa por accidente el estado de quien la llama. La cara incómoda aparece cuando intentas **leer** de dentro algo de fuera sin pasarlo:

```php
<?php
declare(strict_types=1);
// ambito_err.php — intentar leer una variable de fuera SIN pasarla
$iva = 0.21;
function con_iva(float $base): float {
    return $base * (1 + $iva);   // $iva NO existe aquí dentro
}
echo con_iva(100), "\n";
```

```
PHP Warning:  Undefined variable $iva in /ruta/ambito_err.php on line 6
100
```

Lee el desastre con calma, porque es **silencioso y caro**: `$iva` no existe dentro, así que vale `null`; `1 + null` es `1`; el resultado es `100` en vez de `121`. Solo un *warning* — el programa no se detiene — y un cálculo mal hecho colándose en producción. La lección profesional: **lo que una función necesita, se le pasa como parámetro**; no se "hereda" del exterior. (Existe la palabra `global` para forzar el acceso, pero es mala práctica salvo casos muy justificados: hace a la función dependiente de un estado invisible. En este curso: parámetros, siempre.)

Un último tipo de ámbito con un uso concreto: una variable local marcada **`static`** conserva su valor **entre llamadas** a la misma función, sin ser global. Sirve para llevar una cuenta sin exponer un contador al exterior:

```php
<?php
declare(strict_types=1);
// static_demo.php — una local que recuerda entre llamadas
function siguiente_dorsal(): int {
    static $contador = 0;
    $contador++;
    return $contador;
}
echo siguiente_dorsal(), ' ', siguiente_dorsal(), ' ', siguiente_dorsal(), "\n";
```

```
1 2 3
```

**Ejercicios del apartado.**

- **E18.** Escribe la función `precio_final(float $base, bool $es_socio, float $descuento_socio = 0.20): float` que aplique el descuento solo a socios, con el ternario. Pruébala con `(20.0, true)`, `(20.0, false)` y `(20.0, true, 0.5)`; pega las tres salidas y señala cuál usó el valor por defecto y cuál lo sobrescribió.
- **E19.** Predice qué imprime `ambito.php` **antes** de ejecutarlo y explica, con los términos "local" y "ámbito", por qué la última línea no es 6. Después modifica la función para que el cambio **sí** trascienda al exterior — de la única forma correcta: devolviendo el valor y reasignándolo fuera (`$total = suma_una($total);`).
- **E20.** Arregla `ambito_err.php` de las **dos** maneras y compara: (a) pasando `$iva` como parámetro; (b) declarando `$iva` como constante `const IVA = 0.21;` fuera y usándola dentro (las constantes, a diferencia de las variables, **sí** son visibles en cualquier ámbito). Razona en dos líneas por qué (b) funciona y cuál prefieres para un tipo de IVA.
- **E21.** Escribe `total_calendario(array $salidas): array` que reciba la muestra de salidas y devuelva un array asociativo con tres claves: `'total_libres'`, `'completas'` y `'nivel_mas_ofertado'`. Una sola función, un solo `return`. Pruébala con la muestra de cinco y comprueba el resultado a mano.

<div style="page-break-before: always;"></div>

## Apartado 7. Tipos compuestos: tus primeras clases y objetos

Un array asociativo como `['nombre' => 'Peña Oroel', 'plazas' => 25]` describe bien una salida, pero tiene dos debilidades: nada garantiza que traiga los campos correctos (un `$salida['plzas']` mal tecleado no protesta hasta que revienta lejos) y la lógica asociada —"¿está completa?"— anda suelta en funciones separadas. La respuesta del lenguaje es el **tipo compuesto**: una **clase**, plantilla que define qué datos tiene una cosa (**propiedades**) y qué sabe hacer (**métodos**), y de la que se crean **objetos** concretos.

Esto lo traes de Java, así que aquí vamos a lo que **cambia** y a por qué en este módulo la POO entra por la puerta de la *sintaxis*: una clase es, ante todo, **un tipo de dato que te fabricas tú**, al servicio de los criterios RA2.d y RA2.g (sintaxis y tipos). La sintaxis de PHP moderno es notablemente compacta gracias a la **promoción de propiedades en el constructor** — declarar y asignar en un solo sitio:

```php
<?php
declare(strict_types=1);
// clase.php — un tipo compuesto propio
class Salida {
    public function __construct(
        public string $nombre,
        public string $dificultad,
        public int $plazas,
        public int $inscritos,
    ) {}

    public function libres(): int {
        return $this->plazas - $this->inscritos;
    }
    public function estaCompleta(): bool {
        return $this->libres() <= 0;
    }
}
$oroel = new Salida('Peña Oroel', 'baja', 25, 25);
echo $oroel->nombre, ': ', $oroel->libres(), " libres\n";
var_dump($oroel->estaCompleta());
```

```
Peña Oroel: 0 libres
bool(true)
```

Descifra las diferencias con Java, que son casi todas de puntuación. Se accede a lo de dentro con la **flecha `->`** (no con el punto): `$oroel->nombre`, `$oroel->libres()`. Dentro de la clase, el objeto sobre el que se opera es **`$this`**. Y `public function __construct(public string $nombre, ...)` es el azúcar de PHP moderno: cada parámetro marcado con visibilidad (`public`) **se convierte en propiedad y se asigna solo** — el equivalente a declarar la propiedad arriba y escribir `$this->nombre = $nombre;` en el cuerpo, que es como lo hacías en primero. Menos ceremonia, misma idea.

La ganancia se ve al juntar objetos en una colección. Un **array de objetos** se recorre con el mismo `foreach` del apartado 5, pero cada elemento ahora **se autodescribe y trae su lógica encima**:

```php
<?php
declare(strict_types=1);
// objetos.php — array de objetos (mismo recorrido, otra sintaxis de acceso)
class Salida {
    public function __construct(
        public string $nombre,
        public int $plazas,
        public int $inscritos,
    ) {}
    public function libres(): int { return $this->plazas - $this->inscritos; }
}
$calendario = [
    new Salida('Ibón de Estanés', 20, 17),
    new Salida('Peña Oroel', 25, 25),
    new Salida('Cañón de Añisclo', 15, 9),
];
foreach ($calendario as $s) {
    echo $s->nombre, ' — ', $s->libres(), " libres\n";
}
```

```
Ibón de Estanés — 3 libres
Peña Oroel — 0 libres
Cañón de Añisclo — 6 libres
```

Compara este `foreach` con el del apartado 5 sobre arrays asociativos: la forma del bucle es idéntica; cambia el **acceso** (`$s->libres()` en vez de restar campos a mano) y cambia la **garantía** (un objeto `Salida` siempre tiene sus cuatro campos, con sus tipos; un array asociativo, no). Esa es toda la POO que esta unidad necesita: la clase como **tipo compuesto** que da forma y garantías a tus datos.

Aquí paramos a propósito. La herencia, las interfaces, la visibilidad `private`/`protected` y, sobre todo, el uso de las clases para **separar la lógica de negocio de la presentación** —organizar una aplicación entera en capas y patrones— son el corazón de la **UD5 (RA5)**, y allí se evalúan. En esta unidad la clase es sintaxis: un tipo que te fabricas para escribir código más claro y seguro.

**Ejercicios del apartado.**

- **E22.** Amplía la clase `Salida` de `clase.php` con la propiedad `public string $dificultad` (ya está) y un método `nivelLegible(): string` que devuelva, con un `match` sobre `$this->dificultad`, el texto largo del apartado 4 (`'baja'` → `'Paseo — para todos los públicos'`, etc.). Crea dos salidas distintas y muestra su nivel legible.
- **E23.** Convierte la muestra de cinco salidas (arrays asociativos) en un array de objetos `Salida`. Con un `foreach`, imprime solo las que **no** están completas, usando `estaCompleta()`. Compara en dos líneas esta versión con hacerlo sobre arrays asociativos: ¿qué te da el objeto que el array no?
- **E24.** Añade a `Salida` un método `resumen(): string` que devuelva una línea lista para mostrar, del tipo `Peña Oroel (baja) — COMPLETA` o `Ibón de Estanés (media) — 3 plazas libres`, reutilizando `libres()` y `estaCompleta()` dentro del propio método (con `$this`). Recórre el calendario imprimiendo `$s->resumen()`. Esto es "la lógica viaja con el dato": explica por qué es una mejora frente a calcular ese texto fuera de la clase.

<div style="page-break-before: always;"></div>

## Apartado 8. Plantillas PHP embebidas: sintaxis alternativa, escape y parciales

Ya tienes las piezas del lenguaje; toca ensamblarlas para lo que este módulo pide de verdad: **fabricar páginas**. Aquí PHP vuelve a su vocación original —lenguaje embebido en marcas— y cierra el círculo que abriste en la UD1: escribir HTML con huecos que el servidor rellena. La diferencia es que ahora los huecos los llenas **tú**, con todo lo aprendido.

El primer instinto, viniendo de la programación pura, es fabricar el HTML **desde PHP** con `echo` y concatenación. Funciona, pero mira qué resulta — y por qué es un callejón sin salida:

```php
<?php
declare(strict_types=1);
$salidas = [
    ['nombre' => 'Ibón de Estanés', 'dificultad' => 'media', 'plazas' => 20, 'inscritos' => 17],
    ['nombre' => 'Peña Oroel', 'dificultad' => 'baja', 'plazas' => 25, 'inscritos' => 25],
];
echo "<h1>Calendario</h1>\n<ul>\n";
foreach ($salidas as $s) {
    $libres = $s['plazas'] - $s['inscritos'];
    echo "<li>" . $s['nombre'] . " (" . $s['dificultad'] . ") — " . $libres . " libres</li>\n";
}
echo "</ul>\n";
```

Produce el HTML correcto, sí — pero el HTML está **preso dentro de cadenas PHP**: no lo ve tu editor como HTML, no te avisa de una etiqueta mal cerrada, y en cuanto la página crece se vuelve ilegible. La forma profesional invierte la relación: **el HTML manda y el PHP se asoma solo para los huecos**. Y para eso PHP ofrece la **sintaxis alternativa** de las estructuras de control, pensada exactamente para plantillas: `foreach (…):` … `endforeach;`, `if (…):` … `endif;`. Con ella, y con la etiqueta de impresión `<?= ?>`, la misma página se lee como HTML de principio a fin:

```php
<?php declare(strict_types=1); ?>
<?php
$salidas = [
    ['nombre' => 'Ibón de Estanés', 'dificultad' => 'media', 'plazas' => 20, 'inscritos' => 17],
    ['nombre' => 'Peña Oroel', 'dificultad' => 'baja', 'plazas' => 25, 'inscritos' => 25],
];
?>
<h1>Calendario de otoño</h1>
<ul>
<?php foreach ($salidas as $s): ?>
    <?php $libres = $s['plazas'] - $s['inscritos']; ?>
    <li><?= $s['nombre'] ?> (<?= $s['dificultad'] ?>) — <?= $libres ?> libres</li>
<?php endforeach; ?>
</ul>
```

Sírvela con `php -S localhost:8000` y pídela con el navegador. Esto es, letra a letra, lo que llega al cliente:

```html
<h1>Calendario de otoño</h1>
<ul>
        <li>Ibón de Estanés (media) — 3 libres</li>
        <li>Peña Oroel (baja) — 0 libres</li>
</ul>
```

(Los espacios delante de cada `<li>` son la sangría del `foreach` en la plantilla: PHP envía tal cual todo lo que está fuera de las etiquetas, sangrías incluidas. Al navegador le da igual; a ti te conviene saber de dónde salen.) Compara mentalmente esta plantilla con la versión de `echo`: **misma salida, legibilidad opuesta**. El bloque de datos vive arriba, en su zona PHP; el marcado vive abajo, como HTML de verdad; y las etiquetas `<?= ?>` son ventanitas por las que se asoma un valor. Este es el patrón "lógica arriba, presentación abajo" — un primer paso, todavía dentro de un fichero, hacia la separación que la UD5 llevará a su forma plena.

Ahora, la regla de seguridad **irrenunciable** de este apartado, que te va a acompañar toda la vida profesional. Cuando el valor que imprimes **procede del usuario** (un formulario, la URL), no puedes volcarlo crudo en el HTML: si contiene caracteres especiales de marcas (`<`, `>`, `&`, comillas), rompería la página o —peor— inyectaría contenido. La función **`htmlspecialchars()`** convierte esos caracteres en sus entidades inofensivas. Míralo con un texto que trae marcas dentro:

```php
<?php declare(strict_types=1); ?>
<?php
// Un dato con caracteres especiales de HTML (imagina que viene de un formulario)
$comentario = 'Ruta <b>dura</b> pero preciosa & con niebla';
?>
<p>Sin escapar: <?= $comentario ?></p>
<p>Escapado: <?= htmlspecialchars($comentario) ?></p>
```

```html
<p>Sin escapar: Ruta <b>dura</b> pero preciosa & con niebla</p>
<p>Escapado: Ruta &lt;b&gt;dura&lt;/b&gt; pero preciosa &amp; con niebla</p>
```

En la primera línea, el `<b>` del dato se **coló como etiqueta real** y pondría el texto en negrita — un dato tomando el control del marcado. En la segunda, `htmlspecialchars` lo convirtió en `&lt;b&gt;`, que el navegador muestra como texto literal `<b>`, sin obedecerlo. **Regla de la casa, sin excepciones: todo dato de origen externo que se imprime en una página pasa por `htmlspecialchars`.** Cuando llegues a las sesiones y a la autenticación (UD3) esta disciplina será tu primera defensa; empieza a automatizarla ahora.

Cierra el apartado un patrón que evita repetir marcado: los **parciales**. La cabecera, el pie o el menú son idénticos en todas las páginas; en vez de copiarlos, se escriben una vez en un fichero y se **incluyen** con `include`:

```php
<?php declare(strict_types=1); ?>
<header>
    <strong>Club de Montaña Os Ibones</strong> · Temporada de otoño
</header>
```

```php
<?php declare(strict_types=1); ?>
<?php include __DIR__ . '/parciales/cabecera.php'; ?>
<main>
    <p>Consulta el calendario y apúntate a la próxima salida.</p>
</main>
```

Servida, la página compuesta llega así:

```html
<header>
    <strong>Club de Montaña Os Ibones</strong> · Temporada de otoño
</header>
<main>
    <p>Consulta el calendario y apúntate a la próxima salida.</p>
</main>
```

`include` **inserta y ejecuta** el fichero en ese punto, como si su contenido estuviera escrito ahí. `__DIR__` es la carpeta del fichero actual: usarlo hace la ruta robusta sin importar desde dónde se sirva la página. Con parciales, un cambio en la cabecera se hace **una vez** y se ve en todas partes — es la ventaja "una plantilla, mil páginas" de la UD1, ahora en tus manos. (El mismo `include` es, además, la semilla de la organización en capas de la UD5: cuando separes lógica y vista, las vistas serán ficheros que se incluyen.)

**Ejercicios del apartado.**

- **E25.** Reescribe la versión fea (`echo` + concatenación) del calendario como plantilla con sintaxis alternativa y `<?= ?>`, añadiendo un `<?php if (…): ?>` que muestre la palabra `COMPLETA` en lugar del número cuando `$libres` sea 0. Sírvela y pega el HTML recibido; comprueba que Peña Oroel sale como COMPLETA.
- **E26.** Demuestra el peligro y el remedio: crea una plantilla que imprima una variable `$busqueda = '<script>alert(1)</script>'` primero sin escapar y después con `htmlspecialchars`. Sírvela, mira el código fuente recibido en cada caso y explica en dos líneas qué habría pasado en el navegador con la versión sin escapar y por qué la escapada es segura.
- **E27.** Crea un parcial `parciales/pie.php` con el aviso de que los datos son de aula, e inclúyelo al final de `pagina.php` con `include`. Después provoca el error clásico: escribe mal la ruta (`parciales/pei.php`) y pega el mensaje que produce PHP. Explica qué parte del mensaje te dice exactamente qué fichero no encontró.

<div style="page-break-before: always;"></div>

## Apartado 9. Formularios y superglobales: GET y POST

Todo lo anterior fabricaba páginas **de salida**. Ahora invertimos el sentido: el usuario **envía** datos y el servidor los **recibe**. Es la mitad que faltaba —la de RA3.e y RA3.f— y la razón de ser de la generación dinámica que enunciaste en la UD1: "los formularios dejan de ser decorativos; el servidor recibe, valida y actúa".

Un formulario HTML declara dos cosas que deciden cómo viajan los datos: **`method`** (`get` o `post`) y **`action`** (a qué ruta se envían). El navegador empaqueta los campos —identificados por su atributo `name`— y los manda. En el servidor, PHP los deja servidos en dos **superglobales**: arrays especiales, visibles en cualquier ámbito sin pasarlos (son la excepción a la regla del apartado 6, por diseño), llamados **`$_GET`** y **`$_POST`**.

La diferencia entre los dos métodos no es de sintaxis, es de **naturaleza**, y elegir bien es criterio profesional:

| | GET | POST |
|---|---|---|
| Dónde viajan los datos | En la **URL**, tras el `?` (`?dificultad=alta`) | En el **cuerpo** de la petición, no en la URL |
| Se ven en la barra / se guardan en marcadores | Sí | No |
| Uso natural | **Pedir / filtrar / buscar** (la petición se puede repetir sin efectos) | **Enviar / crear / modificar** (cambia algo en el servidor) |
| Ejemplo del club | Filtrar el calendario por dificultad | Solicitar el alta en una salida |

Empecemos por GET, leyendo un filtro de la cadena de consulta. Fíjate en el `??`: un parámetro que no viene es una clave que no existe — exactamente el caso del apartado 5 —, así que le damos un valor por defecto en vez de arriesgar un aviso:

```php
<?php
declare(strict_types=1);
// get-demo.php — filtro por dificultad leído de la cadena de consulta
$filtro = $_GET['dificultad'] ?? 'todas';
echo "Filtro recibido: {$filtro}\n";
var_dump(isset($_GET['dificultad']));
```

Pídelo primero sin parámetros (`http://localhost:8000/get-demo.php`) y luego con uno (`?dificultad=alta`). Las dos salidas, letra a letra:

```
Filtro recibido: todas
bool(false)
```

```
Filtro recibido: alta
bool(true)
```

`isset()` te dice si una clave **existe y no es null** — la pregunta correcta antes de fiarte de un dato de entrada. La pareja `isset()` para comprobar y `??` para dar defecto es el ABC de recibir datos de la web.

POST es igual de sencillo de leer (`$_POST['campo']`), pero introduce un patrón nuevo y muy común: **el formulario se envía a sí mismo**. La misma página se pide primero con GET (y muestra el formulario vacío) y, al enviarlo, se pide con POST (y procesa lo recibido). ¿Cómo distingue el servidor un caso del otro? Preguntando por el **método de la petición**, que PHP deja en otra superglobal, `$_SERVER['REQUEST_METHOD']`:

```php
<?php
declare(strict_types=1);
// post-demo.php — el formulario se envía a sí mismo; distinguimos GET de POST
$metodo = $_SERVER['REQUEST_METHOD'];
$nombre = $_POST['nombre'] ?? '';
?>
<?php if ($metodo === 'POST'): ?>
    <p>Gracias, <?= htmlspecialchars($nombre) ?>. Solicitud recibida.</p>
<?php else: ?>
    <form action="" method="post">
        <label>Nombre: <input type="text" name="nombre"></label>
        <button type="submit">Enviar</button>
    </form>
<?php endif; ?>
```

Al pedirla con el navegador (GET), responde con el formulario:

```html
    <form action="" method="post">
        <label>Nombre: <input type="text" name="nombre"></label>
        <button type="submit">Enviar</button>
    </form>
```

Al rellenar el campo con `A17` y enviar (POST), la misma página responde:

```html
    <p>Gracias, A17. Solicitud recibida.</p>
```

Tres detalles profesionales de este patrón. El `action=""` significa "a mí mismo" —la misma URL—; es lo habitual en este esquema. El valor recibido se imprime con `htmlspecialchars` **siempre**, porque procede del usuario (apartado 8, regla sin excepciones). Y `$nombre = $_POST['nombre'] ?? ''` da por defecto la cadena vacía, de modo que la primera visita (sin POST) no falla al leer una clave que aún no existe.

Con esto ya recibes datos. Pero **recibir no es fiar**: todo lo que llega de `$_GET` y `$_POST` llega como **string** y viene de fuera, es decir, de territorio no fiable — recuerda la regla de oro de la UD1: *el cliente propone, el servidor dispone*. Validar ese material con autoridad, en el servidor, es tan importante que tiene apartado propio. Vamos a él.

**Ejercicios del apartado.**

- **E28.** Sirve `get-demo.php` y pruébalo con tres URLs: sin parámetro, con `?dificultad=alta` y con `?dificultad=`  (parámetro presente pero vacío). Pega las tres salidas y explica la diferencia entre el primer y el tercer caso: ¿por qué `isset` da distinto? ¿Qué valor toma `$filtro` en cada uno y por qué `??` no rescata el tercero?
- **E29.** Amplía `post-demo.php` para que el formulario tenga dos campos (`nombre` y `salida`) y, al recibir el POST, muestre una frase como `A17 solicita plaza en Ibones de Anayet`. Sírvelo, envíalo y pega el HTML recibido; comprueba que ambos valores pasan por `htmlspecialchars`.
- **E30.** Cambia el `method` del formulario de `post` a `get` y observa qué ocurre en la barra de direcciones del navegador al enviar. Explica, con la tabla del apartado, por qué para "solicitar una plaza" es incorrecto GET y correcto POST, y pon un ejemplo de la web del club donde GET **sí** sería la elección adecuada.

<div style="page-break-before: always;"></div>

## Apartado 10. Validación en el servidor, por autoridad

Este es el apartado donde la unidad cierra su círculo y donde se cumple la promesa que hiciste en la UD1. Recuerda la **regla de oro** de aquella unidad, la fila que te pedí grabar: *validar un formulario se hace en ambos lados — en el cliente por cortesía (aviso inmediato), en el servidor por autoridad (la única que cuenta)*. En la UD2 llegas por fin al lado que decide.

El principio, dicho sin rodeos: **nada de lo que llega del navegador es de fiar**. No porque el usuario sea malo, sino porque el canal es manipulable — lo demostraste en la UD1 (E8): con las herramientas de desarrollo se edita el HTML, se desactiva el JavaScript o se reenvía la petición `POST` con los datos que uno quiera. La validación de cliente mejora la experiencia; **no protege nada**. La que protege es la del servidor, porque su código no viaja y su decisión es inmanipulable.

Y hay un motivo técnico que lo refuerza, ya conocido: **todo lo que llega en `$_GET` y `$_POST` llega como `string`**. La edad "25" es la cadena `'25'`, no el número 25. Por eso `declare(strict_types=1)` (apartado 3) es aquí obligatorio: obliga a **convertir conscientemente** en la frontera, en vez de dejar que una cadena disfrazada de número se cuele en los cálculos.

La herramienta central es **`filter_var()`**, que valida un valor contra un filtro y devuelve el valor convertido si es válido o **`false`** si no lo es. Míralo aislado antes de integrarlo:

```php
<?php
declare(strict_types=1);
// filtros.php — validar tipos de dato que llegan como string
var_dump(filter_var('25', FILTER_VALIDATE_INT));
var_dump(filter_var('doce', FILTER_VALIDATE_INT));
var_dump(filter_var('0', FILTER_VALIDATE_INT));
var_dump(filter_var('socio@club.example', FILTER_VALIDATE_EMAIL));
var_dump(filter_var('no-es-correo', FILTER_VALIDATE_EMAIL));
```

```
int(25)
bool(false)
int(0)
string(18) "socio@club.example"
bool(false)
```

Dos lecturas imprescindibles de esa salida. La tercera línea, `int(0)`: la cadena `'0'` **es** un entero válido, y `filter_var` lo devuelve como `0` — que es un valor "falso" (apartado 4). Por eso **nunca** se comprueba el resultado con `if (!$edad)` (el `0` legítimo caería como si fuera error), sino con `if ($edad === false)`: comparación estricta contra el `false` exacto que señala el fallo. La última línea, `bool(false)` para el correo inválido: por defecto, un filtro que no valida devuelve `false`. (Solo si se pasa la opción `FILTER_NULL_ON_FAILURE` devuelve `null` en su lugar — un matiz para cuando lo necesites, con el manual delante.) La comprobación correcta es siempre `=== false`, por la razón de la tercera línea: un `0` legítimo es un valor "falso" y `if (!$edad)` lo confundiría con un error.

Ahora el ejemplo integrador de la unidad, que junta casi todo lo aprendido: una función que valida una **solicitud de alta** del club contra el **catálogo real** —que vive en el servidor, no en el formulario— y devuelve la lista de errores. Es el patrón profesional: recoger todos los problemas y devolverlos juntos, no rendirse al primero.

```php
<?php
declare(strict_types=1);
// alta.php — solicitud de plaza en una salida, VALIDADA en el servidor

// El catálogo real vive en el servidor: el cliente no decide qué salidas existen.
$salidas = [
    'estanes' => ['nombre' => 'Ibón de Estanés', 'plazas' => 20, 'inscritos' => 17],
    'oroel'   => ['nombre' => 'Peña Oroel',      'plazas' => 25, 'inscritos' => 25],
    'anayet'  => ['nombre' => 'Ibones de Anayet','plazas' => 12, 'inscritos' => 12],
];

/**
 * Valida los datos de una solicitud de alta contra el catálogo real.
 * Devuelve la lista de errores (vacía si todo es correcto).
 */
function validar_alta(array $datos, array $salidas): array {
    $errores = [];

    $nombre = trim($datos['nombre'] ?? '');
    if ($nombre === '') {
        $errores[] = 'El nombre es obligatorio.';
    }

    $edad = filter_var($datos['edad'] ?? '', FILTER_VALIDATE_INT);
    if ($edad === false) {
        $errores[] = 'La edad debe ser un número entero.';
    } elseif ($edad < 14) {
        $errores[] = 'Debes tener al menos 14 años para inscribirte.';
    }

    $clave = $datos['salida'] ?? '';
    if (!array_key_exists($clave, $salidas)) {
        $errores[] = 'La salida seleccionada no existe.';
    } elseif ($salidas[$clave]['inscritos'] >= $salidas[$clave]['plazas']) {
        $errores[] = 'Esa salida está completa.';
    }

    return $errores;
}

// Simulamos tres envíos POST (en la web llegarían en $_POST).
$envios = [
    ['nombre' => 'A17', 'edad' => '25', 'salida' => 'estanes'],  // válido
    ['nombre' => '',    'edad' => 'doce', 'salida' => 'oroel'],  // 3 errores
    ['nombre' => 'A08', 'edad' => '30', 'salida' => 'marte'],    // salida inexistente
];

foreach ($envios as $i => $datos) {
    $errores = validar_alta($datos, $salidas);
    echo 'Envío ', $i + 1, ': ';
    if ($errores === []) {
        echo 'ACEPTADO — ', $salidas[$datos['salida']]['nombre'], "\n";
    } else {
        echo 'RECHAZADO', "\n";
        foreach ($errores as $e) {
            echo '  - ', $e, "\n";
        }
    }
}
```

```
Envío 1: ACEPTADO — Ibón de Estanés
Envío 2: RECHAZADO
  - El nombre es obligatorio.
  - La edad debe ser un número entero.
  - Esa salida está completa.
Envío 3: RECHAZADO
  - La salida seleccionada no existe.
```

Recorre la salida y verás la autoridad del servidor en acción. El **envío 2** acumula **tres** errores: nombre vacío (`trim` lo caza aunque llegaran espacios), `'doce'` que no es entero, y —el más instructivo— *"Esa salida está completa"*: aunque el formulario ofreciera Peña Oroel, el servidor consulta su **catálogo real** (25 de 25) y la deniega. El **envío 3** pide la salida `'marte'`, que sencillamente **no existe** en el catálogo: el cliente puede mandar lo que quiera en ese campo —editando el HTML del desplegable, por ejemplo—, pero el servidor solo acepta lo que él conoce. Esto es exactamente el ejemplo de las plazas de la UD1, ahora programado: *el cliente propone, el servidor dispone*.

**Regla profesional de esta unidad, que llevarás a todas las demás**: recibe → valida en el servidor contra los datos reales → si hay errores, recházalos y vuelve a mostrar el formulario con los avisos; si no, actúa. La validación de cliente puede acompañar para dar comodidad, pero la del servidor es la que manda. En la UD3 esta disciplina será la base de la autenticación (¿quién eres de verdad?); en la UD6, la puerta a la base de datos. Empieza aquí y no la sueltes.

**Ejercicios del apartado.**

- **E31.** Demuestra la autoridad del servidor con una petición real. Sirve una versión de `alta.php` que lea de `$_POST` y envíale, con las herramientas de desarrollo del navegador o con `curl`, un POST con `edad=10` (un valor que una validación de cliente habría bloqueado). Comprueba que el servidor lo rechaza igualmente y pega la respuesta. Explica en dos líneas por qué "saltarse el JavaScript" no sirve de nada aquí.
- **E32.** Añade a `validar_alta` una regla más: el nombre no puede superar los 60 caracteres (usa `mb_strlen`). Prueba con un nombre larguísimo y con uno normal, y comprueba que el error aparece solo cuando debe. Explica por qué usas `mb_strlen` y no `strlen` (recuerda el `string(11)` del apartado 2).
- **E33.** El envío 2 devuelve tres errores a la vez. Reescribe el bucle de comprobación para que **se detenga en el primer error** y compara: ¿qué experiencia de usuario da cada versión (todos los errores de golpe vs. de uno en uno)? Argumenta cuál preferirías para el formulario de alta del club y por qué la acumulación suele ser mejor en formularios.

<div style="page-break-before: always;"></div>

## Apartado 11. Errores frecuentes

1. **Punto y coma olvidado.** El error número uno de la unidad. El intérprete no lo señala en la línea que te falta, sino en la **siguiente**, porque hasta ahí no se da cuenta de que algo va mal:

```
PHP Parse error:  syntax error, unexpected token "echo", expecting "," or ";" in pyc.php on line 3
```

   Traducción: "en la línea 3 me encuentro un `echo` cuando esperaba una coma o un punto y coma" — el `;` que falta es el de la **línea 2**. Por eso `php -l` es tu primer reflejo: te da el número de línea antes de ejecutar nada.

2. **Confundir `=` con `==` (o con `===`).** `$x = 5` **asigna**; `$x == 5` **compara**. Escribir `if ($libres = 0)` no comprueba si quedan cero plazas: **pone** `$libres` a 0 y evalúa como falso siempre. La regla de la casa (comparar con `===`) no elimina este error, pero acostumbrarse a leer "triple igual = comparación" ayuda a detectarlo.

3. **Clave de array inexistente.** Leer `$s['plazas']` cuando la clave no existe produce, en modo desarrollo, un aviso — y `null` como valor, que luego causa cálculos raros lejos de aquí:

```
PHP Warning:  Undefined array key "plazas" in /ruta/uak.php on line 3
```

   El remedio ya lo tienes: `$s['plazas'] ?? valor_por_defecto`. Úsalo siempre que la clave pueda no estar — y con datos de formulario, **siempre** puede no estar.

4. **`==` con datos de formulario.** Como todo llega como string, `$_POST['acepta'] == 0` da resultados sorprendentes por la conversión de tipos (apartado 2): `'0' == false` es `true`, pero `'0' === false` es `false`. Compara con `===` y valida con `filter_var`; no dejes que la conversión decida por ti.

5. **Olvidar `htmlspecialchars` al imprimir datos del usuario.** Funciona en tus pruebas (tus datos son inofensivos) y falla el día que un dato trae `<`, `>` o `&` — rompiendo la página o inyectando contenido. La disciplina del apartado 8 no es opcional: todo dato externo que se imprime, escapado.

6. **`Failed to listen on localhost:8000 (Address already in use)`** al lanzar `php -S`: lo heredas de la UD1. Otro proceso —casi siempre tu ejecución anterior sin cerrar— sigue en el puerto. Vuelve a esa terminal y párala con Ctrl+C antes de relanzar.

<div style="page-break-before: always;"></div>

## Apartado 12. La IA en esta unidad

Puedes usar un asistente de IA en esta unidad, con cabeza y con responsabilidad sobre el resultado — la misma regla del módulo: **el uso se declara en el `DECISIONES.md` de cada entrega**, y lo que entregas lo defiendes.

- **Usos razonables aquí**: pedir una explicación alternativa de un concepto que se resista (el ámbito de variables, la diferencia entre `==` y `===`, por qué `match` es exhaustivo); generar más casos de prueba para una función tuya; pedir que te señale por qué un fragmento no compila cuando `php -l` te da un mensaje que no entiendes; o que te proponga ejercicios de repaso antes de la defensa.
- **Lo que debes verificar siempre**: **ejecuta y comprueba con tus dos entornos** todo código que te dé un asistente — la IA se equivoca con soltura en detalles de PHP (confunde `==` con `===`, propone la sintaxis corta `<?` que no usamos, arrastra la comparación `0 == 'hola'` obsoleta del apartado 2, olvida `htmlspecialchars`). Contrasta cualquier afirmación sobre el lenguaje con el **manual oficial** (apartado 14), no con lo que "recuerde" el asistente: la memoria de un modelo mezcla versiones y foros viejos. Y desconfía especialmente de código de validación que te den hecho: un `filter_var` mal comprobado con `if (!$x)` en vez de `=== false` es un bug clásico que la IA reproduce.
- **Lo que se te pedirá defender**: la actividad evaluativa se defiende. En particular, tu validación en servidor de la AE10 la sostendrás explicando **por qué** cada regla vive en el servidor y no en el cliente, y respondiendo a un "¿y si el usuario envía…?". Si el código lo escribió un asistente y no entiendes por qué la decisión de plaza no puede tomarse en el navegador, la defensa lo revela en el primer minuto. Si está en tu entrega, es tuyo — y lo has ejecutado.

<div style="page-break-before: always;"></div>

## Apartado 13. Actividad evaluativa final

**Contexto — la protectora comarcal de animales.** La *Protectora Comarcal Los Galachos* (asociación ficticia de aula) gestiona hoy su día a día con un cuaderno y un grupo de mensajería: un voluntario lleva a mano una lista de los animales acogidos —perros y gatos, con su edad y su estado: disponible, reservado o adoptado— y publica las novedades como puede. Quieren una **web sencilla** que muestre siempre la lista al día desde una única fuente, deje **filtrar** por especie o estado, y permita a quien esté interesado **enviar una solicitud de acogida** que la protectora reciba y revise. No hay presupuesto para nada complejo; quien mantendrá el sitio está aprendiendo PHP —eres tú— y por ahora los datos viven en un array en el servidor (la base de datos llegará en su momento). Los datos son **ficticios**: trabaja siempre con alias.

Este es el fichero de datos con el que trabajarás (muestra de cinco; el conjunto completo está en el repositorio de la unidad):

```php
<?php
declare(strict_types=1);
$animales = [
    ['nombre' => 'Truco',  'especie' => 'perro', 'edad' => 4, 'estado' => 'disponible'],
    ['nombre' => 'Nieve',  'especie' => 'gato',  'edad' => 2, 'estado' => 'reservado'],
    ['nombre' => 'Canela', 'especie' => 'perro', 'edad' => 7, 'estado' => 'disponible'],
    ['nombre' => 'Lúa',    'especie' => 'gato',  'edad' => 1, 'estado' => 'adoptado'],
    ['nombre' => 'Rocco',  'especie' => 'perro', 'edad' => 5, 'estado' => 'disponible'],
];
```

**Instrucciones.** 10 ejercicios, 1 punto cada uno; se responde **con código que se ejecuta** (los fragmentos que no pasen `php -l` no puntúan) y, donde se pida, razonando la decisión. **Tiempo estimado: 3 horas**, más la preparación de evidencias. **Entrega**: carpeta `ud2/` en tu repositorio de la unidad del aula de código, con un fichero por ejercicio (`ae1.php`, `ae2.php`…), las capturas de las páginas servidas en `ud2/evidencias/`, commits por bloques de ejercicios y el `DECISIONES.md` actualizado (incluida la declaración de uso de IA). **Defensa individual de 4–5 minutos** según el calendario publicado: sin defensa, la actividad no puntúa.

- **AE1** `[RA2.a, RA2.b]` La web actual de la protectora es un mensaje que un voluntario reescribe a mano. Explica, con el vocabulario de la unidad, **qué mecanismo** hará que cada visita vea la lista al día sin que nadie reescriba nada (código embebido ejecutado en el servidor a cada petición) y nombra **dos tecnologías asociadas** que intervienen (el intérprete y su servidor de desarrollo, el navegador…), diciendo qué aporta cada una. Sin código: es la justificación del enfoque.
- **AE2** `[RA2.c, RA2.e, RA3.g]` Escribe `ae2.php`: una página que, con las **etiquetas** de inclusión (`<?php ?>` y `<?= ?>`), imprima un titular con el nombre de la protectora y una sentencia simple que muestre cuántos animales hay acogidos (`count`). Comenta el código con un comentario PHP (que no viaje) y uno HTML (que sí). Sírvela, captura el código fuente recibido y señala qué comentario llegó y cuál no.
- **AE3** `[RA2.d, RA2.g]` Predice y verifica. Dado este fragmento, escribe **primero** la salida que esperas y **después** la real, explicando cada diferencia con el concepto que la causa:

```php
<?php
$edad = '7';
var_dump($edad == 7);
var_dump($edad === 7);
echo 'Ficha: ' . 'Canela' . ' (' . $edad . ' años)', "\n";
echo "Ficha: Canela ($edad años)\n";
```

- **AE4** `[RA2.f]` Escribe una función `precio_donativo(int $unidades, float $precio): float` y llámala pasándole `'2'` y `'5.0'` (strings). Ejecuta el fichero **sin** `declare(strict_types=1)` y **con** la directiva en cabecera; pega las dos salidas y explica, con las cuatro informaciones del `TypeError`, qué **comportamiento predeterminado** cambió la directiva y por qué te conviene en una web que recibe todo como string.
- **AE5** `[RA3.a]` Escribe `ae5.php` con una función `mensaje_estado(string $estado): string` que, con un **`match`**, traduzca cada estado a una frase para la web (`'disponible'` → `'¡Busca hogar!'`, `'reservado'` → `'Reserva en trámite'`, `'adoptado'` → `'¡Ya tiene familia!'`), con un `default` para estados desconocidos. Recorre la muestra imprimiendo `nombre — mensaje`. Después provoca un `UnhandledMatchError` quitando el `default` y pasando un estado nuevo, y explica qué te protege esa exhaustividad.
- **AE6** `[RA3.b, RA3.c]` Sobre el array de animales, con **bucles**: (a) imprime la lista completa como `nombre (especie, edad años) — estado`; (b) cuenta cuántos hay **por estado** (array asociativo de recuento) e imprime el recuento; (c) calcula e imprime la **edad media** de los animales disponibles. Verifica los números a mano con la muestra.
- **AE7** `[RA3.d, RA2.h]` Escribe dos funciones tipadas y reutilizables: `disponibles(array $animales): array` (devuelve solo los disponibles) y `mas_joven(array $animales): array` (devuelve el animal de menor edad). Demuestra además el **ámbito**: crea una variable con el mismo nombre dentro y fuera de una de tus funciones y prueba, ejecutando, que modificar la de dentro no toca la de fuera. Explícalo en una línea con los términos "local" y "ámbito".
- **AE8** `[RA2.d, RA2.g, RA3.c]` Define una clase `Animal` (tipo compuesto) con propiedades `nombre`, `especie`, `edad`, `estado` (usa promoción de propiedades en el constructor) y un método `estaDisponible(): bool`. Convierte la muestra en un **array de objetos `Animal`** y, con un `foreach`, imprime solo los disponibles usando tu método. Explica en dos líneas qué te da el objeto que el array asociativo no.
- **AE9** `[RA3.e, RA3.f]` Construye `ae9.php` con dos entradas de usuario: (a) un **filtro GET** por especie (`?especie=perro`) que muestre solo esos animales, con `??` para el caso sin parámetro; (b) un **formulario POST** de solicitud de acogida (campos `solicitante` y `animal`) que, al enviarse a sí mismo, muestre un acuse `X solicita acoger a Y`. Sirve la página, prueba ambos caminos y captura las respuestas. Imprime **todo** dato del usuario con `htmlspecialchars`.
- **AE10** `[RA3.e, RA3.f, RA2.e]` **Validación en el servidor, por autoridad.** Completa `ae9.php` con una función `validar_solicitud(array $datos, array $animales): array` que, contra el **catálogo real**, devuelva la lista de errores: solicitante obligatorio y de longitud razonable (`mb_strlen`); el animal debe **existir** en el catálogo y estar en estado **`disponible`** (no se puede solicitar uno ya adoptado). Muestra los errores o el acuse según corresponda. **Evidencia de autoridad**: envía —con `curl` o las herramientas de desarrollo— un POST que solicite un animal `adoptado` o inexistente, saltándote cualquier control de cliente, y captura el rechazo del servidor. En la defensa explicarás **por qué** esa decisión no puede tomarse en el navegador. Se defiende sin leer.

<div style="page-break-before: always;"></div>

## Apartado 14. Para ampliar

- [La sintaxis básica (Manual de PHP, en español)](https://www.php.net/manual/es/language.basic-syntax.php) — etiquetas de apertura y cierre, escape desde HTML, separación de instrucciones y comentarios: la referencia oficial de todo el apartado 1, con los ejemplos canónicos de `<?php ?>` y `<?= ?>`.
- [Los tipos (Manual de PHP, en español)](https://www.php.net/manual/es/language.types.php) — escalares, `null`, arrays y la manipulación de tipos ("type juggling"): la fuente para entender a fondo por qué `==` y `===` se comportan como viste en los apartados 2 y 4, y por qué los decimales necesitan cuidado con el dinero.
- [Estructuras de control (Manual de PHP, en español)](https://www.php.net/manual/es/language.control-structures.php) — `if`, `match`, los bucles y, muy útil para esta unidad, la **sintaxis alternativa** (`foreach: … endforeach;`) que usaste en las plantillas del apartado 8.
- [Variables desde fuentes externas (Manual de PHP, en español)](https://www.php.net/manual/es/language.variables.external.php) — cómo llegan los datos de un formulario a `$_GET` y `$_POST`, con los ejemplos oficiales de acceso: el complemento directo de los apartados 9 y 10. (Las páginas aún sin traducir se muestran en inglés.)
