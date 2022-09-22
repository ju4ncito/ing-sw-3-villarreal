# Trabajo practico 6

## Conceptos de Dockerfiles

Docker puede crear imágenes automáticamente leyendo las instrucciones de un archivo `Dockerfile`. Un `Dockerfile` es un documento de texto que contiene todos los comandos que un usuario podría llamar en la línea de comandos para ensamblar una imagen. Los usuarios de `docker build` pueden crear una compilación automatizada que ejecuta varias instrucciones de la línea de comandos en sucesión.

### Descripcion de Instrucciones

- FROM

La instruccion `FORM` tiene la forma:

```
 `FROM [--platform=<platform>] <image> [AS <name>]` 
```

La instrucción `FROM` inicializa una nueva etapa de construcción y **establece la imagen base para las instrucciones posteriores**. Como tal, un `Dockerfile` válido debe comenzar con una instrucción `FROM`. La imagen puede ser cualquier imagen válida; es especialmente fácil comenzar extrayendo una imagen de los repositorios públicos.

*El indicador opcional `--platform` se puede usar para especificar la plataforma de la imagen en caso de que FROM haga referencia a una imagen multiplataforma.*


- RUN

La instruccion `RUN` tiene 2 formas:

--> `RUN` <comando> (shell form)


--> `RUN` ["executable", "param1", "param2"] (formulario exec)


La instrucción `RUN` **ejecutará cualquier comando** en una nueva capa **encima de la imagen actual y confirmará los resultados**. La imagen confirmada resultante se usará para el siguiente paso en el Dockerfile.

La superposición de instrucciones `RUN` y la generación de confirmaciones se ajustan a los conceptos básicos de Docker.

- ADD

La instruccion `ADD` cuenta con dos formas principales:

```
--> ADD [--chown=<user>:<group>] <src>... <dest>

--> ADD [--chown=<user>:<group>] ["<src>",... "<dest>"]
```

La instrucción `ADD` copia nuevos archivos, directorios o direcciones URL de archivos remotos desde <src> y los agrega al sistema de archivos de la imagen en la ruta <dest>.

Se pueden especificar varios recursos <src>, pero si son archivos o directorios, sus rutas se interpretan como relativas al origen del contexto de la compilación.


- COPY

La instruccion `COPY` cuenta con dos formas principales:

```
--> COPY [--chown=<user>:<group>] <src>... <dest>

--> COPY [--chown=<user>:<group>] ["<src>",... "<dest>"]
```

La instrucción `COPY` copia nuevos archivos o directorios desde <src> y los agrega al sistema de archivos del contenedor en la ruta <dest>.

Se pueden especificar varios recursos <src>, pero las rutas de los archivos y directorios se interpretarán como relativas al origen del contexto de la compilación.

- EXPOSE

La instruccion `EXPOSE` tiene la forma:

```
EXPOSE <port> [<port>/<protocol>...]
```

La instrucción `EXPOSE` informa a Docker que el contenedor escucha en los puertos de red especificados en tiempo de ejecución. Puede especificar si el puerto escucha en **TCP** o **UDP**, y el valor predeterminado es TCP si no se especifica el protocolo.

La instrucción `EXPOSE` en realidad no publica el puerto. Funciona como un tipo de **documentación entre la persona que construye la imagen y la persona que ejecuta el contenedor, sobre qué puertos se pretende publicar**. Para publicar realmente el puerto al ejecutar el contenedor, use el indicador `-p` en la ejecución de la ventana acoplable para publicar y asignar uno o más puertos, o el indicador `-P` para publicar todos los puertos expuestos y asignarlos a puertos de orden superior.

De forma predeterminada, `EXPOSE` asume TCP. También puede especificar UDP

- CMD

La instruccion `CMD` tiene 3 formas principales:

```
--> CMD ["executable","param1","param2"] (exec form, this is the preferred form)

--> CMD ["param1","param2"] (as default parameters to ENTRYPOINT)

--> CMD command param1 param2 (shell form)
```

El objetivo principal de un `CMD` es proporcionar valores predeterminados para un contenedor en ejecución. Estos valores predeterminados pueden incluir un ejecutable o pueden omitir el ejecutable, en cuyo caso también debe especificar una instrucción `ENTRYPOINT`.

Solo puede haber una instrucción `CMD` en un Dockerfile. Si incluye más de un `CMD`, solo tendrá efecto el último `CMD`.

Si se usa `CMD` para proporcionar argumentos predeterminados para la instrucción `ENTRYPOINT`, las instrucciones `CMD` y `ENTRYPOINT` deben especificarse con el formato de matriz JSON.

- ENTRYPOINT

La instruccion `ENTRYPOINT` tiene 2 formas:

```
--> ENTRYPOINT ["executable", "param1", "param2"] (forma exe, preferida)

--> ENTRYPOINT command param1 param2 (forma shell)
```

Un `ENTRYPOINT` le permite configurar un contenedor que se ejecutará como un ejecutable. Solo tendrá efecto la última instrucción `ENTRYPOINT` en el Dockerfile.