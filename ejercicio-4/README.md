# Trabajo practico 4 - Arquitectura de Microservicios

## Desarrollo

En este trabajo practico se desarrollara una pequena arquitectura absada en **Microservicios**.

Una arquitectura de microservicios consta de una **colección de servicios autónomos y pequeños** Los servicios son independientes entre sí y cada uno debe implementar una funcionalidad de negocio individual.

Luego de clonar el repositorio indicado y correr docker-compose, accedemos a http://localhost/ y tenemos nuestra pagina web con arquitectura de microservicios

```
git clone https://github.com/microservices-demo/microservices-demo.git
docker-compose -f deploy/docker-compose/docker-compose.yml up -d
```

![](screenshots/tp4-1.png)

Creamos un usuario, agregamos datos de compra y simulamos la compra de un articulo

![](screenshots/tp4-2.png)

![](screenshots/tp4-3.png)

