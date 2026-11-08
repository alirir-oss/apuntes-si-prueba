---
title: "9. Tutorial de Docker"
weight: 9
---

## Què és Docker?

Docker es diferencia de les màquines virtuals en què no reserva recursos de l'ordinador amfitrió (*host*), sinó que fa ús del kernel i dels recursos que necessita en cada moment. Per tant, ens permetrà aixecar molts més contenidors en un mateix ordinador amb menys recursos, ja que aquests s'aniran alliberant i ocupant segons la demanda.

### Docker File

El Dockerfile és un fitxer que conté tota la informació de la imatge de Docker. La imatge de Docker conté tant les biblioteques com el sistema operatiu i el codi de la nostra aplicació. Quan volem posar en marxa tot això, el que fem és "executar" aquesta imatge de Docker.

El Dockerfile té una sèrie de dades que indiquen com crear una imatge; l'executem i ens genera una imatge per a desplegar.

## Ordres de Docker

Per a executar una imatge utilitzem:

```bash
docker run <nom imatge>
docker run postgres
```

Per a saber el nom de les imatges podem consultar:

[https://hub.docker.com/](https://hub.docker.com/)

```bash
docker run <imatge>
```

Descarrega simplement la imatge de Docker.

De vegades, algunes imatges ens demanaran variables d'entorn per a poder executar-se; en aquest cas, Postgres ens demana la contrasenya de la base de dades. Li la donem:

```bash
docker run -e POSTGRES_PASSWORD=password postgres
```

### Altres ordres

```bash
# descarrega i executa una imatge de Docker
docker run 
# descarrega només la imatge de Docker
docker pull
# llistar totes les imatges descarregades en la màquina
docker images
# mostrar els contenidors en execució o el llistat dels antics
docker ps
docker ps -a
# Arrencar una imatge de Docker aturada
docker start <docker_id>
# Veure els registres (logs) d'un contenidor
docker logs <docker_id>
# Per a veure com s'actualitzen els registres
docker logs -f <docker_id>
# Executar una ordre dins d'un contenidor que està en execució
docker exec <docker_id>
## Exemple: Generem una sessió interactiva amb un terminal i llancem un shell sh
docker exec -it <docker_id> sh
# Aturar un contenidor
docker stop <docker_id>
# Executar un contenidor en segon pla (background)
docker run -d <imatge>
```

## Exemple de Dockerfile

Descarregarem un codi d'exemple d'una aplicació en NodeJS:

[https://github.com/pablokbs/peladonerd/tree/master/docker/12/app](https://github.com/pablokbs/peladonerd/tree/master/docker/12/app)

Creem un fitxer anomenat Dockerfile.

Per a crear una imatge de Docker sempre caldrà utilitzar un FROM, és a dir, una imatge pare que ens permeta generar la nostra.

```
FROM node:12.22.1-alpine3.11

WORKDIR /app
COPY . .
RUN yarn install --production

CMD ["node", "/app/src/index.js"]
```

Normalment, per al FROM cal buscar una imatge relacionada amb la nostra aplicació per a no barallar-nos amb les biblioteques. Convé utilitzar les etiquetes (*tags*) per a fixar una versió i assegurar-nos que sempre funciona. Per a això, utilitzem Docker Hub. És recomanable utilitzar Alpine com a distribució de Linux en els contenidors, ja que és una distribució molt xicoteta i manejable.

**WORKDIR** crea el directori de treball dins del contenidor; a partir d'ací, tot el que llancem es farà en aquest directori.

**COPY** farà una còpia del directori d'origen al de destinació. En aquest cas s'utilitza un "." com a origen (el directori on som quan llancem el Dockerfile) i el copiem en la destinació ".", que és el WORKDIR definit abans.

**RUN** executarà l'ordre que li demanem; en aquest cas, compila l'aplicació que hem bolcat en el directori intern del contenidor.

Finalment, cal especificar l'ordre que volem que execute el contenidor quan s'inicie. En aquest cas volem llançar l'index.js que es troba en la carpeta "src".

Una altra opció és deixar un ENTRYPOINT, que deixa oberta una finestra que ens permet introduir ordres des de fora del contenidor en el moment d'executar-lo. És útil en contenidors de Python o d'aplicacions la finalitat de les quals canviarem en temps d'execució.

Finalment, per a executar el contenidor, executem en el directori on es troba el Dockerfile:

```
docker build -t <nom_imatge> .
```

Ara tindrem l'aplicació funcionant en el port 3000.

## Ports en Docker

Quan executem contenidors, aquests funcionen en una xarxa interna de Docker, que no està compartida amb la resta de l'equip ni amb la xarxa local. Per això hem de fer connexions entre aquesta xarxa interna i la xarxa local per a poder interactuar amb els serveis que aixequem en Docker.

En l'exemple en què ens trobem, vincularem el port 3000 de la xarxa de Docker amb el del nostre *localhost*.

```
# Hi afegim el -d perquè s'execute en segon pla (background)
# el -p és per a assignar el port que volem vincular
docker run -dp 3000:3000 <nom_imatge>
```

## Persistència de dades

El contingut que es genera en els contenidors és volàtil, i una vegada que el contenidor mor, el contingut desapareix amb ell. Per tant, igual que es fa amb els ports, es pot fer una referència entre el contingut que es genera en el contenidor i l'emmagatzematge persistent de la màquina en què s'executa. A continuació, es mostra com fer-ho.

```
docker run -d -v <ruta local de persistència de dades>:<ruta interna del contenidor>
        -p 3000:3000 <nom_imatge>
```

Un altre avantatge dels volums és que permeten modificar el codi de l'aplicació sense reconstruir la imatge.

```
#Exemple
sudo docker run -d -v /home/usuario/cursos/curso-docker/app/etc/:/etc/todos 
                -p 3000:3000 
                -v /home/usuario/cursos/curso-docker/app/src:/app/src primera_imagen
```

En Windows seria de la manera següent:
```powershell
docker run -d -v C:\Users\Usuario\Downloads\tutorial-docker\app\etc:/etc/todos -p 3000:3000 -v C:\Users\Usuario\Downloads\tutorial-docker\app\src:/app/src tutorial
```


Amb l'exemple de codi anterior, podem modificar el codi i, en tornar a llançar el contenidor, es mostrarà la nova versió de l'aplicació. Ni tan sols és necessari tornar a llançar el contenidor.

Una vegada s'acaba de desenvolupar en un contenidor, sí que cal reconstruir la imatge per a guardar els canvis i donar consistència a tota l'aplicació. Quan es fa això, convé reconstruir l'aplicació canviant-ne l'etiqueta (*tag*) de la manera següent:

```
docker build -t <nom_imatge>:v2 .
```

## Compartir imatges

Quan es té una imatge creada i es vol compartir, cal pujar-la a un registre. De la mateixa manera que es comparteix el codi amb GitHub, les imatges de Docker se solen pujar a Docker Hub.

```
# Per a iniciar sessió n'hi ha prou amb l'ordre
docker login
# Per a pujar les imatges cal etiquetar-les i fer un push
docker tag <id_imatge> <nom_usuari/nom_imatge:versio>
# Finalment es puja la imatge
docker push <etiqueta_imatge:versio>
```

## Multicontenidors

Una bona pràctica és executar diferents contenidors, cadascun amb una finalitat específica. Per a això cal crear una xarxa de Docker que els interconnecte i els permeta tindre accés entre ells.

```
# Creem una xarxa de Docker
docker network create <nom_xarxa>
```

Ara, en llançar contenidors, podrem indicar-los la xarxa en què volem que es despleguen i l'àlies que tindran en aquesta xarxa. Aquest àlies ens permet despreocupar-nos de la IP que agafen en la xarxa i tindre una referència directa al contenidor en qüestió.

```
docker run -d
        --network <nom_xarxa> --network-alias <alies_xarxa>
        -v <volum_local>:<volum:contenidor>
        -e MYSQL_ROOT_PASSWORD=secret
        -e MYSQL_DATABASE=todos
        mysql:5.7
# La resta de paràmetres que es mostren són un exemple per a llançar una base de dades SQL
```

```
# Un altre contenidor que es pot llançar en aquest exemple és el següent
docker run -it --network <nom_xarxa> nicolaka/netshoot
# Aquest contenidor ens permet fer proves d'interconnexió dins de la xarxa
```

```
# Per a connectar una aplicació a la base de dades que hem creat abans es pot fer així
docker run -dp 3000:3000
          --network <nom_xarxa>
          -e MYSQL_HOST=mysql
          -e MYSQL_USER=root
          -e MYSQL_PASSWORD=secret
          -e MYSQL_DB=todos
          <nom_imatge:versio>
```

## Docker Compose

Quan cal gestionar un gran volum d'ordres per a desplegar una sèrie de contenidors, el millor és utilitzar un fitxer de configuració anomenat docker-compose, que ens permet desplegar una sèrie de recursos dins d'una mateixa xarxa de Docker.

```
# Versió de la sintaxi de docker-compose
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

Per a executar el fitxer docker-compose.yml una vegada el tenim preparat, cal utilitzar l'ordre següent:

```
docker-compose up -d
```
