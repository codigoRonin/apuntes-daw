# Instalación de MySQL Server y MySQL Workbench

**Recurso transversal — Módulo 0484, Bases de Datos · 1.º DAW**

Desde la UD4 vas a ejecutar SQL de verdad. Para eso hacen falta dos programas distintos, y conviene no confundirlos:

- **MySQL Server** es el gestor: el servicio que guarda las bases de datos y responde a las consultas. No tiene ventana; trabaja en segundo plano.
- **MySQL Workbench** es el cliente gráfico: la ventana donde escribes las consultas, las envías al servidor y ves los resultados.

Esta guía se sigue **una sola vez**, de principio a fin, y termina con una comprobación. Si la comprobación del apartado 6 sale bien, ya puedes cargar el guion del apartado 0 de la [UD4](../bases-de-datos/ud4-consultas-sql.md).

## Apartado 1. Qué versión se instala

- **MySQL Server, serie 8.4**, que es la serie de soporte largo (LTS): solo recibe correcciones, sin cambios de comportamiento. Es la que usamos todo el curso.
- **MySQL Workbench**, la versión que ofrezca su página de descargas.

Un aviso que conviene conocer desde el principio: el manual de Workbench dice que la herramienta se desarrolla y se prueba con MySQL Server 8.0, y que puede conectarse a la serie 8.4 aunque alguna de sus funciones no esté disponible. Para lo que hacemos en el módulo —escribir consultas, ejecutarlas y ver sus resultados— no supone ningún problema. Si al conectar aparece un aviso sobre la versión del servidor, acéptalo y continúa.

## Apartado 2. Instalar en Windows

En los equipos del aula la instalación necesita una cuenta con permisos de administrador: si no la tienes, la hace el docente. En tu equipo de casa puedes seguir estos pasos tal cual.

### Apartado 2.1. MySQL Server

Para la serie 8.4 el camino es el instalador **MSI del servidor**, que trae consigo la aplicación **MySQL Configurator**. El antiguo «MySQL Installer», que instalaba todos los productos a la vez, no forma parte de la instalación de esta serie: si encuentras un tutorial que lo usa, es de una versión anterior.

1. Descarga el MSI de **MySQL Community Server** desde la página oficial de descargas (`https://dev.mysql.com/downloads/mysql/`), eligiendo la serie **8.4 LTS** y el sistema **Microsoft Windows**. Es un fichero `.msi`.
2. El servidor necesita el paquete **Microsoft Visual C++ 2019 Redistributable**: si no está en el equipo, instálalo antes desde la web de Microsoft.
3. Ejecuta el MSI, acepta la licencia y deja el tipo de instalación que viene. El servidor se instala en `C:\Program Files\MySQL\MySQL Server 8.4\`.
4. Al terminar, el asistente ofrece abrir **MySQL Configurator**. Ábrelo: **el servidor no arranca hasta que se configura.**

En MySQL Configurator, pantalla a pantalla:

| Pantalla | Qué eliges |
|---|---|
| **Type and Networking** | Tipo de configuración de desarrollo (*Development*) y el puerto **3306**, que es el que viene. No cambies nada más |
| **Accounts and Roles** | La contraseña del usuario `root`, el administrador del servidor. **Apúntala**: sin ella no podrás conectarte. En el aula, la que indique el docente. No hace falta crear más usuarios |
| **Windows Service** | Déjalo como viene: el servidor se configura como servicio de Windows y arranca con el equipo |
| **Server File Permissions** | La opción que viene marcada |
| **Sample Databases** | Ninguna: no las usamos |
| **Apply Configuration** | Pulsa **Execute** y espera a que todos los pasos terminen bien |

Si algún día necesitas cambiar algo, MySQL Configurator se puede volver a abrir desde el menú Inicio, en el grupo **MySQL**.

### Apartado 2.2. MySQL Workbench

1. Descarga el MSI de **MySQL Workbench** desde `https://dev.mysql.com/downloads/workbench/`.
2. Ejecútalo y elige la instalación completa (*Complete*). Si pide requisitos de Microsoft (el paquete Visual C++ Redistributable o .NET Framework), instálalos y repite.
3. Ábrelo desde el menú Inicio.

## Apartado 3. Si trabajas en macOS

Los pasos de los apartados 4 a 6 son los mismos; solo cambia la instalación.

- **Servidor:** descarga el paquete **DMG** de MySQL Community Server 8.4 desde la misma página de descargas, ábrelo y ejecuta el fichero `.pkg` que contiene. El asistente te pide la **contraseña de `root`** (apúntala) y te deja marcar que el servidor arranque al terminar. Después, el servidor se arranca y se para desde el panel de preferencias **MySQL**.
- **Workbench:** descarga su DMG, ábrelo y arrastra el icono de MySQL Workbench a la carpeta Aplicaciones.

## Apartado 4. Crear la conexión

Workbench no sabe todavía dónde está tu servidor: hay que decírselo una vez.

1. En la pantalla inicial de Workbench, pulsa el **[+]** que hay junto a **MySQL Connections**. Se abre el cuadro **Setup New Connection**.
2. En **Connection Name** escribe un nombre que reconozcas, por ejemplo `local-daw`. Los demás valores sirven como vienen para un servidor instalado en tu propio equipo: el puerto `3306` y el usuario `root`.
3. Pulsa **Test Connection**. Cuando te pida la contraseña, es la de `root` del apartado 2.1. Si la prueba falla, casi siempre es por una de estas dos cosas: la contraseña no es la que apuntaste, o el servidor no está arrancado.
4. Pulsa **OK**. La conexión queda en la pantalla inicial: un clic sobre ella abre el editor de SQL.

## Apartado 5. Ejecutar SQL y ver los resultados

Al abrir la conexión tienes una **pestaña de consulta**: un editor de texto donde escribes o pegas SQL. Hay dos maneras de ejecutar, y la diferencia importa:

| Botón de la barra de la pestaña | Qué ejecuta | Cuándo usarlo |
|---|---|---|
| El primer rayo: *Execute the selected portion of the script or everything, if there is no selection* | Lo que tengas seleccionado; si no hay nada seleccionado, **todo** lo que hay en la pestaña | Para cargar un guion entero, como el del apartado 0 de la UD4 |
| El rayo con el cursor: *Execute the statement under the keyboard cursor* | Solo la sentencia sobre la que está el cursor | Para probar una consulta suelta |

Las mismas dos órdenes están en el menú **Query**: *Execute (All or Selection)* y *Execute Current Statement*. Para abrir un guion guardado en un fichero `.sql`, el primer botón de la barra (*Open a script file in this editor*) lo carga en la pestaña.

Dónde mirar después de ejecutar:

- **La rejilla de resultados** aparece debajo del editor. Si has ejecutado varias sentencias, hay una pestaña de resultado por cada una que devuelva filas.
- **El panel de salida** (*Output*), en la parte inferior, anota cada sentencia ejecutada y, si ha fallado, el error con su número. Es lo primero que hay que leer cuando algo no sale.

Dos comportamientos de Workbench que te ahorrarán sustos:

- **Limita las filas.** Por defecto añade un límite de 1.000 filas a cada `SELECT`. Con las tablas pequeñas no lo notarás; con la tabla de 100.000 filas del apartado 10 de la UD4, un `SELECT` que las pida todas enseñará solo las primeras 1.000. Los recuentos (`COUNT`) no se ven afectados. Se cambia en el menú **Query → Limit Rows**.
- **Puede pararse en el primer error.** Un botón de la barra (*Toggle whether execution of SQL script should continue after failed statements*) decide si, al fallar una sentencia de un guion, se ejecutan o no las siguientes. Si un guion se queda a medias, mira el panel de salida, corrige la sentencia que falló y vuelve a ejecutar.

## Apartado 6. Comprobación final

En una pestaña de consulta, ejecuta:

```sql
SELECT VERSION(), @@sql_mode;
```

Tiene que devolver una fila con dos columnas:

- En la primera, la versión del servidor, que debe empezar por **8.4**.
- En la segunda, el modo SQL, una lista separada por comas en la que debe figurar **`ONLY_FULL_GROUP_BY`**. Con la instalación de esta guía la lista completa es:

```
ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION
```

El modo SQL decide cuánto de estricto es el servidor. `ONLY_FULL_GROUP_BY` es el que hace que MySQL rechace las agrupaciones mal escritas en vez de devolver un resultado arbitrario (lo verás en el apartado 6 de la UD4), y por eso lo comprobamos: si en tu equipo no aparece, dilo en clase antes de seguir.

Si las dos cosas están bien, la instalación está terminada.

## Apartado 7. Si algo no funciona

| Qué ves | Qué suele ser | Qué hacer |
|---|---|---|
| *Test Connection* falla | El servidor está parado, o la contraseña no es la que apuntaste | Comprueba en los servicios de Windows que el de MySQL está en ejecución; revisa la contraseña |
| `ERROR 1046: No database selected` al ejecutar una consulta | No has elegido con qué base de datos trabajar en esta sesión | Ejecuta `USE tiendadaw;` (o el nombre de la base que toque) y repite |
| `ERROR 1064: You have an error in your SQL syntax` | Un error de escritura en la sentencia | Lee el texto que sigue a `near`: señala el punto donde MySQL dejó de entender |
| La rejilla enseña 1.000 filas cuando esperabas más | El límite de filas de Workbench | **Query → Limit Rows** |
| No sabes qué versión tienes | — | La comprobación del apartado 6 |

## Para ampliar

- [Manual de referencia de MySQL 8.4 — instalación en Windows](https://dev.mysql.com/doc/refman/8.4/en/windows-installation.html) y [pasos de MySQL Configurator](https://dev.mysql.com/doc/refman/8.4/en/mysql-configurator-workflow-server.html).
- [Manual de referencia de MySQL 8.4 — instalación en macOS con paquetes nativos](https://dev.mysql.com/doc/refman/8.4/en/macos-installation-pkg.html).
- [Manual de MySQL Workbench](https://dev.mysql.com/doc/workbench/en/) — instalación, conexiones y editor de SQL.
- [Manual de referencia de MySQL 8.4 — modos SQL del servidor](https://dev.mysql.com/doc/refman/8.4/en/sql-mode.html).
