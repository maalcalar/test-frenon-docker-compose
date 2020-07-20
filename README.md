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

La API consta de 5 funciones las cuales se muestran debajo

### **Listado de usuarios**

La siguiente ruta listará los usuarios de la tabla de usuario y espera el parámetro "limit" como un parámetro numérico para poder entregar esa cantidad de usuarios.
```
GET     localhost:3000/api/usuario/listar/:limit
```

### **Ver usuario**

La siguiente ruta entregará los datos de un usuario y para poder extraerlo espera el parámetro "id".
```
GET     localhost:3000/api/usuario/:id
```

### **Crear usuario**

Esta ruta creará un usuario.
```
POST    localhost:3000/api/usuario
```

Para poder crear el nuevo espera en el cuerpo la siguiente estructura en JSON.
```
{
    "nombre": Miguel,
    "apellido": Alcalá,
    "email": miguel.alcala.rios@gmail.com
}
```

### **Actualizar usuario**

Esta ruta servirá para poder actualizar a un usuario y espera el parámetro "id" para actualizar el usuario indicado por ese id.
```
PUT     localhost:3000/api/usuario/:id
```

Esta consulta espera tener en el cuerpo la misma estructura utilizada en la ruta de creación.
```
{
    "nombre": Miguel,
    "apellido": Alcalá,
    "email": miguel.alcala.rios@gmail.com
}
```

### **Eliminar usuario**

La siguiente ruta servirá para eliminar un usuario de la tabla de usuario y espera el parámetro "id".
```
DELETE  localhost:3000/api/usuario/:id
```

## Arquitectura

Para la arquitectura se usaron servicios, un enrutador, un controlador, y un middleware.

- Los servicios se encargarán de poner en línea al servidor y conectarlo con las bases de datos.
    - El servicio del servidor creará el servidor con express, extraerá las rutas de usuario y las definirá como api y configurará al servidor para esperar cuerpos en json.
- El enrutador definirá las rutas para el api de usuario y definirá el controlador y el middleware que se encargará de procesar cada ruta.
- El middleware tiene un único uso para la ruta de leer un usuario y su función es buscar en Redis si es que ya se había consultado anteriormente al usuario y extraerá los datos para servirlo antes de consultar a la base de datos de Postgres.

## Patrones de diseño

Utilicé el patrón de diseño Singleton para los servicios de base datos, una vez conectada a las base de datos se sirve un único objeto de conexión "Pool" en el caso de Postgres y "Client" para Redis.