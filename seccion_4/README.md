# Sección 4: Multi-container Apps - Docker Compose

## Introducción a la sección
- Docker compose es un archivo de configuración que permite definir y ejecutar aplicaciones multi-contenedor.
- Ejemplo: Si tenemos un contenedor para una db, una db de respaldo y un PHPMyAdmin, podemos definirlos en un solo archivo y ejecutarlos con un solo comando.

---

## Temas puntuales de la sección
- docker-compose.yml
- Crear servicios
- Volúmenes
- Bind
- Name
- Externos
- Imágenes con tags
- Comandos de ejecución al montar una imagen
- Puertos
- Manejo de variables de entorno
- Nombres de servicios y servidores
- Dependencias de otros servicios

---

## Laboratorio: Reforzamiento de lo aprendido
1. Crear un volumen para almacenar la información de la base de datos (docker COMANDO CREAR postgres-db)
2. Montar la imagen de postgres así
- OJO: No hay puerto publicado -p, lo que hará imposible acceder a la base de datos con TablePlus
```bash
MSYS_NO_PATHCONV=1 docker container run \
-d \
--name postgres-db \
-e POSTGRES_PASSWORD=123456 \
-v postgres-db:/PATH/DE/LA/BASE/DE/DATOS \
postgres:15.1
```
3. Tomar pgAdmin de aquí
```bash
MSYS_NO_PATHCONV=1 docker container run \
--name pgAdmin \
-e PGADMIN_DEFAULT_PASSWORD=123456 \
-e PGADMIN_DEFAULT_EMAIL=superman@google.com \
-dp 8080:80 \
dpage/pgadmin4:6.17
```
4. Ingresar a la web con las credenciales de superman
5. Intentar crear la conexión a la base de datos
   1. Click en Servers
   2. Click en Register > Server
   3. Colocar el nombre de: "SuperHeroesDB" (el nombre no importa)
   4. Ir a la pestaña de connection
   5. Colocar el hostname "postgres-db" (el mismo nombre que le dimos al contenedor)
   6. Username es "postgres" y el password: 123456
   7. Probar la conexión
6. Ohhh no!, no vemos la base de datos, se nos olvidó la red
7. Crear la red (docker network ALGO PARA CREAR postgres-net)
```bash
docker network create postgres-net
```
8. Asignar ambos contenedores a la red (docker container ALGO PARA LISTAR LOS CONTENEDORES)
```bash
docker network connect postgres-net postgres-db
docker network connect postgres-net pgAdmin
```
9. Conectar ambos contenedores
10. Intentar el paso 4. de nuevo.
- Si logra establecer la conexión, todo está correcto, proceder a crear una base de datos, schemas, tablas, insertar registros, lo que sea.
11. Saltar de felicidad

---

## Resolución del laboratorio
1. Creamos el volumen
```bash
docker volume create postgres-db
```
2. Montamos la imagen de postgres
```bash
MSYS_NO_PATHCONV=1 docker container run \
-d \
--name postgres-db \
-e POSTGRES_PASSWORD=123456 \
-v postgres-db:/var/lib/postgresql/data \
postgres:15.1
```
3. Tomamos pgAdmin de aquí
```bash
MSYS_NO_PATHCONV=1 docker container run \
--name pgAdmin \
-e PGADMIN_DEFAULT_PASSWORD=123456 \
-e PGADMIN_DEFAULT_EMAIL=superman@google.com \
-dp 8080:80 \
dpage/pgadmin4:6.17
```
4. Ingresamos a la web con las credenciales de superman
5. Intentamos crear la conexión a la base de datos en pgAdmin
6. Creamos la red
```bash
docker network create postgres-net
```
7. Asignar ambos contenedores a la red
```bash
docker container ls
```
8. Asignar ambos contenedores a la red
```bash
docker network connect postgres-net postgres-db
docker network connect postgres-net pgAdmin
```
9. Conectar ambos contenedores
10. Intentar el paso 4. de nuevo.