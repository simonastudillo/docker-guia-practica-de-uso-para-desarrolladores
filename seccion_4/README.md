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

---

## Docker Compose - Multi Container Apps
- Creamos el archivo `docker-compose.yaml`
- La extensión puede ser .yml o .yaml, ambas son válidas.
- Lo primero y lo más importante es definir la versión de docker-compose que vamos a usar
- Luego definimos los servicios que vamos a usar, es importante tener cuidado con los espacios y tabulaciones, ya que es un archivo de configuración en formato YAML.
- Dentro de services definimos cada uno de los servicios que vamos a usar, en este caso db donde indicaremos los parámetros de la imagen de postgres y pgAdmin donde indicaremos los parámetros de la imagen de pgadmin.
- En pgAdmin agregaremos el parámetro de depends_on para indicar que depende del servicio db, esto es importante para que pgAdmin no intente conectarse a la base de datos antes de que esta esté lista.
- [archivo docker-compose.yaml](./postgres-pgadmin/docker-compose.yaml)

---

## Correr, limpiar y otras consideraciones - Docker Compose
- Para ejecutar el archivo docker-compose.yaml usamos el comando:
```bash
docker-compose up -d
```
- Al ejecutar este comando se nos indica que el atributo "version" está obsoleto, por lo que podemos eliminarlo del archivo docker-compose.yaml y volver a ejecutar el comando.
- Al volver a ejecutar recibimos error "service "db" refers to undefined volume postgres-db: invalid compose project"
- Este error es porque el volumen ya está creado y para asignarlo al servicio db debemos indicarlo en el archivo docker-compose.yaml en la sección de volumes
```yaml
volumes:
  postgres-db:
```
- Esto creara un nuevo volumen llamado <nombre_del_proyecto>_postgres-db.

---

## Limpiar el docker compose y conectar volumen externo
- Para usar un volumen externo debemos indicar el parámetro external: true
```yaml
volumes:
  postgres-db:
    external: true
```
- Hay algunos cambios que podemos hacer sin necesidad de eliminar el volumen o los contenedores, pero algunos cambios si requieren eliminar los contenedores y volver a crearlos, para esto usamos el comando:
```bash
docker-compose down
```
- Luego eliminamos el volumen manualmente
- Por último, volvemos a ejecutar el comando docker-compose up -d y todo debería funcionar correctamente.

---

## Bind Volumes - Docker Compose
- Podemos usar bind volumes para montar un directorio de nuestro host en el contenedor
```yaml
volumes:
  - ./postgresdb:/var/lib/postgresql/data
```
- Para pgAdmin es posible que en linux nos da error de permisos
- Esto se soluciona con el comando:
```bash
sudo chown -R 5050:5050 ./pgadmin
```

---

## Multi-container app - Base de datos Mongo
- Instalaremos mongo, mongo-express y aplicación de ejemplo en nodejs
- Se recomienda ir paso a paso, primero instalar mongo, luego mongo-express y por último la aplicación de ejemplo en nodejs.
- De esta forma evitamos errores de conexión y podemos ir probando cada uno de los servicios por separado.

---

## Variables de entorno - MongoDB
- Por defecto docker toma las variables de entorno del archivo .env
- Podemos enviar comandos para levantar mongo con autenticación, esto lo hacemos agregando `command: [ '--auth' ]`
- No se recomienda dejar las variables de entorno en el archivo `docker-compose.yaml`, ya que esto puede ser un riesgo de seguridad, es mejor usar el archivo .env para esto.
- Para llamar a las variables de entorno desde el archivo .env usamos la sintaxis `${VARIABLE}` en el archivo `docker-compose.yaml`

---

## Multi-container app - Visor de Base de datos
- Trabajaremos con monogo-express, que es un visor de base de datos para mongo
- Modificaremos el docker-compose.yaml para agregar el servicio de mongo-express, este servicio depende del servicio de mongo, por lo que agregamos el parámetro `depends_on: - mongo`
- Por seguridad se recomienda mapear la menor cantidad de puertos posibles, por lo que en este caso solo mapearemos el puerto 8081 del host al puerto 8081 del contenedor de mongo-express.