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
```

```bash
docker build -t nombre_de_la_imagen .
```
## Guía para usar Docker Compose

### 1. Instalación de Docker Compose

Antes de usar Docker Compose, asegúrate de tenerlo instalado en tu sistema. Puedes descargar la versión adecuada para tu sistema operativo desde [https://docs.docker.com/compose/install/](https://docs.docker.com/compose/install/) y seguir las instrucciones de instalación.

### 2. Crear un archivo docker-compose.yml

El archivo `docker-compose.yml` es un archivo YAML que define la configuración de tus servicios Docker. Aquí hay un ejemplo básico de un archivo `docker-compose.yml`:

```yaml
version: '3'
services:
  servicio1:
    image: imagen_servicio1:tag
    ports:
      - "puerto_host:puerto_contenedor"
    volumes:
      - ./datos:/datos
  servicio2:
    image: imagen_servicio2:tag
    environment:
      - VARIABLE_DE_ENTORNO=valor
```
En este ejemplo, definimos dos servicios: web y db. El servicio web utiliza la imagen de NGINX y se expone en el puerto 8080 del host. El servicio db utiliza la imagen de MySQL y configura una contraseña de root.

## Comandos Básicos de Docker Compose

- `docker-compose up`: Inicia los contenedores definidos en el archivo docker-compose.yml.
- `docker-compose down`: Detiene y elimina los contenedores definidos en el archivo docker-compose.yml.
- `docker-compose ps`: Muestra el estado de los servicios definidos en el archivo docker-compose.yml.
- `docker-compose logs`: Muestra los logs de los servicios definidos en el archivo docker-compose.yml.
- `docker-compose exec [service] [command]`: Ejecuta un comando en un servicio específico.

## Ejecución de Docker Compose

Para ejecutar tu aplicación usando Docker Compose, sigue estos pasos:

1. Crea un archivo docker-compose.yml con la configuración de tus servicios.
2. Abre una terminal en la misma ubicación que tu archivo docker-compose.yml.
3. Ejecuta el comando docker-compose up para iniciar tus servicios.
4. Una vez que hayas terminado, puedes detener y eliminar los contenedores usando docker-compose down.
# Volúmenes en Docker Compose

Los volúmenes en Docker Compose permiten persistir datos fuera del ciclo de vida de los contenedores, lo que los hace ideales para compartir datos entre contenedores, realizar copias de seguridad y garantizar la persistencia de datos incluso cuando los contenedores se detienen o eliminan.

## Definición de Volúmenes en docker-compose.yml

Puedes definir volúmenes en tu archivo `docker-compose.yml` utilizando la sección `volumes`. Aquí hay un ejemplo básico de cómo hacerlo:

```yaml
version: '3.8'

services:
  db:
    image: mysql:latest
    volumes:
      - mysql_data:/var/lib/mysql

volumes:
  mysql_data:
```
En este ejemplo, hemos definido un volumen llamado `mysql_data` que se monta en el directorio `/var/lib/mysql` dentro del contenedor del servicio `db`. Esto asegura que los datos de MySQL persistan incluso si el contenedor se detiene o se elimina.

## Uso de Volúmenes en Servicios

Una vez que hayas definido un volumen en tu archivo `docker-compose.yml`, puedes montarlo en tus servicios como se muestra en el ejemplo anterior. Simplemente agrega la clave `volumes` bajo tu servicio y lista el volumen que deseas utilizar.

## Creación de Volúmenes Persistentes

Docker Compose automáticamente crea volúmenes persistentes cuando usas la sección `volumes` en tu archivo `docker-compose.yml` como se muestra en el ejemplo anterior. Estos volúmenes persisten incluso cuando los contenedores se detienen o se eliminan.


