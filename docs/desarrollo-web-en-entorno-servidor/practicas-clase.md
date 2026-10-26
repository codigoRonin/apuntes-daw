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

**Nivel** intermedio · **Tiempo orientativo** 35 min (20 + 15 de la tarea desde cero).

**Qué refuerza.** El apartado 6 de la [UD2](ud2-codigo-embebido-php.md): funciones con tipos declarados, valores por defecto, el tipo que admite `null`, argumentos con nombre, variables locales `static` y, sobre todo, el ámbito: lo que una función necesita se le pasa.

**Ficheros de partida.** `cp2-6/utilidades.php` y `cp2-6/datos/parcelas.php`, en el repositorio de recursos (enlace en Classroom).

**Enunciado.** La asociación quiere un único fichero de funciones que use todo lo demás: la cuota anual de una parcela —por tramos de superficie, con 12 € de suplemento si riega por goteo y la bonificación vecinal del 10 % aplicada al final—, una línea de descripción, la generación del siguiente código libre y el recibo. La primera función ya está escrita y probada… y sus pruebas no cuadran con lo que dice el comentario.

**Tareas.**

- **(a) Caza el error.** Ejecuta `php utilidades.php`. La primera prueba debería dar 51,30 y da 57,00; además el intérprete avisa. Lee el aviso, localiza la variable y explica, con los términos «local» y «ámbito», por qué dentro de la función no existe y qué valor toma entonces en el cálculo (de ahí que el resultado sea «sin bonificación» y no un error). Arréglalo de la forma correcta: la bonificación entra como **parámetro con valor por defecto** (`float $bonificacion = 0.10`), no se lee de fuera. Añade una tercera prueba que la anule pasando `0.0`.
- **(b) La descripción.** Escribe `descripcion(string $codigo, int $superficie, ?string $zona = null): string`, que devuelva `H03 · 80 m² · zona Eras` y, si no hay zona, `… · zona por asignar` (usa `??`). Llámala tres veces: con los tres argumentos, con dos, y con **argumentos con nombre** en otro orden.
- **(c) El siguiente código.** Escribe `siguiente_codigo(string $prefijo = 'H'): string` con una variable local `static` que empiece en 8 (las parcelas que hay hoy) y devuelva `H09`, `H10`… con el cero de relleno mientras el número tenga una cifra. Llámala cuatro veces seguidas, la tercera con el prefijo `'T'`, e imprime los cuatro resultados en una línea. Explica qué hace `static` que una variable local normal no hace, y por qué el prefijo no reinicia la cuenta.
- **(d) El recibo.** Escribe `recibo(array $parcela): string`, que reciba una fila del fichero de datos y devuelva la descripción seguida de ` — cuota anual 51,30 €` (con `number_format`), usando `descripcion()` y `cuota_anual()` por dentro. Carga el fichero de datos con `require` e imprime el recibo de cada parcela **adjudicada**.

- **(e) Desde cero.** Fichero vacío. Escribe un programa que, para estas lecturas de agua de cuatro parcelas —H01 1200 litros, H03 2600, H04 800 y H06 3100—, calcule el importe de cada una (0,45 € por cada 100 litros; a partir de 2000 litros, el exceso se cobra a 0,60 € por cada 100) y muestre una línea por parcela, el total, el total con el recargo por retraso (el 5 % si no se indica otro) y el total con un recargo del 10 %. Decide tú las funciones, sus nombres y sus firmas: tiene que haber al menos dos, tipadas, una con un valor por defecto, y una que use a otra. Salida esperada, letra a letra:

```
H01: 1200 l → 5,40 €
H03: 2600 l → 12,60 €
H04: 800 l → 3,60 €
H06: 3100 l → 15,60 €
Total: 37,20 €
Total con recargo por retraso: 39,06 €
Total con recargo del 10 %: 40,92 €
```

**Cómo trabajar.** Una función que lee una variable de fuera «funciona» en tu prueba y falla en silencio en producción: por eso la regla de la casa es parámetros, siempre. Cuando declares tipos, pásale `php -l` y ejecuta con la directiva de tipos estrictos activa: una función que devuelve `float` y recibe un `int` por el camino te lo dirá.

### CP2.7 · La parcela como tipo compuesto

**Nivel** intermedio · **Tiempo orientativo** 35 min (20 + 15 de la tarea desde cero).

**Qué refuerza.** El apartado 7 de la [UD2](ud2-codigo-embebido-php.md): la clase como tipo que te fabricas, la promoción de propiedades en el constructor, los métodos que usan otros métodos a través de `$this`, y el array de objetos recorrido con `foreach`.

**Ficheros de partida.** `cp2-7/parcela.php` y `cp2-7/datos/parcelas.php`, en el repositorio de recursos (enlace en Classroom).

**Enunciado.** El fichero convierte las ocho filas del fichero de datos en objetos `Parcela`, con dos métodos ya escritos: `etiquetaRiego()` y `cuota()`. El recorrido final imprime una línea por parcela. `php -l` lo da por bueno; el intérprete, no.

**Tareas.**

- **(a) Caza el error.** Pásale `php -l` (no detecta nada) y ejecútalo: fatal. Lee el mensaje —habla de una constante que nadie ha definido— y explica qué está interpretando PHP en esa línea, por qué es sintaxis válida (pista: viene de Java) y por qué el comprobador de sintaxis no puede cazarlo. Arréglalo.
- **(b) El recibo, con `$this`.** Añade el método `recibo(): string`, que devuelva una línea como `H01 (Ribera, 60 m², riego localizado) — 51,30 € — adjudicada`, reutilizando `etiquetaRiego()` y `cuota()` **desde dentro** con `$this`, y con `adjudicada`/`libre` decidido con el ternario. Cambia el recorrido para que imprima `$p->recibo()`.
- **(c) El recorrido acumula.** En ese mismo `foreach`, acumula la cuota de todas las parcelas y, al salir del bucle, imprime la suma con `number_format` y cuántas parcelas son (`count`). Comprueba la suma a mano con las ocho líneas.
- **(d) Lo que te da el objeto.** Compara en dos líneas este fichero con el CP2.6: allí `recibo()` era una función suelta que recibía un array; aquí es un método que viaja con el dato. ¿Qué te garantiza el objeto sobre los campos y sus tipos que el array asociativo no?

- **(e) Desde cero.** Fichero vacío. Diseña una clase para **la cosecha de una parcela** (parcela, producto, kilos y precio por kilo en el mercadillo de la asociación), con las propiedades y los métodos que te parezcan. Tiene que poder decir la temporada del producto (tomate, pimiento y calabacín son «de verano»; acelga y borraja, «de invierno»; el resto, «de temporada sin catalogar»), calcular el importe (kilos por precio), aplicar una merma (un porcentaje de kilos que se desecha: el objeto cambia) y dar una línea de resumen que use a los otros métodos. Las cuatro cosechas: H01, 12,5 kg de tomate a 2,00 €/kg; H03, 4,0 kg de acelga a 1,50 €/kg; H06, 20,0 kg de pimiento a 2,50 €/kg; H08, 5,0 kg de borraja a 1,80 €/kg. Créalas en un array de objetos, imprime el resumen de cada una y los totales de kilos y de euros, aplica una merma del 20 % a la primera y vuelve a imprimir su resumen. Nombres de clase, propiedades y métodos, a tu elección. Salida esperada, letra a letra:

```
H01 · tomate (de verano): 12,5 kg → 25,00 €
H03 · acelga (de invierno): 4,0 kg → 6,00 €
H06 · pimiento (de verano): 20,0 kg → 50,00 €
H08 · borraja (de invierno): 5,0 kg → 9,00 €
Total: 41,5 kg · 90,00 €
Tras la merma: H01 · tomate (de verano): 10,0 kg → 20,00 €
```

**Cómo trabajar.** Cuando un error habla de «constante indefinida» y tú no has escrito ninguna constante, busca un punto donde debería haber una flecha. Y recuerda que `cuota()` ya aplica la bonificación: no la vuelvas a aplicar en `recibo()`.

### CP2.8 · El tablón como plantilla

**Nivel** avanzado · **Tiempo orientativo** 45 min (25 + 20 de la tarea desde cero).

**Qué refuerza.** El apartado 8 de la [UD2](ud2-codigo-embebido-php.md): la sintaxis alternativa de las estructuras de control en plantillas (`foreach:` … `endforeach;`, `if:` … `else:` … `endif;`), la etiqueta `<?= ?>`, el escape de salida con `htmlspecialchars` y los parciales con `include` y `__DIR__`.

**Ficheros de partida.** `cp2-8/tablon-parcelas.php`, `cp2-8/parciales/cabecera-huertos.php`, `cp2-8/parciales/pie-huertos.php` y `cp2-8/datos/parcelas.php`, en el repositorio de recursos (enlace en Classroom).

**Enunciado.** El tablón de parcelas es una página: una tabla con una fila por parcela, su estado decidido con `if:`/`else:`, un aviso de la junta en la cabecera y dos parciales. Alguien ha dejado la plantilla a medias.

**Tareas.**

- **(a) Caza el error.** Pásale `php -l`: el mensaje dice que el fichero se acaba antes de lo que el intérprete esperaba. Averigua qué estructura queda sin cerrar y dónde va su cierre (no es al final del fichero). Arréglalo y comprueba que `php -l` pasa.
- **(b) Sírvela y léela.** Lanza `php -S localhost:8000` en la carpeta y pide `tablon-parcelas.php`. En el HTML recibido: cuenta las filas de datos, localiza de dónde sale la sangría irregular de cada `<tr>` y comprueba cómo ha llegado el aviso de la cabecera (tiene `<`, `>` y `&`): ¿qué habría pasado en el navegador sin `htmlspecialchars`? Pruébalo quitándolo un momento y mirando la página y su código fuente.
- **(c) La fila de total.** Añade dentro del `foreach:` una línea PHP que acumule los metros cuadrados en una variable creada arriba, en la zona de lógica, y una fila final `Total` con esa suma después del bucle. Comprueba la cifra contra el fichero de datos.
- **(d) El pie, con dato.** Incluye `parciales/pie-huertos.php` al final de la página y haz que el pie muestre cuántas parcelas hay: define la variable en la plantilla principal **antes** del `include` y úsala dentro del parcial. Explica en una línea por qué el parcial la ve (qué hace `include` con el fichero y en qué ámbito se ejecuta).

- **(e) Desde cero.** Sin ficheros de partida. Haz una página con el **orden del día de la asamblea**: una cabecera en un parcial incluido con `include`, el título, una lista ordenada con los cuatro puntos —que viven en un array en la zona de lógica de la plantilla— con «(se vota)» en los que se votan, y un párrafo con cuántos puntos hay y cuántos se votan (contados en la propia plantilla). Dos títulos traen `&` y `<` `>`, y tienen que llegar como texto. Sintaxis alternativa y escape; decide tú los nombres de los ficheros y la forma de los datos. HTML esperado al servirla (las sangrías pueden diferir de las tuyas):

```html
<header>
    <strong>Asociación de Huertos del Soto</strong> · Asamblea de otoño
</header>
<h1>Orden del día</h1>
<ol>
        <li>Lectura &amp; aprobación de las cuentas (se vota)</li>
        <li>Cuotas 2027: propuesta &lt;provisional&gt; (se vota)</li>
    <li>Turnos de limpieza de la acequia</li>
    <li>Ruegos y preguntas</li>
</ol>
<p>4 puntos, 2 con votación.</p>
```

**Cómo trabajar.** En una plantilla, cada `foreach:` y cada `if:` tiene su cierre con nombre; cuando `php -l` hable de «fin de fichero inesperado», cuenta aperturas y cierres. Para ver la página como la recibe el cliente usa «Ver código fuente», no el inspector.

### CP2.9 · La calculadora de alta

**Nivel** avanzado · **Tiempo orientativo** 45 min (25 + 20 de la tarea desde cero).

**Qué refuerza.** El apartado 9 de la [UD2](ud2-codigo-embebido-php.md): datos que llegan por GET en la URL y por POST en el cuerpo, `$_GET` y `$_POST` con `??` e `isset`, el formulario que se envía a sí mismo distinguido con `$_SERVER['REQUEST_METHOD']`, y el escape de todo lo que viene de fuera. Adelanta `filter_var` con `FILTER_VALIDATE_INT` (apartado 10) para comprobar un número que llega como texto.

**Fichero de partida.** `cp2-9/calculadora.php`, en el repositorio de recursos (enlace en Classroom).

**Enunciado.** Quien se da de alta a mitad de año paga solo los meses que quedan: la cuota se prorratea por los meses desde el de alta hasta diciembre, el de alta incluido. La calculadora trabaja con una **cuota orientativa por tramos** (30, 45 o 60 € según la superficie, sin suplemento por goteo ni bonificación), que ya trae el fichero en `cuota_orientativa()`: **no se usa la `cuota_anual()` del CP2.6**. La página tiene dos entradas: por GET, `?mes=` calcula el prorrateo para una parcela tipo de 60 m²; por POST, un formulario con superficie y mes calcula el importe exacto. La parte GET está hecha y esconde un fallo; la parte POST te toca a ti.

**Tareas.**

- **(a) Siete peticiones.** Sirve la página y pídela con `curl` (o con el navegador) con `?mes=3`, `?mes=03`, `?mes=3.0`, `?mes=%203` (un espacio delante), `?mes=3abc`, `?mes=13` y `?mes=` (vacío). Anota qué responde cada una y explica, con `filter_var` delante, por qué `03` y `3.0` no valen y ` 3` sí. ¿Qué habría pasado con `3.0` si el valor hubiera llegado sin comprobar a una función que espera `int` con la directiva de tipos estrictos?
- **(b) Caza el error.** Pide `?mes=%3Cb%3Ehola%3C/b%3E` (es `<b>hola</b>` codificado para la URL) y mira el código fuente recibido: un dato que viene de la URL ha entrado en el HTML tal cual. Di qué regla del apartado 8 se ha saltado la página, arréglalo y vuelve a pedirlo.
- **(c) El formulario calcula.** Completa la rama POST: recoge `superficie` y `mes` de `$_POST` con `??`, comprueba que la superficie es un entero entre 20 y 120 y el mes un entero entre 1 y 12 (reutiliza la comprobación de la parte GET en una función), y responde con el importe: `Con 75 m² y alta en el mes 10 pagas este año 11,25 € (cuota orientativa de 45 €).` Si algo no vale, responde con el aviso correspondiente. Todo lo que imprimas pasa por `htmlspecialchars`.
- **(d) Pruébalo sin navegador.** Con `curl -X POST -d 'superficie=75&mes=10' …` comprueba el caso válido; después `superficie=75.5`, `mes=0` y un POST sin datos. Pega las cuatro respuestas y explica por qué el POST vacío no provoca ningún aviso de clave indefinida.

- **(e) Desde cero.** Fichero vacío. Haz la página «Riego a manta»: la acequia da 20 litros por minuto y el agua cuesta 0,45 € por cada 100 litros. Por **GET**, `?minutos=` responde cuántos litros son esos minutos (un entero del 1 al 240; si no vale, un aviso que repite el valor recibido, escapado); por **POST**, un formulario con `parcela` y `minutos` que se envía a sí mismo y responde con los litros y los euros, o con un aviso si falta la parcela o los minutos no valen. Decide tú las funciones y la estructura; el HTML es el de las salidas. Pruébala con estas siete peticiones y compara letra a letra:

`curl -s 'http://localhost:8000/riego.php'`

```html
<h1>Riego a manta</h1>
    <form action="" method="post">
        <label>Parcela: <input type="text" name="parcela"></label>
        <label>Minutos: <input type="text" name="minutos"></label>
        <button type="submit">Calcular</button>
    </form>
```

`curl -s 'http://localhost:8000/riego.php?minutos=30'`

```html
<h1>Riego a manta</h1>
    <p>30 minutos de acequia son 600 litros.</p>
    <form action="" method="post">
        <label>Parcela: <input type="text" name="parcela"></label>
        <label>Minutos: <input type="text" name="minutos"></label>
        <button type="submit">Calcular</button>
    </form>
```

`curl -s 'http://localhost:8000/riego.php?minutos=abc'` (y lo mismo con `?minutos=300`, que responde con `recibido: 300`)

```html
<h1>Riego a manta</h1>
    <p>Los minutos tienen que ser un número entero del 1 al 240 (recibido: abc).</p>
    <form action="" method="post">
        <label>Parcela: <input type="text" name="parcela"></label>
        <label>Minutos: <input type="text" name="minutos"></label>
        <button type="submit">Calcular</button>
    </form>
```

`curl -s -X POST -d 'parcela=H05&minutos=45' http://localhost:8000/riego.php`

```html
<h1>Riego a manta</h1>
    <p>La parcela H05 ha regado 45 minutos: 900 litros, 4,05 €.</p>
```

`curl -s -X POST -d 'parcela=&minutos=45' http://localhost:8000/riego.php` (y lo mismo con un POST sin datos)

```html
<h1>Riego a manta</h1>
    <p>Hace falta el código de la parcela y un número de minutos del 1 al 240.</p>
```

**Cómo trabajar.** `isset($_GET['mes'])` te dice si el parámetro vino; `filter_var` te dice si lo que vino sirve. Son dos preguntas distintas y las dos hacen falta. Y la regla del apartado 8 no distingue entre «parece inofensivo» y «es peligroso»: todo lo que viene de fuera se escapa al imprimirlo.

### CP2.10 · El alta de socio, por autoridad

**Nivel** avanzado · **Tiempo orientativo** 45 min (25 + 20 de la tarea desde cero).

**Qué refuerza.** El apartado 10 de la [UD2](ud2-codigo-embebido-php.md): la validación en el servidor como única que cuenta, `filter_var` con `FILTER_VALIDATE_INT` y `FILTER_VALIDATE_EMAIL` comprobados con `=== false`, la lista de errores acumulada y devuelta junta, y la opción de una lista cerrada comprobada con `in_array` estricto.

**Fichero de partida.** `cp2-10/inscripcion.php`, en el repositorio de recursos (enlace en Classroom).

**Enunciado.** El formulario de alta de socio pide nombre, correo, superficie deseada, años en la asociación y tipo de riego. La función `revisar_alta(array $datos): array` aplica a los cinco campos cuatro tipos de regla distintos de los del catálogo de los apuntes: campo obligatorio, correo válido, entero dentro de un rango y opción de una lista fija. Está casi bien: a un socio nuevo, con cero años de antigüedad, no le deja darse de alta.

**Tareas.**

- **(a) Caza el error.** Sirve la página y envía con `curl -X POST -d 'socio=A17&correo=a17@example.org&superficie=60&antiguedad=0&riego=goteo' …`: el servidor rechaza una antigüedad de 0 años con el mensaje de «debe ser un número entero». Localiza la comprobación que lo provoca y explica, con el `int(0)` del apartado 10 delante, por qué `!$antiguedad` confunde un cero legítimo con un fallo. Arréglalo y confirma que el alta se registra.
- **(b) Las cuatro reglas, una a una.** Envía un POST que viole las cinco comprobaciones a la vez (`socio=&correo=a17-example.org&superficie=150&antiguedad=-3&riego=aspersion`) y pega la lista de errores. Señala cuál de las reglas es de tipo, cuál de rango y cuál de lista cerrada, y por qué la superficie 150 recibe un mensaje y no dos.
- **(c) Autoridad.** El campo de riego solo admite `goteo` o `manta`. Envía un alta correcta en todo salvo en `riego=aspersion`, saltándote cualquier control que pudiera tener el navegador. Pega el rechazo y explica por qué esa decisión no puede vivir en el cliente, aunque el formulario tuviera un desplegable con solo dos opciones.
- **(d) Vacío y con marcas.** Envía un POST sin ningún dato y uno con `socio=<b>A17</b>` (codificado en la URL como `%3Cb%3EA17%3C/b%3E`). Explica por qué el primero no provoca avisos de clave indefinida (qué hace el `??` en cada regla) y qué ha pasado con las marcas del segundo en el acuse.

- **(e) Desde cero.** Fichero vacío. Haz el **parte de siembra**: un formulario con `parcela`, `cultivo`, `riego` y `superficie` que se envía a sí mismo y se revisa en el servidor con estas reglas, en este orden: la parcela es una de H01 a H08; el cultivo es tomate, pimiento, acelga o borraja; el riego es goteo o manta; la superficie sembrada es un entero entre 10 y 120 m²; y **una regla que depende de dos campos**: con riego por goteo, la superficie tiene que ser de al menos 40 m². Esa regla solo se aplica cuando el riego y la superficie valen cada uno por separado (cada error se paga una vez: goteo con 5 m² recibe solo el mensaje del rango). Si todo vale, el acuse calcula las plantas estimadas (tomate, 3 por m²; pimiento, 4; acelga, 8; borraja, 6); si no, la lista de errores y el formulario. Decide tú la estructura y las funciones. **Prueba de autoridad con `curl`:** envía goteo con 30 m² —los dos campos valen por separado— y comprueba que el servidor lo rechaza por la regla cruzada; la misma superficie a manta tiene que valer. Salidas esperadas:

`curl -s http://localhost:8000/siembra.php`

```html
<h1>Parte de siembra</h1>
    <form action="" method="post">
        <label>Parcela: <input type="text" name="parcela"></label>
        <label>Cultivo: <input type="text" name="cultivo"></label>
        <label>Riego: <input type="text" name="riego"></label>
        <label>Superficie sembrada (m²): <input type="text" name="superficie"></label>
        <button type="submit">Enviar parte</button>
    </form>
```

`curl -s -X POST -d 'parcela=H03&cultivo=tomate&riego=goteo&superficie=60' http://localhost:8000/siembra.php`

```html
<h1>Parte de siembra</h1>
    <p>Siembra registrada: H03, tomate, 60 m² con riego por goteo. Plantas estimadas: 180.</p>
```

`curl -s -X POST -d 'parcela=H03&cultivo=tomate&riego=goteo&superficie=30' http://localhost:8000/siembra.php`

```html
<h1>Parte de siembra</h1>
    <ul class="errores">
        <li>Con riego por goteo la superficie sembrada tiene que ser de al menos 40 m².</li>
    </ul>
    <form action="" method="post">
        <label>Parcela: <input type="text" name="parcela"></label>
        <label>Cultivo: <input type="text" name="cultivo"></label>
        <label>Riego: <input type="text" name="riego"></label>
        <label>Superficie sembrada (m²): <input type="text" name="superficie"></label>
        <button type="submit">Enviar parte</button>
    </form>
```

`curl -s -X POST -d 'parcela=H03&cultivo=tomate&riego=manta&superficie=30' http://localhost:8000/siembra.php`

```html
<h1>Parte de siembra</h1>
    <p>Siembra registrada: H03, tomate, 30 m² con riego a manta. Plantas estimadas: 90.</p>
```

`curl -s -X POST -d 'parcela=H03&cultivo=tomate&riego=goteo&superficie=5' http://localhost:8000/siembra.php`

```html
<h1>Parte de siembra</h1>
    <ul class="errores">
        <li>La superficie sembrada tiene que ser un entero entre 10 y 120 m².</li>
    </ul>
    <form action="" method="post">
        <label>Parcela: <input type="text" name="parcela"></label>
        <label>Cultivo: <input type="text" name="cultivo"></label>
        <label>Riego: <input type="text" name="riego"></label>
        <label>Superficie sembrada (m²): <input type="text" name="superficie"></label>
        <button type="submit">Enviar parte</button>
    </form>
```

`curl -s -X POST -d 'parcela=H09&cultivo=lechuga&riego=aspersion&superficie=5' http://localhost:8000/siembra.php` (y lo mismo con un POST sin datos)

```html
<h1>Parte de siembra</h1>
    <ul class="errores">
        <li>La parcela tiene que ser una de H01 a H08.</li>
        <li>El cultivo tiene que ser tomate, pimiento, acelga o borraja.</li>
        <li>El riego tiene que ser goteo o manta.</li>
        <li>La superficie sembrada tiene que ser un entero entre 10 y 120 m².</li>
    </ul>
    <form action="" method="post">
        <label>Parcela: <input type="text" name="parcela"></label>
        <label>Cultivo: <input type="text" name="cultivo"></label>
        <label>Riego: <input type="text" name="riego"></label>
        <label>Superficie sembrada (m²): <input type="text" name="superficie"></label>
        <button type="submit">Enviar parte</button>
    </form>
```

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

## UD3 · Estado, cookies, sesiones y autenticación

Las prácticas de esta unidad siguen con el caso del cuaderno de la UD2, distinto del club de montaña de los apuntes y de la escuela de música de la actividad evaluativa: la **Asociación de Huertos del Soto**, que ahora quiere que su web tenga memoria. Un tablón que sabe qué anuncios no has visto, una solicitud de parcela que ocupa tres pantallas, una votación de la asamblea que no se duplica al recargar y, más adelante, cuentas de hortelanos con contraseña, permisos por perfil y un banco de pruebas. Los datos siguen siendo de aula, inventados; las ocho parcelas son las de siempre (H01 a H08, en Ribera, Eras y Soto). Las prácticas forman dos bloques: el primero, de CP3.1 a CP3.4, cubre los apartados 1 a 4 de la unidad; el segundo, de CP3.5 a CP3.10, los apartados 5 a 10 (el de la base de datos, el 9, va el último, en el CP3.10, porque necesita el gestor del aula). La unidad se cierra con un simulacro de examen, el CP3.11, que se publica cuando se haya entregado la actividad de la unidad.

Dos herramientas nuevas respecto a la UD2, las mismas de los apuntes: `curl` con tarro de cookies (`-c` para guardar lo que llegue, `-b` para reenviarlo) y la carpeta donde el servidor guarda las sesiones (`session.save_path`). Para que esa carpeta sea tuya y la encuentres, lanza el servidor de cada práctica así, desde la carpeta de la práctica, con una carpeta `sesiones/` creada antes:

```
php -d session.save_path=sesiones -S localhost:8000
```

Y recuerda la advertencia del apartado 4 de la unidad: el servidor embebido puede **esconder** los avisos de cabeceras enviadas demasiado pronto. En estas prácticas, cuando se te pida ejecutar un fichero «en el terminal», es para ver lo que el servidor se calla.

### CP3.1 · Tres peticiones, ninguna memoria

**Nivel** básico · **Tiempo orientativo** 15 min.

**Qué refuerza.** El apartado 1 de la [UD3](ud3-estado-sesiones-autenticacion.md): HTTP no tiene estado, cada petición es una ejecución entera del programa, y la tabla de mecanismos (parámetros en la URL, campos ocultos, cookies, sesión) con lo que cada uno responde a dos preguntas: dónde viaja el dato y quién lo controla.

**Ficheros de partida.** `cp3-1/contador-turnos.php`, `cp3-1/anuncios-zona.php` y `cp3-1/datos/anuncios.php`, en el repositorio de recursos (enlace en Classroom). Cópialos a tu carpeta de trabajo.

**Enunciado.** El secretario quiere saber cuántos turnos de riego se han pedido hoy y ha escrito un contador: cada petición a `contador-turnos.php` pide un turno. Como en la UD2 aprendió que una variable `static` conserva su valor entre llamadas, la ha usado para contar. El tablón de anuncios, por su parte, se filtra por zona, y la zona llega de dos maneras: en la URL y en un campo oculto de un formulario.

**Tareas.**

- **(a) Caza el error.** Sirve la carpeta y pide `contador-turnos.php` tres veces con `curl -i`. Siempre es el turno 1, y ninguna respuesta trae una cabecera `Set-Cookie`. Antes de tocar nada, añade una segunda llamada a `siguiente_turno()` en el mismo fichero y vuelve a pedirlo: ahora salen 1 y 2 en **cada** petición. Explica, con el vocabulario del apartado 1 (petición, ejecución desde cero, olvidar todo), entre qué dos cosas sobrevive una variable `static` y entre qué dos cosas no, y por qué el servidor no puede saber que las tres peticiones son tuyas aunque vengan del mismo equipo (piensa en qué comparten dos personas detrás del mismo wifi).
- **(b) El dato en la URL.** Pide `anuncios-zona.php?zona=Ribera` y después, sin tarro y como si fueras otra persona a la que han pasado el enlace, vuelve a pedir **la misma dirección**: el filtro «viaja» en la propia dirección, y por eso se puede compartir. Edítala a `?zona=Marte` y mira qué hace el servidor con un valor que no está en su lista. ¿Quién controla ese dato: el cliente o el servidor?
- **(c) El dato en un campo oculto.** Envía el formulario con `curl -d zona=Eras` y comprueba que el filtro se aplica. Después pide `anuncios-zona.php` a secas, que es lo que hace el enlace «Ver todas las zonas»: el filtro ha desaparecido. Explica cuánto dura un dato que viaja en un campo oculto y qué tendría que hacer la página para «arrastrarlo» a la siguiente.
- **(d) La tabla.** Rellena, con lo que has visto, una tabla de tres filas (variable `static`, parámetro en la URL, campo oculto) y tres columnas: dónde vive el dato entre dos peticiones, quién lo controla, y si sobrevive a «ir a otra página y volver». Termina con una línea: ¿cuál de los tres es «memoria del servidor» sobre un cliente concreto? (Pista: ninguno; el apartado 2 empieza por ahí.)

**Cómo trabajar.** `curl -i` enseña las cabeceras de la respuesta; aquí la observación importante es la que **falta** (no hay `Set-Cookie`). La salida real de (a) con el fichero de partida, por el terminal (`php contador-turnos.php`, dos veces; servido, lo mismo tres veces):

```
Turno de riego pedido: n.º 1
Turno de riego pedido: n.º 1
```

Y con la segunda llamada añadida, por el terminal y en dos peticiones servidas seguidas:

```
Turno de riego pedido: n.º 1
Y otro en la misma petición: n.º 2
---
Turno de riego pedido: n.º 1
Y otro en la misma petición: n.º 2
Turno de riego pedido: n.º 1
Y otro en la misma petición: n.º 2
```

### CP3.2 · Desde tu última visita

**Nivel** básico · **Tiempo orientativo** 15 min.

**Qué refuerza.** El apartado 2 de la [UD3](ud3-estado-sesiones-autenticacion.md): `setcookie()` y `$_COOKIE`, las cabeceras `Set-Cookie` y `Cookie`, el tarro de `curl`, la cookie que no está en la misma petición que la crea, los atributos (`expires`, `Max-Age`, `path`, `HttpOnly`, `SameSite`), el borrado con caducidad en el pasado y la regla de oro: lo que viene en una cookie lo escribió el cliente.

**Ficheros de partida.** `cp3-2/novedades.php` y `cp3-2/datos/anuncios.php`, en el repositorio de recursos (enlace en Classroom).

**Enunciado.** El tablón tiene ocho anuncios con fecha. La página `novedades.php` quiere decirte cuántos se han publicado **desde tu última visita**, y para eso guarda en una cookie la fecha del anuncio más reciente que has visto. La primera visita se queja.

**Tareas.**

- **(a) Caza el error.** Sirve la carpeta y pide `novedades.php` con `curl -i -c tarro.txt` (primera visita, guardando el tarro). La respuesta trae la cabecera `Set-Cookie` correcta y la cuenta de anuncios correcta, pero la última línea sale vacía (el aviso «Undefined array key» aparece en la respuesta o en el terminal del servidor, según la configuración de tu PHP). Explica, con el error 2 del apartado 11 de la unidad delante, por qué `$_COOKIE` no tiene todavía la marca que el fichero acaba de pedir que se guarde, y arréglalo **sin leer `$_COOKIE`** en esa línea (el valor ya lo tienes en una variable). Vuelve a pedirla con `-b tarro.txt`: ahora la marca llega, y no hay anuncios nuevos.
- **(b) La cookie la escribe el cliente.** Abre `tarro.txt` y cambia a mano la fecha por `2026-10-10`; pide la página con `-b tarro.txt` y comprueba que te enseña los anuncios posteriores a esa fecha. Ahora cámbiala por `ayer` y después por `2026-13-45`: el servidor se los traga. Escribe una función `fecha_valida(string $f): bool` que acepte solo fechas reales con la forma AAAA-MM-DD (pista: `preg_match` con un patrón de tres grupos de dígitos y `checkdate` para el mes y el día) y haz que cualquier marca inválida cuente **como si no hubiera marca**. Explica en una línea por qué una cookie se valida igual que un campo de formulario.
- **(c) El atributo `path`.** Cambia en `setcookie()` la ruta `/` por `/junta/`, borra el tarro, guarda la marca y mira el tarro: la columna de la ruta ha cambiado. Vuelve a pedir `novedades.php`: el navegador (aquí, `curl`) **no envía** la cookie, porque la página no cuelga de `/junta/`. Explica para qué sirve `path` y por qué en el aula lo dejamos en `/`. Restáuralo.
- **(d) Reiniciar y caducar.** Pide `novedades.php?reiniciar=1` con `curl -i -c tarro.txt -b tarro.txt` y lee la cabecera `Set-Cookie` de la respuesta: valor `deleted`, fecha de 1970 y `Max-Age=0`. Compárala con la de la tarea (a) (`expires` dentro de 30 días, `Max-Age=2592000`). Responde: ¿quién decide que una cookie ha caducado, el servidor o el navegador? ¿Qué sabe el servidor de la cookie después de enviar esa cabecera? ¿Y por qué la marca «todavía llega» en la petición que la borra?

**Cómo trabajar.** Las dos preguntas de siempre, por separado: ¿ha venido la cookie? (`isset`, `??`) y ¿lo que ha venido sirve? (validación). Salidas esperadas con el fichero arreglado (la fecha de `expires` será la tuya; la marca que se guarda es siempre la del anuncio más reciente, `2026-10-24`):

`curl -i -c tarro.txt http://localhost:8000/novedades.php`

```
HTTP/1.1 200 OK
Host: localhost:8000
Date: Mon, 26 Oct 2026 11:50:11 GMT
Connection: close
Set-Cookie: ultima_visita=2026-10-24; expires=Wed, 25 Nov 2026 11:50:11 GMT; Max-Age=2592000; path=/; HttpOnly; SameSite=Lax
Content-type: text/html; charset=UTF-8

Primera visita: hay 8 anuncios en el tablón.
Marca guardada: 2026-10-24
```

`curl -b tarro.txt http://localhost:8000/novedades.php`

```
Desde tu última visita (2026-10-24) no hay anuncios nuevos.
Marca guardada: 2026-10-24
```

Con el tarro editado a `2026-10-10`:

```
Desde tu última visita (2026-10-10) hay 4 anuncios nuevos:
- 2026-10-12 · Convocatoria de la asamblea de otoño
- 2026-10-17 · Cambio del candado de la puerta de Ribera
- 2026-10-20 · Parcela libre en las Eras: lista de espera abierta
- 2026-10-24 · Mercadillo de la cosecha el último domingo del mes
Marca guardada: 2026-10-24
```

Con el tarro editado a `ayer` (y lo mismo con `2026-13-45`), tras escribir la validación:

```
Primera visita: hay 8 anuncios en el tablón.
Marca guardada: 2026-10-24
```

### CP3.3 · La solicitud de parcela en tres pantallas

**Nivel** intermedio · **Tiempo orientativo** 35 min (20 + 15 de la tarea desde cero).

**Qué refuerza.** El apartado 3 de la [UD3](ud3-estado-sesiones-autenticacion.md): `session_start()`, `$_SESSION` como cajón del servidor que el cliente no puede tocar, la llave `PHPSESSID` en el tarro, el fichero `sess_…` de `session.save_path` y su formato, arrays dentro de la sesión, y la pareja `session_unset()` + `session_destroy()`.

**Ficheros de partida.** `cp3-3/paso1.php`, `cp3-3/paso2.php`, `cp3-3/paso3.php` y `cp3-3/datos/parcelas.php`, en el repositorio de recursos (enlace en Classroom).

**Enunciado.** Pedir una parcela lleva tres pantallas: quién la pide (persona y contacto), qué prefiere (zona y riego) y el resumen con las parcelas libres que encajan, que se confirma. El estado viaja de una pantalla a otra en la sesión, y el servidor no deja saltarse pantallas. Pero la pantalla 2 siempre te manda a la 1, aunque la 1 se haya guardado bien. No hay ningún aviso.

**Tareas.**

- **(a) Caza el error.** Con un tarro, envía la pantalla 1 por POST (`persona=Teresa+Ansó&contacto=teresa.anso@example.org`) y después la 2 (`zona=Soto&riego=goteo`): la 2 responde «Empieza por la pantalla 1». Abre la carpeta `sesiones/` del servidor: el fichero `sess_…` **sí** tiene los datos de la pantalla 1. Así que el cajón está lleno y la pantalla 2 no lo abre. Localiza en `paso2.php` qué falta (es el error 3 del apartado 11 de la unidad, el más silencioso), arréglalo y repite la secuencia hasta la pantalla 3.
- **(b) Lee el cajón.** Con las tres pantallas hechas, pega el contenido del fichero `sess_…` y descífralo: qué significa `a:4:`, qué significan los `s:7:` y `s:12:`, y por qué `Teresa Ansó` cuenta 12 y no 11 (recuerda bytes frente a caracteres, apartado 2 de la UD2). Pega también la línea `Set-Cookie: PHPSESSID=…` de la primera respuesta y di qué contiene esa cookie y qué no.
- **(c) Saltarse pantallas.** Sin tarro, pide directamente `paso3.php` y `paso2.php`. Explica quién decide que no puedes entrar (¿el enlace que falta en la página o una comprobación en el servidor?) y qué pasaría si la pantalla 3 confiara en que «nadie llega aquí sin pasar por la 1».
- **(d) Confirmar vacía el cajón.** Confirma la solicitud por POST (`confirmar=1`) y vuelve a pedir `paso3.php` con el mismo tarro: faltan datos. Mira la carpeta `sesiones/`: ¿qué ha pasado con el fichero? Explica qué hace cada una de las dos funciones del cierre y qué le pasa a la llave que sigue en tu tarro.

- **(e) Desde cero.** Fichero vacío. Haz `cesta.php`, la **cesta del mercadillo de la cosecha**: cuatro productos con precio por kilo (tomate 2,00 €; pimiento 2,50 €; acelga 1,50 €; borraja 1,80 €). Por GET, `?anadir=tomate&kg=2` suma kilos a ese producto en la cesta (los kilos, un entero del 1 al 20; un producto que no existe o unos kilos que no valen se rechazan con un aviso y la cesta no cambia); `?retirar=acelga` lo quita; `?terminar=1` cierra la cesta con su importe y la vacía del todo. Siempre se imprime la cesta: cuántos productos, una línea por producto con sus kilos y su importe, y el total. Decide tú la estructura, las funciones y dónde guardas los precios. Pruébala con esta secuencia de ocho peticiones con el mismo tarro y compara letra a letra:

```
$ curl -c tarro.txt -b tarro.txt 'http://localhost:8000/cesta.php?anadir=tomate&kg=2'
Cesta (1 producto):
- tomate: 2 kg → 4,00 €
Total: 4,00 €
$ curl -c tarro.txt -b tarro.txt 'http://localhost:8000/cesta.php?anadir=acelga&kg=3'
Cesta (2 productos):
- tomate: 2 kg → 4,00 €
- acelga: 3 kg → 4,50 €
Total: 8,50 €
$ curl -c tarro.txt -b tarro.txt 'http://localhost:8000/cesta.php?anadir=tomate&kg=1'
Cesta (2 productos):
- tomate: 3 kg → 6,00 €
- acelga: 3 kg → 4,50 €
Total: 10,50 €
$ curl -c tarro.txt -b tarro.txt 'http://localhost:8000/cesta.php?anadir=lechuga&kg=2'
Producto no disponible: lechuga.
Cesta (2 productos):
- tomate: 3 kg → 6,00 €
- acelga: 3 kg → 4,50 €
Total: 10,50 €
$ curl -c tarro.txt -b tarro.txt 'http://localhost:8000/cesta.php?anadir=borraja&kg=25'
Los kilos tienen que ser un entero entre 1 y 20.
Cesta (2 productos):
- tomate: 3 kg → 6,00 €
- acelga: 3 kg → 4,50 €
Total: 10,50 €
$ curl -c tarro.txt -b tarro.txt 'http://localhost:8000/cesta.php?retirar=acelga'
Cesta (1 producto):
- tomate: 3 kg → 6,00 €
Total: 6,00 €
$ curl -c tarro.txt -b tarro.txt 'http://localhost:8000/cesta.php?terminar=1'
Cesta cerrada. Importe: 6,00 €
$ curl -c tarro.txt -b tarro.txt 'http://localhost:8000/cesta.php'
Cesta (0 productos):
Total: 0,00 €
```

**Cómo trabajar.** Cuando una sesión «se olvida de todo» y no hay ningún aviso, busca `session_start()` al principio del fichero antes de buscar nada más: el error no está donde se lee el dato, sino en que nadie abrió el cajón. Y acostúmbrate a mirar `sesiones/`: es la forma más rápida de saber qué guardó el servidor de verdad.

### CP3.4 · Cabeceras que llegan tarde

**Nivel** intermedio · **Tiempo orientativo** 35 min (20 + 15 de la tarea desde cero).

**Qué refuerza.** El apartado 4 de la [UD3](ud3-estado-sesiones-autenticacion.md): `header('Location: …')` con su 302, el `exit` que lo sigue, el patrón Post/Redirect/Get, los mensajes de un solo uso que se leen y se consumen, y la letra pequeña de las cabeceras: cualquier salida anterior, aunque sea una línea en blanco en un fichero incluido, llega antes que ellas.

**Ficheros de partida.** `cp3-4/votar.php`, `cp3-4/recuento.php`, `cp3-4/parciales/encabezado.php`, `cp3-4/parciales/utilidades.php`, `cp3-4/datos/propuestas.php` y `cp3-4/datos/parcelas.php`, en el repositorio de recursos (enlace en Classroom).

**Enunciado.** En la asamblea se votan cuatro propuestas. `recuento.php` muestra el aviso del último voto, tus papeletas y un formulario por propuesta; `votar.php` recibe la papeleta, la anota en la sesión y redirige al recuento. Servido con `php -S` parece que todo funciona. No funciona: esta práctica trae **tres fallos**, y los tres se ven con `curl` o en el terminal.

**Tareas.**

- **(a) Caza el primer error, en el terminal.** Ejecuta `php votar.php` en el terminal (sin servir) y lee los dos avisos: `session_start()` no ha podido arrancar y la cabecera no se ha podido enviar, porque **la salida empezó** en `parciales/utilidades.php`, en una línea concreta. Abre ese fichero por esa línea: es la línea en blanco que sigue a la etiqueta `?>` de cierre, en un fichero de PHP puro. Arréglalo como manda la convención del apartado 1 de la UD2 y comprueba que `php votar.php` ya no avisa. Después explica por qué servido con `php -S` **no se veía nada** (nota de reproducibilidad del apartado 4 de la unidad) y qué habría pasado en otro servidor.
- **(b) Caza el segundo, con `curl -i`.** Envía una papeleta: `curl -i -c tarro.txt -b tarro.txt -d 'propuesta=2&voto=si' http://localhost:8000/votar.php`. El código es `302` y hay `Location`, pero la respuesta **tiene cuerpo**: un párrafo que el navegador nunca enseña. Di qué línea falta en `votar.php` (error 5 del apartado 11), por qué el fichero que procesa un POST no debe imprimir nada, y arréglalo. Repite la petición: 302 y cuerpo vacío.
- **(c) Caza el tercero, con dos GET.** Pide `recuento.php` dos veces seguidas con el tarro. El aviso del voto aparece las dos veces: no es de un solo uso. Localiza en `recuento.php` qué le falta al mensaje para consumirse (el comentario del fichero dice lo que debería pasar) y arréglalo. Repite: la primera vez sale, la segunda no.
- **(d) Lo que PRG no evita.** Con los tres fallos arreglados, envía **dos veces** la misma papeleta con `curl` y mira el recuento: dos papeletas para la misma propuesta. PRG evita que el navegador reenvíe el POST al recargar, pero no un segundo envío hecho a propósito. Cambia la forma de guardar los votos en la sesión para que **cada propuesta tenga una sola papeleta** (la segunda sustituye a la primera) y haz que el recuento muestre, junto a cada propuesta, tu voto o un guion si no has votado. Explica en una línea por qué esta decisión es del servidor y no del formulario.

- **(e) Desde cero.** Fichero vacío (dos ficheros, en realidad). Haz la **puerta del huerto**: `entrada.php?zona=Ribera` responde con una redirección (302) a `huerto.php?zona=Ribera`; si la zona no existe, redirige a `huerto.php?aviso=zona`; sin zona, a `huerto.php`. `huerto.php` muestra, para una zona válida, cuántas parcelas tiene (con sus referencias, leídas de `datos/parcelas.php`) y los metros cuadrados que suman; sin zona válida, el título del huerto, el aviso si lo hay y la lista de zonas con enlace. Sin POST y sin sesión: el aviso viaja en la URL, validado. Decide tú la estructura. Pruébalo con estas seis peticiones y compara letra a letra:

```
$ curl -s -i 'http://localhost:8000/entrada.php?zona=Ribera' | grep -E 'HTTP|Location'
HTTP/1.1 302 Found
Location: huerto.php?zona=Ribera
$ curl -s -L 'http://localhost:8000/entrada.php?zona=Ribera'
<h1>Zona Ribera</h1>
<p>3 parcelas: H01, H02, H08 · 195 m²</p>
$ curl -s -i 'http://localhost:8000/entrada.php?zona=Marte' | grep -E 'HTTP|Location'
HTTP/1.1 302 Found
Location: huerto.php?aviso=zona
$ curl -s -L 'http://localhost:8000/entrada.php?zona=Marte'
<h1>El huerto</h1>
<p class="aviso">Esa zona no existe.</p>
<ul>
    <li><a href="huerto.php?zona=Ribera">Ribera</a></li>
    <li><a href="huerto.php?zona=Eras">Eras</a></li>
    <li><a href="huerto.php?zona=Soto">Soto</a></li>
</ul>
$ curl -s -i 'http://localhost:8000/entrada.php?zona=' | grep -E 'HTTP|Location'
HTTP/1.1 302 Found
Location: huerto.php
$ curl -s -L 'http://localhost:8000/entrada.php?zona='
<h1>El huerto</h1>
<ul>
    <li><a href="huerto.php?zona=Ribera">Ribera</a></li>
    <li><a href="huerto.php?zona=Eras">Eras</a></li>
    <li><a href="huerto.php?zona=Soto">Soto</a></li>
</ul>
```

**Cómo trabajar.** Las tres averías tienen la misma raíz: un fichero que procesa (recibe un POST, redirige) **no imprime** y **termina** justo después de redirigir, y un fichero que muestra **lee y consume** lo que el otro dejó en la sesión. Prueba las redirecciones siempre dos veces: servidas con `curl -i`, para ver el código y el cuerpo, y en el terminal, para ver los avisos que el servidor embebido se calla. Salidas esperadas de la práctica resuelta — la papeleta por `curl -i` (solo las líneas que importan; el identificador de sesión será otro) y las dos peticiones seguidas al recuento:

```
HTTP/1.1 302 Found
Set-Cookie: PHPSESSID=q0nchn0ru8dv3sv6d78n65osiv; path=/
Location: recuento.php
```

```
<header>
    <strong>Asociación de Huertos del Soto</strong> · Recuento</header>
<p class="aviso">Voto registrado: «Pasar las parcelas de las Eras a riego por goteo» → sí.</p>
<p>Propuestas votadas en esta sesión: 1 de 4</p>
===
<header>
    <strong>Asociación de Huertos del Soto</strong> · Recuento</header>
<p>Propuestas votadas en esta sesión: 1 de 4</p>
<ul>
```

### CP3.5 · El coste del hash

**Nivel** intermedio · **Tiempo orientativo** 35 min (20 + 15 de la tarea desde cero).

**Qué refuerza.** El apartado 5 de la [UD3](ud3-estado-sesiones-autenticacion.md): una contraseña nunca se guarda, se guarda su hash; `password_hash` con su sal y su coste, `password_verify` como única forma de comprobar, el prefijo `$2y$NN$` y `password_needs_rehash`.

**Ficheros de partida.** `cp3-5/entrada-terminal.php`, `cp3-5/coste.php` y `cp3-5/datos/cuentas.json` (las contraseñas de aula, en `cp3-5/datos/README.md`), en el repositorio de recursos (enlace en Classroom). Todo se ejecuta en el terminal: en esta práctica no hay servidor.

**Enunciado.** La asociación guarda sus ocho cuentas en un fichero JSON con el hash de cada contraseña. El programa que comprueba una cuenta desde el terminal rechaza a todo el mundo, también con la contraseña buena; y en el fichero hay dos hashes que se generaron hace tiempo con un coste demasiado bajo.

**Tareas.**

- **(a) Caza el error.** Ejecuta `php entrada-terminal.php teresa.anso huerto2026`: acceso denegado. Mira cómo compara el fichero la contraseña con el hash (es el error 4 del apartado 11 de la unidad), explica en dos líneas por qué esa comparación no puede dar nunca verdadero (¿qué guarda `hash`? ¿se puede «deshacer»?) y arréglalo. Comprueba que la contraseña correcta entra, que `Huerto2026` no entra y que una cuenta inexistente recibe el mismo mensaje que una contraseña mal (¿por qué conviene que sea el mismo?).
- **(b) Cuánto cuesta un hash.** Ejecuta `php coste.php` dos veces y anota los tiempos: cada punto de coste **duplica** el tiempo, más o menos. Explica qué es el coste (un exponente: `2^coste` vueltas), por qué un hash de contraseñas tiene que ser lento a propósito y qué le pasaría a un atacante con el fichero robado si el coste fuera 4. ¿Por qué el servidor no pone coste 20?
- **(c) Los hashes débiles.** Escribe `revisar-hashes.php`, que recorra el fichero y diga, para cada cuenta, su prefijo y si `password_needs_rehash` con el coste que exige la asociación (`['cost' => 10]`) pide regenerarlo; al final, cuántos quedan pendientes. Tienen que salir dos. Explica qué comprueba exactamente la función (pista: lee el prefijo, no la contraseña) y por qué **no** puedes regenerar esos dos hashes ahora mismo con un programa (¿qué te falta?).
- **(d) El momento.** Responde en tres líneas: ¿en qué momento tiene el servidor, de forma legítima, la contraseña en claro de una cuenta? ¿Qué haría ahí un programa bien hecho con un hash que `password_needs_rehash` marca? (Lo programarás en el CP3.6.)

- **(e) Desde cero.** Fichero vacío. Haz `politica.php`, con una función que reciba una cuenta y una contraseña y devuelva **la lista de motivos** por los que la contraseña no se admite (vacía si se admite): menos de 8 caracteres —contados como caracteres, no como bytes—, igual que la cuenta (sin distinguir mayúsculas) o incluida en una lista corta de contraseñas prohibidas (`12345678`, `contrasena`, `huerto2026`, `qwertyui`). El programa prueba seis casos e imprime una línea por caso. Decide tú los nombres. Salida esperada, letra a letra:

```
teresa.anso / acequia-del-soto: admitida
teresa.anso / corta: rechazada (tiene menos de 8 caracteres)
teresa.anso / Teresa.Anso: rechazada (es igual que la cuenta)
teresa.anso / 12345678: rechazada (está en la lista de contraseñas prohibidas)
teresa.anso / cañón-7: rechazada (tiene menos de 8 caracteres)
noa / noa: rechazada (tiene menos de 8 caracteres; es igual que la cuenta)
```

**Cómo trabajar.** Un hash no se compara: se verifica. Si tu programa compara dos cadenas en algún sitio donde una de ellas es un hash, está mal. Salida esperada de `php revisar-hashes.php`:

```
teresa.anso: $2y$10$ → correcto
mateo.larraz: $2y$10$ → correcto
julia.berges: $2y$04$ → hay que regenerar
ruben.asin: $2y$10$ → correcto
noa.lecina: $2y$10$ → correcto
ibai.sanjuan: $2y$04$ → hay que regenerar
aitor.lambea: $2y$10$ → correcto
olga.pardo: $2y$10$ → correcto
Pendientes de regenerar: 2 de 8
```

Y la de `php coste.php` en el equipo donde se preparó la práctica (los milisegundos serán otros en el tuyo; la proporción, no):

```
coste  4 ·     1.2 ms · $2y$04$…
coste  8 ·    16.2 ms · $2y$08$…
coste 10 ·    65.8 ms · $2y$10$…
coste 12 ·   260.9 ms · $2y$12$…
```

### CP3.6 · Cambiar la contraseña sin pasar por la web

**Nivel** avanzado · **Tiempo orientativo** 45 min (25 + 20 de la tarea desde cero).

**Qué refuerza.** El apartado 6 de la [UD3](ud3-estado-sesiones-autenticacion.md): el almacén de cuentas encerrado en unas pocas funciones que son las únicas que tocan el fichero, la escritura con `LOCK_EX` y su límite (leer-cambiar-escribir no es atómico), `password_verify` antes de cualquier cambio, el hash nuevo al guardar, el mensaje genérico y el principio de que la contraseña en claro no se guarda jamás.

**Ficheros de partida.** `cp3-6/deposito.php`, `cp3-6/cambiar.php` y `cp3-6/datos/cuentas.json`, en el repositorio de recursos (enlace en Classroom). Todo se ejecuta en el terminal. **Antes de empezar, copia el JSON** (`cp datos/cuentas.json datos/cuentas-original.json`): los programas de esta práctica lo escriben, y querrás volver al principio.

**Enunciado.** Un hortelano cambia su contraseña desde el terminal: `php cambiar.php cuenta actual nueva repetida`. El programa verifica la actual, exige una nueva de al menos 8 caracteres, distinta de la actual y repetida igual, y guarda el hash nuevo a través de `deposito.php`. Dice «Contraseña cambiada»… y la antigua sigue entrando.

**Tareas.**

- **(a) Caza el error.** Cambia la contraseña de `teresa.anso` (`huerto2026` → `acequia-del-soto`, repetida igual) y comprueba con el `entrada-terminal.php` arreglado del CP3.5 que la nueva **no** entra y la antigua **sí**. Lee la línea que calcula el hash nuevo: ¿de qué variable sale? Arréglalo y repite. Explica por qué un fallo así «pasa las pruebas» de quien solo mira el mensaje final.
- **(b) Las cuatro negativas.** Provoca, una a una, las cuatro respuestas negativas del programa (contraseña actual mal, nueva corta, nueva igual que la actual, repetición distinta) y anota el **código de salida** de cada una (`echo $?` o su equivalente en tu sistema). Explica por qué «la cuenta no existe» y «la contraseña actual está mal» responden con el mismo texto y el mismo código.
- **(c) Regenerar en el momento justo.** Escribe `verificar.php cuenta contraseña`: verifica (mensaje genérico si falla) y, si `password_needs_rehash` con coste 10 marca el hash, lo regenera **en ese momento** con la contraseña que acaba de verificar y lo guarda con `actualizar_cuenta`. Pruébalo con `julia.berges` dos veces seguidas: la primera regenera; la segunda, ya no. Explica por qué este es el único momento en que se puede hacer.
- **(d) Lo que no está en el fichero.** Con `grep -c huerto2026 datos/cuentas.json` comprueba que ninguna contraseña está en el fichero, y con `grep -c '"hash"'` que hay un hash por cuenta. Después lee `escribir_cuentas()` y responde: ¿qué protege `LOCK_EX`? ¿Qué **no** protege, si dos personas cambian su contraseña en el mismo instante (leer → cambiar → escribir)? ¿Dónde se resuelve eso (apartado 9 y UD6)?

- **(e) Desde cero.** Fichero vacío. Haz `baja-cuenta.php cuenta contraseña`: verifica la contraseña de la cuenta y, si es correcta, la elimina del fichero (a través de las funciones de `deposito.php`, que puedes ampliar) y dice cuántas cuentas quedan; si no, el mensaje genérico y código de salida 2. Salidas esperadas de tres ejecuciones seguidas (la segunda es la misma baja repetida):

```
$ php baja-cuenta.php noa.lecina huerto2026
Cuenta dada de baja: Noa Lecina (noa.lecina). Quedan 7 cuentas.
$ php baja-cuenta.php noa.lecina huerto2026
Credenciales no válidas.
(código de salida: 2)
$ php baja-cuenta.php ruben.asin otra
Credenciales no válidas.
(código de salida: 2)
```

**Cómo trabajar.** Cada vez que un programa escribe el JSON, mira el fichero: es la prueba de lo que ha pasado de verdad, no el mensaje. Salidas esperadas de la práctica resuelta — el cambio, la verificación con la antigua y con la nueva (`php cambiar.php`, `php verificar.php`), y las cuatro negativas con su código de salida:

```
Contraseña cambiada para Teresa Ansó.
(código de salida: 0)
Credenciales no válidas.
(código de salida: 2)
Acceso correcto: Teresa Ansó (hortelano), hash $2y$10$…
(código de salida: 0)
```

```
No se cambia: la nueva tiene que tener al menos 8 caracteres.
(código de salida: 3)
No se cambia: la nueva no puede ser igual que la actual.
(código de salida: 3)
No se cambia: la repetición no coincide.
(código de salida: 3)
Credenciales no válidas.
(código de salida: 2)
```

Y `php verificar.php julia.berges huerto2026` dos veces seguidas:

```
Acceso correcto: Julia Bergés (hortelano), hash $2y$04$…
Hash regenerado: ahora $2y$10$…
---
Acceso correcto: Julia Bergés (hortelano), hash $2y$10$…
```

### CP3.7 · La matriz de permisos como dato

**Nivel** intermedio · **Tiempo orientativo** 35 min (20 + 15 de la tarea desde cero).

**Qué refuerza.** El apartado 7 de la [UD3](ud3-estado-sesiones-autenticacion.md): autentificación frente a autorización, los permisos asignados a perfiles y no a personas, la matriz escrita antes de programar, y las dos respuestas distintas de las dos puertas (redirigir al que no ha entrado; 403 al que ha entrado pero no puede).

**Ficheros de partida.** `cp3-7/permisos.php` y `cp3-7/casos.php`, en el repositorio de recursos (enlace en Classroom). Todo se ejecuta en el terminal: aquí se **decide**, no se sirve.

**Enunciado.** La zona privada de la asociación tendrá tres perfiles —hortelano, vocal y presidencia— y tres páginas. En vez de repartir comprobaciones por los ficheros, la matriz vive en un único array, y dos funciones la leen: `puede(perfil, página)` y `respuesta(perfil o nadie, página)`, que devuelve el código de estado con el que respondería el servidor. `casos.php` imprime la tabla completa y no se modifica. Una página nueva, `cuotas.php`, se ha olvidado de anotar en la matriz… y la ve todo el mundo.

**Tareas.**

- **(a) Caza el error.** Ejecuta `php casos.php`: la fila de `cuotas.php` responde 200 a cualquier perfil. Lee `puede()`: ¿qué hace con una página que no está en la matriz? Explica por qué **permitir por defecto** es el peor valor por defecto posible en una autorización (piensa en la próxima página que alguien olvide anotar) y arréglalo para que lo que no está anotado esté cerrado. Repite: la fila de `cuotas.php` pasa a 403 (para nadie, sigue 302).
- **(b) Un perfil nuevo solo toca el dato.** La asociación crea el perfil `tesoreria`, que ve `mi-parcela.php`, `riegos.php` y la nueva `cuotas.php` (esta, junto con presidencia). Añádelo **sin tocar ninguna función**: solo la matriz. `casos.php` ya tiene la columna; comprueba que sale bien. Explica en una línea qué separa «cambiar los permisos de un perfil» de «cambiar quién tiene qué perfil».
- **(c) Las tres respuestas.** Con la tabla delante, explica por qué el servidor responde 302 a quien no ha entrado, 403 a quien ha entrado y no puede y 200 a quien puede; y por qué un «no puedes» con 200 es un error aunque la página sea bonita (¿quién lee los códigos de estado además de las personas?).
- **(d) El menú de cada perfil.** Escribe `paginas_de(perfil)`, que devuelva las páginas que ese perfil puede ver, leyéndolas de la matriz, e imprime desde un fichero aparte el menú de los cuatro perfiles, uno por línea. Explica por qué el menú y la puerta tienen que salir del **mismo** dato.

- **(e) Desde cero.** Fichero vacío. Las reglas te llegan al revés, como las escribe la junta: **perfil → páginas** (hortelano: `mi-parcela.php`; vocal: la anterior y `riegos.php`; tesoreria: las anteriores y `cuotas.php`; presidencia: todas y `cuentas.php`). Escribe un programa que, a partir de esas reglas, construya la matriz página → perfiles e imprima la tabla página × perfil con «sí» o «—», más una línea final con cuántas páginas y cuántos perfiles hay. Ojo con los acentos al alinear columnas (bytes frente a caracteres). Salida esperada, letra a letra:

```
                hortelano    vocal        tesoreria    presidencia
mi-parcela.php  sí           sí           sí           sí
riegos.php      —            sí           sí           sí
cuotas.php      —            —            sí           sí
cuentas.php     —            —            —            sí
4 páginas · 4 perfiles
```

**Cómo trabajar.** Una autorización se lee de arriba abajo con dos preguntas, en este orden: ¿hay alguien? y, solo si lo hay, ¿puede? Salida esperada de `php casos.php` con la matriz arreglada y el perfil de tesorería añadido, y del menú de la tarea (d):

```
                (nadie)      hortelano    vocal        tesoreria    presidencia
mi-parcela.php  302          200          200          200          200
riegos.php      302          403          200          200          200
cuentas.php     302          403          403          403          200
cuotas.php      302          403          403          200          200
```

```
hortelano   mi-parcela.php
vocal       mi-parcela.php, riegos.php
tesoreria   mi-parcela.php, riegos.php, cuotas.php
presidencia mi-parcela.php, riegos.php, cuentas.php, cuotas.php
```

### CP3.8 · Enlaces de confirmación de un solo uso

**Nivel** avanzado · **Tiempo orientativo** 45 min (25 + 20 de la tarea desde cero).

**Qué refuerza.** El apartado 8 de la [UD3](ud3-estado-sesiones-autenticacion.md): un secreto que solo conoce quien debe (`random_bytes` + `bin2hex`), la comparación en tiempo constante con `hash_equals`, la caducidad decidida por el servidor y el principio de que un secreto que se envía se gasta. Y, otra vez, la redirección que termina con `exit`.

**Ficheros de partida.** `cp3-8/solicitar.php`, `cp3-8/confirmar.php`, `cp3-8/datos/parcelas.php` y `cp3-8/datos/pendientes.json`, en el repositorio de recursos (enlace en Classroom). Se sirve con `php -S localhost:8000` desde la carpeta.

**Enunciado.** Quien pide una parcela libre tiene que confirmar la solicitud con un enlace que, en la vida real, le llegaría por correo; aquí la página lo imprime. El enlace lleva un secreto de 32 caracteres hexadecimales, vale diez minutos y se gasta al usarlo: `confirmar.php?t=…` lo comprueba, retira la solicitud de las pendientes y redirige a `solicitar.php?hecho=1`. Funciona. Pero se va de la lengua.

**Tareas.**

- **(a) Caza el error.** Solicita una parcela con `curl -d 'contacto=teresa.anso@example.org&parcela=H02' http://localhost:8000/solicitar.php`, copia el enlace de la respuesta, mira `datos/pendientes.json` y pide el enlace con `curl -i`. El código es 302, hay `Location`… y debajo viaja un párrafo con la parcela y el contacto de la solicitud: el fichero sigue ejecutándose después de redirigir (error 5 del apartado 11). Arréglalo y repite: 302 y cuerpo vacío. Explica a quién le llega ese cuerpo y por qué «el navegador no lo enseña» no es un consuelo.
- **(b) Prueba de autoridad.** Con el fichero arreglado, pide `confirmar.php` con `t=abc`, con `t=` vacío y, por último, **dos veces** con un enlace bueno: la primera confirma (302) y la segunda responde 403. Pega los códigos y explica dónde vive la prueba de que un enlace ya se usó (¿en la URL? ¿en el fichero del servidor?) y qué pasaría si el enlace valiera para siempre.
- **(c) Caducidad.** Solicita otra parcela y, antes de confirmar, edita en `datos/pendientes.json` el valor de `caduca` por uno del pasado. Pide el enlace: 403 y «caducado», y la solicitud desaparece del fichero. Responde: ¿quién decide que ha caducado? Compáralo con la caducidad de una cookie (CP3.2, tarea d).
- **(d) Por qué así.** Lee `generar_enlace()` y `consumir_enlace()` y responde en una línea cada una: ¿qué tiene `random_bytes` que no tenga `rand`? ¿Por qué `md5(time())` sería un secreto malo? ¿Qué aporta `hash_equals` frente a `===`? ¿Por qué `confirmar.php` rechaza el enlace antes de buscarlo si no tiene exactamente 32 hexadecimales?

- **(e) Desde cero.** Dos ficheros vacíos. La junta quiere mandar por correo un enlace al **recibo** de cada parcela que no se pueda adivinar ni cambiar: `recibo.php?ref=H03&firma=…`, donde la firma es `hash_hmac` con SHA-256 de la referencia y un secreto que solo conoce el servidor (en esta práctica, `soto-2026`). Un programa de terminal `firmar.php REF` imprime el enlace firmado; `recibo.php` comprueba la firma con `hash_equals` (403 si no cuadra), busca la parcela en `datos/parcelas.php` (404 si no existe) y muestra zona, superficie, riego y cuota anual por tramos (hasta 50 m², 30 €; de 51 a 80, 45 €; más de 80, 60 €). Decide tú el resto. Salidas esperadas (las firmas son exactas: salen de la referencia y del secreto, sin nada aleatorio):

```
$ php firmar.php H03
recibo.php?ref=H03&firma=d849ef5c129a3a0f64e296d14be33b031596e8abb4b06bba2ed94ff92ad85a02
$ curl -s -i 'http://localhost:8000/recibo.php?ref=H03&firma=d849ef5c129a3a0f64e296d14be33b031596e8abb4b06bba2ed94ff92ad85a02'
HTTP/1.1 200 OK

<h1>Recibo de la parcela H03</h1>
<p>Eras · 80 m² · riego por goteo · cuota anual 45 €</p>
$ curl -s -i 'http://localhost:8000/recibo.php?ref=H03&firma=0000'
HTTP/1.1 403 Forbidden

<p class="fallo">Firma no válida.</p>
$ curl -s 'http://localhost:8000/recibo.php?ref=H04&firma=8ff40eb891065f3f92e2deb070b4b170505e60742fbe501726a5ac0e013e7fc4'
<h1>Recibo de la parcela H04</h1>
<p>Eras · 100 m² · riego por manta · cuota anual 60 €</p>
$ curl -s -i 'http://localhost:8000/recibo.php?ref=H99&firma=9591c9f8a19798b5202ca5283e83562944d31e13b7ee4b1b5d0bd68e98b91678' | grep -E 'HTTP|fallo'
HTTP/1.1 404 Not Found
<p class="fallo">No hay ninguna parcela con esa referencia.</p>
```

**Cómo trabajar.** Un secreto que viaja en una URL se gasta o caduca: si no, es una llave que cualquiera que vea la dirección (un historial, un registro del servidor, una captura) puede usar. Salidas esperadas de la práctica resuelta (tu secreto será otro): la solicitud y el fichero, la confirmación con `curl -i`, el aviso al volver, el fichero vacío, y las tres peticiones de la prueba de autoridad:

```
<p class="aviso">Solicitud anotada. Confírmala con este enlace (en la vida real iría por correo): <a href="confirmar.php?t=8f57971818ad2cb8c972776fb1523528">confirmar.php?t=8f57971818ad2cb8c972776fb1523528</a></p>
[
    {
        "t": "8f57971818ad2cb8c972776fb1523528",
        "contacto": "teresa.anso@example.org",
        "parcela": "H02",
        "caduca": 1793017532
    }
]
```

```
HTTP/1.1 302 Found
Location: solicitar.php?hecho=1

===
<p class="aviso">Solicitud confirmada. La junta se pondrá en contacto contigo.</p>
===
[]
```

```
$ curl -s -i 'http://localhost:8000/confirmar.php?t=abc' | grep -E 'HTTP|fallo'
HTTP/1.1 403 Forbidden
<p class="fallo">Enlace no válido.</p>
$ curl -s -i 'http://localhost:8000/confirmar.php?t=' | grep -E 'HTTP|fallo'
HTTP/1.1 403 Forbidden
<p class="fallo">Enlace no válido.</p>
$ curl -s -i 'http://localhost:8000/confirmar.php?t=8f57971818ad2cb8c972776fb1523528' | grep -E 'HTTP|fallo'
HTTP/1.1 403 Forbidden
<p class="fallo">Enlace no válido.</p>
```

### CP3.9 · El banco de pruebas de la asamblea

**Nivel** avanzado · **Tiempo orientativo** 45 min (25 + 20 de la tarea desde cero).

**Qué refuerza.** El apartado 10 de la [UD3](ud3-estado-sesiones-autenticacion.md): el mismo fichero responde distinto según quién lo pide, así que una prueba tiene que decir *como quién*; `curl` con tarro como banco de pruebas reproducible, un guion que se ejecuta mañana igual que hoy, mirar dentro de la sesión y las trampas de las comprobaciones que «pasan» sin comprobar nada.

**Ficheros de partida.** `cp3-9/comprobaciones.sh` y `cp3-9/volcado.php`, en el repositorio de recursos (enlace en Classroom). Hace falta `bash` (en Windows, el de Git Bash) y `curl`. El guion prueba **tu** votación del CP3.4 y **tus** enlaces del CP3.8: lanza `php -d session.save_path=sesiones -S localhost:8000` desde tu `cp3-4` resuelto y `php -S localhost:8001` desde tu `cp3-8` resuelto, y el guion desde `cp3-9`.

**Enunciado.** Diez comprobaciones con `curl` sobre la votación y los enlaces: códigos de estado, redirecciones sin cuerpo, el aviso que aparece una vez, una papeleta por propuesta, el enlace de un solo uso. Cada una imprime OK o FALLO. La primera vez que lo ejecutas, todo OK. La segunda, no.

**Tareas.**

- **(a) Caza el primer error.** Ejecuta el guion dos veces seguidas: la segunda, la comprobación 1 falla. Lee el principio del guion: el tarro de la primera ejecución sigue ahí, y con él la sesión que ya votó. Arréglalo para que **cada ejecución empiece de cero** y repite las dos ejecuciones. Explica por qué un guion de pruebas que depende de lo que dejó la ejecución anterior no sirve para nada.
- **(b) Caza el segundo, con los servidores parados.** Para los dos servidores y ejecuta el guion: algunas comprobaciones dicen **OK**. Localiza cuáles y explica por qué (¿qué recibe `grep` de un servidor que no responde? ¿qué «comprueba» entonces una comprobación de ausencia?). Añade al principio una comprobación 0 que pare el guion con código de salida 1 si alguno de los dos servidores no responde (`curl -f` o el código de estado), haz que las comprobaciones de ausencia exijan primero una respuesta con contenido, y añade al final una línea con el número de fallos y un código de salida 0 o 1. Repite con los servidores parados y con ellos en marcha.
- **(c) Mirar dentro.** Copia `volcado.php` a tu `cp3-4` resuelto, vota una propuesta con `curl` y un tarro, y pide `volcado.php` con ese tarro: ahí está el cajón de tu sesión. Después bórralo y explica en una línea por qué un fichero así **nunca** se entrega.
- **(d) El guion sobre la versión rota.** Sirve en el 8000 la carpeta **de partida** del CP3.4 (la que tenía los tres fallos) y vuelve a lanzar el guion arreglado: cuenta los FALLO y di cuál de los tres fallos caza cada uno. Es la prueba de regresión de la práctica: si mañana rompes algo, el guion lo dice en diez segundos.

- **(e) Desde cero.** Fichero vacío. Haz `estados.sh`: para una lista de seis direcciones de tu `cp3-4` resuelto (el recuento, `votar.php` por GET, la puerta con zona válida, el huerto con zona válida, el huerto con zona inválida y una página que no existe), imprime una línea con la dirección y el código de estado que responde el servidor, sin descargar el cuerpo (pista: `curl -s -o /dev/null -w '%{http_code}'`). Salida esperada, letra a letra:

```
recuento.php                 200
votar.php                    302
entrada.php?zona=Ribera      302
huerto.php?zona=Ribera       200
huerto.php?zona=Marte        200
no-existe.php                404
```

**Cómo trabajar.** Antes de fiarte de un OK, pregúntate qué habría hecho falta para que saliera FALLO; si la respuesta es «nada», la comprobación no comprueba. Salidas esperadas: el guion arreglado con los dos servidores en marcha, con los servidores parados, y sobre la carpeta de partida del CP3.4:

```
== 0. Los dos servidores responden
   OK    responden
== 1. El recuento, sin haber votado, dice 0 de 4
   OK    0 de 4
== 2. Votar responde 302 a recuento.php y sin cuerpo
   OK    302
   OK    Location: recuento.php
   OK    sin cuerpo
== 3. El aviso aparece en el primer GET
   OK    aviso presente
== 4. El aviso NO aparece en el segundo GET
   OK    aviso consumido
== 5. Un segundo voto a la misma propuesta no suma papeletas
   OK    sigue 1 de 4
== 6. Pedir votar.php con GET redirige
   OK    302
== 7. Solicitar una parcela libre devuelve un enlace con 32 hexadecimales
   OK    token de 32
== 8. Un enlace inventado se rechaza con 403
   OK    403
== 9. El enlace bueno confirma: 302 a solicitar.php?hecho=1 y sin cuerpo
   OK    Location
   OK    sin cuerpo
== 10. El mismo enlace, usado por segunda vez, ya no vale
   OK    403: un solo uso
== Resultado: 0 fallos
(código de salida: 0)
===
== Resultado: 0 fallos
```

```
== 0. Los dos servidores responden
   FALLO no responde http://localhost:8000 (lanza php -S en cp3-4)
(código de salida: 1)
```

```
== 0. Los dos servidores responden
   OK    responden
== 1. El recuento, sin haber votado, dice 0 de 4
   FALLO esperaba «0 de 4»
== 2. Votar responde 302 a recuento.php y sin cuerpo
   OK    302
   OK    Location: recuento.php
   FALLO el 302 lleva cuerpo
== 3. El aviso aparece en el primer GET
   OK    aviso presente
== 4. El aviso NO aparece en el segundo GET
   FALLO el aviso se repite (o la respuesta está vacía)
== 5. Un segundo voto a la misma propuesta no suma papeletas
   FALLO esperaba «1 de 4»
== 6. Pedir votar.php con GET redirige
   OK    302
== 7. Solicitar una parcela libre devuelve un enlace con 32 hexadecimales
   OK    token de 32
== 8. Un enlace inventado se rechaza con 403
   OK    403
== 9. El enlace bueno confirma: 302 a solicitar.php?hecho=1 y sin cuerpo
   OK    Location
   OK    sin cuerpo
== 10. El mismo enlace, usado por segunda vez, ya no vale
   OK    403: un solo uso
== Resultado: 4 fallos
(código de salida: 1)
```

### CP3.10 · Las parcelas, en una tabla

**Nivel** avanzado · **Tiempo orientativo** 45 min (25 + 20 de la tarea desde cero). **Necesita el gestor de bases de datos del aula** (MySQL o MariaDB) con las credenciales que publica el docente; si no lo tienes a mano, déjala para la sesión en la que se trabaje el apartado 9.

**Qué refuerza.** El apartado 9 de la [UD3](ud3-estado-sesiones-autenticacion.md), en su uso **instrumental**: una conexión PDO con excepciones y filas como arrays asociativos, una consulta preparada con un marcador con nombre, el dato que viaja como parámetro y nunca pegado en el SQL, y qué pasa cuando el gestor no está. El acceso a datos se trabaja a fondo en la UD6.

**Ficheros de partida.** `cp3-10/parcelas.sql`, `cp3-10/conexion.php`, `cp3-10/consulta.php` y `cp3-10/libres.php`, en el repositorio de recursos (enlace en Classroom). Carga la tabla con `mysql --default-character-set=utf8mb4 -u TU_USUARIO -p TU_BASE < parcelas.sql` y ajusta `conexion.php` a tus credenciales antes de nada.

**Enunciado.** Las ocho parcelas de la asociación pasan a una tabla. `consulta.php` lee una parcela por su referencia desde el terminal con una consulta preparada; `libres.php?zona=` lista las libres de una zona, servido. La consulta funciona con una referencia que existe y revienta con una que no.

**Tareas.**

- **(a) Caza el error.** Ejecuta `php consulta.php H03` y después `php consulta.php H99`: error fatal. Lee el mensaje con calma: habla del tipo de retorno de `parcela_por_ref()` y de un `false`. Explica qué devuelve `fetch()` cuando no hay fila, por qué la directiva de tipos estrictos y el tipo `?array` lo convierten en un error en vez de dejarlo pasar, y arréglalo en la función (no en quien la llama). Comprueba que `H99` responde ahora con su mensaje y código de salida 2.
- **(b) El dato viaja aparte.** Sirve la carpeta y pide `libres.php?zona=Ribera` y `?zona=Marte`. Después envía una zona con una comilla dentro: `curl -s -G --data-urlencode "zona=Ribera' OR '1'='1" http://localhost:8000/libres.php`. El servidor busca **literalmente** una zona que se llama así y no encuentra ninguna. Explica, con el apartado 9 delante, qué habría pasado si la zona se hubiera pegado dentro de la cadena SQL, y por qué el marcador `:zona` lo impide. Mira también cómo llega la comilla al HTML del título (apartado 8 de la UD2: la salida se escapa igual).
- **(c) Sin dato externo.** Escribe `resumen.php`, de terminal, que imprima cuántas parcelas y cuántos metros tiene cada zona (`GROUP BY`). Aquí no hay ningún dato que venga de fuera: usa `query()` en vez de `prepare()` + `execute()` y explica en una línea cuándo basta con una y cuándo hace falta la otra. Comprueba las cifras contra `datos/parcelas.php` del CP3.4 (195, 180 y 215 m²).
- **(d) Cuando el gestor no está.** Cambia la contraseña de `conexion.php` por una incorrecta (o para el gestor) y ejecuta `php consulta.php H03`. Pega la primera línea de la excepción y explica por qué es **mejor** que fallar ruidosamente que devolver «no hay ninguna parcela» a todo el mundo (`ERRMODE_EXCEPTION`). Restaura la contraseña.

- **(e) Desde cero.** Fichero vacío. Haz `metros.php ZONA`, de terminal: cuántas parcelas tiene la zona y cuántos metros suman, cuántos metros están libres y qué referencias son; con una zona que no existe, un aviso y código de salida 2. Todo leído de la tabla con consultas preparadas. Salidas esperadas, letra a letra:

```
$ php metros.php Soto
Soto: 3 parcelas, 215 m², libres 95 m² (H05, H07)
$ php metros.php Eras
Eras: 2 parcelas, 180 m², libres 0 m²
$ php metros.php Marte
Zona sin parcelas: Marte
```

**Cómo trabajar.** Regla de la unidad, desde hoy y para siempre: todo valor que llega de fuera y acaba en una consulta va como parámetro. Salidas esperadas de la práctica resuelta — `consulta.php` con `H03` y con `H99`, y las tres peticiones de la tarea (b):

```
H03 · Eras · 80 m² · riego por goteo · adjudicada
No hay ninguna parcela H99.
(código de salida: 2)
```

```
$ curl -s 'http://localhost:8000/libres.php?zona=Ribera'
<h1>Parcelas libres en Ribera</h1>
<p>Parcelas libres: 1</p>
<ul>
    <li>H02 · 45 m² · riego por manta</li>
</ul>
$ curl -s 'http://localhost:8000/libres.php?zona=Marte'
<h1>Parcelas libres en Marte</h1>
<p>Parcelas libres: 0</p>
<ul>
</ul>
$ curl -s -G --data-urlencode "zona=Ribera' OR '1'='1" http://localhost:8000/libres.php
<h1>Parcelas libres en Ribera&#039; OR &#039;1&#039;=&#039;1</h1>
<p>Parcelas libres: 0</p>
<ul>
</ul>
```

Y la primera línea de la excepción de la tarea (d), con una contraseña incorrecta (tu gestor dirá su propio usuario y su propio puerto):

```
PHP Fatal error:  Uncaught PDOException: SQLSTATE[HY000] [1045] Access denied for user 'huerto'@'localhost' (using password: YES) in /ruta/conexion.php:7
```
