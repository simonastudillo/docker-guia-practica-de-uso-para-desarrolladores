# Bases de Docker

## Introducción a la sección
- Usaremos principalmente docker en terminal, ya que en el mundo laboral es más común usarlo de esta manera.
- Docker CLI es muy intuitivo y fácil de usar, por lo que no es necesario usar Docker Desktop.

---

## Temas puntuales de la sección
- Veremos ¿Qué es Docker?, ¿Por qué debo de aprenderlo? y ¿Para qué me puede servir?.
- Nuestros primeros comandos:
   - docker container:
      - run
      - remove
      - list
      - publish
      - environment variables
      - logs
      - detached
   - docker pull

---

## ¿Qué es Docker? y ¿Por qué debo saberlo?
- Antes de docker, cuando llegaba un nuevo desarrollador a un proyecto, tenía que instalar todas las dependencias necesarias para poder ejecutar el proyecto en su máquina local.
- Además necesitaba instalar versiones específicas de cada dependencia, ya que si no lo hacía, el proyecto podía no funcionar correctamente.
- Adicionalmente podría tener problemas de compatibilidad entre sistemas operativos, ya que un proyecto podría funcionar en Windows y no en Linux, o viceversa.
- Otro problema, es que si tenemos otro proyecto con otras versiones de dependencias, podría generar conflictos entre proyectos, ya que un proyecto podría requerir una versión de una dependencia y otro proyecto podría requerir otra versión de la misma dependencia.
- Maquinas virtuales son muy pesadas y lentas para ejecutar, ya que requieren de un sistema operativo completo para funcionar, además de que consumen muchos recursos de la máquina.
- Docker nos permite manejar imagenes, las imagenes es como una fotografía de una versión específica de una tecnología
- Sin importar el sistema operativo que tengamos, docker nos permite ejecutar contenedores con la misma imagen, por lo que no tendremos problemas de compatibilidad entre sistemas operativos.
- Cada contenedor está aislado entre sí
- Podemos ejecutar varias instancias a la vez de la misma o de diferentes imágenes
- Con 1 comando podemos descargar, levantar y correr todo lo que necesitamos
- Cada contenedor tiene todo lo necesario para ejecutar la aplicación, por lo que no necesitamos instalar nada en nuestra máquina local.
- Ejemplo
   - Necesitamos Mongo, Node, Express y Nest
   - Nos piden un deploy para ver avances del proyecto
   - Generamos nuestra imagen "build process"
   - La imagen contiene todo lo necesario para ejecutar la aplicación
   - La podemos subir a un repositorio de imágenes de Docker
   - Para desplegar esa imagen al servidor, el servidor hace un pull de la imagen y la ejecuta en un contenedor
   - Es muy sencillo cambiar de versión de la aplicación
   - Incluso si no utilizamos un servidor docker, nos sirve para mantener un entorno de desarrollo consistente entre todos los desarrolladores del proyecto

---

## Hola Mundo en Docker
- Comando `docker pull`: Nos permite descargar una imagen de Docker desde un repositorio de imágenes de Docker.
- Una `imagen` es un archivo construido por capas, que contiene todo lo necesario para ejecutar una aplicación, incluyendo el código fuente, las dependencias, las herramientas y las configuraciones necesarias.
- Al ejecutar el comando `docker pull hello-world`
   - Docker indica `docker.io/library/hello-world:latest`, esto indica que es la última imagen que se encuentra en el repositorio oficial de Docker Hub.
   - Indica que está descargando la imagen y genera un hash de la imagen descargada, que es un identificador único de la imagen.
- Para ejecutar la imagen ejecutamos `docker container run hello-world`
   - Indica que el `docker daemon` está corriendo: Esto significa que el servicio de Docker está corriendo en nuestra máquina local y está listo para recibir comandos.
- `Container` es una instancia de una imagen, es decir, es un proceso que se está ejecutando en nuestra máquina local y que está aislado del resto de procesos.
- Si volvemos a ejecutar el comando `docker pull hello-world`, nos indica que la imagen ya está descargada y no necesita descargarla nuevamente.
- `Docker Hub` es un repositorio de imágenes de Docker, donde podemos encontrar imágenes oficiales y de terceros para descargar y usar en nuestros proyectos.
- Se recomienda usar imágenes oficiales, ya que son mantenidas por los desarrolladores de la tecnología y son más seguras y confiables.
- Si ejecutamos el comando `docker container run hello-world` nuevamente, no descarga nada pero crea un nuevo contenedor a partir de la imagen descargada, esto toma más espacio en disco, por lo que es recomendable eliminar los contenedores que ya no necesitamos.

---

## Borrar contenedores e imágenes
- `docker container --help `: Nos muestra todos los comandos disponibles para manejar contenedores.
- `docker container ls`: Nos muestra todos los contenedores que están corriendo actualmente.
- `docker container ls -a`: Nos muestra todos los contenedores, incluyendo los que ya no están corriendo.
- `docker container rm <container_id>`: Nos permite eliminar un contenedor, donde `<container_id>` es el identificador del contenedor que queremos eliminar.
- `docker container prune`: Nos permite eliminar todos los contenedores que ya no están corriendo.
- `docker image --help`: Nos muestra todos los comandos disponibles para manejar imágenes.
- `docker image ls`: Nos muestra todas las imágenes que tenemos descargadas en nuestra máquina local.
- `docker image rm <image_id>`: Nos permite eliminar una imagen, donde `<image_id>` es el identificador de la imagen que queremos eliminar.

---

## Docker Desktop - Mismos comandos ejecutados
- Desde el Docker Desktop podemos ejecutar los mismos comandos que ejecutamos en la terminal, pero de manera visual.
- Buscamos la imagen en el buscador de imágenes, hacemos click en el botón de "pull" y esperamos a que se descargue la imagen.
- Si presionamos la imagen nos muestra los comandos utilizados para ejecutar la imagen, como `docker container run hello-world`.
- Antes de eliminar la imagen debemos eliminar todos los contenedores que se hayan creado a partir de esa imagen
- Podemos ir al listado de contenedores, seleccionar los contenedores que queremos eliminar y hacer click en el botón de "remove".
- Lo mismo para eliminar la imagen, seleccionamos la imagen que queremos eliminar y hacemos click en el botón de "remove".

---

## Publish and Detached modes
- Abrir navegador con `localhost` deberiamos ver un error ya que no hay ningún servicio corriendo
- Ejecutamos el comando `docker container run docker/getting-started`
- Abrimos el navegador con `localhost` la página sigue sin cargar
- Ejecutamos el comando `docker container run -d -p 80:80 docker/getting-started`
   - `-d` indica que el contenedor se ejecutará en modo "detached", es decir, en segundo plano.
   - `-p 80:80` indica que el puerto 80 del contenedor se mapeará al puerto 80 de nuestra máquina local
   - `80:80` el primer 80 es el puerto de nuestra máquina local y el segundo 80 es el puerto del contenedor
- Para detener la imagen ejecutamos el comando `docker container stop <container_id>`, donde `<container_id>` es el identificador del contenedor que queremos detener.
- `docker container start <container_id>`: Nos permite iniciar un contenedor que ya ha sido detenido.
- `docker container rm -f <container_id>`: Nos permite eliminar un contenedor que no ha sido detenido, el flag `-f` indica que queremos forzar la eliminación del contenedor.

---

## Variables de entorno
- Descargamos la imagen de postgres con el comando `docker pull postgres`
- En la documentación de [postgres](https://hub.docker.com/_/postgres) nos indican las variables de entorno que podemos usar para configurar la base de datos, como `POSTGRES_PASSWORD`, `POSTGRES_USER` y `POSTGRES_DB`.
- `docker run --name some-postgres -e POSTGRES_PASSWORD=mysecretpassword -d postgres`
   - `--name some-postgres` indica el nombre del contenedor que estamos creando, en este caso `some-postgres`.
   - `-e POSTGRES_PASSWORD=mysecretpassword` indica la variable de entorno `POSTGRES_PASSWORD` con el valor `mysecretpassword`.
   - `-d` indica que el contenedor se ejecutará en modo "detached", es decir, en segundo plano.

---

## Usar la imagen de Postgres
- Por defecto Postgres no permite conexiones externas, por lo que debemos mapear el puerto 5432 del contenedor al puerto 5432 de nuestra máquina local con el flag `-p 5432:5432`.
- `docker run --name some-postgres -dp 5432:5432 -e POSTGRES_PASSWORD=mysecretpassword postgres`: Ahora asignamos el puerto 5432 del contenedor al puerto 5432 de nuestra máquina local, por lo que podemos conectarnos a la base de datos desde nuestra máquina local.

---

## Multiples instancias de Postgres
- Para ejecutar un comando en la terminal usando múltiples lineas usamos el caracter `\` al final de cada linea, excepto en la última linea.
```bash
docker container run \
--name postgres-alpha \
-e POSTGRES_PASSWORD=mysecretpassword \
-dp 5432:5432 \
postgres
```
- Usaremos una versión más antigua de postgres para crear una segunda instancia de postgres:
```bash
docker container run \
--name postgres-beta \
-e POSTGRES_PASSWORD=mysecretpassword \
-dp 5433:5432 \
postgres:14-alpine3.17
```
>[!WARNING]
> No cambies el puerto del contenedor, ya que Postgres solo escucha en el puerto 5432, si cambias el puerto del contenedor, no podrás conectarte a la base de datos.