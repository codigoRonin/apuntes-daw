# UD1. Python para datos e IA: lenguaje especializado, entorno y librerías

**Módulo optativo AOP1089 — Big Data e Inteligencia Artificial · 2.º DAW · IES Río Arba · Curso 2026-27 · 14 horas · 1.ª evaluación**

**Vinculación con el resultado de aprendizaje.** Esta unidad trabaja completo el **RA1**: *"Programa aplicaciones informáticas, utilizando lenguajes de programación especializados"* (Resolución de 10 de julio de 2025, bloque IFC303). Los criterios d), e) y f) se evalúan aquí **con alcance acotado** (ver más abajo) y se reutilizan en el resto de unidades sin volver a evaluarse.

| CE | Qué exige (resumen) | Apartado donde se trabaja |
|---|---|---|
| RA1.a | Instalar el lenguaje de programación especializado atendiendo a sus especificaciones técnicas | Apartado 1 |
| RA1.b | Identificar las características propias del lenguaje especializado | Apartado 2 |
| RA1.c | Escribir programas simples | Apartados 3, 4 y 5 |
| RA1.d | Obtener Big Data desde distintos orígenes de datos | Apartado 6 |
| RA1.e | Utilizar librerías especializadas en Big Data y/o modelos de inteligencia artificial | Apartado 7 |
| RA1.f | Programar aplicaciones en las que se usan Big Data y/o modelos de inteligencia artificial | Apartado 8 |

**Alcance acotado de RA1.d, RA1.e y RA1.f en esta unidad** (definición publicada en la programación, aplicable también a la actividad evaluativa): d) obtener datos desde orígenes **provistos por el docente o públicos y documentados** — ficheros CSV/JSON dados, una consulta simple a una API pública o a la base de datos de referencia del ciclo —, sin diseñar procesos de captura propios; e) usar librerías especializadas con **modelos preentrenados aplicados tal cual** a datasets dados, sin entrenamiento ni ajuste; f) programas breves que integren la obtención del dato y la llamada al modelo preentrenado **con salida interpretada**. Entrenar, ajustar y validar modelos propios (RA3), configurar plataformas (RA4) y construir aplicaciones completas (RA5) queda fuera de esta unidad y se evalúa en las suyas.

**Al terminar esta unidad sabrás:** instalar Python y montar un entorno de trabajo aislado con sus librerías; explicar qué hace distinto a Python de Java y por qué es el lenguaje del oficio de datos; escribir programas cortos con variables, decisiones, bucles, funciones, listas, diccionarios y clases, comentados y documentados; obtener los mismos datos desde un fichero CSV, un JSON, una API pública y una base de datos; aplicar un modelo preentrenado a un texto y leer con criterio lo que devuelve — incluidos sus errores —; y unir las dos cosas en un programa que lee datos, pregunta al modelo y explica el resultado.

**Entorno de trabajo de la unidad.** Necesitas **Python** instalado (apartado 1), un terminal y un editor; en el aula ya está todo. Trabajarás en dos formatos: **scripts** (`.py`, que se ejecutan de principio a fin) y **cuadernos** de Jupyter (`.ipynb`, celdas que se ejecutan una a una) — los dos se entregan en tu repositorio. Las librerías que aparecen son **spaCy**, con su modelo preentrenado en español, y **requests** para hablar con una API; la biblioteca estándar de Python cubre el resto (`csv`, `json`, `sqlite3`). Distinción que conviene tener clara desde la portada: **spaCy aparece aquí como librería instrumental** con un modelo preentrenado que se aplica tal cual; lo que se evalúa en esta unidad es **RA1.e y RA1.f** — usar una librería especializada y programar con ella —, no el procesamiento de lenguaje natural como tal, que tiene su propia unidad y sus propios criterios (RA5, UD5). Tampoco verás todavía pandas: llega con los datos en la UD2. El caso hilo de la teoría es una **red de alojamientos de turismo rural de la comarca** y las reseñas de sus huéspedes — nombres de negocio inventados, lugares reales, datos ficticios de aula.

<div style="page-break-before: always;"></div>

## Apartado 1. Instalar el lenguaje especializado y montar el taller

¿Por qué Python? Porque es el lenguaje en el que están escritas — o desde el que se manejan — casi todas las librerías con las que trabaja el oficio de datos e inteligencia artificial: preparar tablas, dibujar gráficos, entrenar modelos, llamar a un modelo de lenguaje. Un desarrollador web que sepa Python puede sentarse en la misma mesa que el equipo de datos y, sobre todo, puede incorporar sus resultados a una aplicación. En este módulo Python no es un lenguaje más: es **el lenguaje especializado** que fija el RA1.

**Instalación.** Descarga el instalador de la web oficial (apartado 12) y, en Windows, marca la casilla que añade Python al `PATH` — sin ella el terminal no lo encontrará. En Linux suele venir instalado y en macOS también, aunque en versión antigua; en los dos casos comprueba qué tienes antes de instalar nada. La comprobación es siempre la misma:

```
python --version
```

En algunos sistemas el comando se llama `python3` (y en Windows existe además el lanzador `py`). Si la respuesta empieza por `Python 3.`, estás listo; cualquier cosa que empiece por `Python 2.` no sirve para este módulo. Trabajamos con la **versión soportada actual**: los materiales no fijan números.

**Entornos virtuales.** Cada proyecto de datos arrastra sus propias librerías, y dos proyectos pueden exigir versiones incompatibles de la misma. La solución del ecosistema es el **entorno virtual**: una carpeta con su propia copia de Python y sus propias librerías, aislada del sistema. Se crea una vez por proyecto y se **activa** cada vez que abres un terminal para trabajar:

```
python -m venv .venv            # crea el entorno en la carpeta .venv
source .venv/bin/activate       # activa en Linux y macOS
.venv\Scripts\activate          # activa en Windows
```

Sabrás que está activo porque el terminal muestra `(.venv)` al principio de la línea. A partir de ahí, `pip` — el instalador de paquetes — instala **dentro** del entorno:

```
pip install -U pip
pip install jupyterlab requests spacy
python -m spacy download es_core_news_sm
python -m spacy validate
```

Las tres primeras órdenes traen el entorno de cuadernos, la librería HTTP y la librería de lenguaje natural; la cuarta descarga el **modelo preentrenado en español** que usaremos en los apartados 7 y 8 (pesa unos 13 MB); la última comprueba que modelo y librería son compatibles. La lista de lo instalado se guarda en un fichero para que cualquiera reproduzca tu entorno:

```
pip freeze > requirements.txt   # guarda la lista exacta
pip install -r requirements.txt # la reinstala en otra máquina
```

**El cuaderno.** Un **cuaderno** de Jupyter es un documento formado por **celdas**: unas contienen texto explicativo y otras código que se ejecuta en un proceso de Python que sigue vivo entre celda y celda (el **kernel**). Es el formato natural para explorar datos — escribes, ejecutas, miras, corriges — y por eso lo verás en todas las unidades. Se arranca desde el terminal con el entorno activo:

```
jupyter lab
```

Se abre en el navegador. Dos avisos que ahorran horas: **el orden de ejecución importa** — las celdas guardan estado, y ejecutar la celda 5 antes que la 3 produce resultados que no se corresponden con lo que se lee de arriba abajo —, así que antes de entregar, reinicia el kernel y ejecuta todo desde el principio; y el cuaderno elige un kernel: si instalaste spaCy en `.venv` pero el cuaderno usa el Python del sistema, no lo encontrará.

**Scripts.** Un fichero `.py` se ejecuta entero, de arriba abajo, con `python nombre.py`. Es el formato de todo lo que tenga que funcionar sin nadie delante: el programa del apartado 8, o las piezas que más adelante conectarás a una aplicación web. Cuaderno para explorar, script para entregar algo que se ejecuta solo: usarás los dos.

**El repositorio de la unidad.** Tu carpeta `ud1/` del aula de código lleva los scripts y cuadernos, los datos de la unidad, `requirements.txt` y un `.gitignore` que excluye `.venv/` — el entorno no se sube nunca: se reconstruye desde `requirements.txt`. Y el `DECISIONES.md`, desde el primer commit.

**Ejercicios del apartado.**

- **E1.** Instala Python (o comprueba el que tienes), crea el entorno virtual `.venv` de tu carpeta `ud1/`, actívalo e instala las librerías de la unidad con el modelo en español. Captura la salida de `python --version` y de `python -m spacy validate` y guárdalas en `ud1/evidencias/`. Explica en dos líneas qué demuestra cada captura.
- **E2.** Explica a un compañero que "instala todo con `pip` fuera del entorno virtual porque así siempre lo tiene" qué problema tendrá cuando en la UD3 y en la UD4 necesite dos versiones distintas de una misma librería, y qué le aporta `requirements.txt` cuando cambie de ordenador.
- **E3.** En un cuaderno crea tres celdas: en la primera `x = 10`, en la segunda `x = x + 5`, en la tercera `print(x)`. Ejecuta la segunda tres veces seguidas y después la tercera. Anota qué imprime y por qué no es 15; describe qué harías antes de entregar el cuaderno para que cualquiera obtenga lo que se lee.

<div style="page-break-before: always;"></div>

## Apartado 2. Lo que hace distinto a Python

Vienes de Java. Buena noticia: todo lo que sabes de programar — variables, decisiones, bucles, funciones, objetos — sigue valiendo. Lo que cambia es la forma, y conviene entender por qué cambia, porque de ahí sale el estilo con el que se escribe Python.

1. **Es interpretado y se escribe y prueba al momento.** No hay compilación previa: el intérprete lee y ejecuta. Puedes abrir un terminal, escribir `python` y probar una línea; puedes ejecutar una celda de un cuaderno y ver el resultado. Esa inmediatez es la razón de que el oficio de datos viva en cuadernos.
2. **Tipado dinámico.** Una variable es una **etiqueta** pegada a un valor, no una caja de un tipo fijo: `x = 5` y después `x = "cinco"` es legal. Los valores sí tienen tipo (`type(x)` lo dice) y el tipo importa — `"5" + "5"` es `"55"` mientras que `5 + 5` es `10` —, pero no lo declaras. Ganas velocidad de escritura; pierdes al compilador que te avisaba: ahora los errores de tipo aparecen al ejecutar.
3. **La sangría es la sintaxis.** No hay llaves ni punto y coma: un bloque es lo que está sangrado bajo la línea que termina en dos puntos. El código que se lee bien es, obligatoriamente, código correcto, y al revés.
4. **Todo es un objeto**, y todo tiene métodos: `"luna".upper()`, `[3, 1, 2].sort()`, `(2.5).is_integer()`. No hay tipos primitivos aparte.
5. **Pilas incluidas.** La **biblioteca estándar** trae de serie ficheros CSV y JSON, fechas, expresiones regulares, una base de datos (`sqlite3`), red… Y lo que no está, está en **PyPI**, el repositorio de paquetes que `pip` consulta: ahí viven las librerías del oficio.
6. **Legibilidad como valor.** Hay un estilo de código oficial (PEP 8) y una filosofía escrita (`import this` la muestra) que prefiere lo explícito, lo simple y lo legible. Nombres en `snake_case`, cuatro espacios de sangría, líneas cortas.
7. **Python es lento; sus librerías, no.** El intérprete es más lento que Java, y no importa: las librerías de datos están escritas en C y hacen el trabajo pesado. El estilo profesional es *dejar que la librería haga el bucle*: lo verás en la UD2 con pandas.

Con un fragmento delante se entiende mejor. El mismo comportamiento en los dos lenguajes:

```java
// Java
int total = 0;
for (int p : puntuaciones) {
    if (p >= 4) {
        total++;
    }
}
System.out.println("Buenas: " + total);
```

```python
# Python
total = 0
for p in puntuaciones:
    if p >= 4:
        total += 1
print(f"Buenas: {total}")
```

Y algunas líneas para probar en el intérprete, con lo que devuelven:

```python
x = 5
print(type(x))                       # <class 'int'>
x = "cinco"
print(type(x))                       # <class 'str'>
print(3 / 2, 7 // 2, 7 % 2, 2 ** 10) # 1.5 3 1 1024  (división real, entera, resto, potencia)
print("5" + "5", 5 + 5)              # 55 10
nombre = "Luna"
print(f"{nombre!r} tiene {len(nombre)} letras; en mayúsculas {nombre.upper()}")
                                     # 'Luna' tiene 4 letras; en mayúsculas LUNA
print(bool(""), bool("hola"), bool(0), bool([]), bool([0]))
                                     # False True False False True
```

La última línea enseña algo que usarás sin parar: en una condición, lo **vacío** (cadena vacía, cero, lista vacía, `None`) cuenta como falso y lo demás como verdadero. `if lista:` significa "si la lista tiene algo".

**Ejercicios del apartado.**

- **E4.** Toma el fragmento Java de arriba y señala, línea a línea, qué desaparece en la versión Python y qué lo sustituye (tipos, llaves, punto y coma, `++`, la impresión). Añade una diferencia que **no** se vea en el fragmento.
- **E5.** Predice, sin ejecutar, qué imprime cada línea y después compruébalo: `print(10 / 5)` · `print("3" * 3)` · `print(len("Sádaba"))` · `print(bool("False"))` · `print(7 // 2 * 2 + 7 % 2)`. Explica la sorpresa de la cuarta.
- **E6.** Un compañero afirma: "Como Python no declara tipos, no tiene tipos". Escribe tres líneas de código que demuestren que la afirmación es falsa y una que muestre el error real que aparece cuando se mezclan mal.

<div style="page-break-before: always;"></div>

## Apartado 3. Programas simples: datos, variables, constantes y control

Un programa Python es una secuencia de sentencias que se ejecutan de arriba abajo. Los tipos básicos son `int`, `float`, `str`, `bool` y el valor especial `None` ("no hay valor"). Las cadenas van entre comillas simples o dobles, indistintamente; las **f-strings** (`f"..."`) insertan expresiones entre llaves y son la forma habitual de formatear: `{valor:.2f}` con dos decimales, `{nombre:28}` en un campo de 28 caracteres. Las **constantes** no existen como tales: por convención, un nombre en `MAYÚSCULAS` significa "no lo cambies", y todo el mundo lo respeta.

Las **estructuras de control** son las que conoces: `if` / `elif` / `else`, `while` y `for`. El `for` de Python recorre **directamente los elementos** de cualquier colección — no necesita índice — y `range(n)` genera los enteros de `0` a `n-1` cuando lo que hace falta es contar. `break` sale del bucle, `continue` salta a la siguiente vuelta. Todo ello, en un primer programa completo sobre el caso hilo:

```python
# resumen_resena.py — primer programa: una reseña y una decisión
NOTA_MINIMA_RECOMENDABLE = 4          # constante: por convención, en mayúsculas

resena = {
    "alojamiento": "Hostal Sierra de Luna",
    "puntuacion": 5,
    "texto": "Trato familiar y desayuno casero. Desde Luna se llega rápido a Zaragoza y a Huesca.",
}

palabras = resena["texto"].split()     # una lista de palabras
longitud = len(palabras)

if resena["puntuacion"] >= NOTA_MINIMA_RECOMENDABLE:
    veredicto = "recomendable"
elif resena["puntuacion"] == 3:
    veredicto = "neutra"
else:
    veredicto = "negativa"

print(f"{resena['alojamiento']}: {resena['puntuacion']}/5 → reseña {veredicto}")
print(f"El texto tiene {longitud} palabras.")

# un bucle for recorre directamente los elementos, sin índices
mayusculas = 0
for palabra in palabras:
    if palabra[0].isupper():
        mayusculas += 1
print(f"Palabras que empiezan por mayúscula: {mayusculas}")

# while: se repite mientras la condición sea cierta
puntuaciones = [5, 4, 2, 5, 3, 4, 5, 1]
i = 0
suma = 0
while i < len(puntuaciones):
    suma += puntuaciones[i]
    i += 1
print(f"Media de {len(puntuaciones)} puntuaciones: {suma / len(puntuaciones):.2f}")
```

Salida:

```
Hostal Sierra de Luna: 5/5 → reseña recomendable
El texto tiene 15 palabras.
Palabras que empiezan por mayúscula: 5
Media de 8 puntuaciones: 3.62
```

Tres cosas que mirar en el código. La reseña es un **diccionario** (`{clave: valor}`), la estructura con la que llegarán los datos de cualquier origen en el apartado 6; se accede con `resena["texto"]`. `split()` convierte una cadena en lista de palabras: el primer paso de cualquier tratamiento de texto. Y el bucle `while` de la media está escrito así **para que veas el índice**, pero en Python nadie lo escribiría: `sum(puntuaciones) / len(puntuaciones)` es la forma natural, y en el apartado 4 hay más atajos de ese estilo.

La **entrada** por teclado es `input("mensaje")`, que siempre devuelve una cadena: `int(input("Puntuación: "))` la convierte. Las conversiones explícitas — `int()`, `float()`, `str()` — te acompañarán en el apartado 6, porque de un fichero CSV todo llega como texto.

**Ejercicios del apartado.**

- **E7.** Reescribe el cálculo de la media con `for` (sin índice) y después en una sola línea con `sum` y `len`. Añade un `if` que imprima "Sin datos" si la lista está vacía, en vez de dividir por cero.
- **E8.** Escribe un programa que recorra la lista `puntuaciones` del ejemplo e imprima, para cada valor, `positiva`, `neutra` o `negativa` (4-5, 3, 1-2), y al final cuántas hay de cada clase. Hazlo primero con tres contadores y después con un diccionario de contadores.
- **E9.** Escribe un programa que pida puntuaciones por teclado hasta que el usuario escriba `fin`, ignore las que no estén entre 1 y 5 (avisando), y al terminar muestre cuántas válidas ha recibido y su media con dos decimales.

<div style="page-break-before: always;"></div>

## Apartado 4. Funciones y estructuras de almacenamiento

Las **funciones** se definen con `def`, devuelven con `return` (si no hay `return`, devuelven `None`), admiten **parámetros con valor por defecto** y llamadas **con nombre** (`mejores(resenas, minimo=5)`). La cadena que abre el cuerpo de una función es su **docstring**: la documentación oficial de la función, que `help()` y el editor muestran (apartado 5).

Las **estructuras de almacenamiento** de la biblioteca estándar son cuatro, y elegir bien entre ellas es media programación:

| Estructura | Sintaxis | Cuándo |
|---|---|---|
| Lista `list` | `[5, 4, 2]` | Secuencia ordenada y modificable; es la estructura por defecto |
| Tupla `tuple` | `(42.09, -1.06)` | Secuencia que no debe cambiar: coordenadas, una fila de resultado |
| Diccionario `dict` | `{"nombre": "Luna", "plazas": 12}` | Asociar claves a valores; **la forma natural de un registro de datos** |
| Conjunto `set` | `{"Luna", "Sádaba"}` | Elementos sin repetir; comprobar pertenencia; sin orden garantizado |

Las **comprensiones** (`[r["puntuacion"] for r in resenas]`, `{... for ...}`) construyen una lista, un conjunto o un diccionario en una línea a partir de otra colección; son el equivalente compacto de un bucle que acumula. El **troceado** (`lista[:2]`, `lista[-1]`, `lista[::-1]`) recorta secuencias. `sorted(..., key=...)` ordena por el criterio que le des; `enumerate` y `zip` acompañan a los bucles cuando hace falta la posición o recorrer dos colecciones a la vez. Todo ello sobre las reseñas:

```python
# estadisticas.py — funciones y estructuras de almacenamiento
resenas = [
    {"alojamiento": "Casa Rural El Cierzo", "puntuacion": 5, "localidad": "Uncastillo"},
    {"alojamiento": "Casa Rural El Cierzo", "puntuacion": 2, "localidad": "Uncastillo"},
    {"alojamiento": "Apartamentos Las Bardenas", "puntuacion": 4, "localidad": "Sádaba"},
    {"alojamiento": "Hostal Sierra de Luna", "puntuacion": 5, "localidad": "Luna"},
    {"alojamiento": "Hostal Sierra de Luna", "puntuacion": 1, "localidad": "Luna"},
    {"alojamiento": "Apartamentos Las Bardenas", "puntuacion": 3, "localidad": "Sádaba"},
]

def media(valores):
    """Devuelve la media de una lista de números, o None si está vacía."""
    if not valores:            # lista vacía → False
        return None
    return sum(valores) / len(valores)

def media_por_alojamiento(resenas):
    """Agrupa las puntuaciones por alojamiento y devuelve {alojamiento: media}."""
    puntuaciones = {}                              # diccionario: clave → lista
    for r in resenas:
        puntuaciones.setdefault(r["alojamiento"], []).append(r["puntuacion"])
    return {nombre: media(lista) for nombre, lista in puntuaciones.items()}

def mejores(resenas, minimo=4):
    """Nombres (sin repetir) de los alojamientos con alguna reseña >= minimo."""
    return {r["alojamiento"] for r in resenas if r["puntuacion"] >= minimo}   # un conjunto

medias = media_por_alojamiento(resenas)
for nombre, valor in sorted(medias.items(), key=lambda par: par[1], reverse=True):
    print(f"{nombre:28} {valor:.2f}")

print("Localidades:", sorted({r["localidad"] for r in resenas}))
print("Con alguna reseña de 4 o más:", mejores(resenas))
print("Con alguna reseña de 5:", mejores(resenas, minimo=5))

top = [r["puntuacion"] for r in resenas]        # comprensión de lista
print("Puntuaciones:", top, "| máximo:", max(top), "| ordenadas:", sorted(top))
print("Las dos primeras:", top[:2], "| la última:", top[-1], "| del revés:", top[::-1])

coordenadas = (42.09, -1.06)                    # tupla: no se modifica
lat, lon = coordenadas                          # desempaquetado
print(f"Latitud {lat}, longitud {lon}")

for posicion, r in enumerate(resenas, start=1):
    if r["puntuacion"] <= 2:
        print(f"Reseña {posicion}: {r['alojamiento']} — revisar")
```

Salida:

```
Casa Rural El Cierzo         3.50
Apartamentos Las Bardenas    3.50
Hostal Sierra de Luna        3.00
Localidades: ['Luna', 'Sádaba', 'Uncastillo']
Con alguna reseña de 4 o más: {'Apartamentos Las Bardenas', 'Casa Rural El Cierzo', 'Hostal Sierra de Luna'}
Con alguna reseña de 5: {'Casa Rural El Cierzo', 'Hostal Sierra de Luna'}
Puntuaciones: [5, 2, 4, 5, 1, 3] | máximo: 5 | ordenadas: [1, 2, 3, 4, 5, 5]
Las dos primeras: [5, 2] | la última: 3 | del revés: [3, 1, 5, 4, 2, 5]
Latitud 42.09, longitud -1.06
Reseña 2: Casa Rural El Cierzo — revisar
Reseña 5: Hostal Sierra de Luna — revisar
```

Fíjate en `setdefault`: "dame la lista de este alojamiento y, si no existe todavía, créala vacía" — el patrón de **agrupar** registros por una clave, que repetirás mil veces. Y en que el orden con el que se imprime un conjunto **puede variar** entre ejecuciones: si necesitas orden, ordena.

**Ejercicios del apartado.**

- **E10.** Escribe `contar_por_localidad(resenas)` que devuelva un diccionario `{localidad: número de reseñas}`, primero con un bucle y `setdefault` o `get`, y después comprueba que `collections.Counter` hace lo mismo en una línea. ¿Qué devuelve `Counter(...).most_common(1)`?
- **E11.** Escribe `negativas(resenas, umbral=2)` que devuelva la lista de textos de alojamiento de las reseñas con puntuación ≤ umbral, ordenada alfabéticamente y sin repetidos. Explica por qué has usado cada estructura (lista, conjunto) en cada paso.
- **E12.** Dada `puntuaciones = [5, 4, 2, 5, 3, 4, 5, 1]`, obtén con troceado y funciones de la biblioteca estándar: las tres más altas, cuántas son 5, la mediana (ordena y toma el centro) y la lista sin la primera ni la última. Una línea por resultado.

<div style="page-break-before: always;"></div>

## Apartado 5. Clases y objetos, comentarios y documentación

La orientación a objetos de Python es la que conoces de Java con menos ceremonia. `class` define la clase; `__init__` es el constructor; `self` es la referencia al propio objeto y se escribe **explícitamente** como primer parámetro de todos los métodos; no hay `public`/`private` (una convención: un nombre que empieza por `_` es "de uso interno"). Los atributos se crean al asignarlos en `__init__`. `__str__` define lo que muestra `print(objeto)`. Y para clases que son básicamente **un registro de datos** — la mayoría en este módulo — existe `@dataclass`, que genera el constructor y la representación solos.

**Comentarios y documentación** son dos cosas distintas. Un comentario (`# ...`) explica al lector del código **por qué** se hace algo; no explica *qué* hace una línea evidente. Una **docstring** — la cadena entre triples comillas al inicio de un módulo, clase o función — documenta **qué** hace y cómo se usa, y forma parte del programa: `help(objeto)` la muestra, los editores la enseñan al escribir, y las herramientas de documentación la recogen. Las **anotaciones de tipo** (`nombre: str`, `plazas: int = 4`) no cambian la ejecución, pero documentan la intención y permiten al editor avisarte.

```python
# modelos.py — clases, objetos y documentación
from dataclasses import dataclass
from datetime import date


class Resena:
    """Una reseña de un huésped sobre un alojamiento.

    Atributos:
        alojamiento: nombre del alojamiento reseñado.
        puntuacion: entero de 1 a 5.
        texto: opinión escrita por el huésped.
        fecha: fecha de publicación.
    """

    NOTA_MAXIMA = 5                                     # atributo de clase: compartido

    def __init__(self, alojamiento, puntuacion, texto, fecha):
        if not 1 <= puntuacion <= Resena.NOTA_MAXIMA:
            raise ValueError(f"puntuación fuera de rango: {puntuacion}")
        self.alojamiento = alojamiento                  # atributos de instancia
        self.puntuacion = puntuacion
        self.texto = texto
        self.fecha = fecha

    def es_negativa(self):
        """True si la puntuación es 1 o 2."""
        return self.puntuacion <= 2

    def resumen(self, palabras=6):
        """Primeras `palabras` palabras del texto, con puntos suspensivos."""
        return " ".join(self.texto.split()[:palabras]) + "…"

    def __str__(self):                                  # lo que muestra print()
        return f"[{self.puntuacion}/5] {self.alojamiento} ({self.fecha}): {self.resumen()}"


@dataclass
class Alojamiento:
    """Ficha mínima de un alojamiento: la dataclass genera __init__ y __repr__ sola."""
    nombre: str
    localidad: str
    plazas: int = 4


r = Resena("Hostal Sierra de Luna", 5,
           "Trato familiar y desayuno casero. Desde Luna se llega rápido a Zaragoza.",
           date(2026, 5, 9))
print(r)
print("¿Negativa?", r.es_negativa())
print(r.resumen(palabras=3))

hostal = Alojamiento("Hostal Sierra de Luna", "Luna", plazas=12)
print(hostal)
print(hostal.nombre, "tiene", hostal.plazas, "plazas")

help(Resena.resumen)                                    # la documentación se consulta desde el propio código

try:
    Resena("Casa Rural El Cierzo", 7, "Genial", date(2026, 6, 1))
except ValueError as error:
    print("Error controlado:", error)
```

Salida:

```
[5/5] Hostal Sierra de Luna (2026-05-09): Trato familiar y desayuno casero. Desde…
¿Negativa? False
Trato familiar y…
Alojamiento(nombre='Hostal Sierra de Luna', localidad='Luna', plazas=12)
Hostal Sierra de Luna tiene 12 plazas
Help on function resumen in module __main__:

resumen(self, palabras=6)
    Primeras `palabras` palabras del texto, con puntos suspensivos.

Error controlado: puntuación fuera de rango: 7
```

Las dos últimas líneas presentan el manejo de **excepciones**: `raise` lanza un error con un mensaje; `try` / `except` lo captura donde se sabe qué hacer con él. Lo usarás en el apartado 6 para sobrevivir a un fichero que no existe o a una API que no responde. Un **módulo** es simplemente un fichero `.py`: si guardas las clases en `modelos.py`, otro script puede escribir `from modelos import Resena` — así se reparte un programa en piezas.

**Ejercicios del apartado.**

- **E13.** Añade a `Resena` un método `contiene(palabra)` que devuelva `True` si la palabra aparece en el texto sin distinguir mayúsculas, con su docstring, y un método de clase o función `desde_dict(fila)` que construya una `Resena` a partir de un diccionario como los del apartado 4 (la fecha llega como cadena `"2026-05-09"`: consulta `date.fromisoformat`).
- **E14.** Convierte `Alojamiento` en una clase con un método `ocupacion(personas)` que devuelva el porcentaje de plazas ocupadas, lance `ValueError` si `personas` supera las plazas, y esté documentado. Escribe el `try` / `except` que lo prueba con un valor válido y otro inválido.
- **E15.** Revisa este comentario y esta docstring y reescríbelos como corresponde: `total = total + 1  # suma uno a total` y `def media(v): """función media"""`. Explica en una línea la diferencia de papel entre comentario y docstring.

<div style="page-break-before: always;"></div>

## Apartado 6. Obtener datos desde distintos orígenes

Un programa de datos empieza siempre igual: **traer** el dato desde donde vive y **estructurarlo** en memoria. En esta unidad los orígenes son los cuatro que fija el alcance publicado — un fichero CSV, un fichero JSON, una API pública y una base de datos —, todos ellos **dados**: no diseñas capturas ni recoges nada. Y la meta del apartado es una sola idea: **venga de donde venga, el dato acaba siendo una lista de diccionarios** (o una lista de tuplas), y a partir de ahí todo lo que sabes de los apartados 3 a 5 se aplica igual.

El dataset de la unidad es `resenas.csv`, 24 reseñas ficticias de tres alojamientos. Su cabecera y sus cinco primeras filas *(fichero íntegro en el repositorio de la unidad)*:

```
id,alojamiento,localidad,fecha,puntuacion,texto
1,Casa Rural El Cierzo,Uncastillo,2026-03-14,5,"Casa preciosa en pleno casco histórico de Uncastillo. Subimos al Moncayo un día y otro visitamos Sos del Rey Católico. Repetiremos."
2,Casa Rural El Cierzo,Uncastillo,2026-03-21,4,"Muy bien situada, aunque la calefacción tardó en arrancar. Cenamos en un bar de la plaza y el sábado fuimos al mercadillo de Ejea de los Caballeros."
3,Casa Rural El Cierzo,Uncastillo,2026-04-05,2,"La casa estaba fría y sucia al llegar. El pueblo es bonito, pero no volveremos. La oficina de turismo nos atendió muy bien."
4,Apartamentos Las Bardenas,Sádaba,2026-04-11,5,"Ideal para conocer las Bardenas Reales. Paisaje impresionante, y a media hora de Tudela y de Tauste."
5,Apartamentos Las Bardenas,Sádaba,2026-04-18,3,"Apartamento correcto sin más. El wifi iba lento y la ducha goteaba. Lo mejor: la excursión a Tarazona."
```

Le acompañan `alojamientos.json` — la ficha de los tres alojamientos: nombre, localidad, plazas y coordenadas — y `tiempo_ejemplo.json` — una respuesta guardada de la API del tiempo, para trabajar sin red. Sus cinco primeras líneas *(fichero íntegro en el repositorio de la unidad)*:

```json
{
  "red": "Alojamientos rurales de las Cinco Villas (datos ficticios de aula)",
  "alojamientos": [
    {"nombre": "Casa Rural El Cierzo", "localidad": "Uncastillo", "plazas": 8, "latitud": 42.36, "longitud": -1.13},
    {"nombre": "Apartamentos Las Bardenas", "localidad": "Sádaba", "plazas": 4, "latitud": 42.28, "longitud": -1.27},
```

```json
{
  "latitude": 42.375,
  "longitude": -1.125,
  "timezone": "Europe/Madrid",
  "current": {
```

El programa siguiente obtiene datos de los cuatro orígenes:

```python
# origenes.py — el mismo dato desde cuatro orígenes distintos
import csv
import json
import sqlite3
from collections import Counter

# ── 1. Un fichero CSV ──────────────────────────────────────────────────────────
with open("resenas.csv", encoding="utf-8", newline="") as fichero:
    resenas = list(csv.DictReader(fichero))            # cada fila → un diccionario

print(f"{len(resenas)} reseñas leídas; la primera es de {resenas[0]['alojamiento']}")
print("Columnas:", list(resenas[0].keys()))
print("Tipo de la puntuación tal como llega:", type(resenas[0]["puntuacion"]))

for r in resenas:
    r["puntuacion"] = int(r["puntuacion"])            # del CSV todo llega como texto

por_alojamiento = Counter(r["alojamiento"] for r in resenas)
print("Reseñas por alojamiento:", por_alojamiento.most_common())

# ── 2. Un fichero JSON ─────────────────────────────────────────────────────────
with open("alojamientos.json", encoding="utf-8") as fichero:
    red = json.load(fichero)                           # JSON → diccionarios y listas de Python

plazas = {a["nombre"]: a["plazas"] for a in red["alojamientos"]}
print("Plazas por alojamiento:", plazas)

# ── 3. Una API pública (JSON por HTTP) ─────────────────────────────────────────
import requests

def tiempo_actual(latitud, longitud):
    """Temperatura y precipitación actuales en unas coordenadas (API de Open-Meteo)."""
    url = "https://api.open-meteo.com/v1/forecast"
    parametros = {
        "latitude": latitud,
        "longitude": longitud,
        "current": "temperature_2m,precipitation",
        "timezone": "Europe/Madrid",
    }
    respuesta = requests.get(url, params=parametros, timeout=10)
    respuesta.raise_for_status()                       # lanza un error si no es 2xx
    return respuesta.json()["current"]                 # el cuerpo JSON ya convertido

try:
    actual = tiempo_actual(42.36, -1.13)               # Uncastillo, aproximadamente
except requests.RequestException as error:
    print("Sin conexión con la API:", error)
    with open("tiempo_ejemplo.json", encoding="utf-8") as fichero:
        actual = json.load(fichero)["current"]         # respuesta guardada, para trabajar sin red
print(f"En Uncastillo, a las {actual['time']}: {actual['temperature_2m']} °C, "
      f"{actual['precipitation']} mm de precipitación")

# ── 4. Una base de datos (SQL) ─────────────────────────────────────────────────
conexion = sqlite3.connect("resenas.db")               # fichero local; si no existe, se crea
cursor = conexion.cursor()
cursor.execute("DROP TABLE IF EXISTS resena")
cursor.execute("""
    CREATE TABLE resena (
        id INTEGER PRIMARY KEY,
        alojamiento TEXT NOT NULL,
        localidad TEXT NOT NULL,
        fecha TEXT NOT NULL,
        puntuacion INTEGER NOT NULL,
        texto TEXT NOT NULL
    )
""")
cursor.executemany(
    "INSERT INTO resena VALUES (:id, :alojamiento, :localidad, :fecha, :puntuacion, :texto)",
    resenas,                                           # los diccionarios del CSV, tal cual
)
conexion.commit()

cursor.execute("""
    SELECT alojamiento, ROUND(AVG(puntuacion), 2) AS media, COUNT(*) AS n
    FROM resena
    GROUP BY alojamiento
    ORDER BY media DESC
""")
for alojamiento, media, n in cursor.fetchall():        # cada fila llega como tupla
    print(f"{alojamiento:28} media {media} en {n} reseñas")
conexion.close()
```

Salida (con red; sin ella, la parte de la API imprime el aviso y usa la respuesta guardada):

```
24 reseñas leídas; la primera es de Casa Rural El Cierzo
Columnas: ['id', 'alojamiento', 'localidad', 'fecha', 'puntuacion', 'texto']
Tipo de la puntuación tal como llega: <class 'str'>
Reseñas por alojamiento: [('Casa Rural El Cierzo', 8), ('Apartamentos Las Bardenas', 8), ('Hostal Sierra de Luna', 8)]
Plazas por alojamiento: {'Casa Rural El Cierzo': 8, 'Apartamentos Las Bardenas': 4, 'Hostal Sierra de Luna': 12}
En Uncastillo, a las 2026-08-27T11:15: 27.4 °C, 0.0 mm de precipitación
Apartamentos Las Bardenas    media 3.75 en 8 reseñas
Hostal Sierra de Luna        media 3.63 en 8 reseñas
Casa Rural El Cierzo         media 3.5 en 8 reseñas
```

Lo que enseña cada origen:

- **CSV.** `with open(...)` abre el fichero y lo cierra solo al salir del bloque, pase lo que pase; `encoding="utf-8"` evita que las tildes se conviertan en un error (apartado 9); `csv.DictReader` usa la primera línea como nombres de columna. Y la lección con mayúsculas: **del CSV todo llega como cadena** — `"5"`, no `5` — y conviertes tú lo que necesitas como número o fecha.
- **JSON.** `json.load` traduce directamente objetos a diccionarios, listas a listas, números a números: es el formato con el que hablan las APIs y en el que guardarás resultados (apartado 8, `json.dump`).
- **API.** `requests.get` hace la petición HTTP que ya conoces de servidor; `params` construye la cadena de consulta; `raise_for_status()` convierte un 4xx o 5xx en excepción; `.json()` devuelve el cuerpo ya convertido. Una API es un origen que **puede fallar** — sin red, con el servicio caído, con un parámetro mal escrito —, por eso la llamada va dentro de un `try`, con una respuesta guardada como respaldo. Es el patrón profesional: nunca depender de que la red esté ahí.
- **Base de datos.** El patrón es siempre el mismo — conectar, cursor, ejecutar, recoger, cerrar — y es el de la **DB-API** de Python, común a todos los gestores. Aquí usamos `sqlite3` porque viene de serie y crea la base de datos en un fichero: cada uno tiene la suya, sin servidor. Contra la base de datos de referencia del ciclo cambia solo el conector:

```python
# consulta_mysql.py — el mismo patrón contra la base de datos de referencia del ciclo
import mysql.connector                                 # pip install mysql-connector-python

conexion = mysql.connector.connect(
    host="localhost", user="alumno", password="********", database="referencia"
)
cursor = conexion.cursor()
cursor.execute("SELECT alojamiento, AVG(puntuacion) FROM resena GROUP BY alojamiento")
for alojamiento, media in cursor.fetchall():
    print(alojamiento, round(media, 2))
conexion.close()
```

**Y el "Big" de Big Data?** Los ficheros de este módulo caben en memoria; los de una empresa, a veces no. Python está preparado: un fichero abierto se puede recorrer **línea a línea sin cargarlo entero** — `for linea in fichero:` — y `csv.DictReader` funciona igual sobre él; contar, filtrar o sumar sobre millones de filas no exige guardarlas todas. El código del apartado 8 lo tiene en cuenta.

**Ejercicios del apartado.**

- **E16.** Sobre `resenas.csv`: calcula con Python la media de puntuación por **localidad** y el número de reseñas con puntuación 1 o 2 por alojamiento. Después obtén los mismos dos resultados con dos consultas SQL sobre `resenas.db` y comprueba que coinciden. ¿Cuál de los dos caminos te ha resultado más natural y por qué?
- **E17.** Extiende la función `tiempo_actual` para que reciba también el nombre del lugar y devuelva una frase interpretada ("En Luna hace 27,4 °C y no llueve"), y llámala con las coordenadas de los tres alojamientos leídas de `alojamientos.json`. Cambia deliberadamente el nombre de un parámetro de la URL, ejecuta y explica qué error aparece y quién lo produjo (tu código, la librería o la API).
- **E18.** Escribe una función `contar_negativas(ruta)` que recorra un CSV **sin cargarlo entero** en memoria y devuelva cuántas filas tienen puntuación ≤ 2. Razona por qué la versión con `list(csv.DictReader(...))` dejaría de funcionar con un fichero de varios gigabytes y la tuya no.

<div style="page-break-before: always;"></div>

## Apartado 7. Librerías especializadas con modelos preentrenados

Hasta aquí todo lo que hacen tus programas está **programado**: cada regla la escribiste tú. Un **modelo** de inteligencia artificial es un programa cuyo comportamiento no se escribió, sino que se **aprendió** de ejemplos: alguien lo entrenó con millones de textos o imágenes, y lo que se distribuye es el resultado — un fichero con los parámetros aprendidos — junto con la librería que sabe aplicarlo. Un modelo **preentrenado** es exactamente eso: un modelo que otros entrenaron y que tú usas tal cual, igual que usas una función de una librería… con una diferencia capital: **una función te devuelve lo correcto; un modelo te devuelve lo probable**, y puede equivocarse.

La librería de esta unidad es **spaCy**, especializada en procesamiento de lenguaje natural, con su modelo preentrenado en español `es_core_news_sm` (el que instalaste en el apartado 1). Cargarlo y aplicarlo son dos líneas; lo que devuelve es un objeto `Doc` con el texto ya analizado: frases, tokens con su categoría gramatical y su lema, y **entidades** — nombres propios clasificados como lugar (`LOC`), persona (`PER`), organización (`ORG`) u otros (`MISC`):

```python
# entidades.py — un modelo preentrenado aplicado tal cual
import spacy

nlp = spacy.load("es_core_news_sm")                     # carga el modelo: pesa unos 13 MB

texto = "Trato familiar y desayuno casero. Desde Luna se llega rápido a Zaragoza y a Huesca."
doc = nlp(texto)                                        # el modelo analiza el texto entero

print("Frases:", [frase.text for frase in doc.sents])
print("Primeros tokens:", [(t.text, t.pos_, t.lemma_) for t in doc][:6])
print("Entidades:", [(ent.text, ent.label_) for ent in doc.ents])
print("Qué significa LOC:", spacy.explain("LOC"))

# el mismo modelo sobre una reseña informal: fíjate en lo que falla
doc2 = nlp("Casa preciosa en pleno casco histórico de Uncastillo. Subimos al Moncayo un día. Repetiremos.")
print("Entidades:", [(ent.text, ent.label_) for ent in doc2.ents])
```

Salida:

```
Frases: ['Trato familiar y desayuno casero.', 'Desde Luna se llega rápido a Zaragoza y a Huesca.']
Primeros tokens: [('Trato', 'VERB', 'tratar'), ('familiar', 'ADJ', 'familiar'), ('y', 'CCONJ', 'y'), ('desayuno', 'NOUN', 'desayuno'), ('casero', 'ADJ', 'casero'), ('.', 'PUNCT', '.')]
Entidades: [('Luna', 'LOC'), ('Zaragoza', 'LOC'), ('Huesca', 'LOC')]
Qué significa LOC: Non-GPE locations, mountain ranges, bodies of water
Entidades: [('Uncastillo', 'PER'), ('Subimos', 'LOC'), ('Moncayo', 'LOC'), ('Repetiremos', 'LOC')]
```

Mira las dos salidas de entidades con calma, porque ahí está la lección del apartado. En la primera frase el modelo acierta los tres lugares. En la segunda etiqueta "Uncastillo" como **persona** y toma "Subimos" y "Repetiremos" — dos verbos con mayúscula por abrir frase — como **lugares**. No está roto: fue entrenado sobre **texto periodístico** (el `news` del nombre), donde una palabra con mayúscula a mitad de frase casi siempre es un nombre propio, y nuestras reseñas son texto informal con frases cortas. **Un modelo rinde en el dominio en que fue entrenado y se degrada fuera de él.** De ahí tres hábitos que este módulo te va a exigir siempre: mirar la salida con ojos críticos antes de sumarla, **filtrar** con reglas sencillas lo que sabes que es un error típico (lo harás en el apartado 8), y elegir el modelo por su rendimiento **en tu problema**, no por su nombre. Sobre esto último: spaCy ofrece para el español un modelo pequeño, uno mediano y uno grande; el pequeño resuelve lo que esta unidad necesita, arranca en un segundo y pesa 13 MB — **el modelo más pequeño que resuelva el problema** es un criterio de diseño, también por el coste de cómputo.

En el resto del curso aparecerán más librerías especializadas — cada una con su papel: **scikit-learn** para entrenar modelos clásicos (UD3), las librerías y APIs de **modelos de lenguaje** (UD4), **OpenCV** para imagen (UD5) — y todas se usan con el mismo gesto que acabas de aprender: cargar, aplicar, leer con criterio.

**Ejercicios del apartado.**

- **E19.** Aplica el modelo a estas tres reseñas y anota, para cada entidad devuelta, si es correcta, si está mal etiquetada o si no es una entidad: "Ideal para conocer las Bardenas Reales. Paisaje impresionante, y a media hora de Tudela y de Tauste." · "Nos cancelaron la reserva sin avisar y tuvimos que dormir en Ejea de los Caballeros. Pésima gestión." · "Muy tranquilo. Ideal para trabajar unos días en remoto: el wifi funcionó bien todo el tiempo." Formula en una frase el patrón de error que se repite.
- **E20.** Escribe una función `lugares(texto, nlp)` que devuelva solo las entidades `LOC` que **no** sean una única palabra al inicio de una frase (pista: `ent.start`, `ent.sent.start`, `len(ent)`). Compara su resultado con `doc.ents` sobre las reseñas del E19 y di cuántos falsos positivos elimina y si elimina algún acierto.
- **E21.** (Ampliación) Instala el modelo mediano `es_core_news_md` y compara los dos modelos sobre las 24 reseñas: cuántas entidades `LOC` correctas encuentra cada uno, cuántos errores comete, cuánto pesa y cuánto tarda en cargar (`time.perf_counter()`). Redacta en cinco líneas cuál elegirías para este problema y por qué, con los números delante.

<div style="page-break-before: always;"></div>

## Apartado 8. Programar aplicaciones que integran datos y modelo

Ya tienes las dos mitades: obtener datos (apartado 6) y aplicar un modelo (apartado 7). Una **aplicación** del alcance de esta unidad las junta y añade lo que convierte un experimento en algo útil: estructura en funciones, entrada por argumentos, salida guardada en un formato reutilizable y — la parte que distingue al profesional — una **salida interpretada**: no una lista de números, sino la respuesta a la pregunta que alguien hizo. La pregunta de la red de alojamientos: *¿de qué lugares hablan nuestros huéspedes, y cuáles aparecen cuando la reseña es negativa?*

```python
# informe_lugares.py — datos + modelo preentrenado + salida interpretada
"""Cuenta qué lugares mencionan las reseñas de la red de alojamientos.

Uso: python informe_lugares.py resenas.csv
"""
import csv
import json
import sys
from collections import Counter

import spacy


def cargar_resenas(ruta):
    """Lee el CSV y devuelve una lista de diccionarios con la puntuación como entero."""
    with open(ruta, encoding="utf-8", newline="") as fichero:
        resenas = list(csv.DictReader(fichero))
    for r in resenas:
        r["puntuacion"] = int(r["puntuacion"])
    return resenas


def lugares_mencionados(doc):
    """Entidades de tipo LOC, descartando la palabra suelta que abre una frase (error típico del modelo)."""
    lugares = []
    for ent in doc.ents:
        if ent.label_ != "LOC":
            continue
        if len(ent) == 1 and ent.start == ent.sent.start:
            continue
        lugares.append(ent.text)
    return lugares


def resumir(resenas, nlp):
    """Devuelve un diccionario con los recuentos que forman el informe."""
    lugares = Counter()
    lugares_en_negativas = Counter()
    for r in resenas:
        encontrados = lugares_mencionados(nlp(r["texto"]))
        lugares.update(encontrados)
        if r["puntuacion"] <= 2:
            lugares_en_negativas.update(encontrados)
    return {
        "resenas": len(resenas),
        "menciones_de_lugar": sum(lugares.values()),
        "lugares_mas_citados": lugares.most_common(5),
        "lugares_en_resenas_negativas": lugares_en_negativas.most_common(3),
    }


def main(ruta):
    nlp = spacy.load("es_core_news_sm")
    resenas = cargar_resenas(ruta)
    informe = resumir(resenas, nlp)

    with open("informe_lugares.json", "w", encoding="utf-8") as fichero:
        json.dump(informe, fichero, ensure_ascii=False, indent=2)

    # salida interpretada: números convertidos en una frase que alguien pueda usar
    top = informe["lugares_mas_citados"]
    print(f"Analizadas {informe['resenas']} reseñas con {informe['menciones_de_lugar']} menciones de lugar.")
    if top:
        lugar, veces = top[0]
        print(f"El lugar más citado es {lugar} ({veces} menciones); "
              f"le siguen {', '.join(l for l, _ in top[1:4])}.")
    if informe["lugares_en_resenas_negativas"]:
        print("Lugares que aparecen en reseñas negativas:",
              ", ".join(f"{l} ({n})" for l, n in informe["lugares_en_resenas_negativas"]))
    print("Informe completo guardado en informe_lugares.json")


if __name__ == "__main__":
    if len(sys.argv) != 2:
        print("Uso: python informe_lugares.py <fichero.csv>")
        sys.exit(1)
    main(sys.argv[1])
```

Ejecución:

```
$ python informe_lugares.py resenas.csv
Analizadas 24 reseñas con 18 menciones de lugar.
El lugar más citado es Luna (2 menciones); le siguen Zaragoza, Cinco Villas, Moncayo.
Lugares que aparecen en reseñas negativas: Zaragoza (1)
Informe completo guardado en informe_lugares.json
```

La anatomía del programa es la que repetirás en todas las entregas del módulo:

1. **Docstring de módulo** al principio: qué hace y cómo se usa. Es lo primero que lee quien lo abre — y quien lo defiende.
2. **Una función por responsabilidad**: cargar, filtrar, resumir, presentar. Cada una se puede probar sola en un cuaderno, y `resumir` no sabe de ficheros ni `cargar_resenas` de modelos.
3. **El modelo se carga una vez** (`spacy.load` es lo lento) y se aplica muchas.
4. **La regla que corrige al modelo** vive en su propia función, documentada como lo que es: un remedio a un error conocido, que se podrá quitar el día que el modelo mejore.
5. **Resultados en JSON** para que otro programa — una aplicación web, en la UD6 — los consuma, y una **frase para humanos** en pantalla.
6. **El bloque `if __name__ == "__main__":`** — el programa se ejecuta como script, pero sus funciones se pueden importar desde otro fichero sin que se dispare nada.

Y una lectura crítica del resultado, que es parte del trabajo: "Luna" cuenta como lugar más citado con dos menciones, pero uno de los tres alojamientos está en Luna, así que las reseñas lo nombran por proximidad, no por excursión; "Zaragoza" aparece en una reseña negativa porque los huéspedes **se fueron** allí a dormir tras una gotera. Los números del modelo no se entregan: se **interpretan** — y eso se defiende.

**Ejercicios del apartado.**

- **E22.** Añade al informe el recuento de lugares **por alojamiento** (`{alojamiento: [(lugar, veces), ...]}`), de modo que la red sepa qué excursiones se asocian a cada casa, y una línea interpretada más en la salida. Mantén la estructura de funciones.
- **E23.** Haz que el programa acepte un segundo argumento opcional con la puntuación máxima que se considera "negativa" (por defecto 2) y que rechace con un mensaje claro los valores fuera de 1-5. Prueba con `python informe_lugares.py resenas.csv 3` y explica qué cambia en el informe.
- **E24.** Cambia el origen de datos: el programa debe poder leer las reseñas desde `resenas.db` (la base de datos del apartado 6) cuando el argumento termine en `.db`, sin tocar `resumir` ni `lugares_mencionados`. Explica en tres líneas por qué esa separación en funciones ha hecho posible el cambio.

<div style="page-break-before: always;"></div>

## Apartado 9. Errores frecuentes y depuración

Los mensajes de error de Python son buenos: dicen el tipo de error, la línea y, casi siempre, la causa. Léelos **de abajo arriba** — la última línea es el diagnóstico — y aprende a reconocer estos seis, que verás sin falta:

1. **`IndentationError: expected an indented block after 'if' statement on line 1`** — la línea anterior termina en dos puntos y la siguiente no está sangrada. Variante frecuente: mezclar tabuladores y espacios; configura el editor para cuatro espacios y no vuelve a pasar.
2. **`NameError: name 'resenas' is not defined`** — usas un nombre antes de crearlo. En un script suele ser una errata; en un cuaderno, casi siempre es **orden de ejecución**: la celda que crea `resenas` no se ha ejecutado en esta sesión. Reinicia el kernel y ejecuta desde arriba.
3. **`ModuleNotFoundError: No module named 'spacy'`** — la librería no está instalada **en el Python que está ejecutando**. Tres causas por orden de frecuencia: el entorno virtual no está activado, el cuaderno usa otro kernel, o instalaste con un `pip` distinto. `python -m pip install spacy` instala en el mismo Python que ejecuta.
4. **`OSError: [E050] Can't find model 'es_core_news_lg'. It doesn't seem to be a Python package or a valid path to a data directory.`** — spaCy está, el modelo no: falta `python -m spacy download <modelo>` en este entorno, o el nombre está mal escrito. `python -m spacy validate` lista los que tienes.
5. **`UnicodeDecodeError: 'utf-8' codec can't decode byte 0xe1 in position 12: invalid continuation byte`** — el fichero no está en UTF-8 (normalmente lo guardó una hoja de cálculo en la codificación del sistema). Ábrelo con `encoding="latin-1"` o, mejor, vuelve a exportarlo en UTF-8. El error contrario — tildes convertidas en símbolos raros sin error — es el mismo problema sin avisar.
6. **`TypeError: can only concatenate str (not "float") to str`** — sumar una cadena y un número (`"Media: " + 3.62`). Usa una f-string o `str()`. Su primo del apartado 6: **`KeyError: 'b'`** al pedir una clave que el diccionario no tiene (revisa los nombres de columna con `list(fila.keys())`) y **`FileNotFoundError: [Errno 2] No such file or directory: 'no_existe.csv'`** — la ruta se resuelve desde la carpeta en la que lanzaste `python`, no desde donde está el script.

<div style="page-break-before: always;"></div>

## Apartado 10. La IA en esta unidad

Puedes usar un asistente de IA en esta unidad, con cabeza y con responsabilidad sobre el resultado — la misma regla que te aplicará cualquier empresa, y la que fija este módulo: **el uso se declara en el `DECISIONES.md` de cada entrega**. En un módulo *de* inteligencia artificial, además, conviene que uses la IA sabiendo lo que es: exactamente el tipo de modelo preentrenado que estás aprendiendo a aplicar, con los mismos límites.

- **Usos razonables aquí**: pedir la traducción a Python de una construcción que sabes escribir en Java y no te sale; explicaciones alternativas de una idea que se resista (`self`, las comprensiones, `setdefault`, el bloque `__main__`); que te genere más reseñas ficticias para probar el filtro del apartado 8; o que te proponga preguntas de repaso antes de la defensa.
- **Lo que debes verificar siempre**: todo lo que lleve nombre de librería, comando de instalación o versión — el asistente mezcla versiones y a veces inventa parámetros que no existen; contrástalo ejecutando y con la documentación oficial del apartado 12. Y cualquier código que te dé hecho sobre el modelo: si no sabes explicar por qué filtra lo que filtra, no lo entregues. La propia lección del apartado 7 se aplica al asistente: **devuelve lo probable, no lo correcto**.
- **Lo que se te pedirá defender**: cada entrega evaluable de este módulo se defiende. En particular, el programa de la AE9 lo explicarás función a función y responderás a un "¿y si el modelo se equivoca aquí?" — si lo escribió un asistente y no lo entiendes, la defensa lo revela en el primer minuto. Si está en tu entrega, es tuyo.

<div style="page-break-before: always;"></div>

## Apartado 11. Actividad evaluativa final

**Contexto — el servicio comarcal de deportes.** Un servicio comarcal gestiona **seis instalaciones** (dos piscinas, dos pabellones y dos campos de fútbol) repartidas en cinco localidades, y recibe **avisos de incidencias** — fontanería, electricidad, limpieza, césped, seguridad y atención al usuario — que hoy se anotan en una hoja de cálculo. Quieren un programa que lea los avisos, cruce cada uno con su instalación, calcule los indicadores básicos y, con un modelo de lenguaje, averigüe **qué organizaciones y lugares se citan** en las descripciones. Trabajas con dos ficheros **dados**, en el repositorio de la unidad: `avisos.csv` (20 avisos; los abiertos tienen `fecha_cierre` vacía) e `instalaciones.json` (seis instalaciones). Aquí, la cabecera y las cinco primeras filas del primero y las cinco primeras líneas del segundo *(fichero íntegro en el repositorio de la unidad)*:

```
id,instalacion_id,fecha_aviso,fecha_cierre,categoria,prioridad,descripcion
1,PIS-01,2026-06-02,2026-06-03,fontaneria,alta,"La depuradora de la piscina de Ejea de los Caballeros no arranca. Avisado el técnico de mantenimiento."
2,PAB-02,2026-06-02,2026-06-09,electricidad,media,"Fallan tres focos de la pista central del pabellón de Tauste. El club de baloncesto de Tauste pide arreglo antes del torneo."
3,PIS-01,2026-06-04,2026-06-04,limpieza,baja,"Vestuarios sucios tras el cursillo de natación de la mañana."
4,CAM-03,2026-06-05,,cesped,media,"Riego automático del campo de Sádaba encendido a las 12 del mediodía con gente entrenando."
5,PAB-02,2026-06-08,2026-06-08,seguridad,alta,"Puerta de emergencia del pabellón de Tauste bloqueada con material de la escuela de gimnasia."
```

```json
{
  "servicio": "Servicio comarcal de deportes (datos ficticios de aula)",
  "instalaciones": [
    {"id": "PIS-01", "nombre": "Piscina municipal", "localidad": "Ejea de los Caballeros", "tipo": "piscina", "aforo": 600},
    {"id": "PAB-02", "nombre": "Pabellón polideportivo", "localidad": "Tauste", "tipo": "pabellon", "aforo": 500},
```

**Instrucciones.** 10 ejercicios, 1 punto cada uno; el código se entrega **ejecutable** y las respuestas razonadas, sobre el contexto (código que no se ejecuta o respuestas sin justificar no puntúan completas). Alcance: el publicado en la portada — datos dados, modelo preentrenado tal cual, sin entrenar nada. **Tiempo estimado: 3 horas**, más la preparación de evidencias. **Entrega**: carpeta `ud1/ae/` en tu repositorio de la unidad del aula de código, con un script o cuaderno por bloque de ejercicios, las capturas en `ud1/evidencias/`, `requirements.txt` y el `DECISIONES.md` actualizado; commits por bloques de ejercicios. **Defensa individual de 4–5 minutos** según el calendario publicado. La fecha de referencia para "hoy" en los cálculos es el **6 de julio de 2026**.

- **AE1** `[RA1.a]` Evidencia de entorno: captura de `python --version` y de `python -m spacy validate` ejecutados **con el entorno virtual activo** (se debe ver el prefijo del entorno), el `requirements.txt` generado y el `.gitignore` que excluye el entorno. Explica en tres líneas por qué el entorno no se sube al repositorio y cómo lo reconstruiría un compañero.
- **AE2** `[RA1.b]` Este fragmento está escrito por alguien que piensa en Java: `total = 0; for (i = 0; i < len(avisos); i++) { if (avisos[i]["prioridad"] == "alta") { total = total + 1; } }`. Reescríbelo en Python correcto y explica **cuatro** características del lenguaje que lo hacen distinto (al menos una sobre el tipado, una sobre la sintaxis de bloques y una sobre cómo se recorre una colección).
- **AE3** `[RA1.c]` Lee `avisos.csv` y, solo con estructuras de control y diccionarios (sin `Counter`), calcula: el número de avisos por categoría; cuántos siguen abiertos; la media de días de resolución de los cerrados (usa `date.fromisoformat`); y la lista de identificadores de avisos de prioridad alta sin cerrar. Imprime cada resultado con una f-string.
- **AE4** `[RA1.c]` Escribe `avisos_por_instalacion(avisos)` que devuelva `{instalacion_id: [avisos]}` y `top_instalaciones(avisos, n=3)` que devuelva las `n` instalaciones con más avisos como lista de tuplas `(id, número)`, ordenada de más a menos y, a igualdad, por identificador. Obtén además el conjunto ordenado de categorías distintas con una comprensión.
- **AE5** `[RA1.c]` Define la clase `Aviso` con constructor documentado, `desde_dict(fila)` que la construya desde una fila del CSV (fechas convertidas; `fecha_cierre` vacía → `None`), `esta_abierto()`, `dias_abierto(hoy)` (días hasta el cierre o hasta `hoy` si sigue abierto) y `__str__`. Construye los 20 objetos y muestra los **dos avisos abiertos más antiguos**.
- **AE6** `[RA1.d]` Cruza `avisos.csv` con `instalaciones.json`: añade a cada aviso la localidad y el tipo de su instalación, y calcula el número de avisos por localidad y por tipo de instalación. Indica qué instalaciones no tienen ningún aviso (aunque la respuesta sea "ninguna", el código debe calcularlo).
- **AE7** `[RA1.d]` Carga los avisos en una tabla `aviso` de una base de datos SQLite (o en la base de datos de referencia del ciclo, si el docente lo indica) y obtén con **dos consultas SQL**: los avisos abiertos por categoría, y los avisos de prioridad alta por instalación. Compara el resultado de la primera con el de AE3 y explica en dos líneas por qué coinciden.
- **AE8** `[RA1.e]` Aplica el modelo `es_core_news_sm` a las 20 descripciones y obtén, con `Counter`, las entidades `ORG` y `LOC` más frecuentes. Después revisa los resultados **a mano** y documenta al menos **dos aciertos y dos errores del modelo** (etiqueta equivocada, entidad partida o inventada), explicando en cada error por qué es esperable con un modelo entrenado sobre texto periodístico.
- **AE9** `[RA1.f]` Escribe `informe_avisos.py`, un script con docstring de módulo, funciones separadas (cargar, cruzar, analizar con el modelo, resumir, presentar) y bloque `__main__`, que reciba la ruta del CSV por argumento, aplique una regla de filtrado a las entidades justificada en un comentario, guarde `informe_avisos.json` con los recuentos (por categoría, por localidad, entidades `LOC` y `ORG` frecuentes, avisos de alta prioridad abiertos) e imprima **tres frases interpretadas** que el responsable del servicio pueda leer sin saber programar. Se defiende función a función.
- **AE10** `[RA1.b, RA1.f]` **Informe técnico para el servicio** (250–350 palabras, en `ud1/ae/INFORME.md`): qué hace el programa y qué no; por qué Python y sus librerías son una elección adecuada para esta tarea (cita al menos tres características del lenguaje vistas en la unidad); qué límites tiene el modelo preentrenado y qué harías con ellos antes de tomar decisiones sobre sus cifras; y qué dato personal **no** debería viajar en las descripciones de los avisos y por qué. Se defiende sin leer.

<div style="page-break-before: always;"></div>

## Apartado 12. Para ampliar

- [El tutorial oficial de Python (en español)](https://docs.python.org/es/3/tutorial/) — la referencia con la que aprender el lenguaje de verdad: los capítulos 3 a 9 cubren exactamente los apartados 2 a 5 de esta unidad, y el capítulo sobre entornos virtuales y paquetes, el apartado 1. Ve a él cada vez que una construcción no te quede clara.
- [Proyecto Jupyter](https://jupyter.org/) — la web oficial de la herramienta de cuadernos; documentación general también en español en [docs.jupyter.org/es](https://docs.jupyter.org/es/latest/).
- [Instalación de spaCy](https://spacy.io/usage) y [modelos de spaCy para el español](https://spacy.io/models/es) — la documentación oficial de la librería del apartado 7 (en inglés): los comandos de instalación y descarga de modelos, la guía de resolución de problemas (con los errores del apartado 9), y la ficha de cada modelo en español con su tamaño y lo que sabe hacer.
- [Documentación de la API de previsión de Open-Meteo](https://open-meteo.com/en/docs) — el servicio público (en inglés, sin clave para uso no comercial) con el que hicimos la consulta del apartado 6: parámetros de la petición y estructura del JSON de respuesta. Úsala para el E17 y para entender qué devuelve exactamente `current`.
