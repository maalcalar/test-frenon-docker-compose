# TEST FRENON DOCKER COMPOSE

El proyecto es la resolución del test de FRENON para postular al puesto de Back End Developer.

## Instalación
Para poder utilizar el proyecto sólo es necesario ejecutar lo siguiente

```
docker-compose up -d
```

El comando anterior descargará las imágenes de docker necesarias y levantará el proyecto. Los contenedores son los siguientes:

- "miguelalcalarios/test-frenon-node": Contiene el proyecto de backend
- "miguelalcalarios/test-frenon-postgres": Contiene la base de datos en postgres con las modificaciones necesarias para el proyecto. 
- "redis": Contiene la base de datos de caché Redis

## Backend

Se puede ver el código del backend en https://github.com/maalcalar/test-frenon

## Pruebas

GET     localhost:3000/api/usuario/listar/:limit
GET     localhost:3000/api/usuario/:id
POST    localhost:3000/api/usuario
PUT     localhost:3000/api/usuario/:id
DELETE  localhost:3000/api/usuario/:id