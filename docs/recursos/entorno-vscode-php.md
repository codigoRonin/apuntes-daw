# Tu entorno de trabajo: Visual Studio Code para PHP

**Recurso transversal — Módulo 0613, Desarrollo Web en Entorno Servidor · 2.º DAW · IES Río Arba · Curso 2026-27**

Esta guía monta, una sola vez, el entorno con el que vas a trabajar todo el módulo: el editor **Visual Studio Code** afinado para PHP. No pertenece a ninguna unidad concreta —la consultas cuando la necesites— y es independiente del temario: en los apuntes solo verás comandos del lenguaje (`php -v`, `php -l`, `php -S`), nunca instrucciones atadas a un editor. Si mañana cambiaras de editor, tus apuntes seguirían valiendo; esta guía es la que cambiaría.

Una idea antes de empezar, porque evita la confusión más común: **el editor y el intérprete son dos cosas distintas.** Visual Studio Code es donde escribes; PHP es el programa que ejecuta lo que escribes. VS Code **no trae PHP dentro**: se instalan por separado. El editor, con sus extensiones, te ayuda a escribir (autocompletado, avisos, formato); pero quien ejecuta tu código sigue siendo el intérprete que instalaste, desde el terminal.

## Antes de empezar: PHP instalado

El intérprete de PHP ya lo instalaste en la primera unidad del curso. Comprueba que responde abriendo un terminal y escribiendo:

```
php -v
```

Si te muestra una línea con el nombre y los datos de tu PHP, estás listo. Si te dice que la orden `php` no se reconoce, el intérprete no está en el PATH del sistema: repasa el apartado de instalación de la primera unidad antes de seguir. Sin un `php -v` que responda, el editor puede quedar precioso pero no ejecutarás nada.

## Instalar Visual Studio Code

Descarga VS Code de su web oficial, [code.visualstudio.com](https://code.visualstudio.com/), y sigue el instalador de tu sistema operativo (funciona igual en Windows, macOS y Linux). Es gratuito y de código abierto. En Windows, durante la instalación conviene marcar la casilla de "Agregar a PATH" y las de "Abrir con Code" en el menú contextual: te ahorrarán clics después.

Cuando lo abras por primera vez, familiarízate con dos zonas que usarás siempre: la **barra lateral de la izquierda** (el explorador de archivos y el panel de extensiones) y el **terminal integrado**, que abres con `Ver → Terminal` (o `` Ctrl+` ``). Ese terminal integrado es un terminal normal y corriente dentro del editor: es donde lanzarás `php -l` y `php -S`, sin salir de VS Code.

## Las extensiones que vas a instalar

Las extensiones se instalan desde el **panel de extensiones** (el icono de piezas en la barra lateral, o `Ctrl+Shift+X`): escribe el nombre, comprueba el autor y pulsa *Instalar*. Para que no haya confusión con extensiones de nombre parecido, junto a cada una te doy su **identificador** exacto (con el formato `autor.nombre`), que también puedes pegar en el buscador del panel.

Instala estas tres, y solo estas, para el módulo:

- **PHP Intelephense** (`bmewburn.vscode-intelephense-client`) — es el **motor** del editor para PHP: autocompletado, ir a la definición de una función, buscar dónde se usa algo, avisos de errores mientras escribes y formato del código. Es la pieza central; todo lo demás es accesorio. Se instala gratis con todas las funciones que el curso necesita; tiene además funciones "premium" opcionales de pago que **no hacen falta** aquí, así que no compres nada.
- **PHP DocBlocker** (`neilbrayfield.php-docblocker`) — cuando escribes `/**` encima de una función y pulsas Enter, te genera el esqueleto del comentario de documentación con sus etiquetas. Encaja con los comentarios de bloque que ves en los apuntes; es cómodo y no molesta.
- **EditorConfig for VS Code** (`EditorConfig.EditorConfig`) — respeta un fichero de reglas de formato (indentación, fin de línea) compartido por todo el equipo del aula. Más abajo verás por qué importa en clase.

## La configuración que de verdad importa: un solo motor de PHP

Este es el punto donde más gente tropieza, así que léelo con calma. El editor solo debe tener **un** "motor de lenguaje" de PHP funcionando. Si tienes dos a la vez, se pelean: verás **autocompletado duplicado**, avisos de error contradictorios y el editor irá más lento. Vamos a dejar Intelephense como único motor.

**Paso 1 — si tienes instalada la extensión "PHP IntelliSense" (`felixfbecker.php-intellisense`), desinstálala.** Hace el mismo trabajo que Intelephense y entra en conflicto directo con él. Un solo motor: Intelephense.

**Paso 2 — desactiva el soporte de PHP que trae VS Code de fábrica.** VS Code incluye un pequeño soporte propio para PHP que también duplica los avisos. La forma recomendada (la indica la propia documentación de Intelephense): en el panel de extensiones, escribe `@builtin php` en el buscador; aparecerán dos extensiones internas. Deshabilita **"PHP Language Features"** (es la que genera avisos y duplica el análisis) y **deja activada "PHP Language Basics"** (solo aporta el coloreado de sintaxis, que sí quieres conservar).

**Paso 3 — ajusta cuatro opciones.** Abre la configuración en formato JSON (`Ctrl+Shift+P` → *Preferences: Open User Settings (JSON)*) y añade:

```json
{
  "php.validate.enable": false,
  "editor.formatOnSave": true,
  "[php]": {
    "editor.defaultFormatter": "bmewburn.vscode-intelephense-client"
  }
}
```

Qué hace cada línea: la primera es un cinturón de seguridad extra que apaga el validador incorporado (por si el paso 2 no bastara); la segunda formatea el fichero cada vez que guardas; y el bloque `[php]` le dice a VS Code que, para ficheros PHP, el encargado de formatear es Intelephense. Con esto, guardar un `.php` deja el código ordenado y con un único criterio.

Fíjate en que **no fijamos aquí la versión del lenguaje**: Intelephense funciona bien con la que traigas, y en este ciclo trabajamos siempre con la versión soportada actual de cada herramienta, sin clavar números que envejecen.

## Lanzar tu servidor de desarrollo

Para ver tus páginas en el navegador necesitas el servidor de desarrollo de PHP. En este módulo lo lanzas **desde el terminal integrado**, y esto es a propósito: queremos que entiendas el mecanismo, no que aprietes un botón mágico. Abre el terminal (`` Ctrl+` ``), sitúate en la carpeta de tu proyecto y escribe:

```
php -S localhost:8000
```

Deja esa terminal abierta (el servidor se queda "escuchando") y abre el navegador en `http://localhost:8000`. Para pararlo, vuelve a la terminal y pulsa `Ctrl+C`. Si al arrancar te dice que la dirección ya está en uso, es que dejaste otro servidor abierto: ciérralo con `Ctrl+C` en su terminal antes de relanzar.

Existe una extensión (**PHP Server**) que lanza ese mismo servidor con un botón. Puedes instalarla más adelante como atajo, pero solo **después** de tener claro qué hace por debajo —que es exactamente ese `php -S`—. Primero el mecanismo; el atajo, cuando ya no lo necesites.

## Consistencia en el aula: el fichero `.editorconfig`

Cuando muchas personas trabajan sobre los mismos repositorios con equipos distintos, aparece el clásico "a mí me sale con otra sangría". La extensión EditorConfig resuelve esto: si en la raíz del repositorio hay un fichero llamado `.editorconfig` con las reglas de formato, todos los editores las respetan por igual. Un `.editorconfig` sencillo para trabajar con PHP:

```ini
root = true

[*]
charset = utf-8
end_of_line = lf
insert_final_newline = true
trim_trailing_whitespace = true
indent_style = space
indent_size = 4
```

Con ese fichero en el repositorio, tu código y el de tus compañeros saldrán con la misma indentación (cuatro espacios), el mismo fin de línea y sin espacios sobrantes al final de las líneas, sin que nadie tenga que acordarse de configurarlo. Puedes leer más sobre el formato en [editorconfig.org](https://editorconfig.org/).

## Para más adelante (no en esta unidad)

Estas dos piezas son buenas, pero **no** las necesitas todavía; te las nombro para que sepas que existen y no las instales antes de tiempo:

- **PHP Debug (Xdebug)** — permite pausar el programa en una línea y ver el valor de cada variable paso a paso (depuración con puntos de ruptura). Es potente, pero requiere **instalar Xdebug aparte** (no viene con PHP) y su sitio está en unidades más avanzadas. En este módulo depuramos con `var_dump()` desde el terminal, que para lo que hacemos es más que suficiente.
- **Database Client** — un cliente de bases de datos dentro del propio editor. Te será útil en el **módulo de Bases de Datos** y, dentro de este módulo, cuando lleguemos a la unidad que trabaja con MySQL. En las primeras unidades de DWES no hay base de datos, así que de momento no entra en juego.

## Si algo no funciona

- **Ves cada sugerencia de autocompletado por duplicado** → tienes dos motores de PHP activos. Revisa el "Paso 1" y el "Paso 2" de la configuración: desinstala PHP IntelliSense y deshabilita "PHP Language Features".
- **No aparece ningún autocompletado ni aviso** → comprueba que Intelephense está instalado y activado, y que el archivo está guardado con extensión `.php` (el motor solo actúa sobre ficheros PHP).
- **El terminal dice que `php` no se reconoce** → el intérprete no está en el PATH: es el mismo caso que resolviste en la instalación de la primera unidad, no un problema del editor.
- **`php -S` dice que la dirección ya está en uso** → hay otro servidor tuyo abierto en ese puerto; ciérralo con `Ctrl+C` en su terminal.
