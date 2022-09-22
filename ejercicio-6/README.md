# Trabajo practico 6

## Conceptos de Dockerfiles

Docker puede crear imágenes automáticamente leyendo las instrucciones de un archivo `Dockerfile`. Un `Dockerfile` es un documento de texto que contiene todos los comandos que un usuario podría llamar en la línea de comandos para ensamblar una imagen. Los usuarios de `docker build` pueden crear una compilación automatizada que ejecuta varias instrucciones de la línea de comandos en sucesión.

### Descripcion de Instrucciones

- FROM

Se utiliza de la siguiente manera: `FROM [--platform=<platform>] <image> [AS <name>]` - La instrucción `FROM` inicializa una nueva etapa de construcción y **establece la imagen base para las instrucciones posteriores**. Como tal, un `Dockerfile` válido debe comenzar con una instrucción `FROM`. La imagen puede ser cualquier imagen válida; es especialmente fácil comenzar extrayendo una imagen de los repositorios públicos.

*El indicador opcional `--platform` se puede usar para especificar la plataforma de la imagen en caso de que FROM haga referencia a una imagen multiplataforma.*


- RUN

La instruccion `RUN` tiene 2 formas:

--> `RUN` <comando> (shell form)


--> `RUN` ["executable", "param1", "param2"] (formulario exec)


La instrucción `RUN` **ejecutará cualquier comando** en una nueva capa **encima de la imagen actual y confirmará los resultados**. La imagen confirmada resultante se usará para el siguiente paso en el Dockerfile.

La superposición de instrucciones `RUN` y la generación de confirmaciones se ajustan a los conceptos básicos de Docker, donde las confirmaciones son económicas y los contenedores se pueden crear desde cualquier punto del historial de una imagen, al igual que el control de código fuente.

- ADD


- COPY


- EXPOSE


- CMD


- ENTRYPOINT