# Tu entorno de trabajo: Visual Studio Code para Python

**Recurso transversal — Módulo AOP1089, Big Data e Inteligencia Artificial · 2.º DAW · IES Río Arba · Curso 2026-27**

Ya usas **Visual Studio Code** con PHP en Desarrollo Web en Entorno Servidor: el editor es el mismo y no vas a instalarlo otra vez. Esta guía añade, una sola vez, lo que hace falta para trabajar con Python y con cuadernos de Jupyter dentro del editor. No pertenece a ninguna unidad —la consultas cuando la necesites— y en los apuntes solo verás órdenes del lenguaje (`python`, `pip`, `jupyter`), nunca instrucciones atadas a un editor.

Lo que **no** encontrarás aquí, porque ya está en el apartado 1 de la UD1 del módulo: instalar Python, crear y activar el `.venv` de cada unidad, `pip`, `requirements.txt` y el `.gitignore`. Esta página empieza donde aquello termina. Y la idea que lo gobierna todo: **en tu equipo hay más de un Python** (el del sistema y uno por cada `.venv`), y el editor, el terminal y el cuaderno eligen cada uno el suyo. Casi todos los problemas con el editor en este módulo son el mismo: una de esas tres piezas mirando a un Python que no es el de la unidad.

**Una nota sobre el teclado.** Los atajos de esta página son los de Windows y Linux. **En macOS, donde aquí pongas `Ctrl`, pulsa `Cmd`** (la tecla de la manzana): `Cmd+Mayús+P` abre la paleta de comandos, `Cmd+Mayús+X` el panel de extensiones. Y cuidado con una trampa del Mac: `Ctrl` + clic equivale al clic derecho, así que si intentas un atajo con `Ctrl` te saldrá un menú contextual en lugar de la acción que esperabas.

## Las dos extensiones que vas a instalar

Desde el panel de extensiones (`Ctrl+Mayús+X`), con su identificador exacto para no confundirlas con otras de nombre parecido:

- **Python** (`ms-python.python`) — el motor del editor para Python: autocompletado, ir a la definición, avisos mientras escribes y, sobre todo, el **selector de intérprete**.
- **Jupyter** (`ms-toolsai.jupyter`) — abre y ejecuta los cuadernos `.ipynb` dentro del editor, con su **selector de kernel**.

Las dos arrastran algunas extensiones auxiliares al instalarse: es normal. Si el editor te invita a iniciar sesión en algún servicio, no lo necesitas; si alguna vez lo haces, con tu alias, como en el resto de herramientas externas del ciclo.

## El selector de intérprete: que el editor use el Python de la unidad

Abre **la carpeta de la unidad** (`Archivo → Abrir carpeta`), la que contiene el `.venv`: el selector busca entornos dentro de la carpeta abierta. Luego abre la paleta de comandos (`Ctrl+Mayús+P`) y escribe *Seleccionar intérprete*: la orden aparece como **Python: Seleccionar intérprete** (si tu editor la muestra en inglés, es *Python: Select Interpreter*; la paleta encuentra la orden con cualquiera de los dos nombres). Elige de la lista la entrada cuya ruta contiene `.venv`. Si no aparece ninguna, elige la opción de **escribir la ruta del intérprete** (*Enter interpreter path...*) y escribe la ruta de `.venv/bin/python` (en Windows, `.venv\Scripts\python.exe`).

**Comprobación en el propio editor:** en la **barra de estado** (la franja inferior de la ventana), a la derecha, VS Code muestra el intérprete elegido con el nombre `.venv` a la vista. Si ahí lees un Python sin `.venv`, el editor usa el del sistema; hacer clic en ese texto abre de nuevo el selector.

## El terminal integrado hereda el entorno

Abre el terminal integrado (`Ver → Terminal` o `` Ctrl+` ``). Con el intérprete elegido, la extensión **activa el entorno en cada terminal nuevo**: verás `(.venv)` al principio de la línea. Si tenías un terminal abierto de antes, ciérralo y abre otro.

Para comprobarlo sin fiarte del prefijo, esta orden imprime la ruta del Python que está ejecutando:

```
python -c "import sys; print(sys.executable)"
```

La respuesta tiene que pasar por el `.venv` de la unidad; en Linux o macOS, algo así (el comienzo depende de tu equipo, lo que importa es el final):

```
/home/alumno/ud1/.venv/bin/python
```

En Windows acaba en `.venv\Scripts\python.exe`. Si la ruta no contiene `.venv`, lo que instales con `pip` irá al Python del sistema.

## Cuadernos `.ipynb` dentro de VS Code

**Abrir y crear.** Un `.ipynb` se abre con doble clic desde el explorador. Para crear uno, crea un fichero con extensión `.ipynb` en la carpeta de la unidad y ábrelo: aparece el cuaderno vacío con su barra de herramientas encima.

**Ejecutar una celda.** Escribe y pulsa `Shift+Enter`: ejecuta la celda y salta a la siguiente; el resultado aparece debajo. La primera vez, el editor te pide elegir kernel.

**El selector de kernel.** Arriba a la derecha del cuaderno está el botón **Seleccionar kernel** (*Select Kernel* si el editor está en inglés). El kernel es el Python que ejecuta las celdas, y es una elección **independiente** de la del intérprete: puedes tener el editor apuntando al `.venv` y el cuaderno ejecutando con el Python del sistema. Al pulsarlo se abre una lista: elige **Entornos de Python** y, dentro, el intérprete cuya ruta contiene el `.venv` de la unidad. Ese mismo botón muestra desde entonces el nombre del entorno, que es donde miras para comprobarlo. Si el editor avisa de que al entorno le falta la pieza que ejecuta cuadernos y se ofrece a instalarla, acepta: la instala dentro del `.venv`.

**Antes de entregar: reiniciar y ejecutar todo.** El texto del cuaderno es una cosa y el estado del kernel es otra: las variables viven en el kernel, no en las celdas. Si has ejecutado celdas en desorden, borrado alguna o cambiado un valor sin volver a ejecutar lo que dependía de él, el cuaderno **enseña resultados que su texto no produce**. Por eso, siempre antes de entregar, en la barra de herramientas del cuaderno pulsa **Reiniciar** (*Restart*: el kernel arranca limpio y olvida todas las variables) y después **Ejecutar todo** (*Run All*, que ejecuta las celdas de la primera a la última): si todo se ejecuta de arriba abajo sin error, lo que se lee es lo que se obtiene. Un cuaderno que no supera ese gesto no está terminado.

## Ejecutar un script `.py` desde el terminal integrado

Con el terminal integrado abierto y `(.venv)` en la línea, sitúate en la carpeta de la unidad y lanza el script por su nombre:

```
python analisis.py
```

Lo lanzamos desde el terminal a propósito, igual que `php -S` en el otro módulo: queremos que sepas qué Python lo ejecuta, no que aprietes un botón que lo decide por ti. Si el terminal dice que `python` no se reconoce, es el `PATH` del sistema, resuelto en el apartado 1 de la UD1, no el editor.

## Los dos errores de verdad

**1. `ModuleNotFoundError` con la librería instalada.**

- *Síntoma:* la instalaste con `pip` y, aun así, aparece:

  ```
  ModuleNotFoundError: No module named 'pandas'
  ```

- *Causa:* el terminal o el kernel ejecutan **otro Python** distinto de aquel en el que la instalaste.
- *Comprobación:* en el terminal, `python -c "import sys; print(sys.executable)"`; en un cuaderno, `import sys; print(sys.executable)` en una celda. Si la ruta no contiene `.venv`, cambia el kernel con **Seleccionar kernel** o abre un terminal nuevo. Si la ruta es correcta, la instalaste en el otro Python: repite la instalación con `(.venv)` en la línea.

**2. El cuaderno imprime lo que no se lee.**

- *Síntoma:* una celda muestra un resultado que no cuadra con el código que tiene encima, o falla con algo como:

  ```
  NameError: name 'total' is not defined
  ```

  cuando `total` está definido "más arriba".
- *Causa:* **orden de ejecución.** El kernel solo conoce lo que se ha ejecutado en esta sesión, en el orden en que se ejecutó: la celda que crea `total` no se ha ejecutado, o se ejecutó antes de que la cambiaras. Los números entre corchetes a la izquierda de cada celda son el orden real: si no van de arriba abajo, el cuaderno no cuenta la historia que se lee.
- *Comprobación:* **Reiniciar** y **Ejecutar todo**. Si el error desaparece, era el orden; si persiste, es un error del código, y ahora lo verás en la celda que lo provoca.

## Cuaderno o script

Cuaderno para **explorar** —escribes, ejecutas, miras, corriges—; script para **entregar** algo que funcione de principio a fin sin nadie delante. Cada enunciado dice cuál de los dos pide.
