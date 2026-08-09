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