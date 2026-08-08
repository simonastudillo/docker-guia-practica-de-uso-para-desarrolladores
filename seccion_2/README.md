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