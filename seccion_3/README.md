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