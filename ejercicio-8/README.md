# Trabajo practico 8 

## Herramientas de construcción de software en la nube

Comparando con herramientas como Jenkins, que necesitan un servidor de mantenimiento continuo y con una buena configuracion, Github Actions proporciona corredores gratuitos que se pueden usar para realizar operaciones de CI/CD. Estos corredores son propiedad y están mantenidos por Github, pero también se pueden agregar corredores autohospedados. 

Una desventaja de usar Github Actions podria ser el estar atado a Github como sistema de gestión de código. El uso de Jenkins permite almacenar código en cualquier repositorio: en Github, Gitlab, BitBucket, etc.

### Configurando GitHub Actions

En primer lugar, nos dirigimos a nuestro repositorio con *spring* que utilizamos en el ejercicio anterior

https://github.com/ju4ncito/springboot-tp7

creamos un nuevo workflow

![](screenshots/tp8-1.png)


Definimos nuestro file 

```
# This is a basic workflow to help you get started with Actions

name: CI

# Controls when the workflow will run
on:
  # Triggers the workflow on push or pull request events but only for the master branch
  push:
    paths:
    - './**'
    branches: [ main ]
  pull_request:
    paths:
    - './**'  
    branches: [ main ]

  # Allows you to run this workflow manually from the Actions tab
  workflow_dispatch:

# A workflow run is made up of one or more jobs that can run sequentially or in parallel
jobs:
  # This workflow contains a single job called "build"
  build:
    # The type of runner that the job will run on
    runs-on: ubuntu-latest

    # Steps represent a sequence of tasks that will be executed as part of the job
    steps:
      # Checks-out your repository under $GITHUB_WORKSPACE, so your job can access it
      - uses: actions/checkout@v2

      # Install Java JDK with maven
      - name: Set up JDK 8
        uses: actions/setup-java@v2
        with:
          java-version: '8'
          distribution: 'adopt'
          cache: maven
          
      # Compile the application
      - name: Build with Maven
        run: |
          mvn -B package --file pom.xml
```

![](screenshots/tp8-2.png)

