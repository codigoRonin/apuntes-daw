# Cuaderno de prácticas · Desarrollo Web en Entorno Servidor

**Módulo 0613 — Desarrollo Web en Entorno Servidor · 2.º DAW · IES Río Arba · Curso 2026-27**

Este cuaderno reúne **ejercicios extra de clase**: prácticas más largas que los ejercicios de cada apartado de los apuntes, pensadas para trabajarlas en el aula con el enunciado delante y el intérprete abierto. **No son evaluables**: no se entregan por ningún canal ni cuentan para la nota. Sirven para lo que sirven los apuntes cuando dicen «ejecuta, rompe y arregla»: para que el método te salga solo cuando llegue la actividad evaluativa y el examen.

Cómo funciona:

- Cada práctica lleva el código **CPu.n**: cuaderno de prácticas, unidad *u*, número *n*. CP2.1 es la primera práctica de la UD2.
- Se trabaja **en clase**, de forma individual o en pareja, según se indique ese día. Lo que no te dé tiempo a terminar en el aula lo acabas en casa y lo traes a la sesión de corrección.
- **La solución no está aquí.** Se corrige en clase y, después de corregirla, el docente la publica en el Classroom del módulo, con los ficheros resueltos para que los ejecutes y los compares con los tuyos.
- Las prácticas se hacen con el mismo entorno de la unidad: `php -l` antes de ejecutar, `php -S` para servir y el navegador con su pestaña Red. Trabaja en una carpeta local propia (por ejemplo `cuaderno/ud2/cp2-1/`), fuera de tu repositorio de la unidad en el aula de código, para no mezclarlas con la entrega evaluable. Los ficheros de partida de las prácticas que los necesitan están en el repositorio de recursos (enlace en Classroom).
- Cada práctica indica su **nivel** (básico, intermedio, avanzado o tipo examen) y un **tiempo orientativo**. Son acumulativas: cada una puede usar todo lo visto en las anteriores.
- **Casi todas traen un error que cazar** en su fichero de partida, del tipo de los del apartado de errores frecuentes de cada unidad. Encontrarlo, entender el mensaje y arreglarlo es parte de la práctica.
- Puedes usar un asistente de IA en estas prácticas, con cabeza y con responsabilidad sobre el resultado, con la misma regla del módulo: el uso se declara —aquí, de palabra, cuando se corrige en clase, porque no hay entrega— y lo que traes lo explicas. Ejecuta y comprueba todo código que te dé un asistente: la IA se equivoca con soltura en detalles de PHP (ver el apartado 12 de cada unidad).

El cuaderno crece durante el curso: solo aparecen las unidades que tienen prácticas extra, y cada unidad va ganando prácticas a medida que se trabajan en clase.

## UD2 · Código embebido en lenguajes de marcas: sintaxis, estructuras, funciones y formularios

Las prácticas de esta unidad trabajan sobre un mismo caso, distinto del club de montaña de los apuntes y de la protectora de la actividad evaluativa: la **Asociación de Huertos del Soto**, una asociación vecinal ficticia que gestiona ocho parcelas de huerto en Tauste. Cada parcela tiene un código, está en una zona del huerto (Ribera, Eras o Soto), mide unos metros cuadrados, se riega por goteo o a manta, y está adjudicada a un socio o libre. Los datos son de aula, inventados. Con ellos se repasa la unidad apartado a apartado, de CP2.1 a CP2.10, y se cierra con un simulacro de examen, el CP2.11.

### CP2.1 · Dos ficheros, dos caminos

**Nivel** básico · **Tiempo orientativo** 15 min.

**Qué refuerza.** El apartado 1 de la [UD2](ud2-codigo-embebido-php.md): el intérprete y su servidor embebido, `php -l` como primer reflejo, las etiquetas `<?php ?>` y `<?= ?>`, la convención de omitir el cierre en los ficheros de PHP puro, y los comentarios PHP y HTML con sus dos destinos.

**Ficheros de partida.** `cp2-1/tablon.php` y `cp2-1/cartel.php`, en el repositorio de recursos (enlace en Classroom). Cópialos a tu carpeta de trabajo.

**Enunciado.** La asociación tiene dos textos que mantener: el tablón de anuncios, que el secretario imprime desde el terminal, y el cartel de la puerta, que se sirve como página. El primero se ha roto y el segundo cuenta más de lo que debe.

**Tareas.**

- **(a) El tablón, por el terminal.** Pásale `php -l` a `tablon.php` **antes** de ejecutarlo. Copia el mensaje completo y localiza con él el error: fíjate en que el mensaje te da una línea de inicio, no la línea donde «falta» algo. Arréglalo, vuelve a pasar `php -l` y ejecútalo con `php tablon.php`. Pega la salida como comentario al final del propio fichero y comprueba que el fichero sigue sin etiqueta de cierre.
- **(b) El cartel, servido.** Lanza `php -S localhost:8000` en la carpeta y pide `http://localhost:8000/cartel.php` con el navegador. Abre el código fuente recibido y márcalo línea a línea: cuáles del fichero original viajaron tal cual, cuáles se transformaron y cuáles no viajaron. Mira también la pestaña Red: ¿qué código de estado y qué tipo de contenido trae la respuesta?
- **(c) Lo que no debe viajar.** En el código fuente recibido hay un dato que la asociación no quiere publicar. Cámbialo de sitio para que **se quede en el servidor** sin borrarlo (el secretario quiere seguir viéndolo cuando edita el fichero), vuelve a pedir la página y demuestra con el código fuente recibido que ya no viaja. Explica en una línea qué diferencia hay entre los dos tipos de comentario de este fichero, en términos del reparto cliente/servidor.
- **(d) Dos caminos, un fichero.** Ejecuta `cartel.php` también por el terminal (`php cartel.php`) y compara su salida con la respuesta servida. ¿Qué es idéntico y qué cambia? ¿Quién añade la línea de estado y las cabeceras que ves en la pestaña Red?

**Cómo trabajar.** Primero `php -l`, siempre. Cuando el comprobador te dé una línea, abre el fichero por ahí y lee hacia arriba y hacia abajo: el número que te da es donde el intérprete se dio cuenta, no necesariamente donde está la causa. Para ver el código fuente recibido usa «Ver código fuente» del navegador o la pestaña Red, nunca el inspector de elementos (el inspector te enseña el árbol ya interpretado, no los bytes que llegaron).

### CP2.2 · La ficha de la parcela

**Nivel** básico · **Tiempo orientativo** 15 min.

**Qué refuerza.** El apartado 2 de la [UD2](ud2-codigo-embebido-php.md): los cuatro tipos escalares y `null` con `var_dump`, comillas simples frente a dobles, la interpolación con llaves, la concatenación, bytes frente a caracteres, el formato español con `number_format` y la comparación estricta.

**Fichero de partida.** `cp2-2/ficha.php`, en el repositorio de recursos (enlace en Classroom). Tiene cinco bloques numerados.

**Enunciado.** La ficha de la parcela H06 reúne, en un solo fichero, todo lo que el apartado 2 enseña sobre variables y cadenas. El último bloque busca un código en la lista de espera de la asociación —donde conviven códigos antiguos con un cero delante (`010`) y códigos nuevos (`10`), que son parcelas distintas— y encuentra más de lo que debería.

**Tareas.**

- **(a) Predice sobre papel.** Antes de ejecutar nada, escribe la salida exacta que esperas de cada uno de los cinco bloques, línea a línea. En el bloque 1 incluye el tipo y, en la cadena, el número que `var_dump` va a mostrar entre paréntesis.
- **(b) Ejecuta y compara.** Pásale `php -l` y ejecuta `php ficha.php`. Para cada línea que no coincida con tu predicción, escribe el concepto que la explica: interpolación, literalidad de las comillas simples, llaves, concatenación, bytes frente a caracteres, formato. Si tu intérprete no tiene la extensión `mbstring`, el bloque 3 fallará con una función indefinida: anota el mensaje, es también un dato (y explica por qué los apuntes insisten en `mb_strlen` para texto en español).
- **(c) Caza el error.** El bloque 5 encuentra dos parcelas cuando solo busca una. Explica por qué, con el vocabulario del apartado 2 (qué hace PHP al comparar dos cadenas numéricas con `==`), y arréglalo con la comparación que la regla de la casa manda. Confirma que ahora solo aparece `10`.
- **(d) Lo mismo sin bucle.** Sustituye la búsqueda por `in_array` con su tercer argumento a `true` y comprueba con `var_dump` tres búsquedas: `'10'`, `'010'` y `'0010'`. Razona qué habría pasado con la tercera si el tercer argumento fuera `false`.

**Cómo trabajar.** La predicción escrita es la parte que vale: si ejecutas primero, el ejercicio se reduce a copiar. Ten a mano la tabla de transferencia Java → PHP del apartado 2 y la lista de valores falsos del apartado 4 para la tarea (c) del CP2.4.

### CP2.3 · La directiva que para en la frontera

**Nivel** básico · **Tiempo orientativo** 15 min.

**Qué refuerza.** El apartado 3 de la [UD2](ud2-codigo-embebido-php.md): `declare(strict_types=1)` como directiva que modifica el comportamiento predeterminado, la coerción frente al `TypeError`, y la configuración del intérprete (`php --ini`, opción `-d`).

**Fichero de partida.** `cp2-3/riego.php`, en el repositorio de recursos (enlace en Classroom).

**Enunciado.** La asociación cobra el agua por horas de riego. La función `coste_riego(int $horas, float $precio_hora): float` calcula el importe, y el fichero la llama con los dos valores **como cadenas**, que es exactamente como llegarán de un formulario en el apartado 9. Alguien ha añadido la directiva de tipos estrictos, pero el fichero ya no pasa el comprobador.

**Tareas.**

- **(a) Caza el error.** Pásale `php -l`. Lee el mensaje con calma: te dice qué regla de colocación tiene la directiva. Arregla el fichero moviendo **una sola línea** y confirma que `php -l` pasa.
- **(b) Sin directiva y con directiva.** Ejecuta el fichero arreglado: pega la salida completa. Después haz una copia `riego-sin-directiva.php` sin la línea de la directiva y ejecútala: pega su salida. Identifica en el mensaje del `TypeError` las **cuatro informaciones** que te da (qué función, qué argumento, qué tipo esperaba y desde qué línea se llamó) y escribe en dos líneas qué comportamiento predeterminado modificó la directiva.
- **(c) Cómo lo vería producción.** Ejecuta `php -d display_errors=0 -d log_errors=0 riego.php` y, a continuación, consulta el código de salida del proceso (`echo $?` en macOS y Linux; `echo %errorlevel%` en el símbolo del sistema de Windows; `echo $LASTEXITCODE` en PowerShell). Sin ningún mensaje en pantalla, ¿cómo sabe quien lanza el programa que ha fallado? Ejecuta también `php --ini` y anota qué fichero de configuración carga tu intérprete.
- **(d) La conversión, a conciencia.** Haz una tercera copia `riego-convertido.php` que mantenga la directiva y **convierta antes de llamar**: las horas con `filter_var` y `FILTER_VALIDATE_INT` (comprobando el fallo con `=== false`, como enseña el apartado 10), y la tarifa tomada de una constante del fichero, no del valor recibido. Explica en una línea por qué la tarifa no debe venir de quien envía el formulario.

**Cómo trabajar.** La tarea (d) adelanta una herramienta del apartado 10; si todavía no lo has leído, déjala para la sesión de corrección. Lo importante hoy es la (b): saber leer un `TypeError` y entender que la directiva no «rompe» el programa, lo hace fallar donde toca.

### CP2.4 · Tramos y etiquetas

**Nivel** intermedio · **Tiempo orientativo** 20 min.

**Qué refuerza.** El apartado 4 de la [UD2](ud2-codigo-embebido-php.md): `if`/`elseif`/`else` con las ramas en orden, `match` como expresión con agrupación de casos y `default`, y la veracidad de los valores que no son booleanos.

**Fichero de partida.** `cp2-4/tramos.php`, en el repositorio de recursos (enlace en Classroom).

**Enunciado.** La cuota anual de una parcela depende de su superficie por tramos —hasta 50 m², 30 €; de 51 a 80, 45 €; más de 80, 60 €— y cada parcela lleva una etiqueta según su tipo de riego. El fichero resuelve las dos cosas con cadenas `if`/`elseif`/`else`, pero una de las dos cadenas esconde un fallo que solo asoma con un valor que no está previsto.

**Tareas.**

- **(a) Caza el error.** Ejecuta el fichero con los tres valores de superficie que indica la cabecera (40, 65 y 100) y con los tres de riego (`goteo`, `manta` y `aspersion`). Con `aspersion` la etiqueta es incorrecta y, además, la última línea de la salida delata que ha pasado algo más. Explica qué hace PHP en esa condición, por qué no se detecta con `php -l` y arréglalo. Comprueba las seis combinaciones.
- **(b) De `if` a `match`.** Reescribe la etiqueta del riego como una expresión `match` que agrupe `goteo` y `microaspersion` bajo «riego localizado», dé a `aspersion` «riego por aspersión» y a `manta` «riego a manta», y cubra el resto con `default`. Ejecuta con `goteo`, `manta`, `aspersion` y `microaspersion` y anota en cuáles difiere de la cadena `if` de la tarea (a) y por qué (pista: la cadena `if` no sabía nada de esos dos valores). Escribe en una línea qué te da `match` frente a la cadena `if` ante el error de la tarea (a): ¿puede pasarle lo mismo?
- **(c) Lo que llega de un formulario.** En la asociación, el campo «¿recibo pagado?» de un formulario llega como la cadena `'0'` cuando no está marcado. Añade al final del fichero una variable `$pagado = '0'` y un `if ($pagado)` que imprima «Recibo pagado» o «Recibo pendiente». Ejecuta, y comprueba con `var_dump` qué vale `(bool) $pagado`, qué vale `(bool) 'no'` y qué vale `$pagado === '0'`. Escribe en una línea qué pregunta hay que hacerle a un dato de formulario en vez de «¿eres verdad?».

**Cómo trabajar.** El error de la tarea (a) es de los que «funcionan» en todas tus pruebas hasta que llega el valor imprevisto: por eso se prueban siempre los valores que no están en la lista, no solo los que sí. Cuando reescribas con `match`, recuerda que es una **expresión**: su valor se asigna, no se ejecuta como una sentencia.

### CP2.5 · Recorrer el huerto

**Nivel** intermedio · **Tiempo orientativo** 20 min.

**Qué refuerza.** El apartado 5 de la [UD2](ud2-codigo-embebido-php.md): arrays indexados y asociativos, `foreach` con y sin clave, acumuladores y recuentos, `while` para «hasta el primero que cumple», `in_array` estricto e `implode`.

**Ficheros de partida.** `cp2-5/datos/parcelas.php` (las ocho parcelas) y `cp2-5/recorrido.php`, en el repositorio de recursos (enlace en Classroom). Muestra del fichero de datos (cabecera y cinco filas; fichero íntegro en el repositorio de recursos):

```php
<?php
// datos/parcelas.php — parcelas de la Asociación de Huertos del Soto (datos de aula, ficticios)
// Este fichero devuelve el array: cárgalo con  $parcelas = require __DIR__ . '/datos/parcelas.php';
return [
    ['codigo' => 'H01', 'zona' => 'Ribera', 'superficie' => 60,  'riego' => 'goteo', 'adjudicada' => true],
    ['codigo' => 'H02', 'zona' => 'Ribera', 'superficie' => 45,  'riego' => 'manta', 'adjudicada' => false],
    ['codigo' => 'H03', 'zona' => 'Eras',   'superficie' => 80,  'riego' => 'goteo', 'adjudicada' => true],
    ['codigo' => 'H04', 'zona' => 'Eras',   'superficie' => 100, 'riego' => 'manta', 'adjudicada' => true],
    ['codigo' => 'H05', 'zona' => 'Soto',   'superficie' => 40,  'riego' => 'goteo', 'adjudicada' => false],
    // … (tres filas más en el fichero íntegro, hasta 8)
];
```

**Enunciado.** La asociación quiere saber, antes de la asamblea, cuántos metros tiene libres y dónde. El recorrido de partida calcula las parcelas libres y sus metros, pero los metros salen a cero y el intérprete protesta tres veces.

**Tareas.**

- **(a) Caza el error.** Ejecuta `php recorrido.php` desde la carpeta de la práctica. Lee los avisos: te dicen la clave y la línea. Explica por qué el recuento de parcelas es correcto y el de metros no, qué valor está sumando PHP en realidad (recuerda el apartado 11), y arréglalo. Comprueba que ahora los metros libres cuadran con la suma a mano de las parcelas libres del fichero de datos.
- **(b) Metros por zona.** Con un segundo `foreach` y un array asociativo como acumulador, calcula los metros totales de cada zona (adjudicadas y libres) e imprímelos con un `foreach` con clave, una zona por línea. Usa `??` para que el acumulador no proteste la primera vez que aparece cada zona.
- **(c) La primera que cumple.** Con un bucle `while` —no con `foreach`— localiza la **primera parcela libre con riego por goteo** e imprime su código y su zona. Explica en una línea por qué `while` encaja mejor aquí que recorrer todo el array.
- **(d) Las zonas, sin repetir.** Construye la lista de zonas del huerto sin repeticiones, en el orden en que aparecen, con `in_array` estricto, e imprímela en una sola línea con `implode`, seguida de cuántas son entre paréntesis.

**Cómo trabajar.** El fichero de datos se carga con `require` y devuelve el array: no lo copies dentro de tu programa. Cuando una suma te dé cero o un aviso hable de una clave, comprueba la ortografía de la clave contra el fichero de datos antes de tocar la lógica: la causa casi siempre está ahí.
### CP2.6 · Las utilidades de la asociación

**Nivel** intermedio · **Tiempo orientativo** 20 min.

**Qué refuerza.** El apartado 6 de la [UD2](ud2-codigo-embebido-php.md): funciones con tipos declarados, valores por defecto, el tipo que admite `null`, argumentos con nombre, variables locales `static` y, sobre todo, el ámbito: lo que una función necesita se le pasa.

**Ficheros de partida.** `cp2-6/utilidades.php` y `cp2-6/datos/parcelas.php`, en el repositorio de recursos (enlace en Classroom).

**Enunciado.** La asociación quiere un único fichero de funciones que use todo lo demás: la cuota anual de una parcela —por tramos de superficie, con 12 € de suplemento si riega por goteo y la bonificación vecinal del 10 % aplicada al final—, una línea de descripción, la generación del siguiente código libre y el recibo. La primera función ya está escrita y probada… y sus pruebas no cuadran con lo que dice el comentario.

**Tareas.**

- **(a) Caza el error.** Ejecuta `php utilidades.php`. La primera prueba debería dar 51,30 y da 57,00; además el intérprete avisa. Lee el aviso, localiza la variable y explica, con los términos «local» y «ámbito», por qué dentro de la función no existe y qué valor toma entonces en el cálculo (de ahí que el resultado sea «sin bonificación» y no un error). Arréglalo de la forma correcta: la bonificación entra como **parámetro con valor por defecto** (`float $bonificacion = 0.10`), no se lee de fuera. Añade una tercera prueba que la anule pasando `0.0`.
- **(b) La descripción.** Escribe `descripcion(string $codigo, int $superficie, ?string $zona = null): string`, que devuelva `H03 · 80 m² · zona Eras` y, si no hay zona, `… · zona por asignar` (usa `??`). Llámala tres veces: con los tres argumentos, con dos, y con **argumentos con nombre** en otro orden.
- **(c) El siguiente código.** Escribe `siguiente_codigo(string $prefijo = 'H'): string` con una variable local `static` que empiece en 8 (las parcelas que hay hoy) y devuelva `H09`, `H10`… con el cero de relleno mientras el número tenga una cifra. Llámala cuatro veces seguidas, la tercera con el prefijo `'T'`, e imprime los cuatro resultados en una línea. Explica qué hace `static` que una variable local normal no hace, y por qué el prefijo no reinicia la cuenta.
- **(d) El recibo.** Escribe `recibo(array $parcela): string`, que reciba una fila del fichero de datos y devuelva la descripción seguida de ` — cuota anual 51,30 €` (con `number_format`), usando `descripcion()` y `cuota_anual()` por dentro. Carga el fichero de datos con `require` e imprime el recibo de cada parcela **adjudicada**.

**Cómo trabajar.** Una función que lee una variable de fuera «funciona» en tu prueba y falla en silencio en producción: por eso la regla de la casa es parámetros, siempre. Cuando declares tipos, pásale `php -l` y ejecuta con la directiva de tipos estrictos activa: una función que devuelve `float` y recibe un `int` por el camino te lo dirá.

### CP2.7 · La parcela como tipo compuesto

**Nivel** intermedio · **Tiempo orientativo** 20 min.

**Qué refuerza.** El apartado 7 de la [UD2](ud2-codigo-embebido-php.md): la clase como tipo que te fabricas, la promoción de propiedades en el constructor, los métodos que usan otros métodos a través de `$this`, y el array de objetos recorrido con `foreach`.

**Ficheros de partida.** `cp2-7/parcela.php` y `cp2-7/datos/parcelas.php`, en el repositorio de recursos (enlace en Classroom).

**Enunciado.** El fichero convierte las ocho filas del fichero de datos en objetos `Parcela`, con dos métodos ya escritos: `etiquetaRiego()` y `cuota()`. El recorrido final imprime una línea por parcela. `php -l` lo da por bueno; el intérprete, no.

**Tareas.**

- **(a) Caza el error.** Pásale `php -l` (no detecta nada) y ejecútalo: fatal. Lee el mensaje —habla de una constante que nadie ha definido— y explica qué está interpretando PHP en esa línea, por qué es sintaxis válida (pista: viene de Java) y por qué el comprobador de sintaxis no puede cazarlo. Arréglalo.
- **(b) El recibo, con `$this`.** Añade el método `recibo(): string`, que devuelva una línea como `H01 (Ribera, 60 m², riego localizado) — 51,30 € — adjudicada`, reutilizando `etiquetaRiego()` y `cuota()` **desde dentro** con `$this`, y con `adjudicada`/`libre` decidido con el ternario. Cambia el recorrido para que imprima `$p->recibo()`.
- **(c) El recorrido acumula.** En ese mismo `foreach`, acumula la cuota de todas las parcelas y, al salir del bucle, imprime la suma con `number_format` y cuántas parcelas son (`count`). Comprueba la suma a mano con las ocho líneas.
- **(d) Lo que te da el objeto.** Compara en dos líneas este fichero con el CP2.6: allí `recibo()` era una función suelta que recibía un array; aquí es un método que viaja con el dato. ¿Qué te garantiza el objeto sobre los campos y sus tipos que el array asociativo no?

**Cómo trabajar.** Cuando un error habla de «constante indefinida» y tú no has escrito ninguna constante, busca un punto donde debería haber una flecha. Y recuerda que `cuota()` ya aplica la bonificación: no la vuelvas a aplicar en `recibo()`.

### CP2.8 · El tablón como plantilla

**Nivel** avanzado · **Tiempo orientativo** 25 min.

**Qué refuerza.** El apartado 8 de la [UD2](ud2-codigo-embebido-php.md): la sintaxis alternativa de las estructuras de control en plantillas (`foreach:` … `endforeach;`, `if:` … `else:` … `endif;`), la etiqueta `<?= ?>`, el escape de salida con `htmlspecialchars` y los parciales con `include` y `__DIR__`.

**Ficheros de partida.** `cp2-8/tablon-parcelas.php`, `cp2-8/parciales/cabecera-huertos.php`, `cp2-8/parciales/pie-huertos.php` y `cp2-8/datos/parcelas.php`, en el repositorio de recursos (enlace en Classroom).

**Enunciado.** El tablón de parcelas es una página: una tabla con una fila por parcela, su estado decidido con `if:`/`else:`, un aviso de la junta en la cabecera y dos parciales. Alguien ha dejado la plantilla a medias.

**Tareas.**

- **(a) Caza el error.** Pásale `php -l`: el mensaje dice que el fichero se acaba antes de lo que el intérprete esperaba. Averigua qué estructura queda sin cerrar y dónde va su cierre (no es al final del fichero). Arréglalo y comprueba que `php -l` pasa.
- **(b) Sírvela y léela.** Lanza `php -S localhost:8000` en la carpeta y pide `tablon-parcelas.php`. En el HTML recibido: cuenta las filas de datos, localiza de dónde sale la sangría irregular de cada `<tr>` y comprueba cómo ha llegado el aviso de la cabecera (tiene `<`, `>` y `&`): ¿qué habría pasado en el navegador sin `htmlspecialchars`? Pruébalo quitándolo un momento y mirando la página y su código fuente.
- **(c) La fila de total.** Añade dentro del `foreach:` una línea PHP que acumule los metros cuadrados en una variable creada arriba, en la zona de lógica, y una fila final `Total` con esa suma después del bucle. Comprueba la cifra contra el fichero de datos.
- **(d) El pie, con dato.** Incluye `parciales/pie-huertos.php` al final de la página y haz que el pie muestre cuántas parcelas hay: define la variable en la plantilla principal **antes** del `include` y úsala dentro del parcial. Explica en una línea por qué el parcial la ve (qué hace `include` con el fichero y en qué ámbito se ejecuta).

**Cómo trabajar.** En una plantilla, cada `foreach:` y cada `if:` tiene su cierre con nombre; cuando `php -l` hable de «fin de fichero inesperado», cuenta aperturas y cierres. Para ver la página como la recibe el cliente usa «Ver código fuente», no el inspector.

### CP2.9 · La calculadora de alta

**Nivel** avanzado · **Tiempo orientativo** 25 min.

**Qué refuerza.** El apartado 9 de la [UD2](ud2-codigo-embebido-php.md): datos que llegan por GET en la URL y por POST en el cuerpo, `$_GET` y `$_POST` con `??` e `isset`, el formulario que se envía a sí mismo distinguido con `$_SERVER['REQUEST_METHOD']`, y el escape de todo lo que viene de fuera. Adelanta `filter_var` con `FILTER_VALIDATE_INT` (apartado 10) para comprobar un número que llega como texto.

**Fichero de partida.** `cp2-9/calculadora.php`, en el repositorio de recursos (enlace en Classroom).

**Enunciado.** Quien se da de alta a mitad de año paga solo los meses que quedan: la cuota se prorratea por los meses desde el de alta hasta diciembre, el de alta incluido. La calculadora trabaja con una **cuota orientativa por tramos** (30, 45 o 60 € según la superficie, sin suplemento por goteo ni bonificación), que ya trae el fichero en `cuota_orientativa()`: **no se usa la `cuota_anual()` del CP2.6**. La página tiene dos entradas: por GET, `?mes=` calcula el prorrateo para una parcela tipo de 60 m²; por POST, un formulario con superficie y mes calcula el importe exacto. La parte GET está hecha y esconde un fallo; la parte POST te toca a ti.

**Tareas.**

- **(a) Siete peticiones.** Sirve la página y pídela con `curl` (o con el navegador) con `?mes=3`, `?mes=03`, `?mes=3.0`, `?mes=%203` (un espacio delante), `?mes=3abc`, `?mes=13` y `?mes=` (vacío). Anota qué responde cada una y explica, con `filter_var` delante, por qué `03` y `3.0` no valen y ` 3` sí. ¿Qué habría pasado con `3.0` si el valor hubiera llegado sin comprobar a una función que espera `int` con la directiva de tipos estrictos?
- **(b) Caza el error.** Pide `?mes=%3Cb%3Ehola%3C/b%3E` (es `<b>hola</b>` codificado para la URL) y mira el código fuente recibido: un dato que viene de la URL ha entrado en el HTML tal cual. Di qué regla del apartado 8 se ha saltado la página, arréglalo y vuelve a pedirlo.
- **(c) El formulario calcula.** Completa la rama POST: recoge `superficie` y `mes` de `$_POST` con `??`, comprueba que la superficie es un entero entre 20 y 120 y el mes un entero entre 1 y 12 (reutiliza la comprobación de la parte GET en una función), y responde con el importe: `Con 75 m² y alta en el mes 10 pagas este año 11,25 € (cuota orientativa de 45 €).` Si algo no vale, responde con el aviso correspondiente. Todo lo que imprimas pasa por `htmlspecialchars`.
- **(d) Pruébalo sin navegador.** Con `curl -X POST -d 'superficie=75&mes=10' …` comprueba el caso válido; después `superficie=75.5`, `mes=0` y un POST sin datos. Pega las cuatro respuestas y explica por qué el POST vacío no provoca ningún aviso de clave indefinida.

**Cómo trabajar.** `isset($_GET['mes'])` te dice si el parámetro vino; `filter_var` te dice si lo que vino sirve. Son dos preguntas distintas y las dos hacen falta. Y la regla del apartado 8 no distingue entre «parece inofensivo» y «es peligroso»: todo lo que viene de fuera se escapa al imprimirlo.

### CP2.10 · El alta de socio, por autoridad

**Nivel** avanzado · **Tiempo orientativo** 25 min.

**Qué refuerza.** El apartado 10 de la [UD2](ud2-codigo-embebido-php.md): la validación en el servidor como única que cuenta, `filter_var` con `FILTER_VALIDATE_INT` y `FILTER_VALIDATE_EMAIL` comprobados con `=== false`, la lista de errores acumulada y devuelta junta, y la opción de una lista cerrada comprobada con `in_array` estricto.

**Fichero de partida.** `cp2-10/inscripcion.php`, en el repositorio de recursos (enlace en Classroom).

**Enunciado.** El formulario de alta de socio pide nombre, correo, superficie deseada, años en la asociación y tipo de riego. La función `revisar_alta(array $datos): array` aplica a los cinco campos cuatro tipos de regla distintos de los del catálogo de los apuntes: campo obligatorio, correo válido, entero dentro de un rango y opción de una lista fija. Está casi bien: a un socio nuevo, con cero años de antigüedad, no le deja darse de alta.

**Tareas.**

- **(a) Caza el error.** Sirve la página y envía con `curl -X POST -d 'socio=A17&correo=a17@example.org&superficie=60&antiguedad=0&riego=goteo' …`: el servidor rechaza una antigüedad de 0 años con el mensaje de «debe ser un número entero». Localiza la comprobación que lo provoca y explica, con el `int(0)` del apartado 10 delante, por qué `!$antiguedad` confunde un cero legítimo con un fallo. Arréglalo y confirma que el alta se registra.
- **(b) Las cuatro reglas, una a una.** Envía un POST que viole las cinco comprobaciones a la vez (`socio=&correo=a17-example.org&superficie=150&antiguedad=-3&riego=aspersion`) y pega la lista de errores. Señala cuál de las reglas es de tipo, cuál de rango y cuál de lista cerrada, y por qué la superficie 150 recibe un mensaje y no dos.
- **(c) Autoridad.** El campo de riego solo admite `goteo` o `manta`. Envía un alta correcta en todo salvo en `riego=aspersion`, saltándote cualquier control que pudiera tener el navegador. Pega el rechazo y explica por qué esa decisión no puede vivir en el cliente, aunque el formulario tuviera un desplegable con solo dos opciones.
- **(d) Vacío y con marcas.** Envía un POST sin ningún dato y uno con `socio=<b>A17</b>` (codificado en la URL como `%3Cb%3EA17%3C/b%3E`). Explica por qué el primero no provoca avisos de clave indefinida (qué hace el `??` en cada regla) y qué ha pasado con las marcas del segundo en el acuse.

**Cómo trabajar.** El patrón es siempre el mismo: recoger con `??`, convertir y validar con `filter_var` comprobando `=== false`, acumular los errores y decidir al final. Prueba cada regla con un valor que la cumpla y otro que no; y prueba siempre el `0`, el vacío y el valor que no está en la lista: son los tres que rompen las validaciones escritas con prisa.
### CP2.11 · Simulacro de la unidad: la asociación, de punta a punta

**Nivel** tipo examen · **Tiempo orientativo** 70 min (10 + 25 + 23 + 12). **Para hacer cuando hayas entregado la actividad de la unidad.**

**Qué refuerza.** Toda la UD2, en la forma en que la encontrarás en la prueba escrita: fragmentos que se arreglan cambiando una línea, un fichero de funciones con tipos declarados, un listado generado desde un array con un filtro en la URL, y un formulario validado en el servidor. Se hace **sin apuntes, sin red y sin material**: solo el editor con su ayuda de funciones, el intérprete y el navegador.

**Plantilla de partida.** La carpeta `cp2-11/` del repositorio de recursos (enlace en Classroom), copiada entera a tu carpeta de trabajo. Trae `comprobar-sintaxis.php` en la raíz, la carpeta `simulacro/` con los ficheros de las cuatro partes y `simulacro/datos/parcelas.php` con las ocho parcelas del cuaderno. Las partes 3 y 4 se sirven desde `simulacro/` con `php -S localhost:8000`. No hay corrector: cada parte trae su **salida esperada**, y te compruebas tú. Los espacios de sangría de las páginas pueden diferir de los tuyos; lo que importa es el contenido de cada línea.

**Reloj.** Apunta la hora al empezar. A los 70 minutos, para y mira qué te ha faltado: eso es lo que hay que repasar antes del examen.

#### Parte 1 · Cambio mínimo (10 min)

En `simulacro/arreglos/` hay cuatro fragmentos, `a1.php` a `a4.php`, y cada uno trae en su cabecera o en su cola la salida que **debería** dar. Ninguno la da. En cada uno hay **un fallo que se arregla cambiando una sola línea** (en el `a4`, el fallo no está en `a4.php`). Ejecuta cada fragmento, compara con lo esperado, arregla la línea y vuelve a ejecutar. Hay uno de ámbito de variables, uno de condición mal escrita, uno de clave que puede no estar y uno de etiquetas. Salidas esperadas, letra a letra:

```
$ php a1.php
1 2 3
$ php a2.php
Ribera
Ribera
$ php a3.php
H02: riego a manta
$ php a4.php
<ul>
    <li>Ribera</li>
    <li>Eras</li>
    <li>Soto</li>
</ul>
```

#### Parte 2 · Fichero de funciones (25 min)

Escribe el cuerpo de las cinco funciones de `simulacro/huerto.php` sin cambiar sus firmas (todas con la directiva de tipos estrictos; las firmas y lo que hace cada una están comentados en el propio fichero): `cuota_tramo(int $superficie): int` decide por tramos; `metros_totales(array $parcelas, bool $solo_libres = false): int` recorre y acumula; `codigos_por_zona(array $parcelas): array` devuelve un array asociativo con los códigos agrupados por zona; `buscar_parcela(array $parcelas, string $codigo, ?string $zona = null): ?array` tiene un parámetro con valor por defecto y retorno anulable; `importe_zona(array $parcelas, string $zona): int` usa `cuota_tramo()`. El fichero `simulacro/pruebas.php` las llama a todas sobre el fichero de datos y **no se modifica**. Salida esperada de `php pruebas.php`, letra a letra:

```
1. 30 45 45 60
2. 590 140
3. Ribera=H01+H02+H08 Eras=H03+H04 Soto=H05+H06+H07
4. Soto 120
bool(true)
NULL
NULL
5. 135 105 135 0
```

#### Parte 3 · Listado por GET (23 min)

Completa `simulacro/tabla.php`: una tabla HTML con una fila por parcela cuya superficie sea **mayor o igual** que el parámetro `?minimo=` de la URL (si no viene, no es un entero válido o es negativo, el mínimo es 0 y se listan todas). Cada fila lleva en su `<tr>` la clase `libre` o `adjudicada`, y las celdas: código, zona (escapada), superficie con «m²», cuota calculada con `cuota_tramo()` con «€», y el estado en texto. Debajo de la tabla, un párrafo con el número de filas y los metros cuadrados que suman. Recordatorios: `<table>` es HTML de primero (`<tr>` por fila, `<td>` por celda); la clase de la fila sale de un ternario dentro de `<?= ?>`; el mínimo se lee con `??` y se comprueba con `filter_var`. Salida esperada con `?minimo=90`, letra a letra:

```html
<h1>Parcelas con al menos 90 m²</h1>
<table>
    <tr><th>Código</th><th>Zona</th><th>Superficie</th><th>Cuota</th><th>Estado</th></tr>
        <tr class="adjudicada">
        <td>H04</td>
        <td>Eras</td>
        <td>100 m²</td>
        <td>60 €</td>
        <td>adjudicada</td>
    </tr>
        <tr class="adjudicada">
        <td>H06</td>
        <td>Soto</td>
        <td>120 m²</td>
        <td>60 €</td>
        <td>adjudicada</td>
    </tr>
        <tr class="adjudicada">
        <td>H08</td>
        <td>Ribera</td>
        <td>90 m²</td>
        <td>60 €</td>
        <td>adjudicada</td>
    </tr>
</table>
<p class="resumen">3 parcelas · 310 m²</p>
```

Sin parámetro y con `?minimo=abc` el título dice `Parcelas con al menos 0 m²` y la tabla lista las ocho parcelas con el párrafo `8 parcelas · 590 m²`; con `?minimo=200` la tabla queda solo con la fila de cabecera y el párrafo dice `0 parcelas · 0 m²`.

#### Parte 4 · Formulario POST (12 min)

`simulacro/lectura.php` trae hecho el formulario de **lectura del contador de agua** (campos `parcela`, `correo` y `litros`) y la función `importe_agua()`. Completa el servidor: recupera los tres campos de `$_POST` con `??`, aplica **tres reglas** —parcela obligatoria; correo válido; litros entero entre 0 y 5000— y guarda cada aviso en `$errores` con el **nombre del campo como clave**. Si hay errores, se vuelve a mostrar el formulario con cada `value` repoblado con lo recibido (escapado) y el aviso debajo de su campo; si no, se muestra el acuse con el importe. Recordatorios: el valor recibido se vuelve a escribir en el atributo `value`, escapado, igual que en el cuerpo; los errores van en un array asociativo con el nombre del campo como clave (`isset($errores['correo'])` dice si ese campo tiene aviso). Salidas esperadas, letra a letra:

Petición GET:

```html
<h1>Lectura del contador</h1>
    <form action="" method="post">
        <label>Parcela: <input type="text" name="parcela" id="parcela" value=""></label>
        <label>Correo: <input type="text" name="correo" id="correo" value=""></label>
        <label>Litros: <input type="text" name="litros" id="litros" value=""></label>
        <button type="submit">Enviar lectura</button>
    </form>
```

POST con `parcela=H03&correo=h03@example.org&litros=1200`:

```html
<h1>Lectura del contador</h1>
    <p class="acuse">Lectura registrada: parcela H03, 1200 litros: importe 5,40 €.</p>
```

POST con `parcela=&correo=h03-example.org&litros=12,5`:

```html
<h1>Lectura del contador</h1>
    <form action="" method="post">
        <label>Parcela: <input type="text" name="parcela" id="parcela" value=""></label>
        <p class="error">Indica el código de la parcela.</p>
        <label>Correo: <input type="text" name="correo" id="correo" value="h03-example.org"></label>
        <p class="error">El correo no es válido.</p>
        <label>Litros: <input type="text" name="litros" id="litros" value="12,5"></label>
        <p class="error">Los litros tienen que ser un número entero entre 0 y 5000.</p>
        <button type="submit">Enviar lectura</button>
    </form>
```

POST con `parcela=%3Cb%3EH03%3C/b%3E&correo=h03@example.org&litros=0` (0 litros es una lectura válida; las marcas llegan como texto):

```html
<h1>Lectura del contador</h1>
    <p class="acuse">Lectura registrada: parcela &lt;b&gt;H03&lt;/b&gt;, 0 litros: importe 0,00 €.</p>
```

#### Al terminar

Desde la raíz de la plantilla (donde está `comprobar-sintaxis.php`), ejecuta **`php comprobar-sintaxis.php simulacro`**: tiene que decir `10 ficheros comprobados, 0 con errores de sintaxis.` Si marca algún `FALLA`, arréglalo antes de dar el simulacro por terminado. Después, compara tus salidas con las esperadas parte a parte y anota qué te ha faltado y por qué.
