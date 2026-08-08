# Sección 1: Introducción

## Introducción
- El objetivo es aprender qué es Docker y cómo usarlo para el desarrollo de software.
- Enseñar a entrar en Docker, crear la facilidad para leer y entender los docker composer y dockerfile.
- Seguir buenas prácticas
- Seguir la documentación
- Imagenes de múltiples arquitecturas

---

## ¿Cómo funcionará el curso?
- Realizar el curso en orden
- Realizar los ejercicios propuestos

---

## ¿Cómo hacer preguntas?
- Hacer click en discusiones, verificar si la pregunta ya fue realizada, si no, crear un nuevo tema y hacer la pregunta.
- Crear título específico

---

## Instalaciones necesarias
1. [Node JS](https://nodejs.org/es/) - Necesario para ejercicios, no para docker
2. [VS code](https://code.visualstudio.com/) - Editor de código
3. [Google Chrome](https://www.google.com/chrome/) - Navegador web
4. [Postman](https://www.postman.com/downloads/) - Para probar APIs
5. [Git](https://git-scm.com/downloads) - Para clonar repositorios
6. [Docker Desktop](https://www.docker.com/get-started) - Para ejecutar contenedores
7. [Table Plus](https://tableplus.com/) - Para conectarse a bases de datos
8. [Guía de atajos de docker](./docker-cheat-sheet.pdf) - Para tener a la mano los comandos más usados de docker
9. [Nginx configuration - Extension para VS code](https://marketplace.visualstudio.com/items?itemName=william-voyek.vscode-nginx) - Para tener resaltado de sintaxis en archivos de configuración de Nginx
10. [Activitus Bar - Extensión para VS code](https://marketplace.visualstudio.com/items?itemName=Gruntfuggly.activitusbar)
11. [Jsoin Viewer - Extensión para chrome](https://chrome.google.com/webstore/detail/json-viewer-pro/eifflpmocdbdmepbjaopkkhbfmdgijcc) - Para ver de manera más amigable los jsons en el navegador
12. [Aura themes - Extensión para VS code](https://marketplace.visualstudio.com/items?itemName=DaltonMenezes.aura-theme) - Para tener un tema oscuro en VS code
13. [Material Icon Theme - Extensión para VS code](https://marketplace.visualstudio.com/items?itemName=PKief.material-icon-theme) - Para tener íconos en VS code
14. [Wallpaper Developer](https://drive.google.com/drive/folders/1ItU8rbSGJjnh2USOBGwaCo9nYKifPJ6m?usp=sharing) - Para tener un fondo de pantalla de desarrollador

---

## Instalación Docker - Linux Ubuntu
- Leer guía oficial de instalación de Docker en Linux Ubuntu: [https://docs.docker.com/desktop/install/linux-install/](https://docs.docker.com/desktop/install/linux-install/)
- Se debe de realizar una limpieza de una posible instalación previa de Docker
- Ejecutar los comandos indicados en la terminal para instalar Docker Desktop en Linux Ubuntu
- Para probar que docker está instalado correctamente, ejecutar el comando
```bash
docker container run hello-world
``` 

---

## Instalación Docker - Linux manual
- Leer guía oficial de instalación de Docker en Linux: [https://docs.docker.com/engine/install/](https://docs.docker.com/engine/install/)
- Se debe de realizar una limpieza de una posible instalación previa de Docker
- Ejecutar los comandos indicados en la terminal para instalar Docker Engine en Linux
- Para probar que docker está instalado correctamente, ejecutar el comando
```bash
docker container run hello-world
```
- Revisar sección de "Linux post install" para poder ejecutar docker sin necesidad de usar sudo

---

## Guía de atajos para el curso
- [Guía de atajos de docker](./docker-cheat-sheet.pdf) - Para tener a la mano los comandos más usados de docker
- No es necesario memorizar todos los comandos, pero sí tenerlos a la mano para poder consultarlos cuando sea necesario.
- Hay 2 formas de llamar los comandos de docker:
```bash
docker container run ...
docker run ...
```