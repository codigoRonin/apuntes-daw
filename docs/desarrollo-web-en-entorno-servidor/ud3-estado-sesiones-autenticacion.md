# UD3. Estado, cookies, sesiones y autenticación; seguridad (usuarios, perfiles, roles)

**Módulo 0613 — Desarrollo Web en Entorno Servidor · 2.º DAW · IES Río Arba · Curso 2026-27 · 28 horas · 1.ª evaluación**

**Vinculación con los resultados de aprendizaje.** Esta unidad trabaja completo el **RA4** (RD 686/2010 en la redacción dada por el RD 405/2023): *"Desarrolla aplicaciones web embebidas en lenguajes de marcas analizando e incorporando funcionalidades según especificaciones"*. El verbo vuelve a ser de construir — **desarrolla** —, y la funcionalidad que incorporas es la que convierte un conjunto de páginas en una **aplicación**: memoria entre peticiones y usuarios que se identifican. El RA4 es, además, uno de los tres RA **dualizados** del módulo: un 5 % de su calificación se adquiere en la formación en empresa de marzo a mayo, y por eso esta unidad se imparte y se cierra entera antes de esa fecha.

| CE | Qué exige (resumen) | Apartado donde se trabaja |
|---|---|---|
| RA4.a | Identificar los mecanismos disponibles para mantener la información que concierne a un cliente web concreto y señalar sus ventajas | Apartados 1, 2 y 3 |
| RA4.b | Utilizar mecanismos para mantener el estado de las aplicaciones web | Apartados 3, 4, 6 y 7 |
| RA4.c | Utilizar mecanismos para almacenar información en el cliente web y recuperar su contenido | Apartados 2 y 8 |
| RA4.d | Identificar y caracterizar los mecanismos disponibles para la autentificación de usuarios | Apartados 5 y 6 |
| RA4.e | Escribir aplicaciones que integren mecanismos de autentificación de usuarios | Apartados 6, 7, 8 y 9 |
| RA4.f | Utilizar herramientas y entornos para facilitar la programación, prueba y depuración del código | Apartado 10 (y las herramientas de toda la unidad) |

**Al terminar esta unidad sabrás:** por qué un servidor web no recuerda nada entre dos peticiones y qué mecanismos existen para que parezca que sí — con las ventajas y los límites de cada uno —; guardar y recuperar información en el navegador con **cookies**, decidiendo qué merece viajar en ellas y qué no; mantener el estado de una aplicación con **sesiones** que viven en el servidor; redirigir correctamente tras un formulario y mostrar mensajes de un solo uso; distinguir los mecanismos de **autentificación** y elegir el adecuado; construir el **registro**, el **inicio de sesión** y el **cierre de sesión** de una aplicación real con contraseñas que nunca se guardan en claro; separar *quién eres* de *qué puedes hacer* con **perfiles y roles**; proteger la sesión y los formularios contra los ataques más comunes; leer los usuarios desde una base de datos con una consulta preparada; y **probar y depurar** todo lo anterior con las herramientas de un desarrollador de servidor.

**Entorno de trabajo de la unidad.** Sigues con **PHP instalado**, el comprobador de sintaxis (`php -l fichero.php`) como primer reflejo y el servidor embebido (`php -S localhost:8000`) para servir. Se suman dos herramientas que ya conociste y que aquí se vuelven imprescindibles: la **pestaña Red** del navegador (para leer las cabeceras que viajan en cada petición y respuesta) y **`curl`** en el terminal, con dos opciones nuevas —`-c` para guardar cookies en un fichero y `-b` para reenviarlas— que te permiten reproducir cualquier flujo sin navegador. Trabaja dentro de la carpeta `ud3/` de tu repositorio de la unidad en el aula de código, con tu **alias** en todas las herramientas externas, como fija la política de protección de datos del centro. El caso hilo sigue siendo la **web del Club de Montaña Os Ibones** (ficticio): en la UD2 le diste un calendario y un formulario de alta; en esta le das **socios que entran con su usuario**, **monitores** que ven sus salidas y una **administración** que gestiona a todos. El calendario `datos/salidas.php` de la UD2 se reutiliza tal cual; el fichero nuevo es **`datos/usuarios.php`**, íntegro en el repositorio de la unidad (aquí, una muestra de cinco). Tres fronteras de alcance: la **base de datos** aparece en el apartado 9 solo como **herramienta** para leer usuarios — el acceso a datos se evalúa en la UD6 (RA6), no aquí —; el almacenamiento que hace **JavaScript en el navegador** (`localStorage`) se nombra para situarlo, pero es territorio del módulo de cliente (0612); y las medidas de seguridad son las que el RA4 exige para una aplicación con usuarios, no un curso de seguridad — lo que queda fuera se dice dónde vive.

**Muestra del fichero de usuarios** (cabecera y cinco filas; «fichero íntegro en el repositorio de la unidad»; las contraseñas de aula, en su README):

```php
<?php
// datos/usuarios.php — socios, monitores y administración del Club de Montaña Os Ibones (datos de aula, ficticios)
// Este fichero DEVUELVE el array: cárgalo con  $usuarios = require __DIR__ . '/datos/usuarios.php';
// Las contraseñas NO están aquí: solo su hash (password_hash). Contraseñas de aula: ver README del repositorio.
return [
    1 => ['usuario' => 'lucia.ramos', 'nombre' => 'Lucía Ramos', 'correo' => 'lucia.ramos@example.org', 'rol' => 'socio',
          'hash' => '$2y$10$pl.c1VhKXo9QiPWHKOUH1OeIaq3s.UCkJ5zRED/O7d5uqkLA4OXv.'],
    2 => ['usuario' => 'jorge.aisa', 'nombre' => 'Jorge Aísa', 'correo' => 'jorge.aisa@example.org', 'rol' => 'socio',
          'hash' => '$2y$10$impfV8UY664moovFCu5xn.igHl1D/OCo7byCOB0i41XDSlKWlkzvm'],
    3 => ['usuario' => 'marta.gil', 'nombre' => 'Marta Gil', 'correo' => 'marta.gil@example.org', 'rol' => 'socio',
          'hash' => '$2y$10$WtvhPcn8HremLB/B5bwu8OBr7uU/19HoWuL5hW18IhvmIAf.CR0Lq'],
    4 => ['usuario' => 'pablo.lobera', 'nombre' => 'Pablo Lobera', 'correo' => 'pablo.lobera@example.org', 'rol' => 'socio',
          'hash' => '$2y$10$bui2NXhvYeZKGX2Vbxs59.PSKhYCDVEzBrcD858zhi1vK.Jx9ajbW'],
    5 => ['usuario' => 'nuria.sanz', 'nombre' => 'Nuria Sanz', 'correo' => 'nuria.sanz@example.org', 'rol' => 'socio',
          'hash' => '$2y$10$fNYUjW1KLmygJOlvwwobMugaVjQc4BEy2DB5OppjnAjEsiv4BPVEu'],
    // … (tres filas más en el fichero íntegro: dos monitores y la administración, hasta 8)
];
```

<div style="page-break-before: always;"></div>

## Apartado 1. HTTP no tiene memoria: el problema del estado

Empieza por un experimento que te va a desconcertar un poco. Este fichero pretende contar tus visitas:

```php
<?php
declare(strict_types=1);
// contador-sin-memoria.php — ¿cuántas veces me has visitado? (spoiler: el servidor no lo sabe)
$visitas = 0;
$visitas = $visitas + 1;
echo "Visitas: {$visitas}\n";
```

Sírvelo, pídelo varias veces desde el navegador (o con `curl`, que es lo que hicimos) y mira la salida de dos peticiones seguidas, letra a letra:

```
$ curl http://localhost:8000/contador-sin-memoria.php
Visitas: 1
$ curl http://localhost:8000/contador-sin-memoria.php
Visitas: 1
```

Siempre uno. No es un error del código: es la **naturaleza de HTTP**. Recuerda el viaje de una petición de la UD1: el navegador abre una conexión, envía una petición, el servidor ejecuta tu fichero **desde cero**, devuelve la respuesta y **olvida todo**. Cada petición es una vida entera del programa: nace, ejecuta, muere. La variable `$visitas` no sobrevive a la respuesta, y el servidor no tiene forma de saber que la segunda petición viene del mismo navegador que la primera. HTTP es un protocolo **sin estado** (*stateless*), y esa es una decisión de diseño con ventajas enormes — cualquier servidor puede atender cualquier petición, sin coordinar recuerdos con nadie — y con una consecuencia que ahora es tu problema: si quieres que la aplicación *recuerde* algo de un visitante concreto, tienes que construir ese recuerdo tú.

Piensa qué significa «un cliente web concreto» para el servidor. No tiene su nombre, no tiene su cara; tiene, como mucho, una dirección IP que comparten decenas de personas detrás de la misma conexión y que cambia al pasar del wifi al móvil. Para reconocer a alguien entre dos peticiones, el servidor necesita que **el propio cliente traiga una señal** que lo identifique — y necesita **guardar en algún sitio** lo que quiere recordar de él. Ese es el problema entero, y los mecanismos que lo resuelven se diferencian exactamente en esas dos preguntas: *dónde viaja la señal* y *dónde vive el dato*.

| Mecanismo | Dónde viaja / vive el dato | Quién lo controla | Ventajas | Límites |
|---|---|---|---|---|
| **Parámetros en la URL** (`?dificultad=alta`) | En la propia dirección, petición a petición | El cliente (los escribe, los edita, los comparte) | Sin instalar nada; se puede guardar en marcadores y enviar; ideal para *filtrar* y *buscar* | Visible y editable; hay que arrastrarlo en cada enlace; no sirve para nada privado |
| **Campos ocultos** de formulario (`<input type="hidden">`) | En el cuerpo de cada envío POST | El cliente (los ve en el código fuente y los cambia) | Arrastra datos entre pasos de un formulario largo | Solo dura mientras se encadenan envíos; tan manipulable como la URL |
| **Cookies** | Pequeño texto que el **servidor pide guardar** al navegador y que este **reenvía en cada petición** al mismo sitio | El cliente lo almacena y lo puede editar; el servidor decide qué manda y cuánto dura | Automático: viaja solo; sobrevive al cierre del navegador si se le da caducidad; es el mecanismo estándar para *reconocer* a un navegador | Límite de tamaño; viaja en cada petición; el usuario lo ve y lo edita, así que **nunca datos sensibles ni decisiones** |
| **Sesión** | El dato vive **en el servidor**; en el navegador solo viaja una **llave** (una cookie con un identificador) | El servidor, por completo | El cliente no puede leer ni alterar el contenido; capacidad grande; es el mecanismo para mantener el **estado** de una aplicación (quién está dentro, qué lleva a medias) | Ocupa memoria o disco en el servidor; muere con la llave; hay que protegerla (apartado 8) |
| **Almacenamiento del navegador** (`localStorage`, `sessionStorage`) | Vive en el navegador y **no viaja** en las peticiones; lo lee y escribe **JavaScript** | El cliente | Capacidad mayor que una cookie; no carga cada petición; útil para preferencias de interfaz | El servidor no lo ve (solo si JavaScript se lo envía); es cosa del módulo de cliente (0612) |

Fíjate en el patrón que ordena la tabla: cuanto más abajo, más control del servidor y menos exposición al cliente. Y fíjate en la excepción: el almacenamiento del navegador es potente, pero **invisible para el servidor** — no resuelve el problema del estado en esta unidad, porque el servidor no puede leerlo; solo puede recibir lo que un script decida enviarle, que es otra petición más. Quedan, para construir memoria desde el servidor, dos mecanismos: **cookies** (apartado 2) y **sesiones** (apartado 3). Los usarás a la vez y con papeles distintos, y la regla que decide cuál toca la vas a oír muchas veces: *lo que el usuario pueda ver y tocar sin que pase nada, en cookie; todo lo demás, en sesión*.

Una última observación antes de seguir, porque volverá en el apartado 5: la tabla habla de *dónde vive* el dato, pero no de *quién es* el usuario. Reconocer un navegador (esta cookie es la misma que ayer) no es lo mismo que saber que quien lo maneja es Lucía. Lo primero es **estado**; lo segundo es **autentificación**. La unidad va de ambas cosas, en ese orden.

**Ejercicios del apartado.**

- **E1.** Sirve `contador-sin-memoria.php` y pídelo tres veces con `curl -i` (con cabeceras). Comprueba que la respuesta no trae ninguna cabecera `Set-Cookie` y explica, con el vocabulario de la UD1 (petición, respuesta, conexión), por qué el servidor no puede saber que las tres peticiones son tuyas.
- **E2.** Para cada necesidad de la web del club, elige un mecanismo de la tabla y justifícalo en una línea con una ventaja y un límite: (a) recordar qué dificultad prefiere filtrar el visitante; (b) recordar que el socio ya ha iniciado sesión; (c) compartir con un amigo el calendario filtrado por «alta»; (d) llevar los datos de un formulario de alta que ocupa tres pantallas.
- **E3.** Abre las herramientas de desarrollo de tu navegador en cualquier web que uses (correo, aula virtual…), entra en el panel de almacenamiento (en algunos navegadores se llama *Aplicación*) y mira sus cookies. Sin copiar valores (son datos privados), anota cuántas hay, cuáles tienen fecha de caducidad y cuáles son «de sesión». ¿Qué mecanismo de la tabla crees que usa esa web para saber que estás dentro? Argumenta con lo que ves.

<div style="page-break-before: always;"></div>

## Apartado 2. Cookies: la memoria que vive en el navegador

Una **cookie** es un pequeño texto con nombre que el servidor pide al navegador que guarde, y que el navegador **reenvía automáticamente** en cada petición posterior al mismo sitio. Todo el mecanismo cabe en dos cabeceras HTTP: el servidor la crea con **`Set-Cookie`** en una respuesta, y el navegador la devuelve con **`Cookie`** en las peticiones siguientes. Nada más: sin magia, solo cabeceras que vas a ver con tus ojos.

En PHP la escribes con **`setcookie()`** y la lees en la superglobal **`$_COOKIE`** — otra hermana de `$_GET` y `$_POST`, con la misma regla: *lo que hay dentro lo escribió el cliente*. El caso del club: recordar la dificultad que el visitante prefiere ver en el calendario.

```php
<?php
declare(strict_types=1);
// preferencia.php — recordar la dificultad preferida del visitante con una cookie
$validas = ['baja', 'media', 'alta'];

if (isset($_GET['borrar'])) {
    // Borrar = enviar la misma cookie con caducidad en el pasado
    setcookie('dificultad', '', ['expires' => time() - 3600, 'path' => '/']);
    echo "Preferencia borrada.\n";
} elseif (isset($_GET['dificultad']) && in_array($_GET['dificultad'], $validas, true)) {
    // Guardar = pedir al navegador que la conserve 30 días
    setcookie('dificultad', $_GET['dificultad'], [
        'expires'  => time() + 30 * 24 * 60 * 60,
        'path'     => '/',
        'httponly' => true,
        'samesite' => 'Lax',
    ]);
    echo "Preferencia guardada: {$_GET['dificultad']}\n";
}

// Leer = mirar lo que el navegador nos ha enviado en ESTA petición
$preferida = $_COOKIE['dificultad'] ?? 'sin preferencia';
echo "Cookie recibida en esta petición: {$preferida}\n";
```

Sírvelo y pide `preferencia.php?dificultad=alta` con `curl -i`, para ver las cabeceras de la respuesta:

```
$ curl -i "http://localhost:8000/preferencia.php?dificultad=alta"
HTTP/1.1 200 OK
Host: localhost:8000
Date: Fri, 18 Sep 2026 06:35:43 GMT
Connection: close
Set-Cookie: dificultad=alta; expires=Sun, 18 Oct 2026 06:35:43 GMT; Max-Age=2592000; path=/; HttpOnly; SameSite=Lax
Content-type: text/html; charset=UTF-8

Preferencia guardada: alta
Cookie recibida en esta petición: sin preferencia
```

Dos cosas que aprender de esa salida. La primera es la cabecera **`Set-Cookie`** completa, con todo lo que pediste en el array de opciones traducido al protocolo: el nombre y el valor, la caducidad en dos formatos (`expires` como fecha y `Max-Age` en segundos: 2592000 son exactamente los 30 días), la ruta `path=/` (la cookie vale para todo el sitio), y dos atributos de seguridad que explicaremos ahora. La segunda es la última línea, y es la que más gente confunde: **la cookie recién creada no está en `$_COOKIE` en esta misma petición**. `setcookie()` solo añade una cabecera a la respuesta; el navegador la recibirá *después* de que tu código haya terminado, y solo la enviará de vuelta **en la siguiente petición**. Por eso, en el momento de leer, `$_COOKIE['dificultad']` aún no existe. Es el error 2 del apartado 11, y lo cometerás una vez; la segunda ya no.

Ahora la siguiente petición. Con `curl` reenviamos la cookie a mano (`-b`), que es justo lo que el navegador hace solo:

```
$ curl -b "dificultad=alta" http://localhost:8000/preferencia.php
Cookie recibida en esta petición: alta
```

Ahí está la memoria: sin parámetro en la URL, el servidor sabe qué prefieres, porque el navegador se lo dijo con la cabecera `Cookie`. Para no escribirla a mano, `curl` tiene un **tarro de cookies** (*cookie jar*): con `-c fichero` guarda las que reciba y con `-b fichero` reenvía las que tenga. Guarda una preferencia con `-c` y mira el fichero:

```
$ curl -c tarro.txt "http://localhost:8000/preferencia.php?dificultad=media" > /dev/null
$ cat tarro.txt
# Netscape HTTP Cookie File
# https://curl.se/docs/http-cookies.html
# This file was generated by libcurl! Edit at your own risk.

#HttpOnly_localhost	FALSE	/	FALSE	1792305343	dificultad	media
```

Un fichero de texto plano, con el dominio, la ruta, la caducidad (como marca de tiempo), el nombre y el valor. Y con eso llegamos a la lección más importante de las cookies: **el usuario las ve y las puede editar** — en ese fichero, o en el panel de almacenamiento del navegador, o con `-b "dificultad=lo-que-quiera"`. Una cookie es un dato que **el cliente propone**; el servidor tiene que tratarla como cualquier otra entrada externa. Mira cómo lo hace el calendario, que aplica la preferencia solo si es una de las tres válidas:

```php
<?php
declare(strict_types=1);
// calendario-filtrado.php — el calendario respeta la preferencia guardada en la cookie
$salidas   = require __DIR__ . '/datos/salidas.php';
$validas   = ['baja', 'media', 'alta'];
$preferida = $_COOKIE['dificultad'] ?? '';

// La cookie la escribe el navegador: se valida como cualquier dato externo
$filtro = in_array($preferida, $validas, true) ? $preferida : 'todas';
echo "Filtro aplicado: {$filtro}\n";
foreach ($salidas as $s) {
    if ($filtro === 'todas' || $s['dificultad'] === $filtro) {
        echo "- {$s['nombre']} ({$s['dificultad']})\n";
    }
}
```

Con el tarro que guardó `media`, y con una cookie manipulada a mano, las dos salidas reales:

```
$ curl -b tarro.txt http://localhost:8000/calendario-filtrado.php
Filtro aplicado: media
- Ibón de Estanés (media)
$ curl -b "dificultad=marte" http://localhost:8000/calendario-filtrado.php
Filtro aplicado: todas
- Ibón de Estanés (media)
- Peña Oroel (baja)
- Cañón de Añisclo (alta)
- San Juan de la Peña (baja)
- Ibones de Anayet (alta)
```

`marte` no es una dificultad; el servidor la ignora y muestra todo. La preferencia es **inofensiva**: si alguien la edita, lo peor que consigue es cambiar su propio filtro. Ese es exactamente el tipo de dato que merece una cookie. Lo que **nunca** va en una cookie, porque el usuario la edita: un precio, un rol, un «ya ha pagado», un «es administrador» — el error 6 del apartado 11 muestra la catástrofe con dos líneas de código.

Los atributos que faltan por explicar se entienden mejor ahora que sabes que la cookie es del cliente. **`HttpOnly`** impide que el JavaScript de la página la lea (`document.cookie` no la ve): si un atacante consigue colar un script en tu web, no podrá robar esa cookie — es la protección estándar para la cookie de sesión del apartado 3. **`SameSite=Lax`** hace que el navegador no la envíe en peticiones que *otro sitio* lance contra el tuyo (un formulario malicioso en otra web que dispare un POST a tu `inscribir.php`): la cookie viaja al navegar directamente al club, pero no en ese salto encubierto. **`Secure`**, que no está en el ejemplo, ordena enviarla **solo por HTTPS**; el servidor embebido sirve HTTP, así que en el aula la desactivaríamos sin querer — la verás activada de forma condicional en el apartado 8. Y **borrar** una cookie es un truco de protocolo: no existe «borra esto»; se envía la misma cookie con una caducidad en el pasado y el navegador la descarta:

```
$ curl -i -b tarro.txt "http://localhost:8000/preferencia.php?borrar=1"
HTTP/1.1 200 OK
Host: localhost:8000
Date: Fri, 18 Sep 2026 06:35:43 GMT
Connection: close
Set-Cookie: dificultad=deleted; expires=Thu, 01 Jan 1970 00:00:01 GMT; Max-Age=0; path=/
Content-type: text/html; charset=UTF-8

Preferencia borrada.
Cookie recibida en esta petición: media
```

Otra vez la última línea: en la petición del borrado la cookie **todavía llegó** (`media`), porque el navegador la envió antes de saber que iba a morir. La orden de borrarla viaja en la respuesta; hace efecto en la siguiente petición. Misma regla que al crearla, en sentido contrario.

Si tu servidor añade a las respuestas una cabecera `X-Powered-By` con el nombre y la versión del intérprete, no es parte del mecanismo: es una firma que conviene desactivar en producción (`expose_php`), porque regala información a quien busca vulnerabilidades. Las salidas de estos apuntes se tomaron con esa firma desactivada.

**Ejercicios del apartado.**

- **E4.** Reproduce el ciclo completo con `curl` y un tarro: guarda la preferencia `alta`, comprueba con `calendario-filtrado.php` que filtra, edita el tarro **a mano** para poner `dificultad=marte` y vuelve a pedir el calendario. Pega las salidas y explica, con la regla de oro de la UD1, por qué el servidor valida una cookie igual que un campo de formulario.
- **E5.** Cambia en `preferencia.php` la caducidad a 60 segundos, guarda la preferencia y pide `preferencia.php` sin parámetros dos veces: una inmediatamente y otra pasado más de un minuto. Explica qué ocurrió con `$_COOKIE` en la segunda y qué papel juegan `expires` y `Max-Age` (¿quién decide que la cookie ha caducado, el servidor o el navegador?).
- **E6.** Escribe `idioma.php`: guarda en una cookie `idioma` el valor `es` o `en` (solo esos dos, validados), y en `portada.php` imprime *«Bienvenido al club»* o *«Welcome to the club»* según la cookie, con `es` por defecto. Añade `HttpOnly` y `SameSite=Lax`, captura la cabecera `Set-Cookie` y explica en una línea qué protege cada atributo.

<div style="page-break-before: always;"></div>

## Apartado 3. Sesiones nativas: la memoria que vive en el servidor

Con las cookies el dato viaja y vive en el navegador. Con las **sesiones** se invierte el reparto: el dato **vive en el servidor** y en el navegador solo viaja una **llave** — una cookie que contiene un identificador largo y aleatorio. En cada petición, el navegador manda la llave; el servidor la reconoce, abre el cajón correspondiente y te deja sus datos en la superglobal **`$_SESSION`**. Es el mecanismo para mantener el **estado** de una aplicación, porque el cliente **no puede leer ni modificar** lo que hay en el cajón: solo tiene la llave.

Todo empieza con **`session_start()`**, que debe ejecutarse **antes de cualquier salida** (verás por qué en el apartado 4). El contador del apartado 1, ahora con memoria:

```php
<?php
declare(strict_types=1);
// contador-con-memoria.php — el mismo contador, ahora con sesión
session_start();
$_SESSION['visitas'] = ($_SESSION['visitas'] ?? 0) + 1;
echo "Visitas en esta sesión: {$_SESSION['visitas']}\n";
echo "Identificador de sesión: " . session_id() . "\n";
```

La primera petición, con cabeceras y guardando la llave en un tarro:

```
$ curl -i -c tarro.txt http://localhost:8000/contador-con-memoria.php
HTTP/1.1 200 OK
Host: localhost:8000
Date: Fri, 18 Sep 2026 06:35:43 GMT
Connection: close
Set-Cookie: PHPSESSID=tupl5net5mcqt20q06o8tqgjb4; path=/
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Content-type: text/html; charset=UTF-8

Visitas en esta sesión: 1
Identificador de sesión: tupl5net5mcqt20q06o8tqgjb4
```

Ahí está la llave: una cookie llamada **`PHPSESSID`** (el nombre por defecto; `session_name()` lo devuelve) con un identificador que PHP genera al azar para esta sesión, sin caducidad — una *cookie de sesión* en el sentido del navegador: muere al cerrarlo. Fíjate también en las cabeceras `Expires`, `Cache-Control` y `Pragma`: PHP las añade para que ninguna caché guarde una página que depende de quién la pide. Las peticiones siguientes, reenviando el tarro:

```
$ curl -b tarro.txt http://localhost:8000/contador-con-memoria.php
Visitas en esta sesión: 2
Identificador de sesión: tupl5net5mcqt20q06o8tqgjb4
$ curl -b tarro.txt http://localhost:8000/contador-con-memoria.php
Visitas en esta sesión: 3
Identificador de sesión: tupl5net5mcqt20q06o8tqgjb4
$ curl http://localhost:8000/contador-con-memoria.php
Visitas en esta sesión: 1
Identificador de sesión: glpf137qclar0fs3mknejv60q8
```

Dos, tres… y la cuarta petición, **sin tarro**, vuelve a uno con un identificador distinto: para el servidor es otro cliente, porque no trae la llave. Eso es «un cliente web concreto» resuelto: no una persona, sino *quien tenga esta llave*. Guarda esa frase, porque de ella sale toda la seguridad del apartado 8.

¿Y dónde vive el cajón? Por defecto, en un **fichero por sesión** en la carpeta que indica la directiva `session.save_path` (mírala con `php --ini` o con `ini_get`). Este pequeño fichero te la enseña y te dice cómo se llama tu cajón:

```php
<?php
declare(strict_types=1);
// mira-sesion.php — dónde vive la sesión en el servidor
session_start();
$_SESSION['prueba'] = 'hola';
echo "save_path: " . ini_get('session.save_path') . "\n";
echo "Fichero: sess_" . session_id() . "\n";
```

```
$ curl -b tarro.txt http://localhost:8000/mira-sesion.php
save_path: /ruta/sesiones
Fichero: sess_tupl5net5mcqt20q06o8tqgjb4
$ cat /ruta/sesiones/sess_tupl5net5mcqt20q06o8tqgjb4
visitas|i:3;prueba|s:4:"hola";
```

Ábrelo tú también. Es texto plano en un formato propio de PHP (`visitas`, entero, 3; `prueba`, cadena de 4 caracteres, `hola`): lo que pusiste en `$_SESSION` en dos ficheros distintos, junto en el cajón de esa llave. En el aula la ruta será otra; en tu casa, otra. Lo importante es que **nada de esto viajó al navegador** — solo la llave. Y una consecuencia práctica: los datos de sesión son datos del servidor, sujetos a la misma disciplina de protección que cualquier fichero suyo.

La sesión guarda cualquier valor que se pueda serializar: enteros, cadenas y también arrays. Un ejemplo de estado real del club — las salidas que un visitante ha marcado como «me interesa» —, con las dos operaciones que faltaban, **vaciar** y **destruir**:

```php
<?php
declare(strict_types=1);
// favoritas.php — salidas marcadas como "me interesa", guardadas en la sesión
session_start();
$salidas = require __DIR__ . '/datos/salidas.php';

if (isset($_GET['limpiar'])) {
    session_unset();      // vacía $_SESSION
    session_destroy();    // elimina los datos de la sesión en el servidor
    echo "Lista vaciada.\n";
    exit;
}

$_SESSION['favoritas'] ??= [];               // la primera vez, lista vacía

$indice = filter_var($_GET['marcar'] ?? '', FILTER_VALIDATE_INT);
if ($indice !== false && isset($salidas[$indice]) && !in_array($indice, $_SESSION['favoritas'], true)) {
    $_SESSION['favoritas'][] = $indice;        // guardamos el índice, no el nombre
}

echo "Salidas que te interesan (" . count($_SESSION['favoritas']) . "):\n";
foreach ($_SESSION['favoritas'] as $i) {
    echo "- {$salidas[$i]['nombre']}\n";
}
```

Una secuencia de seis peticiones con el mismo tarro — marcar la 2, la 4, la 2 otra vez, una que no existe, limpiar y volver a listar —, con sus salidas reales:

```
$ curl -c tarro.txt -b tarro.txt "http://localhost:8000/favoritas.php?marcar=2"
Salidas que te interesan (1):
- Cañón de Añisclo
$ curl -c tarro.txt -b tarro.txt "http://localhost:8000/favoritas.php?marcar=4"
Salidas que te interesan (2):
- Cañón de Añisclo
- Ibones de Anayet
$ curl -c tarro.txt -b tarro.txt "http://localhost:8000/favoritas.php?marcar=2"
Salidas que te interesan (2):
- Cañón de Añisclo
- Ibones de Anayet
$ curl -c tarro.txt -b tarro.txt "http://localhost:8000/favoritas.php?marcar=9"
Salidas que te interesan (2):
- Cañón de Añisclo
- Ibones de Anayet
$ curl -c tarro.txt -b tarro.txt "http://localhost:8000/favoritas.php?limpiar=1"
Lista vaciada.
$ curl -c tarro.txt -b tarro.txt "http://localhost:8000/favoritas.php"
Salidas que te interesan (0):
```

Léela con la lupa de la UD2: `??=` asigna la lista vacía solo si aún no existe (la primera vez); `filter_var` con `FILTER_VALIDATE_INT` convierte la cadena `'2'` en el entero `2` y rechaza basura; `isset($salidas[$indice])` comprueba que el índice existe en **el catálogo real** (por eso la `9` no entra), y `in_array` con el tercer parámetro `true` evita duplicados con comparación estricta. Guardamos **índices**, no nombres: si mañana cambia el nombre de una salida, la lista sigue valiendo. Y la pareja **`session_unset()`** + **`session_destroy()`** vacía la variable y borra el cajón del servidor; la llave del navegador sigue existiendo un rato, pero ya no abre nada — en el apartado 6 verás la versión completa del cierre, que también borra la cookie.

Con esto ya puedes formular la regla de reparto que anunció el apartado 1, ahora con conocimiento de causa: en **cookie**, preferencias inofensivas que el usuario podría editar sin consecuencias (idioma, dificultad, tema); en **sesión**, todo lo que define el estado de la aplicación para ese usuario (qué lleva marcado, qué está a medias y, desde el apartado 6, **quién es**). La sesión usa una cookie por debajo — la llave — pero esa cookie no contiene datos, contiene un identificador que no significa nada fuera del servidor.

**Ejercicios del apartado.**

- **E7.** Reproduce el contador con dos tarros distintos (`a.txt` y `b.txt`) alternando peticiones: a, a, b, a, b. Pega las salidas y explica por qué cada tarro lleva su propia cuenta. Después abre los dos ficheros `sess_…` del servidor y comprueba que el contenido cuadra con lo que viste.
- **E8.** Amplía `favoritas.php` con una acción `?quitar=N` que elimine una salida de la lista (pista: reconstruir el array sin ese índice con `array_filter` o `array_values`, para que no queden huecos). Prueba marcar, quitar y volver a listar con un tarro, y pega la secuencia.
- **E9.** Escribe `sesion-vs-cookie.php`, que guarde el mismo dato (`'alta'`) de dos formas — en una cookie `pref` y en `$_SESSION['pref']` — y lo imprima desde ambos sitios en la siguiente petición. Con el tarro, edita a mano el valor de la cookie `pref` y vuelve a pedir la página: ¿cuál de los dos valores cambió y cuál no? Explica en tres líneas qué demuestra eso sobre quién controla cada mecanismo.

<div style="page-break-before: always;"></div>

## Apartado 4. Redirecciones, mensajes de un solo uso y el patrón Post/Redirect/Get

Cuando la aplicación tiene memoria, aparece una pregunta que en la UD2 no existía: después de procesar un formulario, **¿qué página muestro?** La respuesta ingenua — «la misma página, con un mensaje de éxito» — tiene un problema clásico: si el usuario pulsa **recargar**, el navegador vuelve a enviar el POST (avisa con un diálogo, pero muchos aceptan), y la inscripción se registra dos veces. La solución profesional se llama **Post/Redirect/Get** (PRG): el fichero que procesa el POST **no muestra nada**; hace su trabajo y **redirige** a una página que se pide con GET. Recargar esa página repite un GET inofensivo, no el POST.

La herramienta es la función **`header()`**, que añade una cabecera a la respuesta. Con `Location:` el servidor le dice al navegador «ve a esta otra dirección», y PHP pone automáticamente el código de estado **302**. El calendario del club, ahora con un botón de inscripción por salida, y el fichero que procesa el envío:

```php
<?php
declare(strict_types=1);
// inscribir.php — procesa el POST y REDIRIGE: nunca muestra página propia
session_start();
$salidas = require __DIR__ . '/datos/salidas.php';

if ($_SERVER['REQUEST_METHOD'] !== 'POST') {
    header('Location: calendario.php');
    exit;
}

$indice = filter_var($_POST['salida'] ?? '', FILTER_VALIDATE_INT);
if ($indice === false || !isset($salidas[$indice])) {
    $_SESSION['flash'] = 'Esa salida no existe.';
} elseif ($salidas[$indice]['inscritos'] >= $salidas[$indice]['plazas']) {
    $_SESSION['flash'] = 'Esa salida está completa.';
} else {
    // Aquí iría la inscripción real (UD6); por ahora, solo el mensaje
    $_SESSION['flash'] = "Inscripción registrada en {$salidas[$indice]['nombre']}.";
}

header('Location: calendario.php');
exit;
```

Tres detalles que son norma. Primero: tras cada `header('Location: …')` va un **`exit`**. La redirección es solo una cabecera; si no detienes el script, **el resto se ejecuta igual** y se envía — en el error 5 del apartado 11 verás una página «protegida» entregando su contenido secreto después de haber redirigido. Segundo: si alguien pide `inscribir.php` con GET (escribiendo la dirección a mano), no hay nada que procesar y se le devuelve al calendario. Tercero: el resultado del procesado no se imprime — **se guarda en la sesión** con la clave `flash`, para que lo muestre la página de destino. Esa es la técnica de los **mensajes de un solo uso**: se escriben en la sesión antes de redirigir y se **leen y borran** al mostrarlos, de modo que la siguiente petición ya no los vea.

```php
<?php
declare(strict_types=1);
// calendario.php — muestra el calendario y, si lo hay, el mensaje de un solo uso
session_start();
$salidas = require __DIR__ . '/datos/salidas.php';

$mensaje = $_SESSION['flash'] ?? null;   // leer...
unset($_SESSION['flash']);               // ...y consumir: la próxima petición ya no lo verá
?>
<?php if ($mensaje !== null): ?>
<p class="aviso"><?= htmlspecialchars($mensaje) ?></p>
<?php endif; ?>
<ul>
<?php foreach ($salidas as $i => $s): ?>
    <li><?= htmlspecialchars($s['nombre']) ?>
        <form action="inscribir.php" method="post">
            <input type="hidden" name="salida" value="<?= $i ?>">
            <button type="submit">Inscribirme</button>
        </form>
    </li>
<?php endforeach; ?>
</ul>
```

El flujo entero con `curl`. Primero el POST, con cabeceras: mira que la respuesta **no tiene cuerpo**, solo la orden de ir al calendario. Después, dos GET seguidos al calendario:

```
$ curl -i -c tarro.txt -b tarro.txt -d "salida=0" http://localhost:8000/inscribir.php
HTTP/1.1 302 Found
Host: localhost:8000
Date: Fri, 18 Sep 2026 06:35:54 GMT
Connection: close
Expires: Thu, 19 Nov 1981 08:52:00 GMT
Cache-Control: no-store, no-cache, must-revalidate
Pragma: no-cache
Location: calendario.php
Content-type: text/html; charset=UTF-8

$ curl -c tarro.txt -b tarro.txt http://localhost:8000/calendario.php | head -3
<p class="aviso">Inscripción registrada en Ibón de Estanés.</p>
<ul>
    <li>Ibón de Estanés        <form action="inscribir.php" method="post">
$ curl -c tarro.txt -b tarro.txt http://localhost:8000/calendario.php | head -3
<ul>
    <li>Ibón de Estanés        <form action="inscribir.php" method="post">
            <input type="hidden" name="salida" value="0">
```

La primera visita al calendario muestra el aviso; la segunda, ya no: el mensaje se consumió. En el navegador, este mismo flujo es lo que ves como «pulso el botón, la página cambia y aparece un aviso verde que desaparece al navegar». Con `-L`, `curl` sigue la redirección solo, y sirve para probar los otros dos caminos (la salida `1` está completa, 25 de 25; la `7` no existe):

```
$ curl -L -c tarro.txt -b tarro.txt -d "salida=1" http://localhost:8000/inscribir.php | head -1
<p class="aviso">Esa salida está completa.</p>
$ curl -L -c tarro.txt -b tarro.txt -d "salida=7" http://localhost:8000/inscribir.php | head -1
<p class="aviso">Esa salida no existe.</p>
```

Queda la letra pequeña de `header()`, que es la misma que la de `session_start()` y la de `setcookie()`: **las cabeceras solo se pueden enviar antes del cuerpo**. En cuanto tu fichero produce un solo carácter de salida — un `echo`, un espacio antes de `<?php`, una línea en blanco tras un `?>` de cierre —, las cabeceras ya han viajado y cualquier intento posterior falla. Es la razón de la convención que aprendiste en el apartado 1 de la UD2 (no cerrar con `?>` un fichero de PHP puro), y aquí se paga cara. Este fichero tiene su primera línea **en blanco**:

```php

<?php
// cabecera-rota.php — la primera línea del fichero está en blanco: eso YA es salida
header('Location: calendario.php');
echo "Redirigiendo...\n";
```

Ejecutado en el terminal, el aviso lo dice todo — dónde empezó la salida (línea 1) y qué línea intentó la cabecera (línea 4):

```
$ php cabecera-rota.php

PHP Warning:  Cannot modify header information - headers already sent by (output started at /ruta/cabecera-rota.php:1) in /ruta/cabecera-rota.php on line 4
Redirigiendo...
```

Y con `session_start()` en la misma situación, el mensaje es aún más claro: `Warning: session_start(): Session cannot be started after headers have already been sent`. Una advertencia de reproducibilidad, porque te va a pasar: el **servidor embebido** puede **ocultar** este aviso cuando la salida previa es pequeña — envía la respuesta en bloque y la cabecera «llega a tiempo» — y el mismo fichero fallará en otro servidor o cuando la salida crezca. Si un `Location` te funciona en `php -S` y no en otro sitio, busca la salida fantasma antes de la cabecera; y comprueba siempre las redirecciones también en el terminal, donde el aviso no se esconde. (El fichero de arriba no lleva `declare(strict_types=1)` a propósito: esa declaración tiene que ser la **primera sentencia** del fichero, y la línea en blanco ya no lo permite — un síntoma más de la misma avería.)

**Ejercicios del apartado.**

- **E10.** Quita el `exit` que sigue al último `header()` de `inscribir.php` y añade debajo un `echo "Procesado\n";`. Pide el POST con `curl -i` y observa qué código de estado y qué cuerpo llegan. Explica por qué el navegador «parece» funcionar igual y por qué, aun así, es un bug.
- **E11.** Amplía el patrón: haz que `inscribir.php` guarde, además del mensaje, un tipo (`'ok'` o `'error'`) en `$_SESSION['flash_tipo']`, y que `calendario.php` use una clase CSS distinta según el tipo. Verifica con `curl -L` los tres caminos y comprueba que ambos valores se consumen a la vez.
- **E12.** Provoca el aviso `headers already sent` de dos formas distintas de la del ejemplo: (a) con un `echo` antes de `session_start()`; (b) con un `?>` de cierre seguido de una línea en blanco en un fichero incluido con `require` antes de `header()`. Pega los dos avisos y señala en cada uno el «output started at».

<div style="page-break-before: always;"></div>

## Apartado 5. Autenticación: quién eres de verdad

Hasta aquí el servidor reconoce **navegadores**: la llave de sesión dice «este es el mismo cliente de antes», no «este es Jorge». Para que la aplicación tenga usuarios hacen falta dos cosas que conviene nombrar por separado desde el primer día, porque se confunden constantemente. **Autentificación** es comprobar **quién eres** (que quien dice ser Jorge lo demuestre). **Autorización** es decidir **qué puedes hacer** una vez sabemos quién eres (un socio no ve el panel de administración). Este apartado y el siguiente son de autentificación; el 7, de autorización.

¿Cómo demuestra alguien quién es a través de HTTP? Hay tres familias de mecanismos, y conviene conocerlas para elegir con criterio.

| Mecanismo | Cómo funciona | Dónde se usa | Por qué no es el nuestro / sí lo es |
|---|---|---|---|
| **Autenticación HTTP básica** | El servidor responde `401` con la cabecera `WWW-Authenticate`; el navegador pide usuario y contraseña en un diálogo y los reenvía en **cada petición** en la cabecera `Authorization`, codificados (no cifrados) | Paneles internos, APIs sencillas entre máquinas, protección rápida de una carpeta | Las credenciales viajan en cada petición; no hay «cerrar sesión» ni formulario propio; sin HTTPS es texto casi plano. Vas a ejecutarla para **ver el mecanismo desnudo** |
| **Formulario + sesión** | La aplicación muestra su propio formulario; comprueba las credenciales **una vez**; si son correctas, anota en la **sesión** quién es; las peticiones siguientes solo llevan la llave | La inmensa mayoría de las aplicaciones web | Es el mecanismo del curso: controlas la interfaz, la contraseña viaja una sola vez y el estado de «dentro/fuera» lo mantiene la sesión que ya dominas |
| **Tokens y delegación** (*OAuth*, «entrar con…») | Un tercero (o tu propio servidor) emite un **token** firmado que el cliente presenta en cada petición; la identidad la verifica quien emitió el token | Servicios REST, aplicaciones móviles, inicio de sesión con una cuenta de otra plataforma | Lo verás en la unidad de servicios web (UD7), donde no hay sesión de navegador. Aquí solo lo sitúas |

Ejecuta la primera para entenderla. PHP deja las credenciales de la cabecera `Authorization` en `$_SERVER['PHP_AUTH_USER']` y `$_SERVER['PHP_AUTH_PW']`; si no llegan, el servidor pide identificación:

```php
<?php
declare(strict_types=1);
// basica.php — autenticación HTTP básica: el mecanismo "a pelo", para verlo funcionar (no lo usaremos en la zona privada)
$usuarios = require __DIR__ . '/datos/usuarios.php';

$usuario = $_SERVER['PHP_AUTH_USER'] ?? null;
$clave   = $_SERVER['PHP_AUTH_PW'] ?? null;

$valido = false;
foreach ($usuarios as $u) {
    if ($usuario !== null && $u['usuario'] === $usuario && password_verify((string) $clave, $u['hash'])) {
        $valido = true;
    }
}

if (!$valido) {
    header('WWW-Authenticate: Basic realm="Zona privada del club"');
    header('HTTP/1.1 401 Unauthorized');
    echo "Identifícate para entrar.\n";
    exit;
}
echo "Hola, {$usuario}. Has entrado por autenticación básica.\n";
```

```
$ curl -i http://localhost:8000/basica.php
HTTP/1.1 401 Unauthorized
Host: localhost:8000
Date: Fri, 18 Sep 2026 06:37:33 GMT
Connection: close
WWW-Authenticate: Basic realm="Zona privada del club"
Content-type: text/html; charset=UTF-8

Identifícate para entrar.
$ curl -u carlos.mur:monitor2026 http://localhost:8000/basica.php
Hola, carlos.mur. Has entrado por autenticación básica.
```

En el navegador, ese `401` con `WWW-Authenticate` abre el diálogo de usuario y contraseña. Con `curl`, la opción `-u` construye la cabecera. Y esto es lo que viajó de verdad en la segunda petición (`curl -v` la muestra), seguido de su decodificación:

```
> Authorization: Basic Y2FybG9zLm11cjptb25pdG9yMjAyNg==
$ php -r 'echo base64_decode("Y2FybG9zLm11cjptb25pdG9yMjAyNg==");'
carlos.mur:monitor2026
```

Usuario y contraseña, en claro tras una codificación trivial, **en cada petición**. Sin HTTPS es una postal; con HTTPS es aceptable para paneles internos, pero sigue sin darte formulario propio ni cierre de sesión. Ya sabes lo suficiente para descartarla para el club con argumentos.

Antes de construir el mecanismo bueno, la regla que gobierna las contraseñas en cualquier aplicación seria: **una contraseña nunca se guarda**. Ni en un fichero, ni en la base de datos, ni en la sesión, ni en un registro. Lo que se guarda es un **hash**: el resultado de una función diseñada para que sea fácil comprobar si una contraseña corresponde al hash e **inviable** recuperar la contraseña desde él. Si alguien roba el fichero de usuarios, se lleva hashes, no contraseñas. PHP lo resuelve con dos funciones, y solo dos: **`password_hash()`** para crear el hash y **`password_verify()`** para comprobarlo.

```php
<?php
declare(strict_types=1);
// hash-demo.php — la misma contraseña, dos hashes distintos; y la verificación que sí funciona
$hash1 = password_hash('ibones2026', PASSWORD_DEFAULT);
$hash2 = password_hash('ibones2026', PASSWORD_DEFAULT);
echo "Hash 1: {$hash1}\n";
echo "Hash 2: {$hash2}\n";
var_dump($hash1 === $hash2);                          // ¿iguales? no
var_dump(password_verify('ibones2026', $hash1));      // ¿la contraseña encaja con el hash 1?
var_dump(password_verify('ibones2026', $hash2));      // ¿y con el 2?
var_dump(password_verify('Ibones2026', $hash1));      // mayúscula distinta
var_dump(password_needs_rehash($hash1, PASSWORD_DEFAULT));
```

```
Hash 1: $2y$10$q4ZKNIs.ikOoV6rJqttZBu0bQCej7L6su9okQX/iEvDBCLvuuKKTy
Hash 2: $2y$10$WKqcbGeefd0sY5o8VVUY/.m.gn80tnN.7eejrDET5mzDwrYMLjSKm
bool(false)
bool(true)
bool(true)
bool(false)
bool(false)
```

Tus hashes serán otros — cambian **en cada ejecución** — y esa es precisamente la primera lección: la misma contraseña produce dos hashes distintos porque `password_hash` añade una **sal** aleatoria antes de calcular. Así, dos socios con la misma contraseña tienen hashes distintos, y una tabla precalculada de hashes de contraseñas comunes no sirve contra ninguno. La segunda lección: como no puedes comparar hashes entre sí, la comprobación **siempre** es `password_verify(contraseña, hash)`, que extrae la sal del propio hash y repite el cálculo; jamás `===` (error 4 del apartado 11). La tercera está en el prefijo `$2y$10$`: identifica el algoritmo (bcrypt, el `PASSWORD_DEFAULT` actual) y su **coste** (10, un exponente: cuántas vueltas de cálculo se hacen, deliberadamente lentas para frenar ataques por fuerza bruta). Otro intérprete puede usar un coste por defecto distinto — el número cambia, las funciones no —, y para eso existe **`password_needs_rehash()`**: dice si un hash guardado se hizo con parámetros más débiles que los actuales, para regenerarlo en el siguiente inicio de sesión (el momento en que, por una vez, tienes la contraseña en la mano). Y la cuarta, en la última línea del código: `Ibones2026` no valida, porque una contraseña es exacta. Con `md5` o `sha1` **no** se hace nada de esto: son rápidos y sin sal, justo lo contrario de lo que una contraseña necesita.

**Ejercicios del apartado.**

- **E13.** Prueba `basica.php` con tres peticiones: sin credenciales, con usuario correcto y contraseña incorrecta (`-u carlos.mur:nada`), y con credenciales correctas. Pega los códigos de estado, captura con `curl -v` la cabecera `Authorization` de la tercera y decodifícala. Explica con dos argumentos por qué no elegimos este mecanismo para la zona privada del club.
- **E14.** Escribe `alta-hash.php`, que reciba una contraseña por argumento de terminal (`$argv[1]`), imprima su hash y, a continuación, verifique esa misma contraseña y una variante con una letra cambiada. Ejecútalo dos veces con la misma contraseña y explica por qué los hashes difieren y por qué eso no rompe la verificación.
- **E15.** Comprueba con `password_needs_rehash` los hashes de `datos/usuarios.php` en tu intérprete. Si devuelve `true`, explica qué significa y **en qué momento** de la aplicación podrías regenerar el hash (pista: ¿cuándo tiene el servidor la contraseña en claro de forma legítima?). Si devuelve `false`, explica qué comprobó exactamente.

<div style="page-break-before: always;"></div>

## Apartado 6. Registro e inicio de sesión sobre el club

Toca construir el mecanismo del curso: **formulario + sesión**. Tres ficheros — registro, inicio de sesión y cierre — y una decisión previa sobre dónde viven los usuarios. En esta unidad el almacén es de dos piezas: el fichero de solo lectura `datos/usuarios.php` (los ocho de siempre) y un fichero **`datos/usuarios.json`** donde el registro añade los nuevos. Toda la lógica de «dónde están los usuarios» se encierra en tres funciones de `almacen.php`, y la aplicación **solo habla con ellas** — en el apartado 9 verás que esa disciplina permite cambiar el almacén por una base de datos tocando un único fichero.

```php
<?php
declare(strict_types=1);
// almacen.php — dónde viven los usuarios en esta unidad (fichero de solo lectura + fichero JSON de altas)

const FICHERO_ALTAS = __DIR__ . '/datos/usuarios.json';

/** Devuelve TODOS los usuarios: los de datos/usuarios.php (fijos) más los registrados en usuarios.json. */
function cargar_usuarios(): array {
    $fijos = require __DIR__ . '/datos/usuarios.php';
    $altas = file_exists(FICHERO_ALTAS)
        ? (json_decode((string) file_get_contents(FICHERO_ALTAS), true) ?? [])
        : [];
    return array_merge(array_values($fijos), $altas);
}

/** Busca un usuario por su nombre de usuario; null si no existe. */
function buscar_usuario(string $usuario): ?array {
    foreach (cargar_usuarios() as $u) {
        if ($u['usuario'] === $usuario) {
            return $u;
        }
    }
    return null;
}

/** Añade un usuario nuevo al JSON de altas: leer → añadir → escribir (con bloqueo). */
function guardar_usuario_nuevo(array $nuevo): void {
    $altas = file_exists(FICHERO_ALTAS)
        ? (json_decode((string) file_get_contents(FICHERO_ALTAS), true) ?? [])
        : [];
    $altas[] = $nuevo;
    file_put_contents(FICHERO_ALTAS, json_encode($altas, JSON_PRETTY_PRINT | JSON_UNESCAPED_UNICODE | JSON_UNESCAPED_SLASHES), LOCK_EX);
}
```

Mira `guardar_usuario_nuevo`: **leer** el JSON entero, **añadir** el usuario, **escribir** el fichero entero, con `LOCK_EX` para que dos escrituras no se pisen a medias. Funciona, y es suficiente para el aula. Pero fíjate en lo que **no** resuelve: si dos personas se registran en el mismo instante, ambas leen el mismo fichero, cada una añade el suyo y la segunda escritura **borra a la primera** — el bloqueo protege la escritura, no la secuencia leer-añadir-escribir. Un fichero de datos no es un **almacén concurrente**; un gestor de base de datos sí lo es (garantiza que dos inserciones a la vez sobreviven las dos, y que no habrá dos usuarios con el mismo nombre). Eso es exactamente lo que resuelve la UD6; aquí basta con saber que el límite existe y dónde está su solución.

El **registro**, que es un formulario de la UD2 con dos novedades marcadas en los comentarios: el rol lo decide el servidor, y de la contraseña se guarda **solo el hash**.

```php
<?php
declare(strict_types=1);
// registro.php — alta de un socio nuevo: validar en el servidor, guardar SOLO el hash
session_start();
require __DIR__ . '/almacen.php';

$errores = [];
$datos   = ['usuario' => '', 'nombre' => '', 'correo' => ''];

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $datos['usuario'] = trim($_POST['usuario'] ?? '');
    $datos['nombre']  = trim($_POST['nombre'] ?? '');
    $datos['correo']  = trim($_POST['correo'] ?? '');
    $clave            = $_POST['clave'] ?? '';

    if (!preg_match('/^[a-z0-9.]{3,30}$/', $datos['usuario'])) {
        $errores[] = 'El usuario debe tener entre 3 y 30 caracteres: minúsculas, dígitos o punto.';
    } elseif (buscar_usuario($datos['usuario']) !== null) {
        $errores[] = 'Ese nombre de usuario ya está en uso.';
    }
    if ($datos['nombre'] === '' || mb_strlen($datos['nombre']) > 60) {
        $errores[] = 'El nombre es obligatorio (máximo 60 caracteres).';
    }
    if (filter_var($datos['correo'], FILTER_VALIDATE_EMAIL) === false) {
        $errores[] = 'El correo no es válido.';
    }
    if (mb_strlen($clave) < 8) {
        $errores[] = 'La contraseña debe tener al menos 8 caracteres.';
    }

    if ($errores === []) {
        guardar_usuario_nuevo([
            'usuario' => $datos['usuario'],
            'nombre'  => $datos['nombre'],
            'correo'  => $datos['correo'],
            'rol'     => 'socio',                                   // el rol lo decide el servidor, nunca el formulario
            'hash'    => password_hash($clave, PASSWORD_DEFAULT),  // la contraseña en claro no se guarda jamás
        ]);
        $_SESSION['flash'] = 'Alta completada. Ya puedes iniciar sesión.';
        header('Location: login.php');
        exit;
    }
}
?>
<?php foreach ($errores as $e): ?>
<p class="error"><?= htmlspecialchars($e) ?></p>
<?php endforeach; ?>
<form action="" method="post">
    <label>Usuario <input type="text" name="usuario" value="<?= htmlspecialchars($datos['usuario']) ?>"></label>
    <label>Nombre <input type="text" name="nombre" value="<?= htmlspecialchars($datos['nombre']) ?>"></label>
    <label>Correo <input type="email" name="correo" value="<?= htmlspecialchars($datos['correo']) ?>"></label>
    <label>Contraseña <input type="password" name="clave"></label>
    <button type="submit">Darme de alta</button>
</form>
```

Un envío inválido (usuario ocupado, correo mal, contraseña corta) devuelve los tres errores juntos y el formulario relleno con lo que se escribió — menos la contraseña, que no se devuelve nunca al navegador —; un envío válido se guarda y **redirige** al inicio de sesión con un mensaje de un solo uso (patrón del apartado 4). El registro pide **solo lo que la aplicación necesita** para funcionar: usuario, nombre, correo y contraseña. Ni DNI, ni teléfono, ni fecha de nacimiento «por si acaso»: cada dato personal que guardas es un dato que tienes que proteger y justificar. A ese principio se le llama **minimización**, y es una obligación legal en el tratamiento de datos personales, no una cortesía.

```
$ curl -d "usuario=lucia.ramos&nombre=Prueba&correo=no-es-correo&clave=corta" http://localhost:8000/registro.php | grep error
<p class="error">Ese nombre de usuario ya está en uso.</p>
<p class="error">El correo no es válido.</p>
<p class="error">La contraseña debe tener al menos 8 caracteres.</p>
$ curl -i -c tarro.txt -b tarro.txt -d "usuario=sara.otal&nombre=Sara+Otal&correo=sara.otal@example.org&clave=estanes-2026" http://localhost:8000/registro.php | grep -E "HTTP|Location"
HTTP/1.1 302 Found
Location: login.php
$ cat datos/usuarios.json
[
    {
        "usuario": "sara.otal",
        "nombre": "Sara Otal",
        "correo": "sara.otal@example.org",
        "rol": "socio",
        "hash": "$2y$10$w79u/DkW9aIr03tQnp/GnO23ZNV58ey4igelstrw5HpQYV4FMCZcG"
    }
]
```

Ahora el **inicio de sesión**, el fichero central de la unidad:

```php
<?php
declare(strict_types=1);
// login.php — inicio de sesión: comprobar credenciales y abrir una sesión autenticada
session_start();
require __DIR__ . '/almacen.php';

$mensaje = $_SESSION['flash'] ?? null;
unset($_SESSION['flash']);
$error = null;

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    $usuario = trim($_POST['usuario'] ?? '');
    $clave   = $_POST['clave'] ?? '';
    $u       = buscar_usuario($usuario);

    if ($u !== null && password_verify($clave, $u['hash'])) {
        session_regenerate_id(true);               // sesión nueva al autenticar: cierra la fijación de sesión
        $_SESSION['usuario'] = [                   // en sesión, lo mínimo: nunca la contraseña ni el hash
            'usuario' => $u['usuario'],
            'nombre'  => $u['nombre'],
            'rol'     => $u['rol'],
        ];
        header('Location: mi-ficha.php');
        exit;
    }
    $error = 'Credenciales no válidas.';           // genérico: no decimos si falló el usuario o la contraseña
}
?>
<?php if ($mensaje !== null): ?><p class="aviso"><?= htmlspecialchars($mensaje) ?></p><?php endif; ?>
<?php if ($error !== null): ?><p class="error"><?= htmlspecialchars($error) ?></p><?php endif; ?>
<form action="" method="post">
    <label>Usuario <input type="text" name="usuario"></label>
    <label>Contraseña <input type="password" name="clave"></label>
    <button type="submit">Entrar</button>
</form>
```

Cuatro decisiones, todas con motivo. **`buscar_usuario` + `password_verify`**: la contraseña llega en claro **una vez**, por POST, se compara con el hash y se olvida. **`session_regenerate_id(true)`**: al autenticar, PHP crea una llave nueva y descarta la anterior; el porqué (un ataque llamado *fijación de sesión*) está en el apartado 8 — de momento, es norma. **Lo que va a la sesión es lo mínimo**: quién es y su rol; ni la contraseña ni el hash tienen nada que hacer allí. Y el **mensaje de error es genérico**: «Credenciales no válidas», sin decir si el usuario no existe o si la contraseña falló — decirlo regala a un atacante la mitad del trabajo (confirmar qué usuarios existen).

```
$ curl -c tarro.txt -b tarro.txt http://localhost:8000/login.php | head -1
<p class="aviso">Alta completada. Ya puedes iniciar sesión.</p><form action="" method="post">
$ curl -c tarro.txt -b tarro.txt -d "usuario=sara.otal&clave=mal" http://localhost:8000/login.php | head -1
<p class="error">Credenciales no válidas.</p><form action="" method="post">
$ curl -i -c tarro.txt -b tarro.txt -d "usuario=sara.otal&clave=estanes-2026" http://localhost:8000/login.php | grep -E "HTTP|Set-Cookie|Location"
HTTP/1.1 302 Found
Set-Cookie: PHPSESSID=qq3l4lbkeckc7n4llo9njbvt0t; path=/
Location: mi-ficha.php
```

La tercera petición lo cuenta todo: credenciales correctas → **cookie de sesión nueva** (`Set-Cookie` con un identificador distinto del que tenía el tarro: eso es `session_regenerate_id`) → redirección a la ficha. A partir de aquí, cada petición con esa llave encuentra `$_SESSION['usuario']` en su cajón, y la aplicación sabe quién habla.

Y el **cierre de sesión**, que hace las tres cosas que un cierre serio hace — vaciar, invalidar la cookie y destruir el cajón — y redirige:

```php
<?php
declare(strict_types=1);
// logout.php — cerrar sesión: vaciar, borrar la cookie y destruir
session_start();
session_unset();
setcookie(session_name(), '', ['expires' => time() - 3600, 'path' => '/']);
session_destroy();
header('Location: login.php');
exit;
```

```
$ curl -i -c tarro.txt -b tarro.txt http://localhost:8000/logout.php | grep -E "HTTP|Set-Cookie|Location"
HTTP/1.1 302 Found
Set-Cookie: PHPSESSID=deleted; expires=Thu, 01 Jan 1970 00:00:01 GMT; Max-Age=0; path=/
Location: login.php
```

El mismo truco de caducidad en el pasado del apartado 2, aplicado a la llave. Con esto la aplicación ya **sabe quién eres**. Falta que actúe en consecuencia.

**Ejercicios del apartado.**

- **E16.** Regístrate con `curl` (un usuario ficticio con tu alias), entra, pide `mi-ficha.php` con el tarro y cierra sesión; vuelve a pedir la ficha. Pega la secuencia con los códigos de estado y los `Set-Cookie`. Marca en qué petición cambió el identificador de sesión y en cuál se borró.
- **E17.** Cambia el mensaje de error de `login.php` por dos distintos («El usuario no existe» / «Contraseña incorrecta») y, con `curl`, demuestra en dos peticiones cómo un atacante sabría ahora que `lucia.ramos` existe y `lucia.ramoz` no. Vuelve a dejarlo genérico y explica en dos líneas qué es una *enumeración de usuarios*.
- **E18.** El registro abre la sesión (`session_start()`) solo para dejar el mensaje de un solo uso. Propón y programa una alternativa **sin** sesión para llevar ese aviso al login (pista: un parámetro en la URL, validado) y compara ambas: ¿cuál puede manipular el usuario y con qué consecuencia? Argumenta cuál prefieres.

<div style="page-break-before: always;"></div>

## Apartado 7. Autorización: usuarios, perfiles y roles

La aplicación sabe quién eres. Ahora tiene que decidir **qué puedes hacer**, y la forma profesional de decidirlo no es mirar el nombre del usuario, sino su **rol**: una etiqueta que agrupa permisos. En el club hay tres — **socio**, **monitor** y **administración** — y una matriz que dice qué ve cada uno. Escribirla antes de programar es media autorización hecha:

| Página | socio | monitor | administración |
|---|---|---|---|
| `mi-ficha.php` (mi ficha) | ✔ | ✔ | ✔ |
| `mis-salidas.php` (salidas con sus inscritos) | — | ✔ | ✔ |
| `gestion-usuarios.php` (todos los usuarios) | — | — | ✔ |
| Sin iniciar sesión | redirección al login | | |

Dos puertas, entonces: la primera comprueba que hay alguien (autentificación); la segunda, que ese alguien tiene un rol permitido (autorización). Las dos viven en un único fichero que **toda página privada incluye en su primera línea**, para que sea imposible olvidarlas:

```php
<?php
declare(strict_types=1);
// auth.php — las dos puertas de la zona privada: ¿quién eres? y ¿qué puedes hacer?
session_start();

/** Autenticación: si no hay usuario en sesión, al login. */
function requiere_login(): array {
    if (!isset($_SESSION['usuario'])) {
        header('Location: login.php');
        exit;
    }
    return $_SESSION['usuario'];
}

/** Autorización: el usuario existe, pero ¿tiene uno de los roles permitidos? */
function requiere_rol(string ...$roles): array {
    $usuario = requiere_login();
    if (!in_array($usuario['rol'], $roles, true)) {
        http_response_code(403);
        echo "403 — No tienes permiso para ver esta página.\n";
        exit;
    }
    return $usuario;
}
```

Fíjate en las **respuestas** de cada puerta, que no son iguales y no es casual. Si no hay nadie, se **redirige al login**: la persona probablemente solo tiene que identificarse. Si hay alguien pero no puede, se responde **403 (Forbidden)** con un mensaje: la persona ya está identificada y la aplicación le dice que esa puerta no es suya. Devolver un código de estado correcto importa: el navegador, los registros del servidor y las herramientas de prueba lo entienden; un «no puedes» con un `200` es un error de protocolo. Y `requiere_rol` **empieza llamando a `requiere_login`**: no hay autorización sin autentificación previa.

Las tres páginas de la matriz, cada una con su puerta:

```php
<?php
declare(strict_types=1);
// mi-ficha.php — cualquier usuario autenticado
require __DIR__ . '/auth.php';
$yo = requiere_login();
echo "Ficha de {$yo['nombre']} (rol: {$yo['rol']})\n";
```

```php
<?php
declare(strict_types=1);
// mis-salidas.php — solo monitores y administración
require __DIR__ . '/auth.php';
$yo = requiere_rol('monitor', 'administracion');
$salidas = require __DIR__ . '/datos/salidas.php';
echo "Salidas a la vista de {$yo['nombre']}:\n";
foreach ($salidas as $s) {
    echo "- {$s['nombre']}: {$s['inscritos']}/{$s['plazas']} inscritos\n";
}
```

```php
<?php
declare(strict_types=1);
// gestion-usuarios.php — solo administración
require __DIR__ . '/auth.php';
require __DIR__ . '/almacen.php';
requiere_rol('administracion');
foreach (cargar_usuarios() as $u) {
    echo str_pad($u['usuario'], 14), " ", str_pad($u['rol'], 15), " ", $u['correo'], "\n";
}
```

Y la prueba que importa: un socio recién autenticado (`sara.otal`, la del registro) intenta entrar **escribiendo la dirección a mano** en la página de monitores; después, sin tarro, alguien pide su ficha; y por último una monitora y la administración piden lo suyo:

```
$ curl -i -b tarro.txt http://localhost:8000/mis-salidas.php
HTTP/1.1 403 Forbidden

403 — No tienes permiso para ver esta página.
$ curl -i http://localhost:8000/mi-ficha.php | grep -E "HTTP|Location"
HTTP/1.1 302 Found
Location: login.php
$ curl -c monitora.txt -b monitora.txt -d "usuario=elena.pueyo&clave=monitor2026" http://localhost:8000/login.php > /dev/null
$ curl -b monitora.txt http://localhost:8000/mis-salidas.php
Salidas a la vista de Elena Pueyo:
- Ibón de Estanés: 17/20 inscritos
- Peña Oroel: 25/25 inscritos
- Cañón de Añisclo: 9/15 inscritos
- San Juan de la Peña: 12/30 inscritos
- Ibones de Anayet: 12/12 inscritos
$ curl -b monitora.txt http://localhost:8000/gestion-usuarios.php
403 — No tienes permiso para ver esta página.
$ curl -c junta.txt -b junta.txt -d "usuario=junta&clave=junta2026" http://localhost:8000/login.php > /dev/null
$ curl -b junta.txt http://localhost:8000/gestion-usuarios.php
lucia.ramos    socio           lucia.ramos@example.org
jorge.aisa     socio           jorge.aisa@example.org
marta.gil      socio           marta.gil@example.org
pablo.lobera   socio           pablo.lobera@example.org
nuria.sanz     socio           nuria.sanz@example.org
carlos.mur     monitor         carlos.mur@example.org
elena.pueyo    monitor         elena.pueyo@example.org
junta          administracion  junta@example.org
sara.otal      socio           sara.otal@example.org
```


Nueve usuarios: los ocho del fichero fijo más la socia registrada en el apartado 6, porque `cargar_usuarios()` junta las dos fuentes. Y la lección de fondo, una vez más: escribir la dirección de `mis-salidas.php` en la barra es **el cliente proponiendo**; el servidor mira el rol que **él mismo** guardó en la sesión al autenticar y dispone. El rol no viene del formulario, ni de una cookie, ni de la URL: viene del cajón que el cliente no puede tocar. Si mañana el club necesita un cuarto rol («tesorería»), la matriz gana una columna, `requiere_rol` no cambia y cada página añade o no el rol nuevo a su lista.

Una palabra sobre **perfiles**, porque el título del bloque de contenidos los nombra junto a los roles. En la práctica del sector los términos se solapan: *rol* suele ser la etiqueta de permisos (lo que hemos hecho) y *perfil* el conjunto de datos y preferencias de un usuario (su ficha), aunque hay aplicaciones que llaman perfil a un paquete de roles. Lo que importa es la idea que sostienen ambos: **los permisos se asignan a grupos, no a personas**, y una persona entra en un grupo. Cambiar de rol a un usuario es una operación de administración; cambiar qué puede hacer un rol es una operación de programación. Mantener separadas esas dos cosas es lo que hace gobernable una aplicación con cientos de usuarios.

**Ejercicios del apartado.**

- **E19.** Añade a la matriz una página `publicar-salida.php` accesible a monitores y administración, que muestre un formulario mínimo (nombre, fecha, plazas). Protégela con `requiere_rol` y demuestra con `curl`, con tres tarros (socio, monitor, administración) y sin tarro, los cuatro comportamientos con sus códigos de estado.
- **E20.** `requiere_rol` responde `403` con una línea de texto. Mejora la respuesta para que muestre una página con el nombre del usuario y un enlace a su ficha, **sin** cambiar el código de estado. Comprueba con `curl -i` que sigue siendo `403` y explica por qué el código no debe pasar a `200` aunque la página sea «bonita».
- **E21.** Un compañero propone «simplificar» guardando el rol en una cookie `rol` al hacer login y leyéndolo en cada página. Programa esa versión en `auth-mal.php` **solo para demostrar el problema**: entra como socio, edita la cookie a mano con `-b "rol=administracion"` y pide `gestion-usuarios.php`. Pega el resultado y explica en dos líneas, con la tabla del apartado 1, por qué el rol es información de sesión y no de cookie.

<div style="page-break-before: always;"></div>

## Apartado 8. Proteger la sesión y los formularios

Todo el sistema descansa en una frase del apartado 3: *para el servidor, el usuario autenticado es quien tenga esta llave*. La consecuencia es incómoda: **quien consiga la llave de otro, es el otro**. Este apartado cierra las tres puertas por las que una llave se pierde o se fuerza, y añade una cuarta protección para los formularios. Ninguna es opcional en una aplicación con usuarios; todas caben en dos ficheros pequeños.

**La primera puerta: fijación de sesión.** El ataque es sutil: alguien consigue que entres en el club con una llave que **él ya conoce** (te manda un enlace preparado, o la fija en tu navegador de un ordenador compartido). Tú te autenticas, el servidor anota «esta llave es Lucía»… y el atacante, que tiene la misma llave, es Lucía. Cierra la puerta la línea que ya escribiste en el login: **`session_regenerate_id(true)`** al autenticar cambia la llave por una nueva y borra la vieja. Míralo en las cabeceras — identificador antes y después del inicio de sesión:

```
$ curl -i -c tarro.txt -b tarro.txt http://localhost:8000/login.php | grep Set-Cookie
Set-Cookie: PHPSESSID=bet0evvij27hjkim022bqd6e7h; path=/; HttpOnly; SameSite=Lax
$ curl -i -c tarro.txt -b tarro.txt -d "usuario=lucia.ramos&clave=ibones2026" http://localhost:8000/login.php | grep Set-Cookie
Set-Cookie: PHPSESSID=di3hk2st57v9r7qkdcnp7sgks0; path=/; HttpOnly; SameSite=Lax
```

Dos llaves distintas: la que alguien pudiera haber fijado antes del login ya no abre nada.

**La segunda puerta: secuestro de la cookie.** Si un script malicioso en la página consigue leer `document.cookie`, se lleva la llave; si la llave viaja por HTTP sin cifrar, cualquiera en la red la ve; si otro sitio consigue que tu navegador la envíe en una petición que tú no querías, la usa. Las tres se cierran con los **atributos de la cookie de sesión** que conociste en el apartado 2 — `HttpOnly`, `Secure`, `SameSite` —, y PHP permite fijarlos **antes** de `session_start()` con `session_set_cookie_params()`. Fíjate en que la cookie de la salida anterior ya los lleva: es porque toda la zona privada usa, desde ahora, este arranque de sesión en lugar de la llamada desnuda:

```php
<?php
declare(strict_types=1);
// sesion.php — arranque de sesión endurecido: sustituye a session_start() en toda la zona privada
const INACTIVIDAD_MAX = 15 * 60;   // 15 minutos sin peticiones cierran la sesión

session_set_cookie_params([
    'lifetime' => 0,                         // cookie de sesión: muere al cerrar el navegador
    'path'     => '/',
    'secure'   => isset($_SERVER['HTTPS']),  // solo por HTTPS cuando lo haya (php -S sirve HTTP)
    'httponly' => true,                      // JavaScript no puede leerla
    'samesite' => 'Lax',                     // no viaja en peticiones POST desde otros sitios
]);
session_start();

// Caducidad por inactividad: la última actividad se guarda en la propia sesión
$ahora = time();
if (isset($_SESSION['ultima_actividad']) && ($ahora - $_SESSION['ultima_actividad']) > INACTIVIDAD_MAX) {
    session_unset();
    session_destroy();
    session_start();                         // sesión limpia para quien vuelve
    $_SESSION['flash'] = 'Tu sesión caducó por inactividad. Vuelve a entrar.';
}
$_SESSION['ultima_actividad'] = $ahora;
```

El cambio en el resto de ficheros es de una línea: en `auth.php`, `login.php`, `logout.php` y `registro.php`, donde ponía `session_start();` ahora pone `require __DIR__ . '/sesion.php';`. Un solo sitio decide cómo arranca la sesión; nadie puede olvidarse de un atributo en una página concreta. El atributo `secure` se activa **solo si la petición llegó por HTTPS**: escrito a `true` sin condición, el navegador se negaría a enviar la cookie por el HTTP del aula y la sesión «no funcionaría» sin ningún mensaje de error.

**La tercera puerta: la sesión que nadie cierra.** Un socio entra en un ordenador de la biblioteca, se va sin cerrar sesión, y la llave sigue viva. La **caducidad por inactividad** del fichero anterior lo resuelve con la técnica más simple posible: guardar en la sesión la marca de tiempo de la última petición y, si la siguiente llega demasiado tarde, destruir y empezar de cero con un aviso. La prueba: tras iniciar sesión, forzamos en el servidor una última actividad muy antigua y pedimos la ficha:

```
$ curl -i -c tarro.txt -b tarro.txt http://localhost:8000/mi-ficha.php | grep -E "HTTP|Location"
HTTP/1.1 302 Found
Location: login.php
$ curl -c tarro.txt -b tarro.txt http://localhost:8000/login.php | head -1
<p class="aviso">Tu sesión caducó por inactividad. Vuelve a entrar.</p><form action="" method="post">
```

La cuarta protección no es de la sesión sino de los **formularios**, y tiene nombre propio: **CSRF** (*Cross-Site Request Forgery*, falsificación de petición entre sitios). El escenario: Lucía tiene la sesión abierta en el club; visita otra web que contiene un formulario invisible apuntando a `baja.php` del club y que se envía solo; su navegador manda el POST **con la cookie de sesión del club**, y el servidor, que ve una llave válida, ejecuta la baja. `SameSite=Lax` ya frena la mayoría de estos envíos, pero la defensa completa es un **token**: un secreto aleatorio, guardado en la sesión, que el formulario legítimo lleva en un campo oculto y que el servidor exige de vuelta. Un formulario en otra web no conoce el token — no puede leer la sesión ni la página del club — y su POST llega sin él.

```php
<?php
declare(strict_types=1);
// csrf.php — token anti-CSRF: un secreto por sesión que el formulario devuelve y el servidor comprueba

/** Devuelve el token de esta sesión (lo crea la primera vez). */
function csrf_token(): string {
    $_SESSION['csrf'] ??= bin2hex(random_bytes(32));
    return $_SESSION['csrf'];
}

/** Campo oculto listo para pegar en cualquier formulario POST de la zona privada. */
function csrf_campo(): string {
    return '<input type="hidden" name="csrf" value="' . htmlspecialchars(csrf_token()) . '">';
}

/** Comprueba el token recibido; si no coincide, corta la petición. */
function csrf_verificar(): void {
    $recibido = $_POST['csrf'] ?? '';
    if (!isset($_SESSION['csrf']) || !hash_equals($_SESSION['csrf'], $recibido)) {
        http_response_code(403);
        echo "403 — Petición rechazada: token no válido.\n";
        exit;
    }
}
```

Tres funciones de PHP que aquí estrenas: **`random_bytes`** genera bytes aleatorios de calidad criptográfica (no uses `rand` para esto), **`bin2hex`** los convierte en texto imprimible, y **`hash_equals`** compara dos cadenas en tiempo constante — con `===` una comparación termina antes en cuanto difiere un carácter, y medir ese tiempo permite, en teoría, adivinar el secreto letra a letra. Un formulario protegido — el socio se da de baja de una salida —, con el token en su campo oculto y la comprobación como **primera línea del procesado**:

```php
<?php
declare(strict_types=1);
// baja.php — un socio se borra de una salida: formulario POST protegido con token
require __DIR__ . '/auth.php';
require __DIR__ . '/csrf.php';
$yo      = requiere_login();
$salidas = require __DIR__ . '/datos/salidas.php';

if ($_SERVER['REQUEST_METHOD'] === 'POST') {
    csrf_verificar();                                    // primera línea del procesado: sin token válido, nada
    $indice = filter_var($_POST['salida'] ?? '', FILTER_VALIDATE_INT);
    if ($indice === false || !isset($salidas[$indice])) {
        $_SESSION['flash'] = 'Esa salida no existe.';
    } else {
        $_SESSION['flash'] = "Baja registrada en {$salidas[$indice]['nombre']}.";
    }
    header('Location: baja.php');
    exit;
}

$mensaje = $_SESSION['flash'] ?? null;
unset($_SESSION['flash']);
?>
<?php if ($mensaje !== null): ?><p class="aviso"><?= htmlspecialchars($mensaje) ?></p><?php endif; ?>
<form action="" method="post">
    <?= csrf_campo() ?>
    <label>Salida <select name="salida">
        <?php foreach ($salidas as $i => $s): ?>
        <option value="<?= $i ?>"><?= htmlspecialchars($s['nombre']) ?></option>
        <?php endforeach; ?>
    </select></label>
    <button type="submit">Darme de baja</button>
</form>
```

La prueba de autoridad: el formulario legítimo (con su token), un POST fabricado **sin** token, otro con un token inventado y, por fin, uno con el token real copiado del formulario:

```
$ curl -c tarro.txt -b tarro.txt http://localhost:8000/baja.php | head -2
<form action="" method="post">
    <input type="hidden" name="csrf" value="d1a490fba2ae17be63db8e60a279173f4201d147f29277b0a141ee1e9596436e">    <label>Salida <select name="salida">
$ curl -i -c tarro.txt -b tarro.txt -d "salida=0" http://localhost:8000/baja.php | grep -E "HTTP|^403"
HTTP/1.1 403 Forbidden
403 — Petición rechazada: token no válido.
$ curl -c tarro.txt -b tarro.txt -d "salida=0&csrf=abc123" http://localhost:8000/baja.php
403 — Petición rechazada: token no válido.
$ curl -L -c tarro.txt -b tarro.txt -d "salida=0&csrf=d1a490fba2ae17be63db8e60a279173f4201d147f29277b0a141ee1e9596436e" http://localhost:8000/baja.php | head -1
<p class="aviso">Baja registrada en Ibón de Estanés.</p><form action="" method="post">
```

Con la llave correcta y el token correcto, la baja se registra; con la llave correcta pero sin el token, nada. Eso es lo que un formulario en otro sitio no puede reunir.

Queda decir qué **no** hace esta unidad, para que sepas dónde está cada cosa. No limitamos los intentos de inicio de sesión ni bloqueamos cuentas (en producción se hace, y se hace con cuidado para no dejar sin servicio a un usuario legítimo). No hay **segundo factor** (un código en el móvil) ni cookie «recordarme» — las dos son extensiones del mecanismo que ya tienes, no mecanismos nuevos. Y el **escape de salida** con `htmlspecialchars` (UD2) sigue siendo obligatorio en cada impresión de datos externos, porque el ataque que roba cookies —inyectar un script en tu página— entra precisamente por una salida sin escapar. Lo tienes puesto en todos los ficheros de la unidad; es la protección más barata de todas.

**Ejercicios del apartado.**

- **E22.** Demuestra la fijación de sesión y su cierre: quita `session_regenerate_id(true)` del login, fija tú mismo una llave con `-b "PHPSESSID=llavefijada"` en el POST de inicio de sesión y comprueba, con esa misma llave y sin tarro, que `mi-ficha.php` te reconoce. Restaura la línea, repite y pega ambas secuencias con sus `Set-Cookie`.
- **E23.** Baja `INACTIVIDAD_MAX` a 20 segundos, inicia sesión, espera medio minuto y pide la ficha: pega la redirección y el aviso. Después responde: ¿qué diferencia hay entre esta caducidad y el `lifetime` de la cookie de sesión? ¿Cuál de las dos controla el servidor y cuál el navegador?
- **E24.** Protege con el token el formulario de `inscribir.php` del apartado 4 (recuerda que ahora es zona privada: pasa a incluir `auth.php`). Lanza desde `curl` un POST sin token y otro con token válido, pega los resultados y explica en tres líneas por qué `SameSite=Lax` **no** sustituye al token (pista: piensa en un enlace GET, o en un navegador antiguo, o en una petición que no sea de otro sitio sino de una pestaña abierta con tu propia sesión).

<div style="page-break-before: always;"></div>

## Apartado 9. La puerta a los datos: el almacén de usuarios con PDO (uso instrumental)

Una nota de propagación antes de empezar, porque conviene tenerla clara: en este apartado PDO es una **herramienta**, no el objeto de evaluación. El RA4 pide aplicaciones con autentificación; una aplicación real guarda sus usuarios en una base de datos, y sería extraño terminar la unidad sin haberlo visto. Pero el acceso a datos — conexiones, consultas, actualizaciones, transacciones, su prueba y documentación — es el **RA6**, y se evalúa en la **UD6**. Aquí aprendes lo justo para que el inicio de sesión lea de MySQL: una conexión, una consulta preparada, una función.

Y esa función ya existe: `buscar_usuario()`. En el apartado 6 encerraste todo el conocimiento de «dónde viven los usuarios» en `almacen.php`; el login llama a `buscar_usuario` y no sabe si detrás hay un array, un JSON o una tabla. Cambiar el almacén es escribir **otra versión de esa función** — y nada más. El script que crea la tabla, con las mismas ocho filas y los mismos hashes que `datos/usuarios.php`, está en el repositorio de la unidad (`datos/usuarios.sql`; muestra):

```sql
-- datos/usuarios.sql — tabla de usuarios del Club de Montaña Os Ibones (datos de aula, ficticios)
-- Mismas ocho filas y mismos hashes que datos/usuarios.php. Carga: mysql -u club -p club < usuarios.sql

CREATE TABLE IF NOT EXISTS usuarios (
    id      INT UNSIGNED NOT NULL AUTO_INCREMENT PRIMARY KEY,
    usuario VARCHAR(30)  NOT NULL UNIQUE,
    nombre  VARCHAR(60)  NOT NULL,
    correo  VARCHAR(120) NOT NULL,
    rol     ENUM('socio', 'monitor', 'administracion') NOT NULL DEFAULT 'socio',
    hash    VARCHAR(255) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

INSERT INTO usuarios (id, usuario, nombre, correo, rol, hash) VALUES
    (1, 'lucia.ramos', 'Lucía Ramos', 'lucia.ramos@example.org', 'socio', '$2y$10$pl.c1VhKXo9QiPWHKOUH1OeIaq3s.UCkJ5zRED/O7d5uqkLA4OXv.'),
    -- … (siete filas más en el fichero íntegro, hasta 8)
```

Dos detalles del esquema que ya son de esta unidad: `usuario` es **`UNIQUE`** — la base de datos garantiza lo que el fichero JSON no podía (dos altas simultáneas con el mismo nombre, una fallará) — y `hash` mide 255 caracteres, porque los algoritmos futuros pueden producir hashes más largos que los 60 de bcrypt. La conexión, en un fichero aparte:

```php
<?php
declare(strict_types=1);
// bd.php — conexión PDO a la base de datos del club (uso instrumental en esta unidad; RA6 en la UD6)
function conectar(): PDO {
    $pdo = new PDO('mysql:host=localhost;dbname=club;charset=utf8mb4', 'club', 'club', [
        PDO::ATTR_ERRMODE            => PDO::ERRMODE_EXCEPTION,   // los errores lanzan excepción, no pasan en silencio
        PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,         // filas como arrays asociativos, como en usuarios.php
    ]);
    return $pdo;
}
```

`PDO` es la clase de PHP para hablar con bases de datos; el primer argumento (el *DSN*) dice con cuál y dónde; los dos siguientes, con qué usuario y contraseña de la base de datos (en producción, nunca en el código: se leen de la configuración del servidor); las opciones piden que los errores lancen excepciones y que las filas lleguen como arrays asociativos, iguales a los de `usuarios.php`. Y la función, reescrita:

```php
<?php
declare(strict_types=1);
// almacen-pdo.php — la MISMA función buscar_usuario(), ahora contra la tabla usuarios
require __DIR__ . '/bd.php';

function buscar_usuario(string $usuario): ?array {
    $sentencia = conectar()->prepare('SELECT usuario, nombre, correo, rol, hash FROM usuarios WHERE usuario = :usuario');
    $sentencia->execute(['usuario' => $usuario]);   // el dato viaja como PARÁMETRO, nunca pegado en el SQL
    $fila = $sentencia->fetch();
    return $fila === false ? null : $fila;
}
```

Esto es una **consulta preparada**: el SQL se envía con un hueco con nombre (`:usuario`) y el valor viaja aparte, en `execute`. El gestor nunca interpreta el dato como parte de la consulta, y por eso el ataque clásico de **inyección SQL** — escribir en el campo usuario algo como `x' OR '1'='1` para convertir la condición en verdadera — no tiene efecto: se busca literalmente un usuario llamado así, que no existe. Con el dato pegado en la cadena (`"... WHERE usuario = '$usuario'"`) sí lo tendría. En la UD6 verás el mecanismo a fondo; desde hoy, la regla es que **todo valor externo que llega a una consulta va como parámetro**, siempre.

El login contra la base de datos es `login.php` con una línea distinta — `require __DIR__ . '/almacen-pdo.php';` en lugar de `almacen.php` —, y se comporta exactamente igual, inyección incluida:

```
$ curl -c tarro.txt -b tarro.txt -d "usuario=marta.gil&clave=nada" http://localhost:8000/login-pdo.php | head -1
<p class="error">Credenciales no válidas.</p><form action="" method="post">
$ curl -i -c tarro.txt -b tarro.txt -d "usuario=marta.gil&clave=ibones2026" http://localhost:8000/login-pdo.php | grep -E "HTTP|Location"
HTTP/1.1 302 Found
Location: mi-ficha.php
$ curl -b tarro.txt http://localhost:8000/mi-ficha.php
Ficha de Marta Gil (rol: socio)
$ curl -c tarro.txt -b tarro.txt --data-urlencode "usuario=x' OR '1'='1" -d "clave=x" http://localhost:8000/login-pdo.php | head -1
<p class="error">Credenciales no válidas.</p><form action="" method="post">
```

Si la base de datos no está disponible, `ERRMODE_EXCEPTION` hace que la conexión falle **ruidosamente** en la primera línea de `bd.php` (`Uncaught PDOException: SQLSTATE[HY000] [2002] Connection refused`), en vez de devolver silenciosamente «credenciales no válidas» a todo el mundo. Tratar esa excepción con elegancia es parte de la UD6; en esta unidad, verla te dice dónde mirar.

Lo que ha pasado en este apartado es más importante que PDO: la aplicación **no se ha enterado** del cambio de almacén, porque hablaba con una función y no con un fichero. Esa separación entre *lo que la aplicación hace* y *de dónde saca los datos* es la puerta de la UD5, donde la convertirás en arquitectura.

**Ejercicios del apartado.**

- **E25.** Carga `usuarios.sql` en tu MySQL (en el aula, con las credenciales publicadas), ajusta el DSN de `bd.php` y reproduce las cuatro peticiones del apartado con `curl`. Después comprueba en la base de datos, con una consulta, que la contraseña de `marta.gil` **no** está en la tabla (solo su hash) y explica qué pasaría si un atacante consiguiera un volcado de la tabla.
- **E26.** Escribe `almacen-inseguro.php` con una versión de `buscar_usuario` que **pegue** el dato en la cadena SQL (solo para el experimento). Repite la petición con `--data-urlencode "usuario=x' OR '1'='1"` y observa qué devuelve la función (usa `var_dump` desde el terminal). Vuelve a la versión preparada y explica, en tres líneas, qué convirtió el dato en código y por qué el parámetro lo impide.

<div style="page-break-before: always;"></div>

## Apartado 10. Herramientas de prueba y depuración de la zona privada

Una aplicación con estado tiene una propiedad incómoda para quien la prueba: **el mismo fichero responde distinto según quién lo pide**. Ya no basta con «abrir la página y mirar»: hay que saber *como quién* la abres, qué cookies viajan y qué guardó el servidor. Este apartado ordena las herramientas que llevas usando toda la unidad y las convierte en un banco de pruebas reproducible.

**La pestaña Red del navegador.** Es donde ves las cabeceras de verdad. Con una página abierta, pulsa en la petición y mira en la respuesta `Set-Cookie` (¿lleva `HttpOnly`? ¿`SameSite`?) y en la petición siguiente `Cookie` (¿viajó la llave?). Un `302` con su `Location` aparece como una petición que encadena otra. Es la herramienta para responder «¿qué se envió exactamente?».

**El panel de almacenamiento** (en algunos navegadores, *Aplicación*). Lista las cookies del sitio con sus atributos y te deja **editarlas o borrarlas a mano**. Es el laboratorio de la lección central de la unidad: edita `PHPSESSID` y verás que la sesión desaparece; intenta leer la cookie desde la consola con `document.cookie` y comprueba que la marcada `HttpOnly` **no aparece**. Todo lo que puedas hacer ahí, lo puede hacer un usuario.

**`curl` con tarro.** Es la herramienta para reproducir un flujo entero **sin manos y sin navegador**, y por tanto la que sirve para documentar una prueba y repetirla mañana. El guion completo de aceptación de la zona privada, ejecutado tal cual, con sus salidas:

```
$ curl -s -i -c tarro.txt -b tarro.txt http://localhost:8000/mi-ficha.php | grep -E "HTTP|Location"
HTTP/1.1 302 Found
Location: login.php
$ curl -s -i -c tarro.txt -b tarro.txt -d "usuario=lucia.ramos&clave=ibones2026" http://localhost:8000/login.php | grep -E "HTTP|Set-Cookie|Location"
HTTP/1.1 302 Found
Set-Cookie: PHPSESSID=ngabdss93cn7q9qi5lamf4eo90; path=/; HttpOnly; SameSite=Lax
Location: mi-ficha.php
$ curl -s -b tarro.txt http://localhost:8000/mi-ficha.php
Ficha de Lucía Ramos (rol: socio)
$ curl -s -i -b tarro.txt http://localhost:8000/gestion-usuarios.php | grep -E "HTTP|^403"
HTTP/1.1 403 Forbidden
403 — No tienes permiso para ver esta página.
$ curl -s -i -c tarro.txt -b tarro.txt http://localhost:8000/logout.php | grep -E "HTTP|Set-Cookie|Location"
HTTP/1.1 302 Found
Set-Cookie: PHPSESSID=deleted; expires=Thu, 01 Jan 1970 00:00:01 GMT; Max-Age=0; path=/
Location: login.php
$ curl -s -i -b tarro.txt http://localhost:8000/mi-ficha.php | grep -E "HTTP|Location"
HTTP/1.1 302 Found
Location: login.php
```

Seis peticiones, seis comprobaciones: sin sesión redirige; el login regenera la llave y la endurece; la ficha reconoce; la página de administración deniega con `403`; el cierre borra la llave; y sin llave, otra vez al login. Guardado en un fichero `pruebas.sh`, ese guion es la prueba de regresión de tu zona privada: si mañana tocas `auth.php` y algo cambia, lo ves en diez segundos.

**Mirar dentro de la sesión.** Cuando «no funciona» y no sabes por qué, la pregunta es qué hay en el cajón. Un fichero de depuración temporal en la zona privada lo muestra:

```
$ curl -b monitor.txt http://localhost:8000/depura.php
array(2) {
  ["ultima_actividad"]=>
  int(1789713634)
  ["usuario"]=>
  array(3) {
    ["usuario"]=>
    string(10) "carlos.mur"
    ["nombre"]=>
    string(10) "Carlos Mur"
    ["rol"]=>
    string(7) "monitor"
  }
}
```


`var_dump($_SESSION)` tras `requiere_login()` te dice quién es el servidor cree que eres y cuándo fue tu última actividad. El fichero se llama `depura.php` a propósito: **se borra antes de entregar** — una página que vuelca la sesión es información regalada. El complemento son el fichero `sess_…` de `session.save_path` (apartado 3) y el **registro de errores** del servidor: con `php -S` los avisos aparecen en la terminal donde lo lanzaste; en otros servidores, en el fichero que indique `error_log`. Cuando una cabecera «no llega», la traza `headers already sent` está ahí, aunque el navegador no la muestre.

Un depurador paso a paso (Xdebug integrado en el editor) también sirve para seguir una petición línea a línea con `$_SESSION` a la vista; cómo instalarlo está en el recurso «Entorno de trabajo: VS Code para PHP» del sitio, y no es obligatorio en esta unidad.

**Lista de comprobación de la zona privada.** Antes de entregar cualquier aplicación con usuarios, pasa esta lista con `curl` o con el navegador y anota el resultado de cada punto. Es la que reutiliza la AE10.

| # | Comprobación | Herramienta |
|---|---|---|
| 1 | Una página privada sin sesión redirige al login (`302` + `Location`) | `curl -i` sin tarro |
| 2 | Credenciales incorrectas devuelven un mensaje **genérico** y ningún `Set-Cookie` nuevo de sesión autenticada | `curl -i -d` |
| 3 | Credenciales correctas devuelven `302`, un `Set-Cookie` de sesión con **identificador distinto** al anterior, `HttpOnly` y `SameSite` | `curl -i -c -b` |
| 4 | La contraseña no aparece en ningún fichero del servidor ni en la sesión: solo el hash | `grep`, `var_dump($_SESSION)` |
| 5 | Cada rol ve exactamente sus páginas; las ajenas responden `403` (no `200` con un aviso) | tres tarros |
| 6 | Escribir una URL privada a mano no salta ningún control | `curl` con tarro de socio |
| 7 | Editar a mano la cookie de sesión invalida la sesión (no «abre» otra cuenta) | panel de almacenamiento, `-b` |
| 8 | Ningún dato de negocio (rol, precio, permisos) viaja en una cookie editable | pestaña Red |
| 9 | Los formularios POST de la zona privada rechazan un envío sin token o con token falso (`403`) | `curl -d` sin `csrf` |
| 10 | Recargar tras un POST no repite la acción (PRG: el POST responde `302`) | `curl -i -d` |
| 11 | El cierre de sesión borra la cookie y la llave antigua deja de servir | `curl -i`, luego reintento |
| 12 | Todo dato externo impreso pasa por `htmlspecialchars` | lectura del código, registro con `<b>` en el nombre |

**Ejercicios del apartado.**

- **E27.** Convierte el guion de este apartado en `pruebas.sh` (con un `set -e` al principio y `echo` que separen los pasos), ejecútalo y pega su salida. Rompe después una cosa a propósito (por ejemplo, quita `HttpOnly` en `sesion.php`) y muestra cómo se nota en la salida del guion.
- **E28.** En el panel de almacenamiento de tu navegador, con la sesión iniciada, cambia una letra del valor de `PHPSESSID` y recarga una página privada. Después, en la consola, ejecuta `document.cookie` y captura la salida. Explica qué demuestra cada una de las dos pruebas y qué atributo interviene en la segunda.
- **E29.** Pasa las doce comprobaciones de la lista sobre tu zona privada del club y entrega la tabla con una columna «resultado» y otra «evidencia» (el comando o la captura). Para cada punto que falle, explica qué fichero tocarías.

<div style="page-break-before: always;"></div>

## Apartado 11. Errores frecuentes

1. **`headers already sent`: salida antes de una cabecera.** El error número uno de la unidad, y el más traicionero porque a veces no se ve. Cualquier carácter enviado antes de `header()`, `setcookie()` o `session_start()` — un espacio antes de `<?php`, una línea en blanco tras un `?>` de cierre, un `echo` de depuración — lo provoca:

```
PHP Warning:  Cannot modify header information - headers already sent by (output started at /ruta/cabecera-rota.php:1) in /ruta/cabecera-rota.php on line 4
```

   Lee el aviso al revés: «output started at … :1» dice **dónde empezó la salida**; ahí está el arreglo. Con `session_start()` el mensaje es `Session cannot be started after headers have already been sent`. Recuerda que el servidor embebido puede ocultarlo cuando la salida previa es pequeña: prueba las redirecciones también en el terminal.

2. **Leer la cookie en la misma petición que la crea.** `setcookie()` escribe una cabecera de la **respuesta**; `$_COOKIE` contiene lo que llegó en la **petición**. En la petición que guarda la preferencia, `$_COOKIE['dificultad']` aún no existe:

```
$ curl "http://localhost:8000/preferencia.php?dificultad=baja"
Preferencia guardada: baja
Cookie recibida en esta petición: sin preferencia
```

   No es un fallo: es el orden del protocolo. Si en esa misma petición necesitas el valor, ya lo tienes en la variable que acabas de escribir.

3. **Olvidar `session_start()`.** El más silencioso. Con `$_SESSION['visitas'] ??= 0` **no hay ningún aviso**: `$_SESSION` se comporta como un array normal que nace vacío en cada petición, y el contador devuelve `Visitas: 1` eternamente — la sesión «que no guarda». Solo si lees la variable sin `??` aparece la pista:

```
PHP Warning:  Undefined global variable $_SESSION in /ruta/lee.php on line 2
PHP Warning:  Trying to access array offset on null in /ruta/lee.php on line 2
```

   Diagnóstico: si `$_SESSION` «se olvida de todo», busca la llamada a `session_start()` (o el `require` de `sesion.php`) al principio del fichero. En la zona privada, `auth.php` lo hace por ti; fuera de ella, es responsabilidad tuya.

4. **Comparar la contraseña con el hash usando `===`.** El hash no es la contraseña cifrada: no se puede «descifrar» ni comparar directamente. `'ibones2026' === $hash` da `false` siempre; solo `password_verify('ibones2026', $hash)` sabe extraer la sal y repetir el cálculo. Si tu login rechaza a todo el mundo, mira cómo comparas. (Y si tu «hash» empieza por otra cosa que no sea `$2y$` o similar, revisa cómo lo generaste: `md5` no es un hash de contraseñas.)

5. **Redirigir sin `exit`.** La cabecera `Location` viaja, el navegador se va… pero el script **sigue ejecutándose** y envía el resto de la página en el cuerpo de la respuesta de redirección:

```
$ curl -i http://localhost:8000/sin-exit.php | grep -E "HTTP|Location|SECRETO"
HTTP/1.1 302 Found
Location: login.php
SECRETO: lista completa de socios...
```

   El navegador no lo muestra, pero `curl` (o cualquiera) lo lee. Una página «protegida» así regala su contenido a quien no siga la redirección. Después de cada `header('Location: …')`, `exit`.

6. **Guardar el rol (o cualquier decisión) en una cookie.** Funciona en las pruebas y falla en el primer contacto con alguien que sepa editar cookies:

```
$ curl http://localhost:8000/rol-en-cookie.php
Zona de socios
$ curl -b "rol=administracion" http://localhost:8000/rol-en-cookie.php
Panel de administración
```

   Dos líneas de código, una cookie editada, administrador. El rol vive en la **sesión**, que el cliente no puede tocar; en la cookie solo viaja la llave. Misma regla para precios, permisos, «ya pagado» o cualquier dato del que dependa una decisión.

**Ejercicio del apartado.**

- **E30.** Caza de errores. Escribe una versión de `mi-ficha.php` que contenga **cuatro** de los seis errores de este apartado a la vez (una línea en blanco antes de `<?php`, sin `session_start`, redirección sin `exit`, y el rol leído de una cookie). Sírvela y prueba con `curl -i` sin tarro, con un tarro autenticado y con `-b "rol=administracion"`. Para cada error: cita la línea que lo causa, la evidencia con la que lo detectaste (traza, cabecera o salida) y el arreglo.

<div style="page-break-before: always;"></div>

## Apartado 12. La IA en esta unidad

Puedes usar un asistente de IA en esta unidad con la regla del módulo: **el uso se declara en el `DECISIONES.md` de cada entrega**, y lo que entregas lo defiendes.

- **Usos razonables aquí**: pedir una explicación alternativa de un mecanismo que se resista (por qué la cookie no llega en la misma petición, qué diferencia hay entre fijación y secuestro de sesión, qué es una sal); que te proponga casos de prueba para tu guion de `curl`; o que te ayude a leer una traza de `headers already sent` que no localizas.
- **Lo que debes verificar siempre**: **ejecuta y prueba con `curl`** todo código que te dé un asistente, porque en esta unidad la IA se equivoca de formas peligrosas y muy verosímiles: propone `md5` o `sha1` para contraseñas (o compara hashes con `==`); olvida `exit` tras `header()`; guarda el rol, el usuario o hasta la contraseña en una cookie o en la propia sesión «para tenerlo a mano»; construye consultas SQL pegando el dato en la cadena; y pone `secure => true` sin condición, con lo que la sesión deja de funcionar en tu servidor local sin decir por qué. Contrasta cualquier afirmación sobre sesiones o cookies con el **manual oficial** (apartado 14), no con lo que «recuerde» el asistente: es un tema en el que las recomendaciones han cambiado y los foros viejos abundan.
- **Lo que se te pedirá defender**: la actividad evaluativa se defiende. En particular, tu inicio de sesión (AE6) y tu control de acceso (AE8) los sostendrás explicando **dónde vive cada dato y quién puede tocarlo**, y respondiendo a un «¿y si el usuario edita…?» con la cookie, la URL o un POST fabricado. Si el código lo escribió un asistente y no sabes por qué `session_regenerate_id` está donde está, la defensa lo revela en el primer minuto. Si está en tu entrega, es tuyo — y lo has probado.

<div style="page-break-before: always;"></div>

## Apartado 13. Actividad evaluativa final

**Contexto — la escuela comarcal de música.** La *Escuela Comarcal de Música Val d'Onsella* (centro ficticio de aula) tiene seis **cabinas de estudio** que el alumnado reserva por franjas de una hora, y hasta ahora lo gestiona con una hoja en el tablón y muchos malentendidos. Quieren una **zona privada** en su web: el **alumnado** entra, reserva cabina y ve sus reservas; el **profesorado** consulta las reservas de su familia de instrumentos; **secretaría** gestiona los usuarios. Cada persona tiene una **especialidad** (piano, cuerda, viento…). Los datos son **ficticios** y viven en dos ficheros del repositorio de la unidad: `datos/usuarios-escuela.php` (diez usuarios: seis de alumnado, tres de profesorado y secretaría, con sus contraseñas de aula en el README) y `datos/cabinas.php` (seis cabinas con sus franjas). Trabaja siempre con alias.

Muestra de los dos ficheros (cabecera y cinco filas; el conjunto completo está en el repositorio de la unidad):

```php
<?php
// datos/usuarios-escuela.php — alumnado, profesorado y secretaría de la Escuela Comarcal de Música Val d'Onsella (datos de aula, ficticios)
return [
    1 => ['usuario' => 'ines.bercero', 'nombre' => 'Inés Bercero', 'correo' => 'ines.bercero@example.org', 'rol' => 'alumnado', 'especialidad' => 'violin',
          'hash' => '$2y$10$UE4XLIdLJShwkddH2kFeEuJKPfCVcUCI720BL/DB5asA5tqEUuPeW'],
    2 => ['usuario' => 'hugo.lasala', 'nombre' => 'Hugo Lasala', 'correo' => 'hugo.lasala@example.org', 'rol' => 'alumnado', 'especialidad' => 'piano',
          'hash' => '$2y$10$SW2979SSdV3RGPkCvSJNpu4usFCNka73CVaAzR77ZxsVY80.v0g5W'],
    3 => ['usuario' => 'aroa.maestro', 'nombre' => 'Aroa Maestro', 'correo' => 'aroa.maestro@example.org', 'rol' => 'alumnado', 'especialidad' => 'clarinete',
          'hash' => '$2y$10$JgkPTwYFDCec8Kgr2ukka.R/NujagV5e6vTl1Jd/.q6z7HWAUNd6K'],
    // … (siete filas más en el fichero íntegro, hasta 10)
];
```

```php
<?php
// datos/cabinas.php — cabinas de estudio de la Escuela Comarcal de Música Val d'Onsella (datos de aula, ficticios)
return [
    'C1' => ['nombre' => 'Cabina 1 (piano vertical)', 'familia' => 'piano', 'franjas' => ['16:00', '17:00', '18:00', '19:00']],
    'C2' => ['nombre' => 'Cabina 2 (piano vertical)', 'familia' => 'piano', 'franjas' => ['16:00', '17:00', '18:00']],
    'C3' => ['nombre' => 'Cabina 3 (cuerda)', 'familia' => 'cuerda', 'franjas' => ['16:00', '17:00', '18:00', '19:00', '20:00']],
    'C4' => ['nombre' => 'Cabina 4 (cuerda)', 'familia' => 'cuerda', 'franjas' => ['17:00', '18:00', '19:00']],
    'C5' => ['nombre' => 'Cabina 5 (viento)', 'familia' => 'viento', 'franjas' => ['16:00', '18:00', '19:00', '20:00']],
    // … (una fila más en el fichero íntegro, hasta 6)
];
```

**Instrucciones.** 10 ejercicios, 1 punto cada uno; se responde **con código que se ejecuta** (los fragmentos que no pasen `php -l` no puntúan) y, donde se pida, razonando la decisión. **Tiempo estimado: 3 horas**, más la preparación de evidencias. **Entrega**: carpeta `ud3/` en tu repositorio de la unidad del aula de código, con un fichero por ejercicio donde tenga sentido (`ae2.php`, `ae3.php`…) y los ficheros comunes de la aplicación (`sesion.php`, `auth.php`, `csrf.php`, `login.php`…), las evidencias en `ud3/evidencias/` (salidas de `curl` en texto, capturas de la pestaña Red y del panel de almacenamiento), commits por bloques de ejercicios y el `DECISIONES.md` actualizado (incluida la declaración de uso de IA). **Defensa individual de 4–5 minutos** según el calendario publicado: sin defensa, la actividad no puntúa.

- **AE1** `[RA4.a]` La escuela necesita recordar tres cosas de cada persona que usa la web: (a) el **instrumento preferido** para filtrar las cabinas al entrar, (b) la **reserva en curso** mientras elige cabina y franja, y (c) **quién es** una vez identificada. Para cada una elige el mecanismo (cookie, sesión, parámetro en la URL…) y justifícalo con **una ventaja, un límite y quién controla el dato**, usando la tabla del apartado 1. Sin código: es la justificación del diseño.
- **AE2** `[RA4.a, RA4.c]` Escribe `ae2.php`: guarda en una cookie `instrumento` la especialidad preferida (validada contra la lista de familias de `cabinas.php`), léela en la petición siguiente para mostrar solo las cabinas de esa familia, y permite borrarla. Captura con `curl -i` la cabecera `Set-Cookie` de la creación y la del borrado, y explica cada atributo elegido y por qué esta cookie **no** contiene nada sensible.
- **AE3** `[RA4.b]` Escribe `ae3.php`: una lista de **cabinas consultadas** que crece en la sesión con cada `?ver=C3` (sin duplicados, solo cabinas existentes) y se vacía con `?olvidar=1`. Reproduce con `curl` y un tarro una secuencia de cinco peticiones, pega las salidas y añade una sexta **sin tarro** que demuestre que otro cliente no ve tu lista. Abre el fichero `sess_…` del servidor y pega su contenido.
- **AE4** `[RA4.b]` Implementa la reserva de cabina con el patrón **Post/Redirect/Get**: `reservar.php` recibe el POST (cabina y franja, validadas contra `cabinas.php`), guarda un mensaje de un solo uso y redirige a `mis-reservas.php`, que lo muestra una sola vez. Demuestra con `curl -i` que el POST responde `302` sin cuerpo, y con dos GET seguidos que el mensaje aparece una vez y desaparece. Explica por qué recargar `mis-reservas.php` no duplica la reserva.
- **AE5** `[RA4.d]` **Caracteriza** los tres mecanismos de autentificación del apartado 5 en una tabla propia (cómo viajan las credenciales, dónde se guarda el estado, cómo se cierra sesión, para qué caso los usarías) y **demuestra** la autenticación HTTP básica con `curl -u` sobre `datos/usuarios-escuela.php`, capturando la cabecera `Authorization` y decodificándola. Después escribe `ae5-hash.php`, que calcule dos veces el hash de la contraseña de aula de un usuario y verifique ambos: explica por qué difieren, qué significa el prefijo `$2y$…` y por qué `md5` no sirve.
- **AE6** `[RA4.e, RA4.d]` Construye `login.php` y `logout.php` para la escuela sobre `usuarios-escuela.php`, con `password_verify`, `session_regenerate_id(true)` al autenticar, mensaje de error **genérico** y lo mínimo en sesión (usuario, nombre, rol, especialidad). Evidencia con `curl`: credenciales incorrectas, credenciales correctas (con el `Set-Cookie` que muestra el cambio de identificador) y cierre de sesión (con el `Set-Cookie` de borrado).
- **AE7** `[RA4.e]` Escribe `registro.php` para el alumnado nuevo: validación completa **en el servidor** (usuario con formato y no repetido, nombre, correo válido, especialidad de la lista, contraseña de longitud mínima), **minimización de datos** (justifica en un comentario qué campos no pides y por qué), rol fijado por el servidor y hash al guardar en `datos/usuarios-escuela.json` con bloqueo. Pega el JSON resultante y explica, en tres líneas, qué límite tiene este almacén y qué lo resuelve en la UD6.
- **AE8** `[RA4.e, RA4.b]` Control de acceso por rol con `auth.php` (`requiere_login`, `requiere_rol`): `mis-reservas.php` (cualquier usuario autenticado), `reservas-familia.php` (profesorado y secretaría: reservas de las cabinas de su familia de instrumentos), `gestion-usuarios.php` (solo secretaría). Escribe la matriz de permisos y **demuestra los cuatro comportamientos** (sin sesión, alumnado, profesorado, secretaría) sobre la página de secretaría con `curl -i`, incluido el `403` real. Añade la prueba de la URL escrita a mano.
- **AE9** `[RA4.e, RA4.c]` Endurece la aplicación: `sesion.php` con la cookie de sesión `HttpOnly` + `SameSite=Lax` (+ `Secure` condicional) y **caducidad por inactividad**; token **anti-CSRF** en el formulario de reserva. Evidencias: la cabecera `Set-Cookie` de la sesión con sus atributos; un POST de reserva **sin token** rechazado con `403`; y la caducidad provocada (bajando el límite o forzando la marca de tiempo), con su aviso en el login. Explica qué ataque cierra cada una de las tres medidas.
- **AE10** `[RA4.f, RA4.e]` Ejecuta la **lista de comprobación** del apartado 10 (los doce puntos) sobre tu zona privada de la escuela y entrega la tabla con **resultado y evidencia** de cada punto (comando `curl` y su salida, o captura del navegador), más el guion `pruebas.sh` que reproduce el flujo completo (login → página propia → página ajena → logout). Se defiende sin leer: en la defensa se te pedirá ejecutar el guion y explicar dos comprobaciones a elección del docente.

<div style="page-break-before: always;"></div>

## Apartado 14. Para ampliar

- [`setcookie` (Manual de PHP, en español)](https://www.php.net/manual/es/function.setcookie.php) — la referencia oficial de la función del apartado 2: parámetros, el array de opciones con sus atributos, el aviso de que debe llamarse antes de cualquier salida, y los ejemplos canónicos de creación y borrado.
- [`session_start` (Manual de PHP, en español)](https://www.php.net/manual/es/function.session-start.php) — punto de entrada al capítulo de sesiones: qué hace la función, el ejemplo de dos páginas que comparten `$_SESSION`, y los enlaces a las directivas de configuración (`session.save_path`, cookie de sesión) y al resto de funciones que usaste en los apartados 3, 6 y 8.
- [Hash de contraseñas seguro (Manual de PHP, en español)](https://www.php.net/manual/es/faq.passwords.php) — la explicación oficial de por qué se hashean las contraseñas, por qué `md5` y `sha1` no valen, qué es la sal y cómo `password_hash` y `password_verify` lo resuelven: el fundamento del apartado 5.
- [`header` (Manual de PHP, en español)](https://www.php.net/manual/es/function.header.php) — la función del apartado 4: la redirección con `Location` (y su `302` automático), la advertencia sobre la salida previa y el recordatorio de terminar con `exit`.
- [Consultas preparadas y procedimientos almacenados (Manual de PHP, en español)](https://www.php.net/manual/es/pdo.prepared-statements.php) — qué es una consulta preparada, por qué protege de la inyección SQL y los ejemplos con marcadores con nombre: la referencia del apartado 9, que retomarás a fondo en la UD6. (Las páginas aún sin traducir se muestran en inglés.)
