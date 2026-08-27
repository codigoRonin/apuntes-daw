# UD0. Presentación del módulo

**Módulo optativo AOP1089 — Big Data e Inteligencia Artificial · 2.º DAW · IES Río Arba · Curso 2026-27 · 1 hora**

!!! info "Qué es esta unidad"
    La UD0 no tiene contenidos evaluables: es el arranque del curso. Aquí encontrarás qué vas a aprender — y qué queda fuera, porque este módulo tiene un alcance deliberadamente acotado —, cómo trabajaremos y entregaremos, cómo se califica el módulo y con qué datos, modelos y herramientas construiremos casi todo. Su único "trámite" es el arranque de la primera semana (ver apartado 5), que no tiene nota.

**Al terminar esta unidad sabrás:** qué se aprende en Big Data e Inteligencia Artificial y para qué sirve profesionalmente; qué está dentro y qué está fuera del módulo; cómo se trabaja y se entrega (repositorio, registro de decisiones y defensa desde el primer día, igual que en Desarrollo Web en Entorno Servidor); con qué reglas se calcula tu nota — con un reparto distinto al de los demás módulos — y podrás predecir tú mismo si un caso aprueba o no; y en qué consiste el proyecto integrador con el que termina el curso.

<div style="page-break-before: always;"></div>

## Apartado 1. Qué vas a aprender

En primero aprendiste a guardar datos y a consultarlos; en Desarrollo Web en Entorno Servidor estás aprendiendo a construir las aplicaciones que los sirven. Este módulo añade la capa que hoy más se pide junto a esas dos: **sacar partido de los datos** — prepararlos, visualizarlos, entrenar con ellos modelos sencillos — y **usar modelos de inteligencia artificial ya construidos** (clasificadores de texto, detectores en imágenes, grandes modelos de lenguaje) desde tus propios programas y aplicaciones. Con **100 horas (3 a la semana)**, de las que unas 74 son sesiones en el aula — el resto son las semanas de la formación en empresa, en las que el módulo no tiene clase —, es un módulo pequeño comparado con el de servidor, y por eso no intenta abarcarlo todo: elige lo que un desarrollador web necesita para incorporar datos e inteligencia artificial a una aplicación real.

!!! info "Lo que está dentro y lo que queda fuera"
    **Dentro.** Programar en el lenguaje especializado del oficio de datos, **Python**, con sus librerías. Trabajar con **datasets dados**: los proporciona el docente, vienen de portales de datos abiertos o de la base de datos de referencia del ciclo, y llegan anonimizados y revisados — no diseñarás procesos de captura ni recogerás datos personales. Usar **modelos preentrenados tal cual**, por librería o por API, en las unidades 1, 4, 5 y 6. Y **entrenar tú mismo modelos clásicos** — regresión, árboles de decisión y agrupamiento — sobre esos datasets en la UD3, aprendiendo a medir si son buenos.

    **Fuera.** Entrenar redes neuronales o grandes modelos de lenguaje desde cero; montar infraestructuras distribuidas de Big Data (aquí Big Data se trabaja desde el lado del programador: obtener, preparar y explotar datos con librerías); recolectar datos personales; y las matemáticas del aprendizaje automático — usarás las métricas y sabrás interpretarlas, no las demostrarás.

El módulo se organiza en **5 resultados de aprendizaje** (RA): las capacidades que la norma fija y que tendrás que demostrar. En lenguaje llano:

| RA | Qué serás capaz de hacer |
|---|---|
| RA1 | Programar en el lenguaje especializado del oficio de datos: instalarlo, reconocer lo que lo hace distinto, escribir programas sencillos, obtener datos de distintos orígenes y usar librerías con modelos de inteligencia artificial dentro de tus programas |
| RA2 | Preparar datos de verdad: qué exige la ley de protección de datos, qué tipos de datos hay, cómo se limpian y preparan, cómo se visualizan con distintos gráficos, cómo se contrasta la calidad de los resultados y cómo se documenta todo el proceso |
| RA3 | Aprendizaje automático: distinguir los enfoques de aprendizaje, generar modelos con librerías especializadas, ajustar sus parámetros, obtener resultados, verificar la calidad del modelo y documentar el entrenamiento |
| RA4 | Plataformas de modelos de inteligencia artificial: compararlas en local y en la nube, configurar entornos, gestionar la autenticación y la seguridad, construir las interfaces que integran un modelo en una aplicación, evaluar prestaciones y escalabilidad y documentar el despliegue |
| RA5 | Procesamiento de lenguaje natural y visión artificial: conocer sus herramientas, reconocer patrones en texto e imágenes, extraer información de un texto o de una imagen, evaluar los resultados, y crear e integrar aplicaciones que los usen |

El curso recorre esos RA en **6 unidades** repartidas en tres evaluaciones. La secuencia sigue el orden natural del oficio: primero el lenguaje, después los datos, luego los modelos, después las plataformas que los sirven, y por último texto e imagen — para cerrar con un proyecto que lo junta todo.

| Evaluación | Unidades |
|---|---|
| 1.ª | UD1 Python para datos e IA: lenguaje especializado, entorno y librerías · UD2 Preparación, análisis y visualización de datos; protección de datos · UD3 Aprendizaje automático (primera parte: enfoques y modelos supervisados básicos) |
| 2.ª | UD3 (segunda parte: ajuste, validación y documentación) · UD4 Plataformas de modelos de IA: local, cloud, LLMs y APIs; integración en aplicaciones · UD5 Procesamiento de lenguaje natural y visión artificial |
| 3.ª | UD6 Proyecto integrador: IA aplicada en una aplicación web |

!!! tip "La FEOE de marzo a mayo"
    Entre marzo y mayo harás el segundo periodo de formación en empresa (**360 horas**). Este módulo **no está dualizado**: a diferencia del de servidor, ningún resultado de aprendizaje se adquiere en la empresa — el módulo simplemente para durante esas semanas. Lo importante es lo que pasa antes: **todo lo instrumental está impartido antes de la FEOE** — saldrás a la empresa habiendo trabajado datos, modelos, plataformas y APIs de inteligencia artificial. A la vuelta queda la **3.ª evaluación, corta (unas 3 semanas)**, concentrada en la UD6, que **no trae contenido nuevo**: consolida lo que ya sabes sobre un proyecto aplicado y lo defiendes.

<div style="page-break-before: always;"></div>

## Apartado 2. Cómo trabajaremos

Las sesiones (3 semanales, de 1 hora, en aula de informática con un equipo por persona) alternan tres momentos: **explicación breve**, **práctica guiada** (lo hacemos juntos) y **práctica autónoma** (lo haces tú), sobre **cuadernos y scripts**. Buena parte del trabajo se hace en cuadernos, donde el código, su resultado y tu explicación conviven — el formato habitual del oficio de datos —, y en scripts cuando el programa tiene que ejecutarse solo. La progresión del curso va de la práctica guiada al proyecto integrador: la meta es que en junio incorpores un modelo a una aplicación por tu cuenta.

Reglas de funcionamiento que te interesan desde el día 1:

1. **Google Classroom es la plataforma del módulo** (materiales, calendario, avisos y comunicación), pero **el canal oficial de entrega es el repositorio**: en segundo no hay periodo de adaptación — desde la primera entrega evaluable, entregas en tu repositorio Git del aula de código, con un **historial de commits significativos**. Es el mismo flujo que en Desarrollo Web en Entorno Servidor, y lo aprendiste en primero: aquí se da por hecho. El historial es evidencia de tu proceso y de tu autoría: un volcado único de última hora no cuenta la misma historia que un trabajo real.
2. **Toda entrega viaja con su `DECISIONES.md`**: un registro breve de las decisiones técnicas que has tomado — qué modelo elegiste y por qué, qué descartaste, qué medidas te convencieron — y de las herramientas que has usado (incluida la IA, ver regla 4). Es obligatorio en toda entrega; no puntúa por sí mismo, pero sin él la entrega no está completa.
3. **Todo se defiende.** Cada entrega evaluable incluye una defensa breve (4–5 minutos, con rúbrica publicada) sobre tus decisiones técnicas. **Una práctica sin defensa puntúa 0**; para ausencias justificadas hay una única convocatoria alternativa de defensa por evaluación. En la 3.ª evaluación, la defensa individual del proyecto integrador es además tu prueba individual (ver apartado 3).
4. **La IA se usa a cara descubierta — y aquí, además, se estudia.** Los asistentes de IA son herramientas profesionales legítimas y puedes usarlos; lo que no es admisible es no poder explicar, justificar y sostener lo entregado. Pedirle ayuda a un asistente para escribir el código que llama a un modelo es legítimo; entregar un cuaderno cuyo modelo no sabes explicar, no. El uso se declara en `DECISIONES.md`, y ante indicios de falta de autoría se aplica el procedimiento de verificación publicado en la programación (contraste de evidencias y defensa específica, **adicional a la defensa ordinaria que toda entrega ya incluye**). En este módulo la inteligencia artificial es también objeto de estudio: su marco normativo se trabaja en la UD2 como contexto.
5. **Trabajarás con alias**, nunca con tu nombre real, en las herramientas externas (GitHub, aula de código), conforme a la política de identidad digital y protección de datos del centro — **y también en la plataforma cloud de modelos**, en la que te das de alta con tu alias y en su **nivel gratuito** durante la acogida de la semana 1 (sin pagar nada ni dar datos de más); la plataforma se usa a partir de la UD4.
6. **Datos con responsabilidad.** En este módulo la protección de datos no es una nota al pie: es un criterio de evaluación (UD2) y una condición de trabajo — ningún dataset de aula contiene datos personales reales, y los datos y los modelos se revisan buscando **sesgos**. También cuenta el **coste**: entrenar o consultar un modelo consume cómputo y energía, y elegir el modelo más pequeño que resuelva el problema es un criterio de diseño, no tacañería.
7. **Hay refuerzo y ampliación**: si no has programado en Python, la UD1 lleva andamiaje para que arranques; si vas sobrado, hay retos opcionales de nivel — comparativa sistemática de modelos y ajuste fino de hiperparámetros, modelos adicionales de lenguaje o de visión, o exponer tu modelo tras una API propia, el puente natural con los servicios REST del módulo de servidor — que suben nota de verdad.

!!! note "El aula, con cabeza"
    Trabajamos con equipos todo el curso, así que tres normas de prevención desde el primer día: nada de manipular conexiones ni regletas — cualquier desperfecto se comunica al docente —; postura, distancia a la pantalla y pausas visuales periódicas; y el puesto ordenado, con los cables canalizados y sin líquidos junto a los equipos. Son las del plan de prevención del centro, y forman parte del uso responsable del aula.

<div style="page-break-before: always;"></div>

## Apartado 3. Cómo se calcula tu nota

Las reglas completas y oficiales están en la programación del módulo (tienes la versión reducida publicada en Classroom). Aquí va lo esencial explicado con números — y fíjate bien, porque el reparto **no es el mismo que en los otros módulos**.

### Apartado 3.1. Los dos bloques y sus pesos

En cada evaluación tu nota sale de dos bloques:

| Bloque | Peso | Mínimo exigido |
|---|---|---|
| Prueba individual de evaluación (examen en la 1.ª y en la 2.ª; **defensa individual del proyecto integrador** en la 3.ª) | 40% | **5,0** |
| Prácticas y proyecto (con su defensa) | 60% | **4,0** (media del bloque) |

**Nota de evaluación = 0,4 × prueba individual + 0,6 × prácticas y proyecto**, siempre que se cumplan los dos mínimos. Si un bloque no llega a su mínimo, la evaluación queda suspensa aunque la media salga aprobada — y en ese caso el acta refleja como máximo un 4, aunque tu media fuera mayor.

El reparto se inclina hacia las prácticas porque el centro de gravedad de este módulo es el trabajo con datos, modelos y aplicaciones, que se demuestra entregando. Pero el mínimo de 5 en la prueba individual **no se mueve**: el módulo no se supera solo con prácticas — ni solo con examen.

Además hay dos reglas por resultado de aprendizaje: **ningún RA puede quedar por debajo de 4**, y la **media ponderada de los RA debe ser al menos 5** (cada RA pesa en proporción a las horas que se le dedican; los pesos exactos están publicados en la programación). Puedes compensar un RA flojo (entre 4 y 5) con otros mejores, pero no abandonar ninguno.

Dos reglas más que conviene grabarse: **una práctica sin defensa puntúa 0** — defender significa explicar en persona qué hiciste y por qué, y poder modificarlo en el momento si se te pide —; y en el boletín la nota se redondea al entero más próximo, pero **el 5 nunca se alcanza por redondeo**: un 4,5 a 4,99 se consigna como 4.

### Apartado 3.2. Tabla de casos: ¿aprueba o no?

Todos los casos suponen, salvo que se diga lo contrario, que el resto de condiciones se cumple. Los casos 1 a 7 son de la 1.ª o la 2.ª evaluación (la prueba individual es un examen); el caso 8 es de la 3.ª.

| Caso | Prueba individual | Prácticas y proyecto | Cuenta pendiente | ¿Aprueba? | Por qué |
|---|---|---|---|---|---|
| 1 | 4,5 | 9,5 | 0,4×4,5 + 0,6×9,5 = **7,5** | **No** | El examen no llega al mínimo de 5. Unas prácticas brillantes no compensan un examen suspenso — ni aunque aquí pesen más |
| 2 | 5,0 | 3,5 | 0,4×5 + 0,6×3,5 = **4,1** | **No** | Las prácticas no llegan a su mínimo de 4 |
| 3 | 5,0 | 4,0 | 0,4×5 + 0,6×4 = **4,4** | **No** | Cumple los dos mínimos… pero la media no llega a 5. Los mínimos dan derecho a mediar, no aprueban solos |
| 4 | 6,5 | 4,0 | 0,4×6,5 + 0,6×4 = **5,0** | **Sí** | Mínimos cumplidos y media exactamente 5: aprobado por la mínima. Fíjate: con el mismo 4,0 en prácticas, un 6,0 en el examen se queda en 4,8 — aquí las prácticas pesan más y el examen compensa menos |
| 5 | 7,0 | 8,0 | 0,4×7 + 0,6×8 = **7,6** · pero un RA = 3,5 | **No** | Ningún RA puede quedar por debajo de 4. Ese RA habrá que recuperarlo (y mientras tanto el acta marca como máximo 4) |
| 6 | 5,5 | 6,0 | 0,4×5,5 + 0,6×6 = **5,8** | **Sí** | Todo en regla |
| 7 | 7,0 | Tres entregas: 6, 5 y una **sin defender** (cuenta 0) → media (6+5+0)/3 = **3,67** | Bloque prácticas: 3,67 (la media saldría 0,4×7 + 0,6×3,67 = 5,0) | **No** | La entrega sin defensa hunde el bloque por debajo del mínimo de 4. La media daría justo 5 y aun así suspende: los mínimos van antes que la media |
| 8 (3.ª ev.) | Defensa individual: 4,0 | Proyecto integrador: 8,0 | 0,4×4 + 0,6×8 = **6,4** | **No** | En la 3.ª evaluación la defensa individual es la prueba individual, con su mínimo de 5. Un proyecto excelente que no sabes defender no lo has acreditado tú |

!!! warning "Las cuatro formas más tontas de suspender este módulo"
    Con lo de arriba delante: descuidar el examen confiando en las prácticas (caso 1), no defender una entrega (caso 7), abandonar un RA "porque ese tema no me gusta" (caso 5) y presentarte a la defensa final sin dominar tu propio proyecto (caso 8). Las cuatro son evitables. Y una regla rápida para este reparto: si vas al mínimo en un bloque, el otro tiene que tirar — con un 5,0 en el examen necesitas al menos un 5,0 en prácticas; con un 4,0 en prácticas necesitas al menos un 6,5 en el examen.

### Apartado 3.3. Asistencia y evaluación continua

La asistencia es obligatoria y se registra a diario en SIGAD. El derecho a la evaluación continua se pierde al superar cualquiera de estos umbrales: el **15% de las horas impartidas en una evaluación** (o el **10% si las faltas son injustificadas**), o el **15% de las horas totales del módulo** — que en 100 horas equivale a 15 horas: se pierde **a partir de la hora 16** de falta computada. Los valores en horas de los umbrales por evaluación se publicarán con el calendario de cada una. Tres retrasos no justificados cuentan como una falta; durante la FEOE el módulo no computa asistencia. Perder la evaluación continua no te expulsa del módulo: cambia tu forma de ser evaluado — prueba global de todo el módulo más entrega y defensa del repertorio completo de prácticas clave del curso, incluido el proyecto integrador — y eso casi nunca es buena noticia.

Si suspendes una evaluación, la recuperación se organiza sobre los **RA no superados**: prueba sobre sus criterios más entrega y defensa de las prácticas pendientes o rehechas (para el tramo final del RA5, una nueva defensa del proyecto revisado). Recibirás por escrito qué criterios te faltan y qué actividades de refuerzo te convienen; detalles y fechas, por Classroom.

<div style="page-break-before: always;"></div>

## Apartado 4. Con qué construiremos: datos, modelos y el proyecto integrador

Este módulo no gira sobre una única aplicación ancla, sino sobre tres materiales que se combinan unidad a unidad: **datos del entorno**, **modelos** (los que usas y los que entrenas) y, al final, **una aplicación real** en la que todo eso se integra.

| Tramo | Qué construyes |
|---|---|
| UD1 | Arrancar el taller: Python instalado y comprobado, un entorno virtual, un cuaderno de trabajo — y tu primer programa que lee un fichero de datos y le pregunta algo a un modelo preentrenado |
| UD2 | El pipeline de datos: un dataset del entorno limpiado, preparado, visualizado y documentado — con la ley de protección de datos delante, no detrás |
| UD3 | Tus primeros modelos: regresión, árboles de decisión y agrupamiento entrenados sobre datasets dados; medir si aciertan (coeficiente de determinación, matriz de confusión) y ajustarlos |
| UD4 | Modelos como servicio: ejecutar un modelo abierto en local, entrar con tu alias en la plataforma cloud que diste de alta en la semana 1, llamar a un gran modelo de lenguaje desde tu código, construir la interfaz que lo integra en una aplicación, y medir prestaciones y coste |
| UD5 | Texto e imagen: extraer información de un texto y de una imagen con herramientas de procesamiento de lenguaje natural y visión artificial, y evaluar si el resultado sirve en tu contexto |
| UD6 | El proyecto integrador: la capa de inteligencia artificial de una aplicación web real, en equipo, con documentación y defensa |

**Las herramientas.** Todo lo que usarás es de uso libre y se instala en tu propio equipo: **Python** con entornos virtuales y **Jupyter** como entorno de trabajo; **pandas** y **Matplotlib** para preparar, analizar y visualizar datos; **scikit-learn** para el aprendizaje automático; modelos abiertos ejecutados en local y **una plataforma cloud de modelos con nivel gratuito**, que se concretará antes de la UD4; **spaCy** u **OpenCV**, según el caso, para lenguaje natural y visión; y las bases de datos de primero — **MySQL** y **MongoDB** — como orígenes de datos, sin volver a evaluar lo que ya acreditaste allí. Los materiales no fijan números de versión: trabajamos con la versión soportada actual de cada pieza.

**Los datos.** Son siempre **dados**, y vienen de tres sitios: conjuntos de datos de aula, anonimizados y revisados; portales de **datos abiertos** de la administración — en la UD2 trabajaremos con ellos —; y la base de datos de referencia del ciclo. Nunca datos personales reales: la regla no es una precaución de clase, es la ley, y en la UD2 se estudia como criterio de evaluación, reconectando con lo que viste en Bases de Datos en primero.

**El proyecto integrador.** La UD6 se trabaja en **grupos de máximo tres**, y su producto puede ser **el mismo artefacto** que la aplicación web híbrida de la UD8 de Desarrollo Web en Entorno Servidor, y alimentar también la memoria y la defensa del Proyecto Intermodular. Un mismo artefacto, varias miradas: pero **cada módulo lo evalúa por separado, con sus propios instrumentos y su propia nota** — aquí solo cuenta lo que evidencies de la integración de lenguaje natural y visión en la aplicación, y ninguna nota viaja de un módulo a otro. La defensa, como siempre, es individual. Y el alcance no cambia al final: modelos preentrenados y datasets dados, sin entrenar desde cero ni recoger datos personales. Como la UD6 no introduce contenido nuevo, a la vuelta de la empresa no tienes que aprender nada: tienes que aplicarlo y defenderlo.

A lo largo del curso aparecerán también **otros dominios y otros datasets** en ejercicios de repaso, recuperaciones y exámenes: acostúmbrate a razonar sobre datos que no has visto, porque eso es exactamente lo que te pedirá cualquier empresa.

<div style="page-break-before: always;"></div>

## Apartado 5. El arranque de la primera semana

La acogida de segundo es **única**: la actividad de acogida — alta o verificación de tus cuentas con alias, incorporación al aula de código, una primera entrega de prueba con su commit y su `DECISIONES.md` de juguete, y la lectura guiada del manual — se hace una sola vez con el grupo, coordinada con el módulo de servidor. **Aquí no se repite el trámite.** Lo que sí es propio de este módulo, esa misma semana y sin nota:

1. **Comprobar que estás en el aula de código de este módulo** — la invitación llega por el Classroom — y que tu repositorio responde.
2. **Verificar que tu equipo de casa ejecuta Python.** La instalación es, literalmente, el primer contenido de la UD1, así que si algo falla lo resolvemos allí; pero conviene que llegues con ello probado.
3. La **prueba inicial de segundo, sin nota** (compartida con el módulo de servidor, con una parte propia de este módulo), y una actividad breve de diagnóstico al empezar la UD1: leer un programa corto que trata datos, predecir qué imprime y razonar una pequeña modificación. Respóndelas con sinceridad: sirven para ajustar el ritmo al punto de partida real del grupo — incluido el andamiaje de Python para quien no lo haya usado nunca.

## Apartado 6. Tu kit de arranque

Todo lo del inicio de curso, en un sitio:

| Material | Para qué | Dónde |
|---|---|---|
| Manual del alumnado | Flujo de trabajo con repositorios, convenciones de commits, registro de decisiones y funcionamiento de las defensas | En el Classroom del módulo desde el primer día |
| Programación reducida del módulo | Las reglas oficiales de evaluación y calificación, en versión publicada | En el Classroom, tras la aprobación de la programación (septiembre) |
| Ficha de la actividad de acogida de segundo | La lista de pasos de la primera semana (trámite único, coordinado con el módulo de servidor) | En el Classroom, primera semana |
| Aula de código del módulo | Donde vivirán tus repositorios de este módulo | Invitación en el Classroom del módulo, primera semana |
| Formulario de la prueba inicial de segundo | La prueba sin nota de la primera semana | En el Classroom, primera semana |
| Rúbricas de defensa y de verificación | Cómo se puntúa cada defensa — y la defensa específica si hubiera indicios de falta de autoría | En el Classroom, con el primer enunciado evaluable |
| Hoja de pasos del alta en la plataforma cloud de modelos | El alta con alias en el nivel gratuito | En el Classroom y en el aula, en la primera sesión del optativo — el alta se hace en la acogida única de la semana 1 (ficha de acogida, paso 7); la plataforma se usa a partir de la UD4 |

!!! tip "Un consejo de arranque"
    Tres horas a la semana es poco tiempo para un oficio entero, así que este módulo vive de la regularidad: cada unidad se apoya en la anterior y ninguna admite ponerse al día en una tarde. La UD1 empieza en la próxima sesión: si puedes, llega con Python instalado (si no, lo instalamos juntos), y llega con una pregunta — **¿cuántas líneas hacen falta para que un programa lea una tabla de datos y le pregunte a un modelo ya entrenado qué ve en ella?** — porque vamos a contarlas.
