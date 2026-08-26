> **BORRADOR — pendiente de validación del docente. No publicar al alumnado con esta marca.**
> Destino en el sitio: `docs/desarrollo-web-en-entorno-servidor/ud0-presentacion.md` · v1.1 (25/08/2026, Bloque 1 VALIDADO; kit sin marcadores conforme a la regla de canales: el sitio no enlaza artefactos cuyo canal es Classroom o el aula de código — la celda nombra canal y momento). Patrón: UD0 de BBDD. Calificación, canales y funcionamiento: PD 0613 (BORRADOR cerrada, apartados 9 y 11.2). Secuenciación: desglose validado. Artefacto: coherente con el banco v3.
> **v1.2 (25/08/2026):** dos retoques de redacción — la defensa específica de la verificación de autoría se marca como adicional a la ordinaria (apartado 2, regla 4) y la base de datos de partida pasa a formulación sin agente (apartado 4). Nada más cambia.

# UD0. Presentación del módulo

**Módulo 0613 — Desarrollo Web en Entorno Servidor · 2.º DAW · IES Río Arba · Curso 2026-27 · 2 horas**

!!! info "Qué es esta unidad"
    La UD0 no tiene contenidos evaluables: es el arranque del curso. Aquí encontrarás qué vas a aprender, cómo trabajaremos y entregaremos, cómo se califica el módulo y sobre qué aplicación construiremos casi todo. Su única "actividad" es la **actividad de acogida** (ver apartado 5), que no tiene nota.

**Al terminar esta unidad sabrás:** qué se aprende en Desarrollo Web en Entorno Servidor y para qué sirve profesionalmente; cómo se trabaja y se entrega en este módulo (repositorio, registro de decisiones y defensa desde el primer día); con qué reglas se calcula tu nota (y podrás predecir tú mismo si un caso aprueba o no); y en qué consiste la aplicación ancla que haremos crecer durante todo el curso.

<div style="page-break-before: always;"></div>

## Apartado 1. Qué vas a aprender

Hasta ahora has escrito páginas que interpreta el navegador y bases de datos que guardan información. Este módulo pone en marcha la pieza que conecta todo eso: **el servidor** — el lugar donde una web deja de ser un documento y pasa a ser una aplicación: usuarios que inician sesión, formularios que se procesan, datos que se consultan y servicios que otras aplicaciones consumen. Con **267 horas (8 a la semana)** es el módulo de mayor peso de segundo: al terminarlo sabrás construir el lado servidor de una aplicación web completa.

El módulo se organiza en **9 resultados de aprendizaje** (RA): las capacidades que la ley fija y que tendrás que demostrar. En lenguaje llano:

| RA | Qué serás capaz de hacer |
|---|---|
| RA1 | Entender el mapa de la programación en servidor: qué se ejecuta dónde, qué lenguajes y tecnologías existen, cómo se integran con el lenguaje de marcas y qué papel juegan los frameworks |
| RA2 | Escribir código que el servidor ejecuta dentro de páginas web: etiquetas de inserción, tipos de datos, variables y sintaxis |
| RA3 | Programar de verdad sobre ese código: decisiones, bucles, arrays, funciones… y procesar lo que llega de un formulario |
| RA4 | Construir aplicaciones con memoria: mantener el estado, cookies y sesiones, autenticación de usuarios y control por perfiles y roles, con sus pruebas |
| RA5 | Organizar la aplicación como se hace profesionalmente: separar presentación y lógica de negocio con MVC, orientación a objetos y patrones de diseño |
| RA6 | Conectar la aplicación al almacén de datos: recuperar, publicar, actualizar y eliminar información manteniendo su seguridad e integridad |
| RA7 | Crear y consumir servicios web: diseñar una API REST, programarla, verificarla, consumirla y documentarla |
| RA8 | Generar páginas dinámicas con motores de plantillas: interacción con el usuario, verificación de formularios y modificación dinámica del contenido |
| RA9 | Rematar con aplicaciones híbridas: reutilizar código e información de terceros, consumir APIs y repositorios externos y analizar datos con librerías de Big Data e inteligencia de negocios |

El curso recorre esos RA en **8 unidades** repartidas en tres evaluaciones. Fíjate en que el orden de las unidades no sigue el número de los RA: sigue el orden en el que se construye una aplicación real.

| Evaluación | Unidades |
|---|---|
| 1.ª | UD1 Arquitecturas y tecnologías de servidor; panorama de frameworks · UD2 Código embebido: sintaxis, estructuras, funciones y formularios · UD3 Estado, sesiones y autenticación · UD4 Generación dinámica con plantillas |
| 2.ª | UD5 Separación presentación/lógica: MVC, POO y patrones · UD6 Acceso a datos desde el servidor · UD7 Servicios web REST |
| 3.ª | UD8 Aplicaciones web híbridas: APIs y repositorios externos, Big Data e inteligencia de negocios |

!!! tip "La FEOE de marzo a mayo"
    Entre marzo y mayo harás el segundo periodo de formación en empresa (**360 horas**). No es casualidad que para entonces hayas cerrado la programación en el servidor, el estado y la autenticación y la arquitectura MVC: una pequeña parte de tres RA (**RA3, RA4 y RA5**) se adquiere precisamente allí. Durante la FEOE el módulo no tiene clases presenciales; a la vuelta queda la **3.ª evaluación, corta (unas 3 semanas)**, concentrada en la UD8 — el cierre del curso se juega en ese sprint final.

<div style="page-break-before: always;"></div>

## Apartado 2. Cómo trabajaremos

Las sesiones (8 semanales, de 1 hora, en aula de informática con un equipo por persona) alternan tres momentos: **explicación breve**, **práctica guiada** (lo hacemos juntos) y **práctica autónoma** (lo haces tú). En segundo la resolución autónoma pesa desde el primer día, con revisiones de código por pares como práctica habitual: la meta es que trabajes como se trabaja en un equipo de desarrollo.

Reglas de funcionamiento que te interesan desde el día 1:

1. **Google Classroom es la plataforma del módulo** (materiales, calendario, avisos y comunicación), pero **el canal oficial de entrega es el repositorio**: en segundo no hay periodo de adaptación — desde la primera entrega evaluable, entregas en tu repositorio Git del aula de código, con un **historial de commits significativos**. El historial es evidencia de tu proceso y de tu autoría: un volcado único de última hora no cuenta la misma historia que un trabajo real.
2. **Toda entrega viaja con su `DECISIONES.md`**: un registro breve de las decisiones técnicas que has tomado y de las herramientas que has usado (incluida la IA, ver regla 4). Es obligatorio en toda entrega; no puntúa por sí mismo, pero sin él la entrega no está completa.
3. **Todo se defiende.** Cada entrega evaluable incluye una defensa breve (4–5 minutos, con rúbrica publicada) sobre tus decisiones técnicas. **Una práctica sin defensa puntúa 0**; para ausencias justificadas hay una única convocatoria alternativa de defensa por evaluación.
4. **La IA se usa a cara descubierta.** Los asistentes de IA son herramientas profesionales legítimas y puedes usarlos; lo que no es admisible es no poder explicar, justificar y sostener lo entregado. El uso se declara en `DECISIONES.md`, y ante indicios de falta de autoría se aplica el procedimiento de verificación publicado en la programación (contraste de evidencias y defensa específica, **adicional a la defensa ordinaria que toda entrega ya incluye**).
5. **Trabajarás con alias**, nunca con tu nombre real, en las herramientas externas (GitHub, aula de código), conforme a la política de identidad digital y protección de datos del centro.
6. **Hay refuerzo y ampliación**: si una unidad se te atraganta habrá actividades para consolidar la base; si vas sobrado, habrá retos de nivel para subir nota de verdad.

<div style="page-break-before: always;"></div>

## Apartado 3. Cómo se calcula tu nota

Las reglas completas y oficiales están en la programación del módulo (tienes la versión reducida publicada en Classroom). Aquí va lo esencial explicado con números.

### Apartado 3.1. Los dos bloques y sus pesos

En cada evaluación tu nota sale de dos bloques:

| Bloque | Peso | Mínimo exigido |
|---|---|---|
| Examen de evaluación | 50% | **5,0** |
| Prácticas y retos (con su defensa) | 50% | **4,0** (media del bloque) |

**Nota de evaluación = 0,5 × examen + 0,5 × prácticas**, siempre que se cumplan los dos mínimos. Si un bloque no llega a su mínimo, la evaluación queda suspensa aunque la media salga aprobada — y en ese caso el acta refleja como máximo un 4, aunque tu media fuera mayor.

Además hay dos reglas por resultado de aprendizaje: **ningún RA puede quedar por debajo de 4**, y la **media ponderada de los RA debe ser al menos 5** (cada RA pesa en proporción a las horas que se le dedican; los pesos exactos están publicados en la programación). Puedes compensar un RA flojo (entre 4 y 5) con otros mejores, pero no abandonar ninguno.

Dos reglas más que conviene grabarse: **una práctica sin defensa puntúa 0** — defender significa explicar en persona qué hiciste y por qué, y poder modificarlo en el momento si se te pide —; y en el boletín la nota se redondea al entero más próximo, pero **el 5 nunca se alcanza por redondeo**: un 4,5 a 4,99 se consigna como 4.

### Apartado 3.2. Tabla de casos: ¿aprueba o no?

Todos los casos suponen, salvo que se diga lo contrario, que el resto de condiciones se cumple.

| Caso | Examen | Prácticas | Cuenta pendiente | ¿Aprueba? | Por qué |
|---|---|---|---|---|---|
| 1 | 4,5 | 9,5 | Media ponderada: 7,0 | **No** | El examen no llega al mínimo de 5. Unas prácticas brillantes no compensan un examen suspenso |
| 2 | 5,0 | 3,5 | Media ponderada: 4,25 | **No** | Las prácticas no llegan a su mínimo de 4 |
| 3 | 5,0 | 4,0 | 0,5×5 + 0,5×4 = **4,5** | **No** | Cumple los dos mínimos… pero la media no llega a 5 — y el 4,5 **no** redondea a 5. Los mínimos dan derecho a mediar, no aprueban solos |
| 4 | 6,0 | 4,0 | 0,5×6 + 0,5×4 = **5,0** | **Sí** | Mínimos cumplidos y media exactamente 5: aprobado por la mínima |
| 5 | 7,0 | 8,0 | Media ponderada: 7,5 · pero un RA = 3,5 | **No** | Ningún RA puede quedar por debajo de 4. Ese RA habrá que recuperarlo (y mientras tanto el acta marca como máximo 4) |
| 6 | 5,5 | 6,0 | 0,5×5,5 + 0,5×6 = **5,75** | **Sí** | Todo en regla |
| 7 | 7,0 | Tres entregas: 6, 5 y una **sin defender** (cuenta 0) → media (6+5+0)/3 = **3,67** | Bloque prácticas: 3,67 | **No** | La entrega sin defensa hunde el bloque por debajo del mínimo de 4, aunque el examen sea bueno |

!!! warning "Las tres formas más tontas de suspender este módulo"
    Con lo de arriba delante: descuidar el examen confiando en las prácticas (caso 1), no defender una entrega (caso 7) y abandonar un RA "porque ese tema no me gusta" (caso 5). Las tres son evitables.

### Apartado 3.3. Asistencia y evaluación continua

La asistencia es obligatoria y se registra a diario en SIGAD. El derecho a la evaluación continua se pierde al superar cualquiera de estos umbrales: el **15% de las horas impartidas en una evaluación** (o el **10% si las faltas son injustificadas**), o el **15% de las horas totales del módulo** — que en 267 horas equivale a 40 horas: se pierde **a partir de la hora 41** de falta computada. Los valores en horas de los umbrales por evaluación se publicarán con el calendario de cada una. Tres retrasos no justificados cuentan como una falta; durante la FEOE el módulo no computa asistencia presencial. Perder la evaluación continua no te expulsa del módulo: cambia tu forma de ser evaluado — prueba global de todo el módulo más entrega y defensa del repertorio completo de prácticas clave del curso — y eso casi nunca es buena noticia.

Si suspendes una evaluación, la recuperación se organiza sobre los **RA no superados**: prueba sobre sus criterios más entrega y defensa de las prácticas pendientes o rehechas. Recibirás por escrito qué criterios te faltan y qué actividades de refuerzo te convienen; detalles y fechas, por Classroom.

<div style="page-break-before: always;"></div>

## Apartado 4. La aplicación ancla del curso

Casi todo lo que construyas este año girará sobre una misma **aplicación web ancla**: un dominio realista de nuestro entorno productivo, con una **base de datos de partida** — un sistema heredado que recibirás al arrancar el proyecto, esquema y datos incluidos, como cuando llegas a una empresa y el sistema ya existe antes que tú. El dominio concreto se presenta y se decide en el aula en las primeras semanas; lo que no cambia es el recorrido: la aplicación crece unidad a unidad hasta convertirse en un producto completo.

| Tramo | Qué le pasa a la aplicación |
|---|---|
| UD1 | Antes de escribir una línea: entender el mapa — qué se ejecuta en el servidor y qué en el cliente, qué tecnologías existen y con qué criterio se elige la pila de un proyecto |
| UD2 | Las primeras páginas dinámicas del dominio: código embebido y formularios que se procesan en el servidor |
| UD3 | Registro, inicio de sesión y control de acceso por perfiles y roles: la aplicación empieza a tener usuarios |
| UD4 | La capa de presentación con motor de plantillas: vistas limpias y parciales reutilizables |
| UD5 | Refactorización a arquitectura MVC con orientación a objetos y patrones: el mismo producto, organizado como en un equipo profesional |
| UD6 | Persistencia real: la aplicación se conecta a la base de datos de referencia y opera sobre ella |
| UD7 | El dominio expuesto como API REST documentada: tu aplicación, consumible por otras |
| UD8 | Hibridación: APIs y repositorios externos, y análisis de datos con librerías de Big Data e inteligencia de negocios |

La pila tecnológica del curso es **JavaScript de punta a punta**: **Node.js** y **Express** en el servidor, **EJS** como motor de plantillas, arquitectura **MVC**, **Sequelize** sobre **MySQL** (el mismo gestor que usaste en primero), servicios **REST documentados con OpenAPI/Swagger** y consumo de APIs con **fetch/axios**. Un único lenguaje en toda la pila web del ciclo — y uno de los perfiles más demandados del sector.

El tramo final tiene una particularidad: **la UD8 se trabaja en grupos de máximo tres**, y el producto resultante **alimenta también la memoria y defensa del Proyecto Intermodular** — y, si cursas el optativo de Big Data e Inteligencia Artificial, su unidad final. Un mismo artefacto, varias miradas: pero **cada módulo lo evalúa por separado, con sus propios instrumentos y su propia nota** — aquí solo cuenta lo que evidencies del RA9. La defensa, como siempre, es individual.

A lo largo del curso aparecerán también **otros dominios** en ejercicios de repaso, recuperaciones y exámenes: acostúmbrate a razonar sobre dominios nuevos, porque eso es exactamente lo que te pedirá cualquier empresa.

<div style="page-break-before: always;"></div>

## Apartado 5. La actividad de acogida

La primera semana completarás la **actividad de acogida** (sin nota): alta o verificación de tus cuentas con alias, incorporación al aula de código del módulo, una primera entrega de prueba — con su commit y su `DECISIONES.md` de juguete, para rodar el circuito completo antes de que puntúe — y la lectura guiada del manual del alumnado. Tiene su propia ficha con la lista de pasos y comprobaciones — síguela de ahí; aquí no se repite. También harás la **prueba inicial de segundo, sin nota**, que nos sirve para ajustar las clases al punto de partida real del grupo: respóndela con sinceridad.

## Apartado 6. Tu kit de arranque

Todo lo del inicio de curso, en un sitio:

| Material | Para qué | Dónde |
|---|---|---|
| Manual del alumnado | Flujo de trabajo con repositorios, convenciones de commits, registro de decisiones y funcionamiento de las defensas | En el Classroom del módulo desde el primer día |
| Programación reducida del módulo | Las reglas oficiales de evaluación y calificación, en versión publicada | En el Classroom, tras la aprobación de la programación (septiembre) |
| Ficha de la actividad de acogida | La lista de pasos de la primera semana | En el Classroom del módulo, primera semana |
| Tarea de acogida del aula de código | Donde harás tu primer commit (y tu primer `DECISIONES.md`) | En el aula de código, primera semana |
| Formulario de la prueba inicial | La prueba sin nota de la primera semana | En el Classroom del módulo, primera semana |

!!! tip "Un consejo de arranque"
    Este módulo premia la regularidad: 8 horas semanales durante un curso construyen un oficio, pero solo si el repositorio respira cada semana — los commits de última hora se notan, y no a tu favor. La UD1 empieza en la próxima sesión: llega con una pregunta en la cabeza — **¿qué pasa exactamente desde que pulsas Enter en una dirección hasta que la página aparece?** — porque vamos a responderla entera.
