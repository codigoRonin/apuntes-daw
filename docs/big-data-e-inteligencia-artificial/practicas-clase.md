# Cuaderno de prácticas · Big Data e Inteligencia Artificial

**Módulo optativo AOP1089 — Big Data e Inteligencia Artificial · 2.º DAW · IES Río Arba · Curso 2026-27**

Este cuaderno reúne **ejercicios extra de clase**: prácticas más largas que los ejercicios de cada apartado de los apuntes, pensadas para trabajarlas con el enunciado delante y el intérprete abierto. **No son evaluables**: no se entregan por ningún canal ni cuentan para la nota. Sirven para lo que sirven los apuntes cuando dicen «ejecuta, rompe y arregla»: para que el método te salga solo cuando llegue la actividad evaluativa y la prueba.

Cómo funciona:

- Cada práctica lleva el código **CPu.n**: cuaderno de prácticas, unidad *u*, número *n*. CP1.1 es la primera práctica de la UD1.
- Las de la UD1 están pensadas **para casa**, como repaso de una unidad ya impartida; las de la UD2 se trabajan **en clase**, de forma individual o en pareja, según se indique ese día. Lo que no te dé tiempo a terminar en el aula lo acabas en casa y lo traes a la sesión de corrección.
- **La solución no está aquí.** Se corrige en clase y, después de corregirla, el docente la publica en el Classroom del módulo, con los ficheros resueltos para que los ejecutes y los compares con los tuyos.
- Las prácticas se hacen con el mismo entorno de la unidad: el **entorno virtual** de la UD1 activo (con `(.venv)` delante del indicador), `python -m py_compile fichero.py` como primer reflejo antes de ejecutar, scripts desde el terminal y cuadernos donde el enunciado lo pida, y VS Code configurado como explica el recurso [Entorno de trabajo: VS Code para Python](../recursos/entorno-vscode-python.md). Trabaja en una carpeta local propia (por ejemplo `cuaderno/ud1/cp1-1/`), fuera de tu repositorio de la unidad en el aula de código, para no mezclarlas con la entrega evaluable. Los ficheros de partida de las prácticas que los necesitan están en el repositorio de recursos (enlace en Classroom); cada práctica se ejecuta **desde su carpeta**, porque las rutas de los datos se resuelven desde donde lanzas `python`.
- Cada práctica indica su **nivel** (básico, intermedio, avanzado o tipo prueba) y un **tiempo orientativo**. Son acumulativas: cada una puede usar todo lo visto en las anteriores. Las intermedias y avanzadas terminan con una tarea **(e) Desde cero**: fichero vacío, el enunciado dice qué hace el programa y da la salida esperada, y los nombres y la estructura los decides tú.
- **Casi todas traen un error que cazar** en su fichero de partida, del tipo de los del apartado de errores frecuentes de cada unidad (el 9 de la UD1 y el 8 de la UD2). Encontrarlo, entender el mensaje —leído de abajo arriba— y arreglarlo es parte de la práctica. Las trazas que se muestran aquí son las de nuestro equipo: en el tuyo cambiarán las rutas, no el diagnóstico.
- Las salidas esperadas se han obtenido ejecutando: compara **letra a letra**. Una cifra que no cuadra es un dato, no un detalle.
- Puedes usar un asistente de IA en estas prácticas, con cabeza y con responsabilidad sobre el resultado, con la misma regla del módulo: el uso se declara —aquí, de palabra, cuando se corrige en clase, porque no hay entrega— y lo que traes lo explicas. Ejecuta y comprueba todo código que te dé un asistente: la IA se equivoca con soltura en nombres de parámetros y de librerías (ver el apartado 10 de la UD1 y el 9 de la UD2). Y recuerda la regla de la UD2: un fichero con datos personales no se pega en un chat.

El cuaderno crece durante el curso: solo aparecen las unidades que tienen prácticas extra, y cada unidad va ganando prácticas a medida que se trabajan en clase.

## Los datos del cuaderno

Todas las prácticas, de las dos unidades y el simulacro, trabajan sobre un mismo caso, distinto de los alojamientos rurales y del servicio de deportes de la UD1 y de la calidad del aire y los edificios públicos de la UD2: una **red de observadores de aves de las Cinco Villas**, una asociación ficticia de aficionados que censa aves en seis puntos de la comarca —dos sotos, un embalse, dos zonas de estepa y un pinar— y anota cada **avistamiento**: una especie, en un punto, un día, por un observador, con el número de ejemplares contados y, a veces, una nota de campo. Los datos son de aula, inventados: los topónimos son reales; los puntos, las personas, los recuentos y las notas, no. Los correos usan el dominio reservado `example.org`.

Vocabulario fijo, para que una palabra signifique siempre lo mismo: un **avistamiento** es una fila del registro; una **bandada** es un avistamiento de **50 ejemplares o más**; un **punto** es un lugar de observación de la red; la **temporada** de la UD1 y la UD2 es el otoño de 2025, del 1 de septiembre al 30 de noviembre; y la **semana** es la del calendario. Si una tarea cambia el umbral, dice «avistamientos de N o más». El registro de la UD2 es el mismo otoño, ensuciado: dos bandadas perdieron su recuento, y por eso la UD2 cuenta 36 y la UD1 38. Limpiar no recupera lo que se perdió al recoger.
## UD1 · Python para datos e IA: lenguaje especializado, entorno y librerías

Cinco prácticas de repaso, una por tramo de la unidad (apartados 1-3, 4-5, 6, 7 y 8), pensadas para casa. Las tres primeras no necesitan el modelo de lenguaje; las dos últimas usan `es_core_news_sm`, el que instalaste en la UD1, y ninguna descarga nada nuevo. Los datos son los de la red de observadores: `avistamientos.csv` (los 194 avistamientos del otoño de 2025), `puntos.json` (los seis puntos), `especies.json` (las diez especies, con su grupo y su estatus) y `notas_campo.csv` (veinte notas de campo). Muestra del registro principal (cabecera y cinco filas; fichero íntegro en el repositorio de recursos):

```
id,fecha,punto_id,especie,ejemplares,observador,notas
1,2025-09-01,PUN-02,Abejaruco europeo,19,OBS-04,"Abejarucos en los taludes del embalse de San Bartolomé, últimos del año. Repetiremos la semana que viene."
2,2025-09-01,PUN-02,Aguilucho lagunero,2,OBS-04,
3,2025-09-01,PUN-02,Ánade azulón,102,OBS-04,"Azulones en el embalse de San Bartolomé, más de cien. Ejea de los Caballeros celebraba fiestas y había ruido."
4,2025-09-02,PUN-02,Aguilucho lagunero,4,OBS-01,
5,2025-09-02,PUN-02,Garza real,5,OBS-01,
```

Y las primeras líneas de los dos JSON:

```json
{
  "red": "Red de observadores de aves de las Cinco Villas (datos ficticios de aula)",
  "puntos": [
    {"id": "PUN-01", "nombre": "Soto del Arba", "localidad": "Tauste", "habitat": "soto", "latitud": 41.92, "longitud": -1.25},
    {"id": "PUN-02", "nombre": "Embalse de San Bartolomé", "localidad": "Ejea de los Caballeros", "habitat": "embalse", "latitud": 42.15, "longitud": -1.12},
```

```json
{
  "catalogo": "Especies censadas por la red (nombre común, grupo y estatus en la comarca)",
  "especies": [
    {"nombre": "Grulla común", "grupo": "zancuda", "estatus": "de paso"},
    {"nombre": "Milano real", "grupo": "rapaz", "estatus": "invernante"},
```

### CP1.1 · El cuaderno de campo, en Python

**Nivel** básico · **Tiempo orientativo** 20 min.

**Qué refuerza.** Los apartados 1, 2 y 3 de la [UD1](ud1-python-datos-ia.md): el cuaderno y su orden de ejecución, lo que hace distinto a Python (tipos sin declarar, sangría, lo vacío es falso), las f-strings con formato, `if`/`elif`/`else`, `for` sin índice, `while`, `break` y `continue`.

**Fichero de partida.** `cp1-1/primeros_pasos.py`, en el repositorio de recursos (enlace en Classroom). Trae los doce avistamientos de la última semana de octubre escritos en el propio programa: esta práctica no lee ningún fichero.

**Enunciado.** Antes de tocar ficheros de verdad, el cuaderno de campo cabe en una lista de diccionarios. Con ella se repasa lo que la unidad enseña del lenguaje: qué imprime cada cosa, cómo se recorre una colección y cómo se para un bucle cuando ya ha encontrado lo que buscaba.

**Tareas.**

- **(a) Tres celdas.** Abre un cuaderno nuevo (`jupyter lab`, con el entorno activo) y escribe tres celdas: en la primera `vistas = []`, en la segunda `vistas.append("Grulla común")` y en la tercera `print(vistas, len(vistas))`. Ejecuta la primera, después la segunda **tres veces seguidas** y por último la tercera. Anota qué imprime y por qué la lista no tiene un elemento; escribe en una línea qué harías antes de entregar un cuaderno para que cualquiera obtenga lo que se lee de arriba abajo. Salida esperada de la tercera celda:

```
['Grulla común', 'Grulla común', 'Grulla común'] 3
```

- **(b) Predice sobre papel.** Con `especie = "Grulla común"` y `total = 666`, escribe la salida exacta de estas seis líneas **antes** de ejecutarlas, y después compruébalas en el intérprete: `print(type(3 / 2))` · `print(7 // 2)` · `print("5" + "5")` · `print(bool([]))` · `print(f"{especie:28}|")` · `print(f"{total / 7:.2f}")`. Para cada línea que no coincida con tu predicción, escribe el concepto que la explica (división real frente a entera, cadenas que se suman, lo vacío es falso, campo de anchura fija, decimales). Salida esperada:

```
<class 'float'>
3
55
False
Grulla común                |
95.14
```

- **(c) Caza el error.** Pásale `python -m py_compile primeros_pasos.py` al fichero de partida y lee el mensaje: no dice que falte sangría, dice que la que hay **no encaja con ninguna** de las de arriba. Cuenta los espacios de la línea señalada y de las anteriores, arréglalo y ejecuta `python primeros_pasos.py`: tiene que imprimir una línea por avistamiento —la especie en un campo de 28 caracteres, los ejemplares en uno de 5 y el porcentaje sobre el total con dos decimales— y la marca de cada uno. Mensaje de partida y salida esperada tras el arreglo:

```
  File "primeros_pasos.py", line 33
    print(f"{a['especie']:28}{a['ejemplares']:5}  {porcentaje:.2f} %  {marca}")
                                                                               ^
IndentationError: unindent does not match any outer indentation level
```

```
Aguilucho lagunero              4  0.60 %  pocos
Grulla común                  278  41.74 %  bandada
Ánade azulón                   47  7.06 %  grupo
Avutarda común                 30  4.50 %  grupo
Cigüeña blanca                 10  1.50 %  grupo
Grulla común                   45  6.76 %  grupo
Milano real                     7  1.05 %  pocos
Aguilucho lagunero              4  0.60 %  pocos
Grulla común                  170  25.53 %  bandada
Ánade azulón                   48  7.21 %  grupo
Aguilucho lagunero              2  0.30 %  pocos
Cigüeña blanca                 21  3.15 %  grupo
Total de ejemplares: 666
```

- **(d) Hasta la primera bandada.** Añade al final del fichero un `for` con `enumerate(avistamientos, start=1)` que recorra la lista **hasta la primera bandada** (50 ejemplares o más) y salga con `break`, imprimiendo su especie, sus ejemplares, su posición en la lista y cuántos avistamientos ha saltado antes («avistamientos saltados antes: N»). Después, un `while` con índice que cuente cuántos avistamientos tienen 10 ejemplares o más, usando `continue` para saltar los pequeños. Salida esperada de las dos líneas:

```
Primera bandada: Grulla común con 278 ejemplares, en la posición 2 (avistamientos saltados antes: 1)
Avistamientos de 10 ejemplares o más: 8 de 12
```

**Cómo trabajar.** La predicción escrita de la tarea (b) es la parte que vale: si ejecutas primero, el ejercicio se reduce a copiar. En la (c), el número de línea que da el mensaje es donde el intérprete se dio cuenta; mira también la línea de arriba. Configura el editor para que la tecla del tabulador escriba cuatro espacios y este error no vuelve a aparecer.

### CP1.2 · Fichas y recuentos

**Nivel** intermedio · **Tiempo orientativo** 35 min (20 + 15 de la tarea desde cero).

**Qué refuerza.** Los apartados 4 y 5 de la [UD1](ud1-python-datos-ia.md): funciones con `return` y docstring, listas, diccionarios y conjuntos, `setdefault`, las comprensiones, `sorted` con `key`, el troceado, `@dataclass`, `class` con `__init__`, `self` y `__str__`, `raise` y `try`/`except`, y la diferencia entre comentario y docstring.

**Ficheros de partida.** `cp1-2/recuentos.py` y `cp1-2/fichas.py`, en el repositorio de recursos (enlace en Classroom). Los dos trabajan sobre los mismos doce avistamientos del CP1.1.

**Enunciado.** La red quiere saber qué especies se han visto en **todos** los puntos visitados esta semana y entre qué fechas se vio cada una; y quiere una ficha de avistamiento que sepa decir si es una bandada y que admita un segundo recuento del mismo grupo. El primer fichero se para con un error, y el segundo tiene las clases a medias.

**Tareas.**

- **(a) Caza el error.** Ejecuta `python recuentos.py`. El programa imprime tres líneas y se para con un `NameError` sobre `visitados`. La variable existe —se crea dentro de `puntos_visitados()`—, pero no donde se usa. Explica en dos líneas por qué este `NameError` **no** es el caso del cuaderno con celdas en desorden del apartado 9, y arréglalo sin tocar la función: usa lo que la función devuelve. Mensaje de partida:

```
Traceback (most recent call last):
  File "recuentos.py", line 39, in <module>
    print("Puntos visitados esta semana:", sorted(visitados))
                                                  ^^^^^^^^^
NameError: name 'visitados' is not defined
```

- **(b) Conjuntos y fechas, sin ranking.** Escribe en `recuentos.py` dos funciones con docstring: `especies_en_todos_los_puntos(avistamientos)`, que devuelve el **conjunto** de especies que aparecen en todos los puntos presentes en la lista (construye primero `{punto: lista de especies}` y comprueba la pertenencia de cada especie en cada lista; el resultado, con una comprensión de conjunto), y `primera_y_ultima(avistamientos)`, que devuelve `{especie: (primera fecha, última fecha)}` ordenando la lista de fechas de cada especie con `sorted` y troceando `[0]` y `[-1]`. Imprime el conjunto ordenado y una línea por especie, en el orden de `sorted()` (las palabras que empiezan por letra con tilde van al final). Salida esperada completa del fichero:

```
PUN-01: ['Aguilucho lagunero', 'Cigüeña blanca', 'Grulla común', 'Ánade azulón']
PUN-02: ['Aguilucho lagunero', 'Grulla común', 'Ánade azulón']
PUN-03: ['Avutarda común', 'Cigüeña blanca', 'Grulla común', 'Milano real']
Puntos visitados esta semana: ['PUN-01', 'PUN-02', 'PUN-03']
Vistas en todos los puntos: ['Grulla común']
Aguilucho lagunero   2025-10-20 → 2025-10-27
Avutarda común       2025-10-21 → 2025-10-21
Cigüeña blanca       2025-10-21 → 2025-10-27
Grulla común         2025-10-20 → 2025-10-26
Milano real          2025-10-21 → 2025-10-21
Ánade azulón         2025-10-20 → 2025-10-26
```

- **(c) La ficha.** Completa `fichas.py` sin tocar el bloque de pruebas del final: en `Avistamiento`, la validación del constructor (`ejemplares` negativo lanza `ValueError` con un mensaje que diga el valor), `es_bandada()` (50 ejemplares o más), `sumar_ejemplares(n)` —un segundo recuento del mismo grupo: **el objeto cambia**— con su docstring, y `__str__` con la fecha, el nombre del punto (que es un objeto `Punto`: `self.punto.nombre`) y el recuento. Ejecuta y compara con la salida esperada; fíjate en que `help()` muestra tu docstring y en que el mismo objeto pasa de no ser bandada a serlo.
- **(d) Comentario y docstring.** La función `porcentaje()` del fichero tiene una docstring que no dice nada y un comentario que repite lo que la línea ya dice. Reescribe los dos como corresponde (la docstring dice qué hace y cómo se usa; el comentario, un porqué que no se ve en el código, o nada). Salida esperada completa de `python fichas.py`:

```
2025-10-26 · Soto del Arba: 170 de Grulla común
¿Bandada? True
2025-10-26 · Soto del Arba: 48 de Ánade azulón
¿Bandada? False
2025-10-26 · Soto del Arba: 53 de Ánade azulón
¿Bandada? True
Help on function sumar_ejemplares in module __main__:

sumar_ejemplares(self, n)
    Añade `n` ejemplares contados después (un segundo recuento del mismo grupo); el objeto cambia.

Error controlado: ejemplares no puede ser negativo: -3
Punto(codigo='PUN-01', nombre='Soto del Arba', habitat='soto')
Las grullas son el 76.23 % de los ejemplares del día
```

- **(e) Desde cero.** Fichero vacío, sin ficheros de datos. Estos ocho avistamientos de la primera quincena de noviembre van en una lista en el propio programa: 5/11 PUN-04 Avutarda común 21 · 5/11 PUN-04 Cigüeña blanca 20 · 5/11 PUN-04 Grulla común 308 · 7/11 PUN-03 Cigüeña blanca 3 · 7/11 PUN-03 Grulla común 179 · 7/11 PUN-03 Milano real 7 · 9/11 PUN-02 Garza real 5 · 9/11 PUN-05 Milano real 5. Escribe un programa que imprima, por cada especie y en el orden en que aparece por primera vez, la lista de puntos donde se vio (también en orden de primera aparición y sin repetir), y al final las especies vistas en un solo punto, en el orden de `sorted()`. Sin totales ni orden por cantidad. Decide tú las funciones y los nombres. Salida esperada, letra a letra:

```
Avutarda común   PUN-04
Cigüeña blanca   PUN-04, PUN-03
Grulla común     PUN-04, PUN-03
Milano real      PUN-03, PUN-05
Garza real       PUN-02
Vistas en un solo punto: Avutarda común, Garza real
```

**Cómo trabajar.** En la (b) no hay que contar nada: hay que preguntar «¿está?» a cada lista; si te sale un ranking, has resuelto otra cosa. En la (c), `sumar_ejemplares` modifica `self.ejemplares`: por eso la tercera llamada a `es_bandada()` cambia de respuesta sin crear ningún objeto nuevo. Y una docstring que solo repite el nombre de la función es peor que ninguna: `help()` la mostrará como si fuera documentación.

### CP1.3 · Tres orígenes, un listado

**Nivel** intermedio · **Tiempo orientativo** 40 min (25 + 15 de la tarea desde cero).

**Qué refuerza.** El apartado 6 de la [UD1](ud1-python-datos-ia.md): `with open` y `encoding`, `csv.DictReader` y la lección de que del CSV todo llega como texto, `json.load`, la conversión con `int()` y `date.fromisoformat`, la comparación de fechas, y el patrón conectar-cursor-ejecutar-recoger-cerrar de `sqlite3` con `executemany` y una consulta con `WHERE` y `ORDER BY`.

**Ficheros de partida.** `cp1-3/origenes_aves.py` y la carpeta `cp1-3/datos/` con `avistamientos.csv`, `puntos.json`, `especies.json` y `censo_2019.csv`, en el repositorio de recursos (enlace en Classroom). El programa se ejecuta desde `cp1-3/`.

**Enunciado.** La red guarda sus datos en tres sitios: el registro de la temporada en CSV, las fichas de puntos y especies en JSON, y un censo antiguo que alguien exportó desde una hoja de cálculo hace años. El programa de partida intenta leerlo todo y se para en el primer fichero. Esta práctica no calcula totales ni medias: **lista y selecciona**, que es lo primero que se hace con cualquier origen antes de fiarse de él.

**Tareas.**

- **(a) Caza el error.** Ejecuta `python origenes_aves.py`. La última línea de la traza dice qué byte no entiende el códec UTF-8 y en qué posición: el fichero antiguo está en otra codificación. Ábrelo con `encoding="latin-1"`, imprime sus tres primeras especies con su máximo de 2019 y, a continuación, provoca el **error contrario**: abre `avistamientos.csv` (que sí está en UTF-8) con `encoding="latin-1"` e imprime la especie de la sexta fila. No hay error, y por eso es peor. Última línea de la traza de partida y salida esperada de esta tarea:

```
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xfa in position 41: invalid start byte
```

```
Censo antiguo: 10 especies; la primera es Grulla común
  Grulla común         máximo de 2019: 54
  Milano real          máximo de 2019: 8
  Aguilucho lagunero   máximo de 2019: 3
Abierto con la codificación equivocada, sin error: Alondra comÃºn
```

- **(b) Listado entre dos fechas.** Convierte `ejemplares` a entero y `fecha` a `date` en todas las filas, y escribe el listado de los avistamientos entre el 3 y el 9 de noviembre de 2025 (las dos fechas incluidas, comparando objetos `date`, no cadenas): una línea por avistamiento con fecha, código del punto, especie en un campo de 20 y ejemplares en uno de 4. Salida esperada:

```
194 avistamientos leídos; columnas: ['id', 'fecha', 'punto_id', 'especie', 'ejemplares', 'observador', 'notas']
Tipo de ejemplares tal como llega: <class 'str'>
Avistamientos del 2025-11-03 al 2025-11-09:
  2025-11-05  PUN-04  Avutarda común         21
  2025-11-05  PUN-04  Cigüeña blanca         20
  2025-11-05  PUN-04  Grulla común          308
  2025-11-07  PUN-03  Cigüeña blanca          3
  2025-11-07  PUN-03  Grulla común          179
  2025-11-07  PUN-03  Milano real             7
  2025-11-08  PUN-04  Avutarda común         25
  2025-11-08  PUN-04  Grulla común          149
  2025-11-09  PUN-02  Garza real              5
  2025-11-09  PUN-02  Ánade azulón          112
  2025-11-09  PUN-05  Alondra común          46
  2025-11-09  PUN-05  Milano real             5
```

- **(c) Los JSON sirven para seleccionar.** Carga los dos JSON y construye con una comprensión el **conjunto** de códigos de punto cuyo hábitat es `estepa` y el de especies cuyo estatus es `de paso`. Con ellos, repite el listado de la (b) solo con los avistamientos de los puntos de estepa, y después di cuántos avistamientos de especies de paso hay en toda la temporada y en qué fechas fue el primero y el último (la lista ya está ordenada por fecha). Sin añadir columnas a las filas y sin sumar ejemplares: seleccionar es decidir qué filas entran. Salida esperada:

```
Puntos de la red: 6 | especies del catálogo: 10
Puntos de estepa: ['PUN-03', 'PUN-04'] | especies de paso: ['Grulla común']
En los puntos de estepa, del 2025-11-03 al 2025-11-09:
  2025-11-05  PUN-04  Avutarda común         21
  2025-11-05  PUN-04  Cigüeña blanca         20
  2025-11-05  PUN-04  Grulla común          308
  2025-11-07  PUN-03  Cigüeña blanca          3
  2025-11-07  PUN-03  Grulla común          179
  2025-11-07  PUN-03  Milano real             7
  2025-11-08  PUN-04  Avutarda común         25
  2025-11-08  PUN-04  Grulla común          149
Avistamientos de especies de paso en la temporada: 31; el primero el 2025-10-05 y el último el 2025-11-30
```

- **(d) Una consulta, con filtro y orden.** Carga las filas en la tabla `avistamiento` de `avistamientos.db` con `executemany` (la fecha, convertida de vuelta a texto con `str()`), y escribe **una** consulta que devuelva fecha, punto, especie y ejemplares de los avistamientos de 100 ejemplares o más, ordenados por fecha y, a igual fecha, de más a menos ejemplares. Imprímela fila a fila con el mismo formato que el listado de la (b). Salida esperada:

```
Avistamientos de 100 ejemplares o más, por fecha:
  2025-09-01  PUN-02  Ánade azulón          102
  2025-10-10  PUN-02  Grulla común          133
  2025-10-19  PUN-02  Grulla común          146
  2025-10-20  PUN-02  Grulla común          278
  2025-10-26  PUN-01  Grulla común          170
  2025-10-30  PUN-06  Grulla común          133
  2025-11-05  PUN-04  Grulla común          308
  2025-11-07  PUN-03  Grulla común          179
  2025-11-08  PUN-04  Grulla común          149
  2025-11-09  PUN-02  Ánade azulón          112
  2025-11-12  PUN-01  Grulla común          114
  2025-11-19  PUN-06  Grulla común          140
  2025-11-20  PUN-04  Grulla común          134
  2025-11-24  PUN-04  Grulla común          131
  2025-11-28  PUN-06  Grulla común          137
  2025-11-29  PUN-03  Grulla común          112
```

- **(e) Desde cero.** Fichero vacío en `cp1-3/`. Escribe un programa que lea `datos/puntos.json` y `datos/avistamientos.csv` y escriba una línea por **bandada de octubre** en los puntos cuyo hábitat es `soto` —fecha, código del punto, especie en un campo de 20 y ejemplares en uno de 4—, ordenadas por fecha, y al final cuántas líneas ha escrito. El hábitat se decide con el JSON; el mes, con la fecha convertida a `date`. Salida esperada, letra a letra:

```
2025-10-26  PUN-01  Grulla común          170
2025-10-30  PUN-06  Grulla común          133
2 bandadas de octubre en los sotos
```

**Cómo trabajar.** El error de la (a) es de los que no se arreglan «probando codificaciones»: pregunta de dónde salió el fichero. En la (b), compara fechas convertidas: dos cadenas `AAAA-MM-DD` también se ordenan bien, hasta el día en que un fichero llegue con `03/11/2025`. En la (d), la consulta ordena; no ordenes tú después en Python lo que ya pediste ordenado.

### CP1.4 · Lo que el modelo cree ver

**Nivel** intermedio · **Tiempo orientativo** 35 min (20 + 15 de la tarea desde cero).

**Qué refuerza.** El apartado 7 de la [UD1](ud1-python-datos-ia.md): cargar un modelo preentrenado con `spacy.load`, aplicarlo a un texto, leer `doc.ents` con `ent.text` y `ent.label_`, y la lección del apartado: el modelo fue entrenado sobre texto periodístico y **devuelve lo probable, no lo correcto**.

**Ficheros de partida.** `cp1-4/entidades_notas.py` y la carpeta `cp1-4/datos/` con `notas_campo.csv` y `puntos.json`, en el repositorio de recursos (enlace en Classroom). Se ejecuta desde `cp1-4/`, con el entorno de la UD1 activo (el modelo `es_core_news_sm` ya está instalado desde el apartado 1; no instales nada). Las salidas de esta práctica se obtuvieron con la versión 3.8 del modelo; con otra versión las entidades pueden cambiar: comprueba la tuya con `python -m spacy validate`.

**Enunciado.** Las notas de campo de la red están llenas de lugares: Tauste, Gallocanta, las Bardenas, el Moncayo. El modelo de lenguaje debería encontrarlos. Lo hace… y además encuentra lugares donde hay verbos y pájaros. Esta práctica no cuenta cuáles son los más frecuentes: mide **cuánto depende el modelo de cómo está escrita la frase** y coteja lo que devuelve contra una lista cerrada.

**Tareas.**

- **(a) Caza el error.** Ejecuta el fichero de partida. La última línea de la traza es un error de spaCy con un código entre corchetes: el modelo «no se encuentra». Antes de instalar nada, ejecuta `python -m spacy validate` y compara el nombre que lista con el que pide el programa. Arréglalo con una letra. Última línea de la traza de partida:

```
OSError: [E050] Can't find model 'es_cores_news_sm'. It doesn't seem to be a Python package or a valid path to a data directory.
```

- **(b) La misma nota, escrita de dos formas.** El fichero arreglado aplica el modelo a una nota y a su reescritura (la misma información, con el nombre del ave y el verbo fuera del inicio de frase) e imprime las entidades de cada una. Explica la diferencia con el apartado 7: qué hace el modelo con una palabra con mayúscula que abre una frase. Repite el experimento con la nota «Grullas en el embalse al atardecer, entrando desde las Bardenas Reales. Contamos con Marta y Jorge por separado y cuadró.»: reescríbela tú para que ni «Grullas» ni «Contamos» abran frase y compara. Y una tercera: «Fuimos hasta Uncastillo a comer y volvimos por Luna.» frente a «A comer fuimos hasta Uncastillo y volvimos por Luna.» —aquí reescribir **empeora**: di qué etiqueta pierde el topónimo y por qué eso también es esperable. Salida esperada de los tres pares (la segunda y la tercera reescritura son las nuestras; si la tuya es distinta, la salida también):

```
[('Cigüeña', 'LOC'), ('Luna', 'LOC'), ('Llovía', 'LOC'), ('Pirineo', 'LOC')]
[('Luna', 'LOC'), ('Pirineo', 'LOC')]
[('Grullas', 'LOC'), ('Bardenas Reales', 'LOC'), ('Contamos', 'LOC'), ('Marta', 'PER'), ('Jorge', 'PER')]
[('Bardenas Reales', 'LOC'), ('Marta', 'PER'), ('Jorge', 'PER')]
[('Uncastillo', 'LOC'), ('Luna', 'LOC')]
[('Uncastillo', 'PER'), ('Luna', 'LOC')]
```

- **(c) Cotejo contra una lista cerrada.** La red tiene seis localidades (las de `puntos.json`). Escribe `lugares_conocidos(doc, localidades)`, que devuelve las entidades `LOC` cuyo texto es exactamente una localidad de la red, y `fuera_de_lista(doc, localidades)`, que devuelve las `LOC` que no lo son. Aplícalas a las veinte notas e imprime cuántas menciones de localidades de la red hay y la lista ordenada de lo que queda fuera. Salida esperada:

```
Menciones de localidades de la red: 8
Entidades LOC fuera de la lista: 33 → ['Arba', 'Bardenas Reales', 'Castejón', 'Castejón', 'Contamos', 'Contamos', 'El Ebro', 'Fuimos', 'Gallocanta', 'Gallocanta', 'Grullas', 'Grullas', 'Las grullas', 'Llovía', 'Milano', 'Moncayo', 'Monte de Luna', 'Monte de Luna', 'Navarra', 'Pirineo', 'Pradilla', 'Pradilla', 'Subimos', 'Tudela', 'Uncastillo', 'Valdejasa', 'Zaragoza', 'Zaragoza', 'embalse de San Bartolomé', 'embalse de San Bartolomé', 'observatorio de Ejea de los Caballeros', 'puente de Tauste', 'telescopio de Pilar']
```

- **(d) Dos reglas, dos pérdidas.** En lo que queda fuera de la lista conviven lugares de verdad lejanos (Gallocanta, Bardenas Reales, Moncayo, Zaragoza), nombres de lugar incompletos (Pradilla, Castejón) y errores del modelo (Grullas, Contamos, Llovía, Milano). Compara esta regla de lista cerrada con la de la palabra suelta al inicio de frase del apartado 8 (`len(ent) == 1 and ent.start == ent.sent.start`): escribe tres líneas con qué elimina cada una, qué acierto se lleva por delante cada una, y cuál usarías para un informe que solo hable de la comarca y cuál para uno que hable de excursiones.

- **(e) Desde cero.** Fichero vacío en `cp1-4/`. Escribe un programa que cargue el modelo una vez, lea las notas y las localidades, y por cada nota imprima su identificador (en un campo de 2), cuántas entidades `LOC` encuentra el modelo y cuáles de ellas **no** están en la lista de localidades (o `-` si todas lo están); al final, cuántas notas no mencionan ningún lugar. Salida esperada, letra a letra:

```
Nota  1: lugares 1; fuera de la lista: embalse de San Bartolomé
Nota  2: lugares 1; fuera de la lista: embalse de San Bartolomé
Nota  3: lugares 2; fuera de la lista: Monte de Luna
Nota  4: lugares 1; fuera de la lista: -
Nota  5: lugares 3; fuera de la lista: Pradilla, El Ebro, Navarra
Nota  6: lugares 3; fuera de la lista: Llovía, Pirineo
Nota  7: lugares 2; fuera de la lista: Castejón, Zaragoza
Nota  8: lugares 0; fuera de la lista: -
Nota  9: lugares 3; fuera de la lista: Grullas, Bardenas Reales, Contamos
Nota 10: lugares 0; fuera de la lista: -
Nota 11: lugares 3; fuera de la lista: Moncayo, Castejón, Valdejasa
Nota 12: lugares 2; fuera de la lista: Gallocanta
Nota 13: lugares 3; fuera de la lista: Gallocanta, Zaragoza, puente de Tauste
Nota 14: lugares 4; fuera de la lista: Grullas, Contamos, observatorio de Ejea de los Caballeros, telescopio de Pilar
Nota 15: lugares 2; fuera de la lista: Monte de Luna, Subimos
Nota 16: lugares 2; fuera de la lista: Pradilla, Las grullas
Nota 17: lugares 1; fuera de la lista: -
Nota 18: lugares 2; fuera de la lista: Arba, Tudela
Nota 19: lugares 2; fuera de la lista: Milano
Nota 20: lugares 4; fuera de la lista: Fuimos, Uncastillo
Notas sin ningún lugar: 2 de 20
```

**Cómo trabajar.** El código entre corchetes del error de la (a) sale en la guía de resolución de problemas de spaCy (apartado 12): acostúmbrate a buscarlo así. En la (b), el experimento vale porque cambias **una sola cosa** (la posición de la palabra) y miras qué pasa; eso es medir un modelo. Y en la (c) fíjate en que «Pradilla» no está en la lista pero «Pradilla de Ebro» sí: la comparación exacta es honesta y tacaña a la vez.

### CP1.5 · Un programa que no se cae

**Nivel** avanzado · **Tiempo orientativo** 50 min (30 + 20 de la tarea desde cero).

**Qué refuerza.** El apartado 8 de la [UD1](ud1-python-datos-ia.md): la anatomía de un programa que integra datos y modelo —docstring de módulo, una función por responsabilidad, el modelo cargado una vez, el bloque `if __name__ == "__main__":`, `sys.argv` y `sys.exit`—, más dos cosas que un programa que se ejecuta sin nadie delante tiene que saber hacer: rechazar argumentos malos y fallar con un mensaje claro y un código de salida cuando el fichero no existe.

**Ficheros de partida.** `cp1-5/lugares_bandadas.py` y la carpeta `cp1-5/datos/` con `avistamientos.csv` y `puntos.json`, en el repositorio de recursos (enlace en Classroom). Se ejecuta desde `cp1-5/`, con el entorno de la UD1 activo.

**Enunciado.** El programa de partida está **a medias**: lee los avistamientos, aplica el modelo a la nota de cada bandada y escribe `lugares_bandadas.csv` con la fecha, la especie, los ejemplares y los lugares que la nota cita. No escribe nada más que ese CSV. Pero se cae de tres maneras distintas, y una de ellas no es un error: es que nadie le enseñó a fallar bien.

**Tareas.**

- **(a) Caza el error.** Ejecuta `python lugares_bandadas.py datos/avistamientos.csv`. La última línea de la traza dice qué dos cosas no se pueden concatenar; arréglalo con una f-string en las dos líneas que lo hacen, y comprueba el CSV resultante. Última línea de la traza de partida, salida esperada tras el arreglo (la línea de segundos es orientativa: depende del equipo) y contenido del CSV:

```
TypeError: can only concatenate str (not "int") to str
```

```
Avistamientos leídos: 194
Filas escritas en lugares_bandadas.csv: 6
Segundos de modelo: 0.7
```

```
fecha,especie,ejemplares,lugares
2025-09-01,Ánade azulón,102,embalse de San Bartolomé
2025-10-19,Grulla común,146,Bardenas Reales
2025-11-12,Grulla común,114,Tauste; Gallocanta
2025-11-14,Grulla común,51,Gallocanta; Zaragoza; puente de Tauste
2025-11-14,Grulla común,71,observatorio de Ejea de los Caballeros; telescopio de Pilar
2025-11-22,Ánade azulón,53,Arba
```

- **(b) Argumentos.** Sin argumento, el programa se cae con un `IndexError` sobre `sys.argv` (pruébalo). En el bloque `__main__`, comprueba cuántos argumentos llegan: sin el fichero, imprime una línea de uso y termina con `sys.exit(1)`. Añade un **segundo argumento opcional**, el umbral de ejemplares (por defecto 50, una bandada): si no es un entero (`int()` dentro de un `try`) o no está entre 1 y 5000, mensaje claro y `sys.exit(1)`. Comprueba el código de salida en el terminal justo después de cada ejecución (`echo $?` en macOS y Linux; `echo $LASTEXITCODE` en PowerShell). Salidas esperadas, por orden: sin argumento, con `20x`, con `9000` y con `100`:

```
Uso: python lugares_bandadas.py <fichero.csv> [umbral]
El umbral tiene que ser un número entero, no '20x'
El umbral tiene que estar entre 1 y 5000, no 9000
Avistamientos leídos: 194
Filas escritas en lugares_bandadas.csv: 3
```

- **(c) El fichero que no existe.** Ejecuta `python lugares_bandadas.py datos/no_existe.csv`: hoy es una traza larga con un `FileNotFoundError` en la última línea. Captúralo en `leer_avistamientos()` con `try`/`except FileNotFoundError` (el apartado 9 nombra este error; la forma `except ... :` es la del apartado 5), imprime un mensaje que diga qué fichero y desde dónde se resuelve la ruta, y termina con `sys.exit(2)`: un código distinto del de los argumentos, para que quien lance el programa sepa qué ha fallado sin leer el mensaje. Salida esperada y tabla de códigos de salida que tiene que cumplir tu programa:

```
No encuentro el fichero datos/no_existe.csv: comprueba la ruta (se resuelve desde la carpeta en que lanzas python)
```

```
python lugares_bandadas.py datos/avistamientos.csv → código de salida 0
python lugares_bandadas.py → código de salida 1
python lugares_bandadas.py datos/avistamientos.csv 20x → código de salida 1
python lugares_bandadas.py datos/avistamientos.csv 9000 → código de salida 1
python lugares_bandadas.py datos/no_existe.csv → código de salida 2
```

- **(d) El modelo, una vez; y el programa, función a función.** `lugares_de_la_nota()` carga el modelo **cada vez** que se llama: una bandada, una carga. Sácalo a `main()`, pásalo como parámetro y mide la diferencia con `time.perf_counter()` antes y después del análisis (en nuestro equipo, de 2,6 a 0,7 segundos; en el tuyo serán otros números, pero la proporción se parecerá). Después escribe, para cada función del programa, una línea con qué hace y qué **no sabe** (`analizar` no sabe de ficheros; `escribir` no sabe de modelos): es la pregunta de la defensa.

- **(e) Desde cero.** Fichero vacío en `cp1-5/`; guárdalo como `avistamientos_punto.py` (la salida esperada lo nombra). Escribe un programa que reciba dos argumentos, la ruta del CSV y un código de punto, y escriba `<codigo>.csv` (por ejemplo `PUN-03.csv`) con la fecha, la especie y los ejemplares de los avistamientos de ese punto, ordenados por fecha, e imprima cuántas filas ha escrito. Si faltan argumentos o el punto no está en `datos/puntos.json`, mensaje y salida 1; si el CSV no existe, mensaje y salida 2; si todo va bien, salida 0. Sin modelo. Salidas esperadas (pantalla con `PUN-03`, primeras cuatro líneas del CSV, los tres fallos y la tabla de códigos):

```
32 avistamientos de PUN-03 escritos en PUN-03.csv
```

```
fecha,especie,ejemplares
2025-09-03,Alondra común,17
2025-09-03,Avutarda común,29
2025-09-03,Cigüeña blanca,14
```

```
El punto PUN-09 no está en la red
No encuentro el fichero otro.csv
Uso: python avistamientos_punto.py <fichero.csv> <codigo de punto>
```

```
python avistamientos_punto.py datos/avistamientos.csv PUN-03 → código de salida 0
python avistamientos_punto.py datos/avistamientos.csv PUN-09 → código de salida 1
python avistamientos_punto.py otro.csv PUN-03 → código de salida 2
python avistamientos_punto.py → código de salida 1
```

**Cómo trabajar.** Un programa que se ejecuta solo no tiene a nadie que lea la traza: por eso el mensaje y el código de salida son parte del programa, no un adorno. Prueba siempre los tres casos malos antes del bueno. Y cuando midas el tiempo, mide dos veces: la primera carga del modelo paga el disco; la segunda, no. El fichero de partida escribe el CSV línea a línea con `fichero.write()`, que es lo que hace cualquier programa cuando no hay librería de por medio; para leerlo sigue valiendo `csv.DictReader`.
## UD2 · Preparación, análisis y visualización de datos; protección de datos

Las prácticas de esta unidad se trabajan en clase, una por apartado, sobre la misma red de observadores: el registro de la temporada tal como lo exportó la asociación —`avistamientos_bruto.csv`, 199 filas que llegan **sucias**— más las fichas de puntos y especies y, solo para el apartado 1, la lista de observadores. Limpiar el bruto no es el objetivo de ninguna práctica entera: cada técnica se ensaya por separado, sobre la tabla que mejor la enseña, y desde el CP2.5 se trabaja sobre `avistamientos_limpio.csv`, que es el bruto después de pasar por todo el apartado 4. Muestra del bruto (cabecera y cinco filas; fichero íntegro en el repositorio de recursos):

```
id,fecha,punto_id,especie,ejemplares,temperatura,duracion_h,observador_id,notas
1,2025-09-01,PUN-02,Abejaruco europeo,19,"26,9",1.0,OBS-04,"Abejarucos en los taludes del embalse de San Bartolomé, últimos del año. Repetiremos la semana que viene."
2,2025-09-01,PUN-02,Aguilucho lagunero,2,"23,4",1.5,OBS-04,
3,2025-09-01,PUN-02,Ánade azulón,102,"21,8",2.0,OBS-04,"Azulones en el embalse de San Bartolomé, más de cien. Ejea de los Caballeros celebraba fiestas y había ruido."
4,2025-09-02,PUN-02,Aguilucho lagunero,4,"20,6",2.5,OBS-01,
5,2025-09-02,PUN-02,Garza real,5,"17,3",3.0,OBS-01,
```

Las columnas: `fecha` del censo, `punto_id` del punto de la red, `especie`, `ejemplares` contados, `temperatura` a la hora del censo en grados, `duracion_h` horas que duró el censo, `observador_id` (el código del observador: su nombre y su contacto viven en otra tabla) y `notas` de campo, libres y a menudo vacías.

### CP2.1 · ¿Quién está detrás de estos datos?

**Nivel** básico · **Tiempo orientativo** 15 min.

**Qué refuerza.** El apartado 1 de la [UD2](ud2-preparacion-visualizacion-datos.md): qué es un dato personal y qué es «identificable», los principios del artículo 5, la diferencia entre seudonimizar (artículo 4.5) y anonimizar (considerando 26), las cuatro cajas de la lista de comprobación del 1.3 y la k-anonimidad.

**Ficheros de partida.** `cp2-1/publicar.py` y la carpeta `cp2-1/datos/` con `observadores.csv` (los ocho observadores de la red, con nombre, correo, teléfono, edad y localidad: **personas inventadas**, y así lo dice el fichero) y `avistamientos_muestra.csv` (veinte avistamientos con su `observador_id` y su nota), en el repositorio de recursos (enlace en Classroom). Se ejecuta desde `cp2-1/`.

**Enunciado.** Alguien de la red escribió `publicar.py` para colgar en la web la lista de observadores «sin datos personales»: quita el correo y guarda el resto. Tú no cargas nada con pandas en esta práctica: **auditas** lo que ese script hace, con el Reglamento delante y con Python puro de la UD1.

**Tareas.**

- **(a) Caza el error.** Ejecuta `python publicar.py`. Se para con un `KeyError`: la columna que pide no existe con ese nombre. `print(obs.columns.tolist())` —que el script ya hace— te dice cómo se llama de verdad. Arréglalo y ejecuta: tiene que escribir `publicable.csv`. Última línea de la traza de partida y salida esperada tras el arreglo:

```
KeyError: 'Correo'
```

```
['observador_id', 'nombre', 'correo', 'telefono', 'edad', 'localidad'] (8, 6)
Correos retirados: 8
Escrito publicable.csv con 8 filas
```

- **(b) Las cuatro cajas, con titular.** En un fichero `respuestas.md`, clasifica cada columna de los dos ficheros en las cuatro cajas del apartado 1.3 (identificador directo, cuasi-identificador, categoría especial, dato no personal) y escribe de **quién** es cada dato: del observador o de un tercero. Lee las veinte notas antes de contestar por la columna `notas`: hay un vecino de Tauste, un pastor de Castejón y una Pilar con telescopio.
- **(c) k por parejas, con Python puro.** Con `csv.DictReader` (apartado 6 de la UD1), lee `publicable.csv` y cuenta cuántas filas comparten cada pareja localidad-edad (un diccionario con la pareja como clave). Imprime cuántas parejas hay, la k mínima y cuántas parejas tienen una sola persona; después señala **la fila que identifica a una persona concreta aunque no llevara nombre**: la única de su localidad. Salida esperada:

```
Columnas publicadas: ['observador_id', 'nombre', 'edad', 'localidad']
8 filas, 8 parejas localidad-edad distintas; k mínima = 1; parejas con una sola persona: 8
La única persona de Sádaba: 71 años → Pilar Beltrán
```

- **(d) Veredicto.** Responde en `respuestas.md`, en cinco líneas: lo que hace `publicar.py`, ¿anonimiza o seudonimiza? (piensa en que `observadores.csv` sigue existiendo con el código de cada persona, y en que el fichero publicado conserva el nombre); qué principio del artículo 5 incumple publicar edad y localidad de ocho personas; qué columnas retirarías y cuál generalizarías; y qué es lo único que la red podría publicar sin riesgo (pista: no es una tabla de personas).

**Cómo trabajar.** Esta práctica se resuelve con el Reglamento abierto y el fichero cerrado: la decisión va antes que el código. El `KeyError` de la (a) es el más tonto y el más frecuente de la unidad: mayúsculas, tildes y espacios en el nombre de una columna; `columns.tolist()` antes de escribir ninguna.

### CP2.2 · Tres cargas del mismo fichero

**Nivel** básico · **Tiempo orientativo** 15 min.

**Qué refuerza.** El apartado 2 de la [UD2](ud2-preparacion-visualizacion-datos.md): lo que el dato es y lo que pandas cree que es, `dtypes`, `shape`, `describe()`, por qué `decimal=","` es un ajuste **global** del fichero, `parse_dates`, la conversión columna a columna con `.str.replace` y `astype`, y por qué `category` no toca hasta que los valores son los que deben ser.

**Ficheros de partida.** `cp2-2/tipos.py` y `cp2-2/datos/avistamientos_bruto.csv`, en el repositorio de recursos (enlace en Classroom). Se ejecuta desde `cp2-2/`.

**Enunciado.** El mismo fichero, cargado de tres maneras, produce tres tablas que no se parecen. Esta práctica no entrega una tabla bien tipada: entrega la **comparación** —qué arregla y qué rompe cada opción de carga— y lo que `describe()` deja ver en cada una sin mirar una sola fila.

**Tareas.**

- **(a) Predice y carga.** Antes de ejecutar, con la cabecera y las cinco filas de arriba delante, escribe qué `dtype` esperas para cada una de las nueve columnas en una carga sin opciones. Ejecuta `python tipos.py`: imprime los `dtypes` de las tres cargas y se para en la conversión. Compara con tu predicción. Salida esperada de las tres cargas:

```
── carga 1: sin opciones ──
id                 int64
fecha                str
punto_id             str
especie              str
ejemplares       float64
temperatura          str
duracion_h       float64
observador_id        str
notas                str
dtype: object
(199, 9)
── carga 2: con decimal=',' ──
id                 int64
fecha                str
punto_id             str
especie              str
ejemplares       float64
temperatura      float64
duracion_h           str
observador_id        str
notas                str
dtype: object
── carga 3: con parse_dates ──
id                        int64
fecha            datetime64[us]
punto_id                    str
especie                     str
ejemplares              float64
temperatura                 str
duracion_h              float64
observador_id               str
notas                       str
dtype: object
```

- **(b) Caza el error y convierte.** La última línea de la traza dice qué cadena no se puede convertir a número: la coma. Arregla la conversión columna a columna —primero `.str.replace(",", ".")`, después `astype(float)`— y comprueba con `describe()` que la temperatura ya es numérica. Última línea de la traza de partida y salida esperada:

```
ValueError: could not convert string to float: '26,9'
```

```
count    193.0
mean      15.5
std        7.8
min        4.1
25%       10.5
50%       14.7
75%       19.8
max       85.0
Name: temperatura, dtype: float64
```

- **(c) Tres `describe()`.** Imprime `describe().round(1)` de las tres cargas y rellena una tabla de tres filas: qué columnas numéricas aparecen en cada una y por qué (`duracion_h` desaparece en la carga 2: la coma como separador decimal convierte en texto una columna que usaba el punto; la temperatura solo aparece en la 2 y, tras convertirla, en la 3; la fecha solo en la 3). Y anota, sin mirar las filas, los tres valores sospechosos que las tablas enseñan: el mínimo de `ejemplares`, su máximo y el máximo de `temperatura`. Salida esperada:

```
── describe, carga 1 ──
          id  ejemplares  duracion_h
count  199.0       196.0       199.0
mean    97.1       235.4         1.7
std     55.7      2855.2         0.9
min      1.0        -1.0         0.5
25%     49.5         5.0         1.0
50%     97.0        16.0         1.5
75%    144.5        39.2         2.5
max    194.0     40000.0         3.0
── describe, carga 2 ──
          id  ejemplares  temperatura
count  199.0       196.0        193.0
mean    97.1       235.4         15.5
std     55.7      2855.2          7.8
min      1.0        -1.0          4.1
25%     49.5         5.0         10.5
50%     97.0        16.0         14.7
75%    144.5        39.2         19.8
max    194.0     40000.0         85.0
── describe, carga 3 (con la temperatura ya convertida) ──
          id                       fecha  ejemplares  temperatura  duracion_h
count  199.0                         199       196.0        193.0       199.0
mean    97.1  2025-10-18 17:22:00.603015       235.4         15.5         1.7
min      1.0         2025-09-01 00:00:00        -1.0          4.1         0.5
25%     49.5         2025-09-27 00:00:00         5.0         10.5         1.0
50%     97.0         2025-10-19 00:00:00        16.0         14.7         1.5
75%    144.5         2025-11-12 00:00:00        39.2         19.8         2.5
max    194.0         2025-11-30 00:00:00     40000.0         85.0         3.0
std     55.7                         NaN      2855.2          7.8         0.9
```

- **(d) `category` antes de tiempo.** Convierte `especie` a `category` en la carga 3 e imprime cuántas categorías salen. La red censa diez especies. Escribe en una línea por qué convertir ahora consolida la suciedad y en qué momento del apartado 4 toca hacerlo. Salida esperada:

```
categorías de especie: 15
```

**Cómo trabajar.** Haz la predicción de la (a) de verdad: la columna `ejemplares` sale `float64` y no `int64` por una razón que verás en el CP2.3 (hay celdas vacías). Y la lección de la (c) es la del apartado 2.2: una opción global arregla una columna y rompe otra; la conversión se hace columna a columna.

### CP2.3 · Mirar antes de tocar

**Nivel** intermedio · **Tiempo orientativo** 35 min (20 + 15 de la tarea desde cero).

**Qué refuerza.** El apartado 3 de la [UD2](ud2-preparacion-visualizacion-datos.md): la exploración sistemática —`value_counts`, `isna`, `nunique`—, el filtro booleano con `&`, `|`, `~` y cada condición entre paréntesis, `between` y `notna`, `sort_values`, y el accesor `.dt` con `isocalendar().week`.

**Ficheros de partida.** `cp2-3/explorar.py` (la carga correcta ya hecha) y `cp2-3/datos/avistamientos_bruto.csv`, en el repositorio de recursos (enlace en Classroom). Se ejecuta desde `cp2-3/`.

**Enunciado.** `describe()` ha anunciado tres valores que no pueden ser, y `value_counts` va a anunciar que las diez especies son quince. Esta práctica no redacta un parte de exploración ni una lista de problemas: responde a **sospechas concretas con filtros concretos**, y descubre por el camino que un filtro mal escrito puede no fallar.

**Tareas.**

- **(a) Caza el error.** Ejecuta `python explorar.py`. El filtro «grandes o fríos» usa `or` entre dos condiciones y pandas responde que «la verdad de una Series es ambigua». Escríbelo con `|` y **cada condición entre paréntesis**: 22 filas. Después quita solo el segundo paréntesis y vuelve a ejecutar: no hay error y salen 199 filas, todas. Explica en dos líneas qué comparó pandas en ese caso (la precedencia de `|` frente a `<`). Última línea de la traza de partida y salida esperada:

```
ValueError: The truth value of a Series is ambiguous. Use a.empty, a.bool(), a.item(), a.any() or a.all().
```

```
22
sin el segundo paréntesis: 199
```

- **(b) Cuántas formas tiene cada especie.** `value_counts()` de `especie` y, debajo, cuántas formas distintas hay tal cual (`nunique`) y cuántas quedan con una clave `str.strip().str.lower()`. Anota, especie por especie, qué variantes esconde cada una (mayúsculas, un espacio delante o detrás, una tilde que falta) y cuál de las tres **no** arregla la clave. Después `value_counts()` de `punto_id`: hay un detalle parecido. Salida esperada:

```
especie
Alondra común         34
Cigüeña blanca        26
Grulla común          25
Garza real            21
Aguilucho lagunero    18
Milano real           17
Ánade azulón          15
Ganga ortega          13
Avutarda común        12
Abejaruco europeo      7
Grulla comun           3
GRULLA COMÚN           3
avutarda común         2
 Milano real           2
Ánade azulón           1
Name: count, dtype: int64
formas distintas tal cual: 15 | claves distintas: 11
punto_id
PUN-04    45
PUN-06    37
PUN-03    33
PUN-02    32
PUN-01    29
PUN-05    19
pun-01     2
pun-06     2
Name: count, dtype: int64
```

- **(c) Las filas que `describe()` anunció.** Localiza con filtros booleanos e imprime (con `id`, fecha, punto, especie y la columna en cuestión) las filas con temperatura fuera de −10 a 45 grados (`between` negado con `~`, y `notna()` para no contar lo vacío), las de ejemplares negativos y las de ejemplares por encima de 5000. Salida esperada:

```
    id      fecha punto_id      especie  temperatura
58  59 2025-10-04   PUN-05  Milano real         85.0
      id      fecha punto_id             especie  ejemplares
41    42 2025-09-23   PUN-06  Aguilucho lagunero        -1.0
127  128 2025-11-07   PUN-03      Cigüeña blanca        -1.0
134  135 2025-11-09   PUN-05       Alondra común        -1.0
160  161 2025-11-19   PUN-06        Grulla común        -1.0
      id      fecha punto_id         especie  ejemplares
100  101 2025-10-21   PUN-03  Avutarda común     40000.0
```

- **(d) Dónde faltan las temperaturas.** Con `isna()` y el accesor `.dt.isocalendar().week`, cuenta en qué semanas del calendario caen las temperaturas vacías; y con `sort_values` localiza la fila más fría de la temporada. Salida esperada:

```
week
36    1
39    2
41    1
44    1
47    1
Name: count, dtype: Int64
      id      fecha punto_id       especie  temperatura
126  127 2025-11-05   PUN-04  Grulla común          4.1
```

- **(e) Desde cero.** Fichero vacío en `cp2-3/`. Escribe un programa que, para la especie que fija el propio programa en una constante («Grulla común»), imprima cuántos avistamientos encuentra comparando `especie == "Grulla común"` y cuántos comparando la clave `str.strip().str.lower()` con `"grulla común"`; después el listado de los segundos ordenado por fecha (fecha, punto, especie tal cual viene y ejemplares, como imprime pandas una tabla); y por último cuántos quedan fuera incluso así, escritos sin tilde. Salida esperada, letra a letra (sin las filas intermedias del listado, que son 28):

```
Con == : 25 avistamientos
Con strip y lower: 28 avistamientos
         fecha punto_id       especie  ejemplares
60  2025-10-05   PUN-04  Grulla común        21.0
68  2025-10-07   PUN-03  Grulla común        85.0
76  2025-10-10   PUN-02  Grulla común       133.0
…
190 2025-11-29   PUN-03  Grulla común       112.0
193 2025-11-30   PUN-06  Grulla común        98.0
Y aún quedan fuera 3 escritas sin tilde: la normalización no sabe de tildes
```

**Cómo trabajar.** El filtro sin paréntesis de la (a) es el error más caro de la unidad: no avisa. Cada vez que un filtro devuelva «todas» o «ninguna», sospecha de los paréntesis antes que de los datos. Y en la (e) fíjate en que la clave arregla las mayúsculas y los espacios pero no la tilde: por eso el apartado 4 usa un diccionario canónico además de la clave.

### CP2.4 · Técnicas sueltas, tablas pequeñas

**Nivel** avanzado · **Tiempo orientativo** 45 min (25 + 20 de la tarea desde cero).

**Qué refuerza.** El apartado 4 de la [UD2](ud2-preparacion-visualizacion-datos.md): `drop_duplicates` con y sin `subset` y `keep`, la normalización con clave y diccionario canónico (`map`), el centinela y los rangos declarados que pasan a ausente con `loc`, `interpolate` con `limit`, y la regla de que cada transformación se comprueba con un recuento.

**Ficheros de partida.** `cp2-4/preparar.py` y la carpeta `cp2-4/datos/` con `avistamientos_bruto.csv`, `parejas.csv` (seis filas: tres parejas de avistamientos repetidos) y `serie_hueco.csv` (ocho días de temperatura de un punto con huecos), en el repositorio de recursos (enlace en Classroom). Se ejecuta desde `cp2-4/`.

**Enunciado.** Nadie limpia la tabla entera en esta práctica: cada técnica se ensaya **suelta** sobre una tabla de seis u ocho filas en la que puedes predecir el resultado antes de ejecutar, y solo dos de ellas se aplican al bruto. No hay cruce, no hay columnas derivadas, no hay agregados y no se guarda ninguna tabla limpia.

**Tareas.**

- **(a) Caza el error.** Ejecuta `python preparar.py`. La asignación encadenada `bruto[...]["ejemplares"] = np.nan` imprime un aviso largo y, debajo, el mínimo **sigue siendo −1**: la tabla no ha cambiado. Reescríbela como una sola operación con `loc` y comprueba que el mínimo pasa a 1. Primera línea del aviso de partida (el aviso sigue con la explicación y un enlace a la documentación), la última línea de la partida y la salida esperada tras el arreglo:

```
preparar.py:11: ChainedAssignmentError: A value is being set on a copy of a DataFrame or Series through chained assignment.
…
mínimo de ejemplares después: -1.0
```

```
mínimo de ejemplares antes: -1.0
mínimo de ejemplares después: 1.0
```

- **(b) Tres formas de quitar duplicados.** Imprime `parejas.csv` entero y, **antes de ejecutar cada orden**, escribe qué índices sobreviven a `drop_duplicates()`, a `drop_duplicates(subset=["fecha", "punto_id", "especie"])` y a lo mismo con `keep="last"`. Ejecuta y compara. Escribe en dos líneas qué decisión te obliga a tomar `keep` que `drop_duplicates()` sin argumentos no te pide (dos observadores, el mismo día, el mismo punto, la misma especie, cifras distintas: ¿cuál es la buena?). Salida esperada:

```
        fecha punto_id       especie  ejemplares observador_id
0  2025-09-01   PUN-02  Ánade azulón         102        OBS-04
1  2025-09-01   PUN-02  Ánade azulón         102        OBS-04
2  2025-09-14   PUN-02  Ánade azulón          12        OBS-01
3  2025-09-14   PUN-02  Ánade azulón          19        OBS-02
4  2025-09-26   PUN-02  Ánade azulón          66        OBS-06
5  2025-09-26   PUN-02  Ánade azulón          63        OBS-07
drop_duplicates(): [0, 2, 3, 4, 5]
por clave, keep='first': [0, 2, 4]
por clave, keep='last':  [1, 3, 5]
```

- **(c) Interpolar con límite.** Sobre `serie_hueco.csv`, añade una columna con `temperatura.interpolate(limit=1)` e imprime la tabla: un hueco se rellena y otro queda a medias. Explica por qué (qué significa `limit=1` con dos ausentes seguidos) y, en dos líneas, por qué en el registro de avistamientos **no** procede interpolar `ejemplares`: no es una serie diaria por punto, y el vecino de una fila no es el día anterior. Salida esperada:

```
       fecha punto_id  temperatura  interpolada
0 2025-10-20   PUN-01         12.4        12.40
1 2025-10-21   PUN-01         11.8        11.80
2 2025-10-22   PUN-01          NaN        10.95
3 2025-10-23   PUN-01         10.1        10.10
4 2025-10-24   PUN-01          NaN         9.60
5 2025-10-25   PUN-01          NaN          NaN
6 2025-10-26   PUN-01          8.6         8.60
7 2025-10-27   PUN-01          8.9         8.90
huecos antes: 3 | después: 1
```

- **(d) Centinela y rangos, sobre el bruto.** Solo dos técnicas sobre el bruto: el centinela −1 ya pasó a ausente en la (a); ahora los **rangos declarados** por escrito —ejemplares de 1 a 5000, temperatura de −10 a 45— con `between` negado, `notna()` y `loc`, imprimiendo las filas que caen antes de anularlas, y `isna().sum()` antes y después. Salida esperada:

```
ausentes antes: {'ejemplares': 7, 'temperatura': 6}
      id      fecha punto_id         especie  ejemplares
100  101 2025-10-21   PUN-03  Avutarda común     40000.0
    id      fecha punto_id      especie  temperatura
58  59 2025-10-04   PUN-05  Milano real         85.0
ausentes después: {'ejemplares': 8, 'temperatura': 7}
mínimo y máximo de ejemplares: 1.0 308.0
```

- **(e) Desde cero.** Fichero vacío en `cp2-4/`. Escribe un programa que cargue el bruto y normalice **solo** la columna `especie`: una clave con `str.strip().str.lower()` y un diccionario canónico que lleve cada clave a su nombre oficial (incluida la grulla escrita sin tilde). Imprime cuántas formas distintas había antes, cuántas claves quedan sin correspondencia en el diccionario (tiene que ser 0: es la comprobación del `map`), cuántas formas quedan después y, para cada especie que tuviera más de una forma, la lista de sus variantes. Salida esperada, letra a letra:

```
formas distintas antes: 15
sin correspondencia en el diccionario: 0
formas distintas después: 10
Avutarda común: ['Avutarda común', 'avutarda común']
Grulla común: ['GRULLA COMÚN', 'Grulla comun', 'Grulla común']
Milano real: [' Milano real', 'Milano real']
Ánade azulón: ['Ánade azulón', 'Ánade azulón ']
```

**Cómo trabajar.** Predecir antes de ejecutar es toda la práctica: con seis filas no hay excusa. En la (d), los rangos se **declaran** (van escritos en el programa, con su unidad) y son generosos: anular un dato real es peor que conservar uno dudoso. Y el `isna().sum()` de antes y después es el recuento que va al registro de decisiones del CP2.7.
### CP2.5 · Gráficos que engañan

**Nivel** intermedio · **Tiempo orientativo** 35 min (20 + 15 de la tarea desde cero).

**Qué refuerza.** El apartado 5 de la [UD2](ud2-preparacion-visualizacion-datos.md): figura y ejes con `plt.subplots`, `ax.bar`, `ax.plot` y `ax.hist`, los rótulos (`set_title`, `set_xlabel`, `set_ylabel`), `resample("W")` para la serie semanal, `savefig` a `figuras/`, y las reglas de legibilidad y honestidad del 5.7: barras desde cero, orden que diga algo, unidades, un título que diga qué, dónde y cuándo, y `bins` que dejen ver la forma.

**Ficheros de partida.** `cp2-5/figuras.py` (tres figuras hechas deprisa), `cp2-5/describir_figura.py` (un **ayudante dado**: describe con datos lo que hay dibujado; no se modifica ni hace falta entender su interior) y la carpeta `cp2-5/datos/` con `avistamientos_limpio.csv` (el bruto tras todo el apartado 4) y `especies.json`, en el repositorio de recursos (enlace en Classroom). Se ejecuta desde `cp2-5/` y guarda las figuras en `figuras/`.

**Enunciado.** Alguien de la red dibujó tres figuras para la asamblea de otoño: unas barras, una serie y un histograma. Las tres «funcionan» y las tres engañan. No se comprueban mirando la imagen, sino con el ayudante: después de dibujar sobre unos ejes, `describir_barras(ax)`, `describir_lineas(ax)` o `describir_histograma(ax)` imprimen una línea con lo que de verdad hay dibujado —número de barras, alturas, orden, si el eje arranca en cero, si las fechas van en orden, el título y los rótulos—, y esa línea es la salida esperada. En esta práctica no se dibuja ninguna figura nueva de agregados: se **corrigen** tres.

**Tareas.**

- **(a) Caza el error.** Ejecuta `python figuras.py`. La figura 1 es un panel de dos ejes, y `plt.subplots(1, 2)` devuelve una **rejilla** de ejes, no un eje: se dibuja sobre `axes[0]` y `axes[1]`. Arregla las cuatro líneas que usan `axes` como si fuera un eje y vuelve a ejecutar: ya no falla, e imprime cinco líneas del ayudante. Última línea de la traza de partida y salida de partida tras el arreglo (es la salida de tres gráficos engañosos; léela con calma antes de seguir):

```
AttributeError: 'numpy.ndarray' object has no attribute 'bar'. Did you mean: 'var'?
```

```
10 barras · alturas [103.0, 49.0, 1048.0, 224.0, 279.0, 247.0, 59.0, 3105.0, 117.0, 876.0] · etiquetas ['Abejaruco europeo', 'Aguilucho lagunero', 'Alondra común', 'Avutarda común', 'Cigüeña blanca', 'Ganga ortega', 'Garza real', 'Grulla común', 'Milano real', 'Ánade azulón'] · de mayor a menor: False · eje y desde 0: False · título: 'Gráfico 1' · eje x: '' · eje y: ''
6 barras · alturas [951.0, 1239.0, 1028.0, 1622.0, 404.0, 863.0] · etiquetas ['PUN-01', 'PUN-02', 'PUN-03', 'PUN-04', 'PUN-05', 'PUN-06'] · de mayor a menor: False · eje y desde 0: True · título: 'Gráfico 2' · eje x: '' · eje y: ''
194 puntos · en orden cronológico: False · con marcador: False · valores [19.0, 14.0, nan, 8.0, 33.0, 16.0, 13.0, 4.0, 3.0, 4.0, 2.0, nan, 2.0, 4.0, 1.0, 4.0, 3.0, 2.0, 4.0, 4.0, 2.0, 1.0, 3.0, 4.0, 2.0, 40.0, 36.0, 55.0, 49.0, 22.0, 24.0, 15.0, 21.0, 6.0, 48.0, 38.0, nan, 36.0, 23.0, 31.0, 42.0, 6.0, nan, 54.0, 17.0, 42.0, 23.0, 6.0, 57.0, 53.0, 34.0, 43.0, 17.0, 53.0, 30.0, 49.0, 50.0, 14.0, 14.…
3 intervalos · recuentos [172, 12, 2] · título: 'Histograma' · eje x: '' · eje y: ''
```

- **(b) Las barras.** En las dos barras de la figura 1: el eje y del primero arranca en 300 (`set_ylim`), las especies van en orden alfabético, no hay unidades y los títulos no dicen nada. Corrige: ordena las dos series de mayor a menor (`sort_values(ascending=False)` antes de dibujar), quita el recorte del eje, pon como título «Ejemplares contados por especie, otoño de 2025» y «Ejemplares contados por punto, otoño de 2025», `Especie` y `Punto` en el eje x y `Ejemplares` en el eje y. Escribe en una línea qué conclusión falsa sacaba un lector del gráfico de partida (pista: compara la barra de las cigüeñas con la de las grullas en los dos). Salida esperada de las dos líneas del ayudante:

```
10 barras · alturas [3105.0, 1048.0, 876.0, 279.0, 247.0, 224.0, 117.0, 103.0, 59.0, 49.0] · etiquetas ['Grulla común', 'Alondra común', 'Ánade azulón', 'Cigüeña blanca', 'Ganga ortega', 'Avutarda común', 'Milano real', 'Abejaruco europeo', 'Garza real', 'Aguilucho lagunero'] · de mayor a menor: True · eje y desde 0: True · título: 'Ejemplares contados por especie, otoño de 2025' · eje x: 'Especie' · eje y: 'Ejemplares'
6 barras · alturas [1622.0, 1239.0, 1028.0, 951.0, 863.0, 404.0] · etiquetas ['PUN-04', 'PUN-02', 'PUN-03', 'PUN-01', 'PUN-06', 'PUN-05'] · de mayor a menor: True · eje y desde 0: True · título: 'Ejemplares contados por punto, otoño de 2025' · eje x: 'Punto' · eje y: 'Ejemplares'
```

- **(c) La serie.** La figura 2 dibuja los ejemplares de cada avistamiento en el orden del fichero ordenado **por especie**: las fechas van y vienen, y la línea es un garabato. La serie que debía ser es la de **ejemplares por semana**: `set_index("fecha")["ejemplares"].resample("W").sum()`, dibujada con marcador, título «Ejemplares contados por semana, otoño de 2025», `Semana` y `Ejemplares` en los ejes. El ayudante dice si las fechas van en orden cronológico y cuántos puntos hay (trece semanas, la última acaba el domingo 30 de noviembre). Salida esperada:

```
13 puntos · en orden cronológico: True · con marcador: True · valores [428.0, 176.0, 192.0, 293.0, 233.0, 596.0, 510.0, 613.0, 302.0, 831.0, 624.0, 515.0, 794.0] · título: 'Ejemplares contados por semana, otoño de 2025' · eje x: 'Semana' · eje y: 'Ejemplares'
```

- **(d) El histograma.** La figura 3 usa `bins=3`: tres intervalos para recuentos que van de 1 a 308. Cambia a `bins=20`, pon título «Reparto de los recuentos por avistamiento, otoño de 2025», `Ejemplares por avistamiento` y `Número de avistamientos` en los ejes, y escribe en dos líneas qué ocultaban tres intervalos y qué enseñan veinte (dónde está la masa de los avistamientos y qué son los dos de la derecha). Salida esperada:

```
20 intervalos · recuentos [91, 38, 17, 14, 4, 4, 4, 3, 5, 2, 0, 2, 0, 0, 0, 0, 0, 0, 1, 1] · título: 'Reparto de los recuentos por avistamiento, otoño de 2025' · eje x: 'Ejemplares por avistamiento' · eje y: 'Número de avistamientos'
```

- **(e) Desde cero.** Fichero vacío en `cp2-5/`. Escribe un programa que dibuje un panel de dos ejes con `plt.subplots(1, 2, sharey=True)`: a la izquierda los ejemplares por **grupo** y a la derecha por **estatus**, los dos de mayor a menor, desde cero, con título («Ejemplares por grupo, otoño de 2025» y «Ejemplares por estatus, otoño de 2025»), `Grupo` y `Estatus` en el eje x y `Ejemplares` en el eje y de los dos; guárdalo en `figuras/` e imprime la línea del ayudante de cada eje. El grupo y el estatus de cada especie están en `especies.json`: se traducen con un **diccionario y `map`** sobre la columna `especie` (no se cruzan tablas), comprobando con `isna().sum()` que ninguna especie se queda sin traducir. Salida esperada, letra a letra:

```
especies sin grupo: 0 | sin estatus: 0
5 barras · alturas [3443.0, 1151.0, 876.0, 471.0, 166.0] · etiquetas ['zancuda', 'pequeña', 'acuática', 'esteparia', 'rapaz'] · de mayor a menor: True · eje y desde 0: True · título: 'Ejemplares por grupo, otoño de 2025' · eje x: 'Grupo' · eje y: 'Ejemplares'
4 barras · alturas [3105.0, 2782.0, 117.0, 103.0] · etiquetas ['de paso', 'residente', 'invernante', 'estival'] · de mayor a menor: True · eje y desde 0: True · título: 'Ejemplares por estatus, otoño de 2025' · eje x: 'Estatus' · eje y: 'Ejemplares'
```

**Cómo trabajar.** Las tres figuras se dejan dibujadas y se miran después de corregirlas: la imagen es para el lector; la línea del ayudante, para ti. Un gráfico se entrega cuando cumple la lista del apartado 5.7, no antes; y «de mayor a menor» no es una opción estética: es la diferencia entre una figura que se lee en un segundo y una que hay que descifrar.

### CP2.6 · Seguir la pista a una fila

**Nivel** avanzado · **Tiempo orientativo** 45 min (25 + 20 de la tarea desde cero).

**Qué refuerza.** El apartado 6 de la [UD2](ud2-preparacion-visualizacion-datos.md): contrastar la calidad de lo obtenido con procedimientos que se puedan repetir —aquí, la **trazabilidad**: cada fila del limpio se puede seguir hasta el bruto y explicar—, las aserciones como afirmaciones sobre los datos que detienen el programa, y la coherencia entre una tabla y la figura que la dibuja.

**Ficheros de partida.** `cp2-6/calidad.py` y la carpeta `cp2-6/datos/` con `avistamientos_bruto.csv` y `avistamientos_limpio.csv`, en el repositorio de recursos (enlace en Classroom). Se ejecuta desde `cp2-6/`.

**Enunciado.** La tabla limpia tiene 194 filas y el bruto 199. ¿Qué ha pasado con cada una de las cinco que faltan, y con las que siguen ahí pero cambiaron? Cinco identificadores sirven de pista; después, una función lo hace sola para todos.

**Tareas.**

- **(a) Caza el error.** Ejecuta `python calidad.py`: se para al pedir `.dt` sobre la fecha del limpio. El bruto se cargó con `parse_dates`; el limpio, no, y **el CSV no guarda tipos**: al releerlo la fecha vuelve como texto. Arréglalo en la carga y comprueba las semanas de la temporada. Última línea de la traza de partida y salida esperada:

```
AttributeError: Can only use .dt accessor with datetimelike values. Did you mean: 'at'?
```

```
(199, 9) (194, 9)
36 48
```

- **(b) Cinco pistas.** Para cada `id` de la lista `PISTAS` (43, 82, 42, 101 y 127), imprime la fila en el bruto y en el limpio (filtro por `id`, con fecha, punto, especie, ejemplares y temperatura) y rellena en `respuestas.md` una tabla de cinco filas: está en el bruto (cuántas veces) · está en el limpio · qué cambió · qué paso del apartado 4 lo explica. Salida esperada:

```
── id 43 ──
     id      fecha punto_id     especie  ejemplares  temperatura
42   43 2025-09-23   PUN-06  Garza real         3.0         26.6
196  43 2025-09-23   PUN-06  Garza real         3.0         26.6
    id      fecha punto_id     especie  ejemplares  temperatura
42  43 2025-09-23   PUN-06  Garza real         3.0         26.6
── id 82 ──
    id      fecha punto_id       especie  ejemplares  temperatura
81  82 2025-10-12   PUN-03  Grulla comun        93.0         19.4
    id      fecha punto_id       especie  ejemplares  temperatura
81  82 2025-10-12   PUN-03  Grulla común        93.0         19.4
── id 42 ──
    id      fecha punto_id             especie  ejemplares  temperatura
41  42 2025-09-23   PUN-06  Aguilucho lagunero        -1.0         24.7
    id      fecha punto_id             especie  ejemplares  temperatura
41  42 2025-09-23   PUN-06  Aguilucho lagunero         NaN         24.7
── id 101 ──
      id      fecha punto_id         especie  ejemplares  temperatura
100  101 2025-10-21   PUN-03  Avutarda común     40000.0         18.8
      id      fecha punto_id         especie  ejemplares  temperatura
100  101 2025-10-21   PUN-03  Avutarda común         NaN         18.8
── id 127 ──
      id      fecha punto_id       especie  ejemplares  temperatura
126  127 2025-11-05   PUN-04  Grulla común       308.0          4.1
      id      fecha punto_id       especie  ejemplares  temperatura
```

- **(c) Aserciones de trazabilidad.** Escribe seis `assert`: uno por pista —la duplicada aparece dos veces en el bruto y una en el limpio; la variante está en su forma canónica; el centinela y el imposible son ausentes; la fila normal conserva sus ejemplares— y uno global: `len(bruto.drop_duplicates()) == len(limpio)`, que dice que solo cayeron las copias exactas. Si todos pasan, imprime una línea que lo diga. Salida esperada:

```
126  127 2025-11-05   PUN-04  Grulla común       308.0          4.1
```

- **(d) Coherencia con la figura.** Imprime `groupby("especie")["ejemplares"].sum()` ordenado de mayor a menor y compáralo con las alturas que imprimió el ayudante en la figura 1 del CP2.5: tienen que ser las mismas diez cifras en el mismo orden. Si no lo son, una de las dos cosas está mal. Salida esperada:

```
las seis aserciones pasan
especie
Grulla común          3105.0
Alondra común         1048.0
Ánade azulón           876.0
Cigüeña blanca         279.0
Ganga ortega           247.0
Avutarda común         224.0
Milano real            117.0
Abejaruco europeo      103.0
Garza real              59.0
Aguilucho lagunero      49.0
Name: ejemplares, dtype: float64
```

- **(e) Desde cero.** Fichero vacío en `cp2-6/`. Escribe una función que reciba el bruto y el limpio y devuelva, en un diccionario, cuatro listas de identificadores: los que tienen más de una copia en el bruto (`value_counts` sobre `id`), los que tenían recuento en el bruto y lo perdieron en el limpio, los que perdieron la temperatura, y los que cambiaron de nombre de especie (alinea las dos tablas por `id` con `set_index` después de quitar las copias del bruto). El programa imprime cada lista con su número y, al final, las filas de cada tabla y cuántos identificadores distintos hay en las dos. Salida esperada, letra a letra:

```
copias             5 → [43, 60, 67, 110, 121]
sin_recuento       5 → [42, 101, 128, 135, 161]
sin_temperatura    1 → [59]
renombrados       11 → [54, 65, 82, 87, 97, 131, 132, 134, 147, 168, 175]
filas: 199 → 194 | ids distintos en los dos: 194 194
```

**Cómo trabajar.** Una aserción es una frase sobre los datos que tú afirmas y el programa comprueba; si no sabes qué afirmar, no sabes qué has limpiado. Y la (e) es la (b) hecha para todas las filas a la vez: cuando la tabla de la semana que viene llegue con otra suciedad, la función te dirá qué ha cambiado sin que mires fila a fila.

### CP2.7 · El registro que no cuadra

**Nivel** avanzado · **Tiempo orientativo** 45 min (25 + 20 de la tarea desde cero).

**Qué refuerza.** El apartado 7 de la [UD2](ud2-preparacion-visualizacion-datos.md): el registro de decisiones como respuesta a «¿por qué esta cifra?» —cada línea con un número que sale de una orden—, los resultados exportados con `to_csv(index=False)` y releídos, y la reproducibilidad: lo que no se puede señalar con el dedo en una salida no va al registro.

**Ficheros de partida.** `cp2-7/exportar.py`, `cp2-7/DECISIONES-ajeno.md` (el registro de limpieza que escribió otro analista de la red) y la carpeta `cp2-7/datos/` con `avistamientos_limpio.csv` y `avistamientos_bruto.csv`, en el repositorio de recursos (enlace en Classroom). Se ejecuta desde `cp2-7/` y escribe en `resultados/`.

**Enunciado.** Te pasan un registro de decisiones ajeno con ocho líneas y un script que exporta las bandadas de la temporada. El script no falla y el registro parece completo. Tres de sus cifras no cuadran con la tabla, una de sus afirmaciones no se puede comprobar con ella, y el fichero exportado tiene una columna que nadie pidió.

**Tareas.**

- **(a) Caza la columna fantasma.** Ejecuta `python exportar.py`: escribe 36 filas con 4 columnas y relee 36 filas con **5**. Mira el nombre de la columna que sobra y de dónde ha salido (`to_csv` guarda el índice si no se le dice lo contrario). Arréglalo con `index=False` y comprueba que lo escrito y lo releído coinciden. Salida de partida y salida esperada tras el arreglo:

```
escritas: 36 filas, 4 columnas
releídas: 36 filas, 5 columnas
['Unnamed: 0', 'fecha', 'punto_id', 'especie', 'ejemplares']
   Unnamed: 0      fecha punto_id        especie  ejemplares
0           2 2025-09-01   PUN-02   Ánade azulón       102.0
1           9 2025-09-04   PUN-01  Alondra común        55.0
2          16 2025-09-07   PUN-05  Alondra común        57.0
```

```
escritas: 36 filas, 4 columnas
releídas: 36 filas, 4 columnas
['fecha', 'punto_id', 'especie', 'ejemplares']
       fecha punto_id        especie  ejemplares
0 2025-09-01   PUN-02   Ánade azulón       102.0
1 2025-09-04   PUN-01  Alondra común        55.0
2 2025-09-07   PUN-05  Alondra común        57.0
```

- **(b) Revisar el registro ajeno.** Para cada una de las siete cifras de `DECISIONES-ajeno.md`, escribe la orden de pandas que la comprueba (sobre el bruto o sobre el limpio, según diga la línea) e imprime el resultado; marca cada línea con cuadra o no cuadra, y corrige las que no cuadran en tu copia del registro, dejando la orden junto a cada cifra. Salida esperada de las siete comprobaciones:

```
1. filas bruto → limpio: 199 → 194 | duplicadas exactas: 5
2. formas de especie: 15 → 10
3. centinela -1: 4
4. fuera de rango: [101] [59]
5. ausentes en el limpio: {'ejemplares': 8, 'temperatura': 7}
6. punto en minúscula: 4 | puntos: 6
7. bandadas: 36
```

- **(c) Las bandadas, exportadas.** `resultados/bandadas.csv` lleva fecha, punto, especie y ejemplares de los avistamientos de 50 o más, ordenados por fecha, sin índice; relee el fichero y comprueba filas y columnas (ya lo hace el script arreglado de la (a)). Es el único resultado que se exporta en esta práctica.
- **(d) Lo que la tabla no puede decir.** La línea 8 del registro ajeno explica por qué faltan temperaturas. Escribe en dos líneas qué de esa afirmación **sí** se puede comprobar con la tabla (qué filas tienen la temperatura vacía) y qué **no** (la causa), de dónde tendría que venir esa información y qué se hace con una frase así en un registro.

- **(e) Desde cero.** Fichero vacío en `cp2-7/`. Escribe un programa que lea `resultados/bandadas.csv` y escriba `tabla_bandadas.md`: un título y una tabla markdown con una fila por bandada (fecha, punto, especie, ejemplares como entero), generada línea a línea con f-strings; al final imprime cuántas bandadas ha escrito. Contenido esperado (las cinco primeras líneas, las dos últimas y la pantalla):

```
# Bandadas del otoño de 2025

| Fecha | Punto | Especie | Ejemplares |
|---|---|---|---|
| 2025-09-01 | PUN-02 | Ánade azulón | 102 |
…
| 2025-11-29 | PUN-03 | Grulla común | 112 |
| 2025-11-30 | PUN-06 | Grulla común | 98 |
```

```
36 bandadas escritas en tabla_bandadas.md
```

**Cómo trabajar.** Revisar un registro ajeno es el ejercicio de la defensa al revés: en vez de explicar tus cifras, pides a cada cifra que se explique. Tres de ocho no cuadran y la cuarta no es comprobable: es una proporción normal en un registro escrito de memoria, y la razón de que el apartado 7 pida que cada línea salga de una salida del cuaderno.
### CP2.8 · Simulacro de la prueba: la primavera, de punta a punta

**Nivel** tipo prueba · **Tiempo orientativo** 100 min, en dos sesiones de 50: **Parte RA1** (la UD1) y **Parte RA2** (la UD2). **Para hacer antes de la prueba del 19/11, cuando lo indique el docente, una vez entregada la actividad de la UD2** (orientativamente, el jueves 12/11 la Parte RA1 y el viernes 13/11 la Parte RA2).

**Qué refuerza.** Las dos unidades, en la forma en que las encontrarás en la prueba de la 1.ª evaluación: preguntas cerradas que se contestan en un fichero de respuestas, un script que hay que hacer idiomático, funciones sin pandas sobre dos ficheros relacionados, el modelo sobre unos textos; y, en la segunda parte, la columna que no se carga, los tipos, una limpieza con efectos comprobables, dos gráficos honestos y una comprobación con aserciones. La prueba de verdad es otra —otros datos, otros nombres, otro dominio—, pero las **tareas son de estos tipos**. Se hace **sin apuntes, sin red y sin material**: solo el editor con su ayuda de funciones, el intérprete y la plantilla. Solo ficheros `.py`. En la Parte RA1 **no se usa pandas** (en la prueba, usarlo anula la tarea).

**Plantilla de partida.** La carpeta `cp2-8/` del repositorio de recursos (enlace en Classroom), copiada entera a tu carpeta de trabajo: `datos/` (la **primavera de 2026** de la red, con su propia suciedad: `avistamientos_primavera.csv`, `salidas.csv`, `puntos.json`, `notas_primavera.csv`), `parte-ra1/` y `parte-ra2/`. Cada programa se ejecuta **desde la carpeta de su parte**, con el entorno de la UD1 activo. No hay corrector: cada tarea abierta trae su salida esperada y te compruebas tú; las respuestas cerradas van en `respuestas_ra1.py` y `respuestas_ra2.py` (una letra por clave; el propio fichero te dice cuáles te faltan al ejecutarlo) y sus claves se publican con la solución, después de corregir.

**Reloj.** Apunta la hora al empezar cada parte. A los 50 minutos, para y mira qué te ha faltado: eso es lo que hay que repasar antes del 19/11.

#### Parte RA1 · Python, datos y modelo (50 min)

**Tarea 1 · Test (10 min).** Seis preguntas sobre el lenguaje, sus librerías y su ecosistema. Escribe la letra en `respuestas_ra1.py`, claves `t1` a `t6`.

- **t1.** En Python, `x = 5` y después `x = "cinco"`: (a) error, `x` ya es entero · (b) legal: la variable es una etiqueta, y son los valores los que tienen tipo · (c) legal: Python convierte el 5 a texto · (d) legal, pero `type(x)` sigue siendo `int`.
- **t2.** Un bloque de código se delimita con: (a) llaves · (b) `begin` y `end` · (c) la sangría bajo una línea que termina en dos puntos · (d) un punto y coma al final.
- **t3.** El entorno virtual sirve para: (a) acelerar el intérprete · (b) aislar las librerías de un proyecto y reproducirlas en otro equipo con `requirements.txt` · (c) compilar el programa · (d) subir las librerías al repositorio.
- **t4.** En un cuaderno, ejecutas la celda 3 antes que la 2 y sale `NameError`: (a) el cuaderno está roto · (b) las celdas comparten estado y el nombre todavía no existe · (c) hay que reinstalar el kernel · (d) es un error de sangría.
- **t5.** `csv.DictReader` devuelve cada fila como: (a) una lista de cadenas · (b) un diccionario con los números ya convertidos · (c) un diccionario cuyas claves son la cabecera y cuyos valores son cadenas · (d) una tupla.
- **t6.** Un modelo preentrenado: (a) devuelve siempre lo correcto · (b) lo entrenaron otros, se aplica tal cual y devuelve lo probable · (c) hay que entrenarlo antes de usarlo · (d) solo funciona con texto periodístico.

**Tarea 2 · A la manera de otro lenguaje (12 min).** `parte-ra1/arreglar.py` funciona, pero está escrito como quien piensa en otro lenguaje: bucles `while` con índice manual, punto y coma, comparaciones con `True` y `False`, concatenaciones con `str()`. Reescríbelo de forma idiomática —sin índice manual, sin punto y coma, sin comparar con `True` ni `False`, con f-strings y con las construcciones de la UD1 que correspondan— **manteniendo exactamente lo que imprime**. Después contesta `t7`: el rasgo del lenguaje que hace innecesario el índice manual es (a) el tipado dinámico · (b) que `for` recorre directamente los elementos de una colección · (c) las f-strings · (d) `import this`. Salida esperada, letra a letra (antes y después del arreglo):

```
Salidas: 175
Ejemplares en total: 5056
Bandadas: 34
Mayor recuento: 139 de Grulla común
Puntos: PUN-01, PUN-02, PUN-03, PUN-04, PUN-05, PUN-06
```

**Tarea 3 · Dos ficheros, sin pandas (15 min).** Escribe el cuerpo de las cuatro funciones de `parte-ra1/cruce.py` (sus nombres, parámetros y docstrings ya están): `cargar_salidas(ruta)` lee `salidas.csv` a una lista de diccionarios con `ejemplares` como entero; `cargar_puntos(ruta)` lee `puntos.json` y devuelve `{id: diccionario del punto}`; `ejemplares_por_habitat(salidas, puntos)` cruza cada salida con su punto por `punto_id` y agrupa en un diccionario `{hábitat: suma}`; `puntos_con_recuento_de(salidas, umbral)` devuelve el **conjunto** de puntos con algún avistamiento de `umbral` o más ejemplares, con una comprensión. `pruebas_ra1.py` las llama y **no se modifica**. Salida esperada de `python pruebas_ra1.py`, letra a letra:

```
1. 175 int Grulla común
2. 6 estepa Pradilla de Ebro
3. embalse  1065
3. estepa   1301
3. pinar    432
3. soto     2258
4. ['PUN-01', 'PUN-02', 'PUN-03', 'PUN-04', 'PUN-05', 'PUN-06']
4. ['PUN-01', 'PUN-02', 'PUN-03', 'PUN-04', 'PUN-06']
4. ['PUN-01', 'PUN-04']
```

**Tarea 4 · El modelo sobre las notas (13 min).** Completa `parte-ra1/lugares_citados.py`: cuenta con un `Counter` las entidades `LOC` de las quince notas de primavera (el texto de la entidad tal cual lo devuelve el modelo), imprime una frase con el número de notas, el de menciones y las **tres más citadas** con su número (a igual número, en el orden en que `most_common` las devuelve), y escribe `lugares_primavera.json` con las claves fijas `notas`, `menciones` y `mas_citados`. Después contesta `t8`: el modelo clasifica mal (a) «Subimos» en «Subimos hasta Luna a comer», como lugar, por abrir frase con mayúscula · (b) «Tauste» como persona · (c) «Pamplona» como organización · (d) nada: no se equivoca. Salida esperada en pantalla y contenido del JSON:

```
15 notas con 34 menciones de lugar; las más citadas: Tauste (4), Monte de Luna (3), Arba (3).
{
  "notas": 15,
  "menciones": 34,
  "mas_citados": [
    [
      "Tauste",
      4
    ],
    [
      "Monte de Luna",
      3
    ],
    [
      "Arba",
      3
    ]
  ]
}
```

#### Parte RA2 · Protección de datos, tipos, limpieza, gráficos y calidad (50 min)

**Tarea 1 · Datos personales (10 min).** `avistamientos_primavera.csv` trae una columna más que el registro de otoño. Contesta en `respuestas_ra2.py`: **p1** la columna de dato personal es (a) `observador_id` · (b) `correo_observador` · (c) `notas` · (d) `punto_id`; **p2** su titular es (a) la red · (b) el observador que hizo el censo · (c) el punto · (d) nadie, es un código; **p3** el principio del artículo 5 que obliga a no tratarla para contar avistamientos es (a) minimización · (b) exactitud · (c) limitación del plazo de conservación · (d) integridad y confidencialidad; **p4** la decisión correcta es (a) cargarla y borrarla al final · (b) sustituirla por un resumen criptográfico sin sal · (c) no cargarla (`usecols`) · (d) publicarla con el agregado. Después completa `parte-ra2/proteccion.py`: `cargar_publicable(ruta)` carga el fichero **sin esa columna** (escribe tú la lista de las que sí entran, y pide la fecha como fecha), y `asegurar_sin_dato_personal(tabla)` **lanza `AssertionError`** con un mensaje si esa columna está en la tabla. El bloque de pruebas del final no se cambia. Salida esperada:

```
['id', 'fecha', 'punto_id', 'especie', 'ejemplares', 'temperatura', 'duracion_h', 'observador_id', 'notas'] (180, 9)
la tabla de trabajo pasa la comprobación
la comprobación salta con el fichero completo: la tabla contiene la columna correo_observador
```

**Tarea 2 · Naturaleza y tipo (7 min).** Solo cerradas, en `respuestas_ra2.py`. Para cada columna, su naturaleza (apartado 2.1) y lo que pandas hará con ella en una carga sin opciones: **p5** `fecha`: (a) cuantitativo continuo · (b) temporal, que llega como texto si no se pide `parse_dates` · (c) identificador · (d) nominal; **p6** `punto_id`: (a) identificador, una etiqueta con la que no se hace aritmética · (b) ordinal · (c) cuantitativo discreto · (d) booleano; **p7** `ejemplares`: (a) continuo · (b) discreto, un recuento, que pandas carga como `float64` porque hay celdas vacías · (c) nominal · (d) texto; **p8** `temperatura`: (a) continuo, que llega como texto por la coma decimal · (b) discreto · (c) ordinal · (d) booleano; **p9** `duracion_h`: (a) nominal · (b) identificador · (c) cuantitativo continuo, `float64` · (d) temporal. Y dos sobre herramientas: **p10** para limpiar el bruto de forma reproducible, (a) una hoja de cálculo · (b) pandas en un cuaderno o un script · (c) SQL · (d) Matplotlib; **p11** no se limpia en una hoja de cálculo porque (a) no abre CSV · (b) no es reproducible, no deja traza de lo que se tocó y convierte tipos por su cuenta · (c) es más lenta · (d) no tiene gráficos.

**Tarea 3 · Limpieza con efectos comprobables (13 min).** En `parte-ra2/preparacion.py`, primero los **recuentos de la suciedad** del bruto (duplicadas exactas, centinela −1, ausentes de ejemplares y de temperatura, formas de especie frente a canónicas, temperaturas con coma), y después la función `preparar(tabla)`, que devuelve la tabla limpia sin modificar la recibida: sin duplicadas exactas, especie canónica con la clave y el diccionario `CANONICO`, punto en mayúsculas, temperatura numérica, centinela y fuera de los `RANGOS` declarados a ausente. Al final, los **cinco efectos comprobables** sobre la tabla limpia: filas, especies distintas (y ninguna sin correspondencia), mínimo y máximo de ejemplares, `dtype` y máximo de temperatura, ausentes por columna, y los seis puntos. Salida esperada de `python preparacion.py`, letra a letra:

```
filas: 180 | duplicadas exactas: 5
centinela -1: 4
ausentes: {'ejemplares': 3, 'temperatura': 6}
formas de especie: 15 | canónicas: 10
temperaturas con coma: 174
filas limpias: 175
especies distintas: 10 | sin correspondencia: 0
mínimo y máximo de ejemplares: 1.0 139.0
dtype de temperatura: float64 | máximo: 23.9
ausentes: {'ejemplares': 8, 'temperatura': 7}
puntos: ['PUN-01', 'PUN-02', 'PUN-03', 'PUN-04', 'PUN-05', 'PUN-06']
```

**Tarea 4 · Dos gráficos honestos (10 min).** Cerradas: **p12** para «¿cómo se comparan los seis puntos por ejemplares contados?» (a) serie temporal · (b) barras · (c) histograma · (d) dispersión; **p13** para «¿están relacionadas la temperatura y los ejemplares de cada avistamiento?» (a) serie temporal · (b) barras · (c) histograma · (d) dispersión; **p14** un gráfico de barras de ejemplares por especie con el eje y que empieza en 500 y sin unidades tiene como defecto (a) que faltan colores · (b) que el eje recortado exagera las diferencias y faltan las unidades · (c) demasiadas barras · (d) ninguno. Abiertas, en `parte-ra2/graficos.py` (que importa tu `preparar`): las barras de ejemplares por especie de mayor a menor, desde cero, con título «Ejemplares contados por especie, primavera de 2026» y rótulos `Especie` y `Ejemplares`; y la serie de ejemplares por semana (`resample("W").sum()`) en orden cronológico y con marcador, título «Ejemplares contados por semana, primavera de 2026» y rótulos `Semana` y `Ejemplares`. Las dos se comprueban con el ayudante `describir_figura.py` (el mismo del CP2.5). Salida esperada:

```
10 barras · alturas [1414.0, 1102.0, 1063.0, 577.0, 261.0, 185.0, 165.0, 67.0, 64.0, 36.0] · etiquetas ['Ánade azulón', 'Grulla común', 'Alondra común', 'Abejaruco europeo', 'Ganga ortega', 'Avutarda común', 'Cigüeña blanca', 'Garza real', 'Milano real', 'Aguilucho lagunero'] · de mayor a menor: True · eje y desde 0: True · título: 'Ejemplares contados por especie, primavera de 2026' · eje x: 'Especie' · eje y: 'Ejemplares'
14 puntos · en orden cronológico: True · con marcador: True · valores [211.0, 390.0, 352.0, 527.0, 146.0, 169.0, 399.0, 410.0, 533.0, 213.0, 600.0, 364.0, 235.0, 385.0] · título: 'Ejemplares contados por semana, primavera de 2026' · eje x: 'Semana' · eje y: 'Ejemplares'
```

**Tarea 5 · Calidad (10 min).** Cerrada: **p15** la suma de `ejemplares` del bruto tal cual da 45 103 y la de la tabla limpia 4934; la causa es (a) que pandas redondea · (b) que el bruto suma el 40 000 imposible, los −1 y las copias · (c) que la tabla limpia perdió filas al cruzar · (d) la coma decimal. Abierta, en `parte-ra2/calidad.py`: el **agregado correcto** —ejemplares por punto, de mayor a menor, sobre la tabla limpia— y la función `revisar_tabla(tabla)` con aserciones que **pasa** con la tabla limpia y **salta** con tres defectos inyectados cada uno en una tabla nueva (llama a `preparar` otra vez): una fila con la clave fecha-punto-especie de otra, un −1 en ejemplares y una fecha fuera de la temporada. El programa imprime «pasa con la tabla limpia» y los tres mensajes. Rellena después los huecos de `DECISIONES.md` con las cifras de la tarea 3. Salida esperada:

```
punto_id
PUN-01    1386.0
PUN-02     998.0
PUN-06     863.0
PUN-04     815.0
PUN-03     440.0
PUN-05     432.0
Name: ejemplares, dtype: float64
total de ejemplares en la tabla limpia: 4934.0
total en el bruto, tal cual (con duplicadas y centinelas): 45103.0
pasa con la tabla limpia
salta con fila con clave duplicada: hay claves fecha-punto-especie repetidas
salta con un -1 en ejemplares: hay ejemplares fuera de 1-5000
salta con una fecha fuera de la temporada: hay fechas fuera de la temporada
```

#### Al terminar

Ejecuta `python respuestas_ra1.py` y `python respuestas_ra2.py`: tienen que decir `ninguna` en la línea de sin contestar. Después compara tus salidas con las esperadas tarea a tarea y anota qué te ha faltado y por qué: lo que falte en la Parte RA1 se repasa en la UD1; lo de la Parte RA2, en la UD2. Las claves de las cerradas se publican en Classroom con la solución.
