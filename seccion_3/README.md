# Volúmenes y Redes

## Introducción a la sección
- Veremos la forma de hacer persistentes los datos de nuestros contenedores
- Tomaremos la data que guarda nuestro contenedor y la guardaremos en volumenes
- Esto nos permite destruir el contenedor y mantener la data en el volumen, para luego crear un nuevo contenedor y volver a usar la data que guardamos en el volumen
- Tomaremos la imagen de PHPMyAdmin para unirla a un contenedor de MariaDB y así poder administrar la base de datos de forma visual
- Usaremos los mismos nombres de los contenedores como nombres de servidores

---

## Temas puntuales de la sección
- Terminal interactiva dentro del contenedor
- Aplicaciones con múltiples contenedores
- Redes
- Volúmenes
- Mapeo de directorios y relaciones
- Montar un servidor Apache con PHPMyAdmin junto a MariaDB
- Revisar el file system de alpine y node

---

## Ejercicio sin volúmenes - Montar Base de Datos
1. Montar la imagen de MariaDB con el tag jammy, publicar en el puerto 3306 del contenedor con el puerto 3306 de nuestro equipo, colocarle el nombre al contenedor de world-db (--name world-db) y definir las siguientes variables de entorno:

MARIADB_USER=example-user
MARIADB_PASSWORD=user-password
MARIADB_ROOT_PASSWORD=root-secret-password
MARIADB_DATABASE=world-db
2. Conectarse usando Table Plus a la base de datos con las credenciales del usuario (NO EL ROOT)
3. Conectarse a la base de datos world-db
4. Ejecutar el query de creación de tablas e inserción proporcionado. [Query](./seccion_3/tarea_1.sql)
5. Revisar que efectivamente tengamos la data
```bash
docker container run \
-e MARIADB_USER=example-user \
-e MARIADB_PASSWORD=user-password \
-e MARIADB_ROOT_PASSWORD=root-secret-password \
-e MARIADB_DATABASE=world-db \
-dp 3306:3306 \
--name world-db \
mariadb:jammy
```

---

## Tipos de volúmenes
- Hay 3 tipos de volúmenes
   - Named Volumes: Nosotros le damos un nombre al volumen y podemos referenciarlo luego
   - Bind Volumes: Se mapea un directorio de nuestro equipo a un directorio del contenedor, podemos referenciarlo luego
   - Anonymous Volumes: Docker asigna un nombre aleatorio al volumen, no es recomendable usarlo ya que no podemos referenciarlo luego
- Comandos:
   - `docker volume create <nombre_del_volumen>`: Crea un volumen con el nombre especificado
   - `docker volume ls`: Lista todos los volúmenes creados
   - `docker volume inspect <nombre_del_volumen>`: Muestra información del volumen especificado
   - `docker volume rm <nombre_del_volumen>`: Elimina el volumen especificado
   - `docker volume prune`: Elimina todos los volúmenes no utilizados
   - `docker container run -v <nombre_del_volumen>:<directorio_del_contenedor> <imagen>`: Monta el volumen en el contenedor
- `docker create world-db`: Crear una "carpeta" en el computador para guardar la información, esto resiste aunque borremos el contenedor, reinicios del computador, etc.
- Para utilizar el volumen creado lo podemos añadir al comando de creación del contenedor con la opción `--volume <nombre_del_volumen>:<ruta_del_contenedor>`, esto nos permite que la data que se guarde en el contenedor se guarde en el volumen y no se pierda aunque borremos el contenedor.
- Busca en el docker hub oficial para ver donde guarda la data cada imagen, por ejemplo en MariaDB guarda la data en `/var/lib/mysql`, por lo que si queremos que la data se guarde en un volumen debemos mapear el volumen a ese directorio.
```bash
docker container run \
-e MARIADB_USER=example-user \
-e MARIADB_PASSWORD=user-password \
-e MARIADB_ROOT_PASSWORD=root-secret-password \
-e MARIADB_DATABASE=world-db \
-dp 3306:3306 \
--name world-db \
--volume world-db:/var/lib/mysql \
mariadb:jammy
```
- Ejecutamos nuevamente el [script](./seccion_3/tarea_1.sql) para crear las tablas e insertar la data, luego borramos el contenedor y creamos uno nuevo con el mismo volumen, al conectarnos a la base de datos veremos que la data sigue ahí.

---

## PHPMyAdmin
- PHPMyAdmin es un servicio muy popular para administrar bases de datos MySQL y MariaDB de forma visual
- Vamos a hub.docker.com y buscamos la imagen oficial de PHPMyAdmin, la cual es `phpmyadmin/phpmyadmin`
- Usaremos la versión `5.2-apache` para que sea compatible con la versión de MariaDB que estamos usando
- Aunque en documentaciones se haga referencia al parámetro `--link`, este está obsoleto y no se recomienda su uso, en su lugar usaremos redes para que los contenedores puedan comunicarse entre sí.
```bash
docker container run \
--name phpmyadmin \
-dp 8080:80 \
-e PMA_ARBITRARY=1 \
phpmyadmin:5.2-apache
```
- Solo si 2 contenedores están en la misma red, pueden comunicarse entre sí
- Esto se hace usando el comando `network`

---

## Redes de contenedores
- [Documentación oficial de redes de Docker](https://docs.docker.com/engine/network/tutorials/standalone/)
- Crearemos nuestra propia red llamada `world-app`
```bash
docker network create world-app
```
- Para conectar un contenedor a una red, usamos el parámetro `--network <nombre_de_la_red>` al crear el contenedor
- Otra forma de conectar un contenedor a una red es usando el comando `docker network connect <nombre_de_la_red> <nombre_del_contenedor>`
```bash
docker network connect world-app phpmyadmin
```
- Ahora conectamos tambien el contenedor de MariaDB a la red `world-app`
```bash
docker network connect world-app world-db
```
- Podemos verificar la red usando el comando `inspect` 
```bash
docker network inspect world-app
```
- Ahora nos conectamos a PHPMyAdmin desde el navegador usando la dirección `http://localhost:8080`
- Para las credenciales de conexión, usamos el nombre del contenedor de MariaDB como servidor, en este caso `world-db`, y el usuario y contraseña que definimos al crear el contenedor de MariaDB.

---

## Asignar la red desde la inicialización
- Podemos asignar la red a los contenedores desde el inicio usando el parámetro `--network <nombre_de_la_red>` al crear los contenedores
```bash
docker container run \
-e MARIADB_USER=example-user \
-e MARIADB_PASSWORD=user-password \
-e MARIADB_ROOT_PASSWORD=root-secret-password \
-e MARIADB_DATABASE=world-db \
-dp 3306:3306 \
--name world-db \
--volume world-db:/var/lib/mysql \
--network world-app \
mariadb:jammy
```
```bash
docker container run \
--name phpmyadmin \
-dp 8080:80 \
-e PMA_ARBITRARY=1 \
--network world-app \
phpmyadmin:5.2-apache
```
- Más adelante veremos docker compose, que nos permite crear múltiples contenedores y redes de forma más sencilla y rápida.
- Adicionalmente nos permite ver de manera visual los contenedores, redes y volúmenes que tenemos creados, así como sus relaciones.

---

## Bind Volumes
- Los bind volumes nos permiten mapear un directorio de nuestro equipo a un directorio del contenedor
- Tambien es posible entrar a la terminal interactiva del contenedor
- Desde el contenedor haremos un pnpm install, esto se verá reflejado en nuestro equipo
- El problema de esto es que no es tan rápido como hacerlo localmente, además de usar más recursos de nuestro equipo.

---

## Ejercicio - Bind Volumes
- Bajamos el [repositorio](https://import.cdn.thinkific.com/643563/courses/2100309/nestgraphqlapp-221207-123302.zip) con un proyecto de NestJS y GraphQL
- Descomprimimos el proyecto y nos ubicamos en la carpeta del proyecto
- Usaremos una imagen de NodeJS en la versión `18.20.8-alpine3.21` para crear un contenedor con el proyecto
```bash
MSYS_NO_PATHCONV=1 docker container run \
--name nest-app \
-w /app \
-p 3000:3000 \
-v "$(pwd)":/app \
node:18.20.8-alpine3.21 \
sh -c "yarn install && yarn start:dev"
```
- El comando `-w` nos permite definir el directorio de trabajo dentro del contenedor, en este caso `/app`
- Para GitBash en Windows, debemos usar `//app` en lugar de `/app` para definir el directorio de trabajo
- El comando `-v` nos permite mapear un directorio de nuestro equipo a un directorio del contenedor, en este caso estamos mapeando el directorio actual de nuestro equipo (usando `$(pwd)`) al directorio `/app` del contenedor
- El comando `sh -c` nos permite ejecutar múltiples comandos dentro del contenedor, en este caso estamos ejecutando `yarn install` para instalar las dependencias y luego `yarn start:dev` para iniciar el servidor de desarrollo

>[!INFO] Nota: Si estás usando Windows, es posible que debas usar `MSYS_NO_PATHCONV=1` antes del comando `docker container run` para evitar problemas con la conversión de rutas generadas por GitBash.

---

## Probar el enlace de directorios
- Volvemos a ejecutar el comando para crear el contenedor, ahora será más rápido ya que las dependencias ya están instaladas
```bash
MSYS_NO_PATHCONV=1 docker container run \
--name nest-app \
-w /app \
-dp 3000:3000 \
-v "$(pwd)":/app \
node:18.20.8-alpine3.21 \
sh -c "yarn install && yarn start:dev"
```
- Ahora podemos modificar el código del proyecto en nuestro equipo y ver los cambios reflejados en el contenedor, ya que estamos usando un bind volume para mapear el directorio del proyecto a un directorio del contenedor.
- Para ejecutar desde PowerShell, ejecuta el comando de la siguiente forma
```powershell
docker container run --name nest-app -w /app -dp 3000:3000 -v "${PWD}:/app" node:18.20.8-alpine3.21 sh -c "yarn install && yarn start:dev"
```
- El directorio debe estar completo entre comillas dobles, y la ruta del directorio debe ser `${PWD}` en lugar de `$(pwd)`


---

## Terminal interactiva -it
- Para entrar a la terminal interactiva de un contenedor, usamos el comando:
```bash
MSYS_NO_PATHCONV=1 docker container exec -it <nombre_del_contenedor> /bin/sh
```
- Podemos usar los mismos comandos que usaríamos en una terminal normal, como `ls`, `cd`, `cat`, etc.
- Para editar un archivo dentro del contenedor, podemos usar el comando `vi <nombre_del_archivo>`, aunque no es tan cómodo como usar un editor de texto en nuestro equipo.

>[!TIP] Debido a problemas conocidos de Docker con Windows para notificar sobre la modifación de archivos, se recomienda añadir las siguientes lineas al archivo de tsconfig.json
```json
"watchOptions": {
   "watchFile": "dynamicPriorityPolling",
   "interval": 1000
}
```