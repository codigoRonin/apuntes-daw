# Presentación del módulo — Proyecto Intermodular (0616)

## Apartado 1. Qué es el Proyecto Intermodular

El Proyecto Intermodular es el módulo que **culmina el ciclo**. En él tu equipo desarrolla una **aplicación web completa** —cliente y servidor, con persistencia de datos y despliegue documentado— que responde a una necesidad real de un sector productivo, y tú demuestras individualmente, con una memoria y una **defensa pública ante tribunal**, que has adquirido las competencias del título.

Es un módulo distinto a todos los demás:

- **No tiene unidades didácticas.** Se organiza por **hitos** (H0 a H6): documentos, seguimientos y entregas que marcan el avance del proyecto durante todo el curso, a razón de 2 horas semanales (67 horas en total).
- **No evalúa tu código.** Programar la aplicación es cosa de los módulos técnicos del ciclo, cada uno con sus propios instrumentos y su propia nota. Lo que este módulo evalúa son sus cuatro resultados de aprendizaje propios: **analizar** las necesidades de un sector, **diseñar** el proyecto que las resuelve, **planificar** su ejecución y **definir su seguimiento y control**. El desarrollo técnico es el vehículo; lo que aquí se califica es tu trabajo como profesional que lleva un proyecto, no como programador que escribe una función.
- **La defensa no es opcional.** Presentar y defender el proyecto en público es un requisito del módulo, no una actividad más: sin defensa no hay evaluación posible.

## Apartado 2. El proyecto y los equipos

Trabajarás en un **equipo de máximo tres personas**, con **evaluación individual en todo caso**: dos miembros del mismo equipo pueden legítimamente obtener notas distintas, porque lo que se califica es la contribución y la comprensión de cada cual, no el producto a secas.

El proyecto de tu equipo saldrá del **catálogo de cinco propuestas** que se presenta en la primera sesión del módulo y cuyo material queda en el Classroom del 0616. Las cinco comparten estructura y se diferencian en el dominio; todas se plantean como encargos profesionales y todas **parten de una base de datos heredada** que provee el docente (esquema y datos de partida), como ocurre en la vida real cuando llegas a un sistema que ya existe. Ese sistema heredado se documenta en el anteproyecto. También es posible que el equipo proponga un proyecto propio, que deberá aprobar el profesor del módulo antes de arrancar.

Además, la misma aplicación se trabaja al final del curso en las unidades de cierre de otros módulos de 2.º —Desarrollo Web en Entorno Servidor y, para quien lo curse, el optativo de Big Data e Inteligencia Artificial—, **con instrumentos y notas estrictamente separados por módulo**: un solo producto, varias evaluaciones independientes. Nada de lo que hagas en un módulo "cuenta" automáticamente en otro.

## Apartado 3. El calendario: hitos H0 a H6

El curso del proyecto está condicionado por la **formación en empresa (FEOE)**, que ocupa el tramo de marzo a mayo: durante esas semanas no hay sesiones del módulo. Por eso el calendario carga el peso del proyecto **antes** de la FEOE y reserva para la vuelta solo el cierre. Las fechas concretas de cada hito se publicarán en el Classroom del 0616 en cuanto el centro cierre su calendario; la estructura es esta y no cambia:

| Hito | Evaluación | Qué se hace y qué se entrega | RA |
|---|---|---|---|
| **H0 · Arranque** | 1.ª | Presentación del módulo y de las propuestas; formación de equipos; alta del repositorio del equipo con alias y `DECISIONES.md` (trámite sin calificación) | — |
| **H1 · Anteproyecto** | 1.ª | Análisis del sector y de sus necesidades, oportunidad, obligaciones fiscales, laborales y de prevención de riesgos, ayudas y guion de trabajo → **documento de anteproyecto aprobado** | RA1 |
| **H2 · Diseño y viabilidad** | 1.ª | Viabilidad técnica, fases, objetivos y alcance, recursos, presupuesto, financiación y documentación de diseño → **dossier de diseño** (seguimiento 1) | RA2 |
| **H3 · Planificación** | 2.ª | Secuenciación de actividades, recursos y logística, permisos, plan de prevención de riesgos, tiempos y valoración económica → **plan de ejecución** (seguimiento 2) | RA3 |
| **H4 · Desarrollo con seguimiento y control** | 2.ª | Procedimientos de evaluación e indicadores de calidad, gestión de incidencias y cambios con registro, participación de usuarios → **núcleo del artefacto + borrador de memoria y demo** (seguimiento 3) | RA4 |
| *(FEOE)* | — | Sin sesiones del módulo; tutoría asíncrona voluntaria | — |
| **H5 · Memoria final** | 3.ª | Integración de la versión final de la aplicación y **entrega de la memoria** | Integrador |
| **H6 · Defensa ante tribunal** | 3.ª | **Defensa pública** del proyecto | Integrador |

**La regla de oro del calendario:** antes de irte a la empresa tienen que estar hechos el **núcleo de la aplicación**, el **borrador de la memoria** y una **demo que funcione** — es decir, los hitos H1 a H4 completos. A la vuelta solo quedan la integración final, la memoria y la defensa, en unas pocas semanas de tercera evaluación: quien llegue a marzo sin núcleo, sin borrador y sin demo no tiene margen para recuperarlo después. El seguimiento 3 verifica expresamente esta regla.

Tres notas más sobre el calendario:

- En las primeras sesiones del curso se realiza además la **evaluación inicial** coordinada con los otros módulos de 2.º; sirve para orientar equipos y proyectos y **no comporta calificación**.
- La FEOE **no sustituye nada de este módulo**: sus resultados de aprendizaje no pueden realizarse en la empresa. Lo que sí puede hacer tu estancia es alimentar el análisis de necesidades y la validación con usuarios del proyecto.
- El Proyecto Intermodular **no es convalidable**: se cursa siempre.

## Apartado 4. Cómo se trabaja

**Un repositorio por equipo, desde el día uno.** En la sesión de arranque (H0) cada equipo da de alta su repositorio de proyecto con los alias de sus miembros y un archivo `DECISIONES.md` inicial. Todo lo que el proyecto produce —anteproyecto, dossier, plan, código, memoria— vive y se entrega en ese repositorio.

**Alias siempre.** Conforme a la política de identidad digital y protección de datos del centro, en el repositorio y en todos los materiales del proyecto trabajas con tu alias: ningún nombre real, ningún dato personal.

**`DECISIONES.md` es el diario técnico del proyecto.** Cada decisión relevante (una tecnología, un cambio de alcance, un descarte) se registra con su **autoría** —quién la tomó y por qué— y con la **declaración del uso de herramientas de inteligencia artificial** cuando lo haya habido. Los roles dentro del equipo rotan y esa rotación también queda documentada ahí.

**Commits que cuentan la historia.** El historial del repositorio, con commits significativos por alias individual, es la evidencia principal de tu contribución. Una memoria que narra un proceso que el historial no sostiene es, por sí misma, un indicio en tu contra.

**La autoría se demuestra, no se presume.** Cada entrega de hito lleva una **defensa breve individual**: explicas lo tuyo, en persona, en cada tramo del curso. El uso **declarado** de herramientas de IA está admitido; lo que no es admisible es no poder explicar, justificar y sostener lo entregado. La regla es simple: **sin defensa sostenible, la entrega no puntúa**.

**Canal oficial.** Las comunicaciones, las fechas y el material de las propuestas van por el **Classroom del 0616**. Las dudas de trabajo, en las sesiones semanales y en los seguimientos.

## Apartado 5. Cómo se evalúa

La evaluación es **individual** y se divide en **dos bloques cuyos pesos fija la norma** (artículo 30.4 del Decreto 91/2024), no el centro ni el docente:

| Bloque | Quién lo otorga | Peso |
|---|---|---|
| Valoración del profesor | El profesor del módulo, a la vista del proceso de elaboración **y** de la presentación y defensa | **60%** |
| Valoración del tribunal | El tribunal, tras leer la memoria y presenciar la defensa pública | **40%** |

### Apartado 5.1. El bloque del profesor (60%)

Dentro del 60%, cada hito pesa en proporción a los criterios de evaluación que evidencia, y se reserva una parte para la memoria final y para la valoración docente de la defensa:

| Instrumento | Peso |
|---|---|
| H1 · Anteproyecto (documento + defensa breve) | 11% |
| H2 · Dossier de diseño y viabilidad (+ seguimiento 1) | 11% |
| H3 · Plan de ejecución (+ seguimiento 2) | 10% |
| H4 · Procedimientos de control + borrador de memoria y demo (+ seguimiento 3) | 8% |
| H5 · Memoria final | 10% |
| Valoración docente de la presentación y defensa | 10% |
| **Total del bloque** | **60%** |

Cada hito se califica con una **rúbrica publicada por adelantado** — en el Classroom del módulo, junto al enunciado del hito — cuyos indicadores son, literalmente, los criterios de evaluación oficiales del resultado de aprendizaje correspondiente: sabrás desde el principio exactamente qué se mide.

### Apartado 5.2. El bloque del tribunal (40%)

El tribunal lo forman **al menos tres docentes del ciclo**; el profesor del módulo **no** forma parte de él. Recibe tu memoria y acceso de lectura a tu repositorio y a `DECISIONES.md`, y valora con rúbrica publicada por el mismo canal: la calidad y claridad de la memoria, la exposición y demostración de la aplicación, la justificación de las decisiones adoptadas y tus respuestas a sus preguntas.

**El acto de defensa** es individual: **15 minutos** de exposición y demostración más **10 minutos** de preguntas. Las preguntas pueden dirigirse a decisiones concretas registradas en `DECISIONES.md` y a tu historial de commits, así que la mejor preparación posible es haber hecho —y entendido— tu parte durante todo el curso.

### Apartado 5.3. Nota final y mínimos

**Nota final = valoración del profesor × 0,60 + valoración del tribunal × 0,40**, expresada de 1 a 10 sin decimales. El módulo se supera con 5 o más, con estos mínimos:

| Mínimo | Por qué |
|---|---|
| Realizar la presentación y defensa pública | Sin defensa, la evaluación no puede llevarse a cabo; no presentarse a la defensa en convocatoria supone figurar como **No Evaluado** |
| Al menos **4 en cada resultado de aprendizaje** (sobre sus evidencias: hito + dimensión de la memoria) y **media final de 5 o más** | Regla del centro, idéntica a la del resto de módulos del ciclo |
| Toda entrega de hito lleva su **defensa breve individual**; sin ella, la entrega **no puntúa** | En un módulo que es 100% proyecto, la defensa de cada tramo es la garantía de autoría y de evaluación individual |
| Calificación final igual o superior a 5 | Norma general de superación |

### Apartado 5.4. Asistencia y recuperación

- **Asistencia.** La evaluación continua se pierde al superar el 15% de las horas del módulo: **a partir de la hora 11 de falta computada**. Quedan excluidas del cómputo, previa acreditación y decisión del equipo docente, las situaciones de conciliación con la actividad laboral y las de deportistas de alto nivel o alto rendimiento. Perder la continua **no exime de nada**: hay que presentar igualmente todos los entregables de H1 a H5, superar una defensa individual ante el profesor previa a la pública, y la defensa ante tribunal (el 40%) se mantiene intacta.
- **Recuperación.** En junio hay dos convocatorias de evaluación final. Si no superas el módulo en la primera, un plan de recuperación individual identifica qué resultados de aprendizaje debes mejorar y qué entregables rehacer, y concluye con **nueva defensa pública ante tribunal**: el bloque del 40% rige en todas las convocatorias.

## Apartado 6. Por dónde empezar

En la primera sesión (H0) se presentan las propuestas, se forman los equipos y cada equipo crea su repositorio con `DECISIONES.md`. El primer trabajo real del proyecto es el **anteproyecto** del hito H1: el documento donde tu equipo analiza el sector, detecta la necesidad, valora la oportunidad y define qué va a construir y cómo.

Para elaborarlo tienes la [plantilla de anteproyecto](plantilla-anteproyecto.md): cópiala al repositorio del equipo y trabajad sobre ella desde el primer día.
