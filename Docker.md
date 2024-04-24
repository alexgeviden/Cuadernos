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
- `docker logs--follow [nombre]` : Nos permite ver logs en tiempo real

## Creación de un Contenedor

Para crear y ejecutar un contenedor a partir de una imagen, utiliza el comando `docker run`. Por ejemplo:
docker run -d -p 8080:80 --name mi_contenedor nginx

Este comando ejecutará un contenedor a partir de la imagen de NGINX, exponiendo el puerto 80 del contenedor en el puerto 8080 del host.
# Dockerfile Template

```Dockerfile
# Selecciona la imagen base
FROM ubuntu:latest

# Información del mantenedor
LABEL maintainer="tu_nombre <tu_correo@ejemplo.com>"

# Actualiza el índice de paquetes e instala paquetes adicionales si es necesario
RUN apt-get update && \
    apt-get install -y \
    paquete1 \
    paquete2 \
    paquete3 \
    && rm -rf /var/lib/apt/lists/*

# Configuraciones adicionales, comandos de configuración, copia de archivos, etc.
...

# Define el directorio de trabajo dentro del contenedor
WORKDIR /app

# Copia los archivos locales al directorio de trabajo del contenedor
COPY . .

# Define el comando por defecto a ejecutar cuando el contenedor se inicia
CMD ["comando", "argumento"]

## Más Recursos

- [Docker Hub](https://hub.docker.com/): Registro público de imágenes Docker.

