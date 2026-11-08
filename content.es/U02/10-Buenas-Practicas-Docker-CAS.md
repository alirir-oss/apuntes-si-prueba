---
title: 10. Buenas Prácticas con Docker
weight: 10
---

# En qué consiste
El dockerfile es un fichero que como su propio nombre indica se encarga de fijar las ordenes que se han de ejecutar dentro de una imagen docker para la configuración de esta misma.

Estas instrucciones no difieren mucho de las que se harían en un servidor Linux para su configuración básica, pero se dejan resumidas dentro de este fichero.

Lo primero que se suele hacer dentro de un dockerfile es marcar un punto de partida para la creación de esta imagen, este punto de partida suele ser otra imagen docker de base, y se indica dentro del fichero mediante la palabra reservada `FROM`.

Una vez indicada la imagen de base de la que se quiere partir se le pueden añadir nuevas instrucciones que modifiquen y adapten a nuestras necesidades la imagen que estamos creando. Desde añadir librerías, instalar programas que necesitemos para la correcta ejecución de nuestra aplicación, etc.

Algunos comandos útiles para este fichero son los siguientes:
- COPY: Al igual que en linux el comando `cp` este comando copia el fichero que se indica en una ubicación del sistema base, a una determinada ruta dentro de la imagen.
- RUN: Este comando permite ejecutar instrucciones de la consola dentro de la imagen. Hay que tener en cuenta que estas instrucciones tienen que ir en concordancia con el sistema base sobre el que se ejecute la construcción de la imagen.

Este seria un ejemplo de un dockerfile para montar un servidor web:
```Dockerfile
FROM nginx:latest
COPY /index.html /usr/share/nginx/html
RUN cat /usr/share/nginx/html/index.html
```

# Build de la imagen
Una vez se tiene el dockerfile listo es cuando se construye la imagen. Para ello se tiene que tener Docker instalado en el sistema y usar el siguiente comando:

``` bash
docker build --pull -t servidorweb:v1 .
```

# Repositorios de imágenes docker
Son muchos los desarrolladores que usan imágenes docker para publicar sus proyectos y garantizar su funcionamiento en el equipo de otros. Al igual que existen repositorios de código para compartir aplicaciones a nivel de código, se puede hacer lo mismo con las imágenes.

Servicios como Docker Hub, permiten almacenar tus imágenes docker de manera gratuita hasta un cierto limite. También disponen de soluciones para empresas con mas prestaciones. El proceso de subida seria el siguiente:

```bash
# Nos logueamos en nuestra cuenta desde la consola
docker login -u <nombre-usuario> -p <token-contraseña> docker.io

# Una vez logueados hacemos push de nuestra imagen local
docker push usuario/repositorio:tag
```

# Descargar y usar una imagen
Cuando queremos usar una imagen en nuestro entorno, lo que podemos hacer es un `pull`de la imagen desde un repositorio desde el que se quiera traer la imagen. Una vez hecho esto, mediante un `run` se puede lanzar la imagen en el equipo donde se esté ejecutando docker.
```bash
# Se descarga la imagen
docker pull usuario/repositorio:tag

# Se lanza la imagen en el equipo
docker run -d -p 8080:80 usuario/repositorio:tag
```

# Buenas prácticas
A continuación, se recogen una serie de buenas prácticas en la elaboración de Dockerfiles.
1. Lo primero que se ha de usar es un **FROM** para indicar la imagen base a usar.
2. Las lineas están cacheadas, es decir si no se produce ningún cambio anterior, al recrear la imagen se mantendrán los cambios existentes. Por tanto, hay que ordenar los comandos de tal forma que se mantengan las máximas lineas posibles dentro del caché y así agilizar la construcción de nuevas imágenes.
3. No es necesario copiar los directorios completos de las aplicaciones, con copiar el archivo que se va a ejecutar, en muchas ocasiones sobra.
4. Mejor usar **COPY** en vez de **ADD**.
5. Al instalar paquetes, mantener en la misma linea el update y el install, de tal forma que usen el mismo cache.
6. Con Debian se puede usar `--no-install-recommends` para evitar que el apt-get instale paquetes recomendados que no necesitamos y ocupan espacio.
7. También, si se ha instalado algo, conviene borrar la lista de paquetes para que no ocupen espacio de mas
```Dockerfile
RUN apt-get update && \
    apt-get -y install --no-install-recommends \
    openjdk-8-jdk \
    && rm -rf /var/lib/apt/lists/*
```
8. Especificar siempre la versión de la imagen **FROM** ya que nos podemos evitar errores con versiones nuevas al reconstruir la imagen en el futuro.
9. En el caso de que haya que construir o compilar ficheros en el proyecto es mejor hacerlo dentro de la imagen docker, ya que así nos aseguramos de que sea compatible vaya donde vaya.
10. Usar **WORKDIR** para fijar el directorio de trabajo dentro del proyecto. De esta forma nos ahorramos confusiones y queda todo mas claro.
11. Comprobar las dependencias del motor de construcción o compilación antes de construir el código fuente, de esta forma aseguramos que no haya fallos.

## Multi-stage builds
Cuando se tienen que realizar construcciones dentro de una imagen, en ocasiones es recomendable usar el multi-stage, ya que en nuestro entorno de producción no es necesario que corra el constructor del código, simplemente el código.

Por tanto, lo que se hace es una primera stage que comprueba dependencias y construye el código y una vez eso finaliza, se crea la imagen final exclusivamente con el código corriendo.

Ejemplo:
```Dockerfile
FROM maven:3.6-jdk-8-alpine AS builder
WORKDIR /app
COPY pom.xml .
RUN mvn -e -B dependency:resolve
COPY src ./src
RUN mvn -e -B package

FROM openjdk:8-jre-alpine
COPY --from=builder /app/target/app.jar
CMD ["java", "-jar", "/app.jar"]
```

## Resumen
Finalmente a modo de resumen estos serian los puntos esenciales de buenas prácticas a cumplir en la elaboración de un Dockerfile:
1. Usar siempre la imagen más pequeña que se pueda.
2. Aprovechar el caché lo máximo que se pueda.
3. Asegurarse de que los paquetes que se tengan que construir, lo hagan siempre en Docker.



