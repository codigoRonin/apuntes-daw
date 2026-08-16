# UD0. Presentación del módulo

**Módulo 0484 — Bases de Datos · 1.º DAW · IES Río Arba · Curso 2026-27 · 2 horas**

!!! info "Qué es esta unidad"
    La UD0 no tiene contenidos evaluables: es el arranque del curso. Aquí encontrarás qué vas a aprender, cómo trabajaremos, cómo se califica el módulo y sobre qué dominio real construiremos casi todo. Su única "actividad" es la **actividad de acogida** (ver apartado 5), que no tiene nota.

**Al terminar esta unidad sabrás:** qué se aprende en Bases de Datos y para qué sirve profesionalmente; cómo se trabaja y se entrega en este módulo; con qué reglas se calcula tu nota (y podrás predecir tú mismo si un caso aprueba o no); y en qué consiste el dominio sobre el que trabajaremos todo el curso.

<div style="page-break-before: always;"></div>

## Apartado 1. Qué vas a aprender

Los datos son el activo más valioso de casi cualquier organización: una empresa que pierde su web tiene un mal día; una que pierde su base de datos puede no volver a abrir. Este módulo (200 horas, 6 a la semana) te enseña a **diseñar, construir, consultar y proteger** bases de datos, el oficio que está debajo de cualquier aplicación seria.

El módulo se organiza en **7 resultados de aprendizaje** (RA): las capacidades que la ley fija y que tendrás que demostrar. En lenguaje llano:

| RA | Qué serás capaz de hacer |
|---|---|
| RA1 | Entender el mapa completo: qué tipos de bases de datos existen, qué hace un sistema gestor y por qué usarlo, qué pasa cuando los datos se reparten entre varias sedes, qué obliga la ley de protección de datos y qué son Big Data y la inteligencia de negocios |
| RA2 | Crear bases de datos relacionales de verdad: tablas, relaciones, tipos de datos, claves, restricciones, vistas y usuarios con sus permisos |
| RA3 | Consultar la información: desde el `SELECT` más simple hasta consultas de resumen, combinaciones de varias tablas, subconsultas y consultas optimizadas para que respondan rápido con millones de filas |
| RA4 | Modificar datos con cabeza: insertar, borrar y actualizar sin romper nada, usando transacciones para que las operaciones delicadas se hagan enteras o no se hagan |
| RA5 | Programar dentro de la base de datos: procedimientos, funciones, disparadores que reaccionan solos a los cambios, cursores y manejo de errores |
| RA6 | Diseñar antes de construir: pasar de un problema real a un diagrama entidad/relación y de ahí a un modelo relacional normalizado (esto se estudia antes de crear nada: diseñar mal sale carísimo) |
| RA7 | Manejar bases de datos no relacionales (NoSQL): cuándo tienen sentido, qué tipos hay y cómo se trabaja con ellas |

El curso recorre esos RA en **8 unidades** repartidas en tres evaluaciones:

| Evaluación | Unidades |
|---|---|
| 1.ª | UD1 Sistemas de almacenamiento, SGBD y el valor del dato · UD2 Modelo entidad-relación · UD3 Modelo relacional y normalización · UD4 Consultas SQL (primera parte) |
| 2.ª | UD4 (segunda parte: consultas avanzadas y optimización) · UD5 Definición de datos (DDL) · UD6 Manipulación de datos, transacciones y concurrencia |
| 3.ª | UD7 Programación en la base de datos · UD8 Bases de datos no relacionales (NoSQL) |

!!! tip "La FEOE de febrero"
    En febrero harás la formación en empresa (FEOE). No es casualidad que antes de irte hayamos trabajado a fondo las consultas y la modificación de datos: llegarás a la empresa sabiendo hacer lo que allí se hace a diario. Una pequeña parte de tres RA (RA2, RA3 y RA4) se adquiere precisamente allí.

<div style="page-break-before: always;"></div>

## Apartado 2. Cómo trabajaremos

Las sesiones (6 semanales, en aula de informática con un equipo por persona) alternan tres momentos: **explicación breve**, **práctica guiada** (lo hacemos juntos) y **práctica autónoma** (lo haces tú). A lo largo del curso el peso se desplaza de lo guiado a lo autónomo: la meta es que en junio resuelvas retos por tu cuenta.

Reglas de funcionamiento que te interesan desde el día 1:

1. **Google Classroom es el canal oficial**: materiales, entregas, fechas y avisos. Lo que no está entregado en Classroom (o en el canal que se anuncie formalmente en su momento) no está entregado.
2. **Trabajarás con alias**, nunca con tu nombre real, en las herramientas externas (GitHub, aula de código): es la política de protección de datos del centro y, de paso, tu primera lección práctica del RA1 sobre datos personales.
3. **El trabajo se construye sobre casos realistas**, principalmente el dominio del curso (apartado 4). El trabajo individual es la base en primero, con actividades en grupo puntuales en las unidades de diseño y en el reto final.
4. **Las prácticas se defienden.** Entregar no basta: hay que saber explicar lo entregado. Una práctica que no se defiende cuando se pide no puntúa (ver apartado 3).
5. **Hay refuerzo y ampliación**: si una unidad se te atraganta habrá actividades para consolidar la base; si vas sobrado, habrá retos de nivel para subir nota de verdad.

<div style="page-break-before: always;"></div>

## Apartado 3. Cómo se calcula tu nota

Las reglas completas y oficiales están en la programación del módulo (tienes la versión reducida publicada en Classroom). Aquí va lo esencial explicado con números.

### Apartado 3.1. Los dos bloques y sus pesos

En cada evaluación tu nota sale de dos bloques:

| Bloque | Peso | Mínimo exigido |
|---|---|---|
| Exámenes | 60% | **5,0** |
| Prácticas (con su defensa) | 40% | **4,0** |

**Nota de evaluación = 0,6 × examen + 0,4 × prácticas**, siempre que se cumplan los dos mínimos. Si un bloque no llega a su mínimo, la evaluación queda suspensa aunque la media aritmética salga aprobada.

Además hay dos reglas por resultado de aprendizaje: **ningún RA puede quedar por debajo de 4**, y la **media de los RA debe ser al menos 5**. Es decir: puedes compensar un RA flojo (entre 4 y 5) con otros mejores, pero no abandonar ninguno.

Y una regla de las prácticas que conviene grabarse: **una práctica sin defensa puntúa 0**. Defender significa explicar en persona qué hiciste y por qué, y poder modificarlo en el momento si se te pide.

### Apartado 3.2. Tabla de casos: ¿aprueba o no?

Todos los casos suponen, salvo que se diga lo contrario, que el resto de condiciones se cumple.

| Caso | Examen | Prácticas | Cuenta pendiente | ¿Aprueba? | Por qué |
|---|---|---|---|---|---|
| 1 | 4,5 | 9,5 | Media ponderada: 6,5 | **No** | El examen no llega al mínimo de 5. Unas prácticas brillantes no compensan un examen suspenso |
| 2 | 5,0 | 3,5 | Media ponderada: 4,4 | **No** | Las prácticas no llegan al mínimo de 4 |
| 3 | 5,0 | 4,0 | 0,6×5 + 0,4×4 = **4,6** | **No** | Cumple los dos mínimos… pero la media ponderada no llega a 5. Los mínimos dan derecho a mediar, no aprueban solos |
| 4 | 6,0 | 4,0 | 0,6×6 + 0,4×4 = **5,2** | **Sí** | Mínimos cumplidos y media ≥ 5 |
| 5 | 7,0 | 8,0 | Media ponderada: 7,4 · pero un RA = 3,5 | **No** | Ningún RA puede quedar por debajo de 4. Ese RA habrá que recuperarlo |
| 6 | 5,5 | 6,0 | 0,6×5,5 + 0,4×6 = **5,7** | **Sí** | Todo en regla |
| 7 | 7,0 | Tres prácticas: 6, 5 y una **sin defender** (cuenta 0) → media (6+5+0)/3 = **3,67** | Bloque prácticas: 3,67 | **No** | La práctica sin defensa hunde el bloque por debajo del mínimo de 4, aunque el examen sea bueno |

!!! warning "Las tres formas más tontas de suspender este módulo"
    Con lo de arriba delante: descuidar el examen confiando en las prácticas (caso 1), no defender una práctica (caso 7) y abandonar un RA "porque ese tema no me gusta" (caso 5). Las tres son evitables.

### Apartado 3.3. Asistencia y evaluación continua

La asistencia es obligatoria y se registra a diario en SIGAD. Se pierde el derecho a la evaluación continua al superar los umbrales de faltas que fija el centro; sobre el total del módulo, el umbral del 15 % equivale en este módulo (200 horas) a **30 horas**. Los umbrales por evaluación se publicarán con el calendario de cada una. Tres retrasos no justificados cuentan como una falta. Perder la evaluación continua no te expulsa del módulo: cambia tu forma de ser evaluado (prueba final sobre todos los contenidos), y eso casi nunca es buena noticia.

Si suspendes una evaluación habrá recuperación según lo publicado en la programación; los detalles y fechas se anunciarán por Classroom.

<div style="page-break-before: always;"></div>

## Apartado 4. El dominio del curso: mantenimiento de parques renovables

Casi todo lo que construyas este año girará sobre un mismo caso real de nuestra comarca: una **empresa de servicios que mantiene parques eólicos y plantas fotovoltaicas**. Si has viajado por los alrededores los has visto: hileras de aerogeneradores y campos de placas. Alguien tiene que mantener todo eso funcionando — y ese trabajo es, en gran parte, un problema de datos.

La empresa gestiona **activos** (aerogeneradores, seguidores, inversores), **sensores** que envían lecturas continuamente (temperatura, vibración, producción), **órdenes de trabajo** preventivas y correctivas, **cuadrillas de técnicos** con sus cualificaciones y un **almacén de repuestos**. La telemetría aporta la escala: millones de lecturas, donde un diseño descuidado convierte una consulta de segundos en una de minutos.

Recorrido del curso sobre el dominio, en versión breve:

| Tramo | Qué harás con la empresa de mantenimiento |
|---|---|
| UD1 | Entender su problema de información: ¿base de datos centralizada o repartida por parques? ¿Qué exige la ley sobre los datos de los técnicos? ¿Es su telemetría un caso de Big Data? |
| UD2-UD3 | Diseñar su base de datos: del papel al diagrama entidad/relación y de ahí al modelo relacional normalizado |
| UD4 | Consultarla a fondo sobre una **versión de referencia con datos masivos que te dará el docente**: disponibilidad por activo, órdenes sin cerrar, consumos… y el reto estrella: hacer que las consultas vuelen sobre 5 millones de lecturas |
| UD5 | Construir **tu propia versión** del diseño que validaste en UD3 |
| UD6 | Operarla: cargas masivas, y la transacción de cierre de una orden de trabajo (parte + repuestos + estado del activo: todo o nada) |
| UD7 | Automatizarla: generación de preventivas por calendario, alertas por umbral de sensor, informes |
| UD8 | Repensarla: la telemetría como serie temporal en una base de datos NoSQL |

A lo largo del curso aparecerán también **otros dominios** (una cooperativa agroalimentaria, una red de turismo rural, un servicio comarcal de deportes) en ejercicios de repaso, recuperaciones y exámenes: acostúmbrate a razonar sobre dominios nuevos, porque eso es exactamente lo que te pedirá cualquier empresa.

<div style="page-break-before: always;"></div>

## Apartado 5. La actividad de acogida

La primera semana completarás la **actividad de acogida** (sin nota): alias, cuenta de GitHub, alta en Classroom, primera entrega de prueba y tu primer commit en el aula de código. Tiene su propia ficha con la lista de pasos y comprobaciones — síguela de ahí; aquí no se repite. También harás una **prueba inicial sin nota** que nos sirve para ajustar las clases al punto de partida real del grupo: respóndela con sinceridad.

## Apartado 6. Tu kit de arranque

Todo lo del inicio de curso, en un sitio:

| Material | Para qué | Dónde |
|---|---|---|
| Manual del alumnado | Normas de funcionamiento, canales y herramientas del módulo | [ENLACE: se completará al publicar en septiembre] |
| Programación reducida del módulo | Las reglas oficiales de evaluación y calificación, en versión publicada | [ENLACE: se completará al publicar en septiembre] |
| Ficha de la actividad de acogida | La lista de pasos de la primera semana | [ENLACE: se completará al publicar en septiembre] |
| Tarea de acogida del aula de código | Donde harás tu primer commit | [ENLACE: se completará al publicar en septiembre] |
| Formulario de la prueba inicial | La prueba sin nota de la primera semana | [ENLACE: se completará al publicar en septiembre] |

!!! tip "Un consejo de arranque"
    Este módulo premia la regularidad: 6 horas semanales durante un curso construyen un oficio, pero solo si no dejas que las unidades se acumulen. La UD1 empieza en la próxima sesión — trae las preguntas de negocio de tu equipo.
