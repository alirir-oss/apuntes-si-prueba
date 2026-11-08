---
title: 9. Tutorial Docker
weight: 9
---

## ¿Qué es Docker?

Docker se diferencia de las maquinas virtuales en que no reserva recursos del ordenador host, sino que hace uso del kernel y de los recursos que necesita en ese momento, por tanto nos permitirá levantar muchos mas contenedores en un mismo ordenador con menos recursos, ya que estos se irán liberando y ocupando, según demanda.

### Docker File

El docker file es un archivo que contiene toda la información de la imagen de docker. La imagen de docker contiene tanto las librerías, como el sistema operativo, así como el código de nuestra aplicación. Cuando nosotros queremos aplicar todo eso lo que hacemos es “correr“ esa imagen de docker.

El docker file tiene una serie de datos que indican como crear una imagen, lo ejecutamos y este nos genera una imagen para desplegar.

## Comandos de Docker

Para correr una imagen usamos

```bash
docker run <nombre imagen>
docker run postgres
```

Para saber el nombre de las imágenes podemos consultar:

[https://hub.docker.com/](https://hub.docker.com/)

```bash
docker run <imagen>
```

Descarga simplemente la imagen de docker

A veces algunas imágenes nos van a pedir variables de entorno para que puedan ejecutarse, en este caso Postgres nos pide la contraseña de la base de datos. Se la damos:

```bash
docker run -e POSTGRES_PASSWORD=password postgres
```

### Otros comandos

```bash
# descarga y corre una imagen de docker
docker run 
# descarga solo la imagen de docker
docker pull
# listar todas las imagenes descargadas en la máquina
docker images
# mostrar los contenedores corriendo o el listado de los antiguos
docker ps
docker ps -a
# Lanzar una imagen parada de docker
docker start <docker_id>
# Ver los logs de un contenedor
docker logs <docker_id>
# Para ver los logs como se actualizan
docker logs -f <docker_id>
# Ejecutar un comando dentro de un docker que esta corriendo
docker exec <docker_id>
## Ejemplo: Generamos una sesion interactiva junto con una terminal y lanzamos una shell sh
docker exec -it <docker_id> sh
# Detener un contenedor
docker stop <docker_id>
# Correr un contenedor en background
docker run -d <imagen>
```

## Ejemplo Docker File

Vamos a descargar un codigo de ejemplo de una APP en NodeJS

[https://github.com/pablokbs/peladonerd/tree/master/docker/12/app](https://github.com/pablokbs/peladonerd/tree/master/docker/12/app)

Creamos una archivo llamado Dockerfile

Para crear una imagen docker, siempre se va a necesitar usar un FROM, es decir una imagen padre que nos permita generar la nuestra

```
FROM node:12.22.1-alpine3.11

WORKDIR /app
COPY . .
RUN yarn install --production

CMD ["node", "/app/src/index.js"]
```

Normalmente para los FROM hay que buscar una imagen relacionada con nuestra app para asi no pelearnos con librerías. Conviene usar los tags para fijar una versión y asegurarnos de que siempre funciona. Para ello, usamos DockerHub. Es recomendable usar Alpine en los contenedores como subversion de linux ya que es una distro muy pequeña y manejable.

**WORKDIR** genera el directorio de trabajo dentro del container, a partir de ahi, todo lo que lancemos se hará en ese directorio.

**COPY** realizará una copia de el directorio origen al destino. En este caso se usa un “.” para el origen (en el que estamos al lanzar el dockerfile) y lo copiamos en el destino “.” que es el WORKDIR definido arriba.

**RUN** correrá el comando que le pidamos, en este caso compila la aplicación que hemos volcado en el directorio interno del container

Finalmente se ha de especificar el comando que queremos que corra el container al iniciarse. En este caso queremos lanzar el index.js que se encuentra en la carpeta “src”.

Otra opcion es dejar un ENTRYPOINT, esto deja abierto una ventana que nos permite introducir comandos desde fuera del container a la hora de correrlo. Es útil en contaniers de python o de aplicaciones que vayamos a cambiar su finalidad en tiempo de ejecución.

Por ultimo, para correr el contenedor ejecutamos en el directorio donde esta el docker file:

```
docker build -t <nombre_imagen> .
```

Ahora tendremos la aplicacion corriendo en el puerto 3000.

## Puertos en Docker

Cuando ejecutamos contenedores, estos corren en una red de Docker interna, que no esta compartida con el resto del equipo ni de la red local. Es por eso que tenemos que hacer conexiones entre esa red interna y la red local para poder interactuar con los servicios que levantamos en Docker.

En el ejemplo en el que nos encontramos, vamos a vincular el puerto 3000 de la red de Docker, con el de nuestro local host.

```
# Le añadimos el -d para que corra en el background
# el -p es para asignar el puerto que queremos vincular
docker run -dp 3000:3000 <nombre_imagen>
```

## Persistencia de datos

El contenido que se genera en los contenedores es volatil, y una vez que el contenedor muere, el contenido despaarace con el. Por tanto, al igual que se hace con los puertos, se puede realizar una referencia entre el contenido que se genera en el contenedor y el almacenamiento persistente de la maquina en la que corre. A continuación, se muestra como hacerlo.

```
docker run -d -v <ruta local de persistencia de datos>:<ruta interna del contenedor>
        -p 3000:3000 <nombre_imagen>
```

Otra cosa buena de los volumenes, es que permite modificar el codigo de la aplicacion sin reconstruir la imagen.

```
#Ejemplo
sudo docker run -d -v /home/usuario/cursos/curso-docker/app/etc/:/etc/todos 
                -p 3000:3000 
                -v /home/usuario/cursos/curso-docker/app/src:/app/src primera_imagen
```

En Windows sería de la siguiente manera:
```powershell
docker run -d -v C:\Users\Usuario\Downloads\tutorial-docker\app\etc:/etc/todos -p 3000:3000 -v C:\Users\Usuario\Downloads\tutorial-docker\app\src:/app/src tutorial
```


Con el ejemplo de codigo anterior, se nos permite modificar el codigo y al relanzar el contenedor se mostrará la nueva version de la aplicacion. No es siquiera necesario relanzar el contenedor.

Una vez se termina de desarrollar en un contenedor, si que es necesario reconstruir la imagen para salvar los cambios y darle consisitencia a toda la aplicacion. Cuando se realiza esto es conveniente reconstruir la app cambiand su tag de la siguiente forma.

```
docker build -t <nombre_imagen>:v2 .
```

## Compartir imagenes

Cuando se tiene una imagen creada y se quiere compartir, se ha de subir a un registro, imitando asi la dinamica que se siguie en la comparticion de codigo con GitHub, las imagenes de docker se suele subir a DockerHub.

```
# Para hacer login basta con el comando
docker login
# Para subir las imagenes hay que tagearlas y hacer un push
docker tag <id_imagen> <nombre_user/nombre_img:version>
# Finalmente se sube la imagen
docker push <tag_imagen:version>
```

## Multi-Containers

Una buena practica es la de correr diferentes containers, cada uno de ellos con una finalidad en especifico. Para ello es necesario crear una red de docker que interconecte cada uno de estos y les permita tener acceso entre los mismos.

```
# Creamos una red de docker
docker network create <nombre_red>
```

Ahora, al lanzar contenedores, podremos indicarles la red en la que queremos que se desplieguen y el alias que van a tener en ella. Este alias nos permite despreocuparnos de la IP que cojan en dicha red y tener una referencia directa al contenedor en cuestion

```
docker run -d
        --network <nombre_red> --network-alias <alias_red>
        -v <volumen_local>:<volumen:container>
        -e MYSQL_ROOT_PASSWORD=secret
        -e MYSQL_DATABASE=todos
        mysql:5.7
# El resto de parametros que se muestran son un ejemplo para lanzar una base de datos SQL
```

```
# Otro contenedor que se puede lanzar en este ejemplo es el siguiente
docker run -it --network <nombre_red> nicolaka/netshoot
# Este contenedor nos permite hacer pruebas de interconexion dentro de la red
```

```
# Para conectar una app a la base de datos que hemos creado antes se puede hacer asi
docker run -dp 3000:3000
          --network <nombre_red>
          -e MYSQL_HOST=mysql
          -e MYSQL_USER=root
          -e MYSQL_PASSWORD=secret
          -e MYSQL_DB=todos
          <nombre_imagen:version>
```

## Docker Compose

Cuando se tiene que manejar un alto volumen de comandos para desplegar una serie de contenedores, lo mejor es usar un archivo de configuracion llamado docker-compose que nos permite desplegar una serie de recursos dentro de una misma red de docker.

```
# Version de la sintaxis de docker-compose
version: "3.7"

services:

#docker run -dp 3000:3000 --network todo-app -e MYSQL_HOST=mysql -e MYSQL_USER=root -e MYSQL_PASSWORD=secret -e MYSQL_DB=todos getting-started:v2

  app:
    image: pablokbs/getting-started:v2
    ports:
      - 3000:3000
    environment:
      MYSQL_HOST: mysql
      MYSQL_USER: root
      MYSQL_PASSWORD: secret
      MYSQL_DB: todos
  
# docker run -d     --network todo-app --network-alias mysql     -v todo-mysql-data:/var/lib/mysql     -e MYSQL_ROOT_PASSWORD=secret     -e MYSQL_DATABASE=todos     mysql:5.7

  mysql:
    image: mysql:5.7
    volumes:
      - ./todo-mysql-data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: secret
      MYSQL_DATABASE: todos
```

Para correr el fichero docker-compose.yml una vez que se tiene listo hay que usar el siguiente comando:

```
docker-compose up -d
```