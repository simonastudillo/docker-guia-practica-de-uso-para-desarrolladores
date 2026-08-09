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