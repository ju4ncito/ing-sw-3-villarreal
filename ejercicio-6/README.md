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

Un `ENTRYPOINT` permite configurar un contenedor que se ejecutará como un ejecutable. Solo tendrá efecto la última instrucción `ENTRYPOINT` en el Dockerfile.

## Generar imagen Docker

Seguimos los siguientes pasos para generar nuestra imagen de Docker

```
mvn clean package spring-boot:repackage  

docker build -t test-spring-boot .

docker run -p 8080:8080 test-spring-boot
```

y configuramos nuestro archivo `dockerfile`:

```
FROM openjdk:8-jre-alpine

RUN apk add --no-cache bash

WORKDIR /app

COPY target/*.jar ./spring-boot-application.jar 

ENV JAVA_OPTS="-Xms32m -Xmx128m"
EXPOSE 8080

ENTRYPOINT exec java $JAVA_OPTS -Djava.security.egd=file:/dev/./urandom -jar spring-boot-application.jar
```

y obtenemos lo siguiente:

```
juan@juannet:~/ucc/ingenieria-sw-3/ing-sw-3-villarreal/ejercicio-6/spring-boot$ docker run -p 8080:8080 test-spring-boot

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.0.2.RELEASE)

2022-09-22 17:48:22.792  INFO 1 --- [           main] s.actuator.SampleActuatorApplication     : Starting SampleActuatorApplication v2.0.2 on 23a08da16134 with PID 1 (/app/spring-boot-application.jar started by root in /app)
2022-09-22 17:48:22.796  INFO 1 --- [           main] s.actuator.SampleActuatorApplication     : No active profile set, falling back to default profiles: default

(...)
```
Controlamos si retorna un mensaje

```
juan@juannet:~/ucc/ingenieria-sw-3/ing-sw-3-villarreal$ curl -v localhost:8080
*   Trying 127.0.0.1:8080...
* Connected to localhost (127.0.0.1) port 8080 (#0)
> GET / HTTP/1.1
> Host: localhost:8080
> User-Agent: curl/7.81.0
> Accept: */*
> 
* Mark bundle as not supporting multiuse
< HTTP/1.1 200 
< Content-Type: application/json;charset=UTF-8
< Transfer-Encoding: chunked
< Date: Thu, 22 Sep 2022 17:52:33 GMT
< 
* Connection #0 to host localhost left intact
{"message":"Spring boot says hello from a Docker container"}
```
Funciona!

## Dockerfiles multi-etapas

Se recomienda crear compilaciones de varias etapas para todas las aplicaciones (incluso las heredadas). Las compilaciones de múltiples etapas también son esenciales en organizaciones que emplean múltiples lenguajes de programación. La facilidad de crear una imagen de Docker por cualquier persona sin la necesidad de JDK / Node / Python / etc. no puede ser sobrestimado.

Modificamos nuestro `dockerfile` y corremos el nuevo build

```
docker build -t test-spring-boot .
```

Nuestro nuevo `dockerfile`:


```
FROM maven:3.5.2-jdk-8-alpine AS MAVEN_TOOL_CHAIN
COPY pom.xml /tmp/
RUN mvn -B dependency:go-offline -f /tmp/pom.xml -s /usr/share/maven/ref/settings-docker.xml

#Especica la imagen, le cambia el nombre, copia el archivo pom.xml dentro de tmp y ejecuta el comando de maven que resuelve las dependencias

COPY src /tmp/src/
WORKDIR /tmp/
RUN mvn -B -s /usr/share/maven/ref/settings-docker.xml package

#Copia el contenido de src en el contexto de la imagen (tmp/src) y especifica cual es el dir de trabajo.


FROM openjdk:8-jre-alpine

#Comienza otra seccion/etapa de compilacion, como vimos es posible utilizar multiples

EXPOSE 8080

RUN mkdir /app
COPY --from=MAVEN_TOOL_CHAIN /tmp/target/*.jar /app/spring-boot-application.jar

#Expone el puerto 8080, crea el directorio app/ y copia los ejecutables especificados de la imagen anterior

ENV JAVA_OPTS="-Xms32m -Xmx128m"

ENTRYPOINT exec java $JAVA_OPTS -Djava.security.egd=file:/dev/./urandom -jar /app/spring-boot-application.jar

HEALTHCHECK --interval=1m --timeout=3s CMD wget -q -T 3 -s http://localhost:8080/actuator/health/ || exit 1

```

La diferencia principal entre el ultimo dockerfile y el primero es que luego de ejecutar nuestro `build`, primero se **construye** la aplicacion y posteriormente se realiza la **creacion de imagen** utilizando los ejecutables obtenidos.

## Python flask

Copiamos el codigo y corremos

```
docker compose up -d
```

Obtenemos

```
[+] Running 4/4
 ⠿ Network python-flask_default      Created                                                                                                                                              0.1s
 ⠿ Volume "python-flask_redis_data"  Created                                                                                                                                              0.0s
 ⠿ Container python-flask-redis-1    Started                                                                                                                                              1.0s
 ⠿ Container python-flask-app-1      Started 
 ```

El elemento `build` se encuentra en el archivo `docker-compose`, en donde se definen las opciones de configuracion que se aplican al monento de la construccion de la imagen. 

Observando nuestro .yml, vemos que 

```
    build:
      context: ./
```

Esto significa que se parte desde el propio archivo de` docker-compose`. Cuando se indica un path relativo como este caso se parte la ubicación del archivo `docker-compose`, por lo tanto se utiliza el archivo de `Dockerfile`.


## Imagen para app web con nodeJS

Creamos nuestro `dockerfile`

```
FROM node:16.17.0-alpine
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app
RUN npx create-react-app my-app
RUN cd my-app
WORKDIR /usr/src/app/my-app
RUN npm install
CMD ["npm","start"]
EXPOSE 3000
```
Utilizando los comandos `docker build -t test-node .` y `docker run -p 3000:3000 test-node` construimos la imagen.

```
Successfully built f0fc00dfe9af
Successfully tagged test-node:latest
```

```
Compiling...
Compiled successfully!
webpack compiled successfully
```
## Publicando en dockerhub

```
juan@juannet:~/ucc/ingenieria-sw-3/ing-sw-3-villarreal/ejercicio-6$ docker tag test-node 0xsn4ke/test-node:latest
juan@juannet:~/ucc/ingenieria-sw-3/ing-sw-3-villarreal/ejercicio-6$ docker push 0xsn4ke/test-node:latest
The push refers to repository [docker.io/0xsn4ke/test-node]
6e04cabbbfcf: Pushed 
ea1615183ec1: Pushed 
071acd00eeda: Pushed 
f1ed0bba6314: Mounted from library/node 
2808ff9120f2: Mounted from library/node 
cb6eda6d73f0: Mounted from library/node 
994393dc58e7: Mounted from library/node 
latest: digest: sha256:8d2aadd6d729c4a9fd42fa85c040949f051dfbc88f7b2fb15d7527fa60a39371 size: 1788
```