# Guía de Docker

Docker es una plataforma de código abierto que permite a los desarrolladores empaquetar, distribuir y ejecutar aplicaciones en contenedores. Los contenedores son entornos ligeros y portátiles que incluyen todo lo necesario para ejecutar una aplicación, incluidas bibliotecas, herramientas y código.

## Instalación de Docker

Para instalar Docker en tu sistema, sigue estos pasos:

**Windows:**
   - Descarga e instala Docker Desktop desde [la página oficial de Docker](https://www.docker.com/products/docker-desktop).

## Comandos Básicos de Docker

- `docker version`: Muestra la versión de Docker instalada.
- `docker info`: Muestra información detallada sobre la instalación de Docker.
- `docker run [opciones] [imagen] [comando] [argumentos]`: Ejecuta un contenedor a partir de una imagen.
- `docker pull [imagen]`: Descarga una imagen desde Docker Hub u otro registro de imágenes.
- `docker ps`: Muestra los contenedores en ejecución.
- `docker ps -a`: Muestra todos los contenedores (en ejecución y detenidos).
- `docker images`: Muestra las imágenes descargadas/localmente disponibles.
- `docker stop [contenedor]`: Detiene un contenedor en ejecución.
- `docker rm [contenedor]`: Elimina un contenedor.
- `docker rmi [imagen]`: Elimina una imagen.

## Creación de un Contenedor

Para crear y ejecutar un contenedor a partir de una imagen, utiliza el comando `docker run`. Por ejemplo:
docker run -d -p 8080:80 --name mi_contenedor nginx

Este comando ejecutará un contenedor a partir de la imagen de NGINX, exponiendo el puerto 80 del contenedor en el puerto 8080 del host.

## Más Recursos

- [Docker Hub](https://hub.docker.com/): Registro público de imágenes Docker.

