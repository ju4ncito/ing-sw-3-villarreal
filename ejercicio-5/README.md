# Trabajo practico 5 - Herramientas de Construccion de Software

## Desarrollo

En primer lugar, configraumos todas las las variables de entorno necesarias luego de instalar JDK y Maven. A continuacion, una explicacion mas detallada sobre el ultimo.


## MAVEN

### 1. Qué es Maven?

Apache Maven es una herramienta de codigo abierto y gratuita, enfocada a la gestion de proyectos. Es utilizado principalmente para:

- Gestion de dependencias
- Herramienta de Compilacion
- Herramienta de Documentacion

El objetivo principal de Maven es permitir que un desarrollador comprenda el estado completo de un esfuerzo de desarrollo en el menor tiempo posible. Para lograr este objetivo, Maven se ocupa de varias áreas de preocupación:

- Facilitando el proceso de construcción
- Proporcionar un sistema de construcción uniforme
- Proporcionar información de proyectos de calidad.
- Fomentar mejores prácticas de desarrollo




### 2. Qué es el archivo POM?

POM hace referncia a "Project Object Model" (modelo de objetos del proyecto). Es una representación XML de un proyecto Maven contenido en un archivo llamado ``pom.xml``. 

El POM contiene toda la información necesaria sobre un proyecto, así como configuraciones de complementos que se utilizarán durante el proceso de construcción. Es una ventanilla única para todo lo relacionado con el proyecto. De hecho, en el mundo de Maven, un proyecto no necesita contener ningún código, simplemente un archivo ```pom.xml```.


```
<proyecto xmlns = "http://maven.apache.org/POM/4.0.0" xmlns:xsi = "http://www.w3.org/2001/XMLSchema-instance"  
  xsi:schemaLocation = "http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd" >
  <versión del modelo> 4.0.0 </versión del modelo>
 
  <groupId> org.codehaus.mojo </groupId>
  <artifactId> mi-proyecto </artifactId>
  <versión> 1.0 </versión>
</proyecto>
```
POM minimo indispensable permitido por MAVEN.

- modelVersion - contiene la version del modelo del POM

- groupId - Define el dominio, el proyecto real al que pertenece el proyecto MAVEN actual, suele ser unico por organizacion/proyecto. Cada ```artifact``` tiene un ```groupID```

- artifactId - Define un modulo MAVEN. Un artifact puede ser un JAR. Salida generada despues de construir un proyecto MAVEN.

- versionId - especifica la version del artifactId

### Repositorios locales, centrales y remotos

Un **repositorio** en MAVEN contiene artefactos de compilación y dependencias de diferentes tipos.

Existen 2 tipos de repositorios:

- **Locales** : el repositorio local es un directorio en la computadora donde se ejecuta Maven. Almacena en caché las descargas remotas y contiene artefactos de compilación temporales que aún no ha publicado.

- **Remotos** : Los repositorios remotos se refieren a cualquier otro tipo de repositorio, al que se accede mediante una variedad de protocolos como ```file://``` y ```https://```. Pueden ser un repositorio verdaderamente remoto configurado por un tercero para proporcionar sus artefactos para descargar (por ejemplo, repo.maven.apache.org) o pueden ser repositorios internos configurados en un servidor de archivos o HTTP dentro de su empresa.

### Ciclos de vida de BUILD

Hay tres ciclos de vida de compilación integrados: ```default```, ``clean`` y ``site``. El ciclo de vida DEFAULT maneja la implementación de un proyecto, el ciclo de vida CLEAN maneja la limpieza del proyecto, mientras que el ciclo de vida SITE maneja la creación del sitio web de su proyecto.

### Ejemplo de construccion de MVN 

Creamos a un archivo pom.xml lo siguiente:

```
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0
                      http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>ar.edu.ucc</groupId>
    <artifactId>proyecto-01</artifactId>
    <version>0.1-SNAPSHOT</version>
</project>
```

y ejecutamos 

```
mvn clean install
```

Como ejecutamos *mvn ***CLEAN*** install*, la instalacion comienza desde un estado limpio.

Se instalan archivos jar desde el repositorio central y se crea el proyecto MAVEN (con su .jar ejecutable)

## Maven continuacion

```
-------------------------------------------------------
 T E S T S
-------------------------------------------------------
Running ar.edu.ucc.AppTest
Tests run: 1, Failures: 0, Errors: 0, Skipped: 0, Time elapsed: 0.006 sec

Results :

Tests run: 1, Failures: 0, Errors: 0, Skipped: 0

[INFO] 
[INFO] --- maven-jar-plugin:2.4:jar (default-jar) @ ejemplo ---
[INFO] Building jar: /home/juan/ucc/ing-sw-3-villarreal/ejercicio-5/MAVEN/maven-cont/ejemplo/target/ejemplo-1.0-SNAPSHOT.jar
[INFO] ------------------------------------------------------------------------
[INFO] BUILD SUCCESS
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  4.076 s
[INFO] Finished at: 2022-09-08T16:04:21-03:00
[INFO] ------------------------------------------------------------------------
```
```
juan@juannet:~/ucc/ing-sw-3-villarreal/ejercicio-5/MAVEN/maven-cont/ejemplo$ java -cp target/ejemplo-1.0-SNAPSHOT.jar ar.edu.ucc.App
Hello World!
```


## Manejo de dependencias

Una vez creado nuestro proyecto, cuando compilamos obtenemos el siguiente error:

```
[INFO] BUILD FAILURE
[INFO] ------------------------------------------------------------------------
[INFO] Total time:  0.835 s
[INFO] Finished at: 2022-09-08T16:20:59-03:00
[INFO] ------------------------------------------------------------------------
[ERROR] Failed to execute goal org.apache.maven.plugins:maven-compiler-plugin:3.1:compile (default-compile) on project ejemplo-uber-jar: Compilation failure: Compilation failure: 
[ERROR] Source option 5 is no longer supported. Use 6 or later.
[ERROR] Target option 1.5 is no longer supported. Use 1.6 or later.
[ERROR] -> [Help 1]
```

Por lo tanto, debemos agregar la dependencia necesaria

```
    <dependency>
      <groupId>ch.qos.logback</groupId>
      <artifactId>logback-classic</artifactId>
      <version>1.2.1</version>
    </dependency>
```