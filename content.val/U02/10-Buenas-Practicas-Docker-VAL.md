---
title: "10. Bones Pràctiques amb Docker"
weight: 10
---

# En què consisteix
El Dockerfile és un fitxer que, com indica el seu nom, s'encarrega de fixar les ordres que s'han d'executar dins d'una imatge de Docker per a configurar-la.

Aquestes instruccions no difereixen gaire de les que es farien en un servidor Linux per a la seva configuració bàsica, però queden resumides dins d'aquest fitxer.

El primer que se sol fer dins d'un Dockerfile és marcar un punt de partida per a crear la imatge. Aquest punt de partida sol ser una altra imatge de Docker de base, i s'indica dins del fitxer mitjançant la paraula reservada `FROM`.

Una vegada indicada la imatge de base de la qual es vol partir, s'hi poden afegir noves instruccions que modifiquen i adapten a les nostres necessitats la imatge que estem creant: des d'afegir biblioteques fins a instal·lar els programes que necessitem per a l'execució correcta de la nostra aplicació, etc.

Algunes ordres útils per a aquest fitxer són les següents:
- COPY: Igual que l'ordre `cp` de Linux, aquesta ordre copia el fitxer que s'indica, des d'una ubicació del sistema base, a una ruta determinada dins de la imatge.
- RUN: Aquesta ordre permet executar instruccions de consola dins de la imatge. Cal tindre en compte que aquestes instruccions han de ser coherents amb el sistema base sobre el qual s'executa la construcció de la imatge.

Aquest seria un exemple de Dockerfile per a muntar un servidor web:
```Dockerfile
FROM nginx:latest
COPY /index.html /usr/share/nginx/html
RUN cat /usr/share/nginx/html/index.html
```

# Construcció (build) de la imatge
Una vegada el Dockerfile està preparat, es construeix la imatge. Per a això cal tindre Docker instal·lat en el sistema i utilitzar l'ordre següent:

``` bash
docker build --pull -t servidorweb:v1 .
```

# Repositoris d'imatges de Docker
Molts desenvolupadors utilitzen imatges de Docker per a publicar els seus projectes i garantir que funcionen en l'equip d'altres persones. De la mateixa manera que hi ha repositoris de codi per a compartir aplicacions en forma de codi, es pot fer el mateix amb les imatges.

Serveis com Docker Hub permeten emmagatzemar les teves imatges de Docker de manera gratuïta fins a un cert límit. També disposen de solucions per a empreses amb més prestacions. El procés de pujada seria el següent:

```bash
# Iniciem sessió en el nostre compte des de la consola
docker login -u <nom-usuari> -p <token-contrasenya> docker.io

# Una vegada hem iniciat sessió, fem push de la nostra imatge local
docker push usuari/repositori:etiqueta
```

# Descarregar i utilitzar una imatge
Quan volem utilitzar una imatge en el nostre entorn, podem fer un `pull` de la imatge des del repositori d'on la vulguem obtindre. Una vegada fet això, mitjançant un `run` es pot llançar la imatge en l'equip on s'executa Docker.
```bash
# Es descarrega la imatge
docker pull usuari/repositori:etiqueta

# Es llança la imatge en l'equip
docker run -d -p 8080:80 usuari/repositori:etiqueta
```

# Bones pràctiques
A continuació, es recullen una sèrie de bones pràctiques per a elaborar Dockerfiles.
1. El primer que s'ha d'utilitzar és un **FROM** per a indicar la imatge base.
2. Les línies es guarden en memòria cau (*cache*); és a dir, si no hi ha cap canvi en les línies anteriors, en tornar a crear la imatge es mantindran les capes existents. Per tant, cal ordenar les ordres de manera que es mantinguen en la memòria cau el màxim de línies possible i així agilitzar la construcció de noves imatges.
3. No cal copiar els directoris complets de les aplicacions: moltes vegades n'hi ha prou amb copiar el fitxer que s'executarà.
4. És millor utilitzar **COPY** en lloc d'**ADD**.
5. En instal·lar paquets, cal mantindre l'update i l'install en la mateixa línia, de manera que utilitzen la mateixa memòria cau.
6. Amb Debian es pot utilitzar `--no-install-recommends` per a evitar que apt-get instal·le paquets recomanats que no necessitem i que ocupen espai.
7. També, si s'ha instal·lat alguna cosa, convé esborrar la llista de paquets perquè no ocupe espai de més.
```Dockerfile
RUN apt-get update && \
    apt-get -y install --no-install-recommends \
    openjdk-8-jdk \
    && rm -rf /var/lib/apt/lists/*
```
8. Especificar sempre la versió de la imatge del **FROM**, ja que així podem evitar errors amb versions noves quan reconstruïm la imatge en el futur.
9. Si cal construir o compilar fitxers del projecte, és millor fer-ho dins de la imatge de Docker, ja que així ens assegurem que siga compatible allà on vaja.
10. Utilitzar **WORKDIR** per a fixar el directori de treball dins del projecte. D'aquesta manera evitem confusions i tot queda més clar.
11. Comprovar les dependències del motor de construcció o compilació abans de construir el codi font; d'aquesta manera ens assegurem que no hi haja errors.

## Construccions en diverses etapes (multi-stage builds)
Quan s'han de fer construccions dins d'una imatge, de vegades és recomanable utilitzar el *multi-stage*, ja que en el nostre entorn de producció no cal que s'execute el constructor del codi, sinó simplement el codi.

Per tant, el que es fa és una primera etapa (*stage*) que comprova les dependències i construeix el codi i, una vegada acaba, es crea la imatge final exclusivament amb el codi en execució.

Exemple:
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

## Resum
Finalment, a manera de resum, aquests serien els punts essencials de bones pràctiques que cal complir en l'elaboració d'un Dockerfile:
1. Utilitzar sempre la imatge més xicoteta possible.
2. Aprofitar la memòria cau al màxim.
3. Assegurar-se que els paquets que s'hagen de construir es construïsquen sempre en Docker.



