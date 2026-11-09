---
title: "Unitat 2"
weight: 2
translationKey: "u02"
bookCollapseSection: true
---

# Unitat 2. Instal·lació de sistemes operatius

El **sistema operatiu** és el programa que fa d'intermediari entre el maquinari que vas estudiar en la Unitat 1 i les aplicacions que desenvolupareu. Quan el vostre programa obri un fitxer, reserva memòria o es connecta a la xarxa, en realitat li ho està demanant al sistema operatiu. Per això, entendre com funciona i saber instal·lar-lo i configurar-lo és la base de tot el que fareu en la resta del mòdul.

Aquesta unitat té tres parts:

1. **Què és un sistema operatiu**: la seua arquitectura, els seus components i els tipus de llicència amb què es distribuïx.
2. **Preparar i instal·lar un sistema**: sistemes de fitxers, particionament de discos, instal·lació de Windows i Linux, actualitzacions, aplicacions i controladors.
3. **Virtualització i contenidors**: màquines virtuals, Docker i WSL, les eines que utilitzareu diàriament per a provar sistemes i desplegar aplicacions sense risc per al vostre equip.

## Resultat d'aprenentatge

Aquesta unitat treballa el **RA2** del mòdul *Sistemes Informàtics* (0483): **instal·la sistemes operatius planificant el procés i interpretant documentació tècnica**.

| Criteri d'avaluació | On es treballa |
| --- | --- |
| a) S'han identificat els elements funcionals d'un sistema informàtic. | Apartats 1 i 2 |
| b) S'han analitzat les característiques, funcions i arquitectura d'un sistema operatiu. | Apartats 1 i 2 |
| c) S'han comparat sistemes operatius sobre la base dels seus requisits, característiques, camps d'aplicació i llicències d'ús. | Apartats 3 i 6 |
| d) S'ha planificat el procés de la instal·lació de sistemes operatius. | Apartats 4, 5 i 6 |
| e) S'han instal·lat i actualitzat sistemes operatius lliures i propietaris. | Apartat 6 |
| f) S'han aplicat tècniques d'actualització i recuperació del sistema. | Apartat 6 (actualitzacions). Les tècniques de recuperació estan pendents d'ampliar. |
| g) S'han utilitzat tecnologies de virtualització per a instal·lar i provar sistemes operatius. | Apartats 7 a 11 |
| h) S'han instal·lat, desinstal·lat i actualitzat aplicacions. | Apartat 6 |
| i) S'han documentat els processos realitzats. | Pràctiques de la unitat (memòries d'instal·lació) |

## Contingut de la unitat

**Bloc A: el sistema operatiu**

| Apartat | Què aprendràs |
| --- | --- |
| [1. Arquitectura de sistemes operatius]({{< relref "/U02/01-Arquitectura-SO-VAL.md" >}}) | Què és un sistema operatiu, com ha evolucionat i quins tipus d'arquitectura hi ha (monolítica, micronucli, híbrida…). |
| [2. Elements d'un sistema operatiu]({{< relref "/U02/02-Elementos-SO-VAL.md" >}}) | Funcions principals, modes d'operació del processador i components clau: gestió de processos, memòria, fitxers i dispositius. |
| [3. Llicències dels sistemes operatius]({{< relref "/U02/03-Licencias-SO-VAL.md" >}}) | Programari lliure i propietari, tipus de llicència i criteris per a comparar sistemes operatius. |

**Bloc B: preparar i instal·lar el sistema**

| Apartat | Què aprendràs |
| --- | --- |
| [4. Sistemes de fitxers]({{< relref "/U02/04-Sistemas-Archivos-VAL.md" >}}) | FAT32, NTFS i ext4: característiques, comparativa i quan utilitzar cadascun. |
| [5. Particionament de discs]({{< relref "/U02/05-Particionado-Discos-VAL.md" >}}) | Particions primàries, esteses i lògiques, esquemes MBR i GPT, i particionament típic en Windows i Linux. |
| [6. Instal·lació i actualitzacions]({{< relref "/U02/06-Actualizaciones-Sistemas-VAL.md" >}}) | Instal·lació d'un sistema client (Windows i Ubuntu), configuració inicial, actualitzacions, aplicacions i controladors de dispositius. |

**Bloc C: virtualització i contenidors**

| Apartat | Què aprendràs |
| --- | --- |
| [7. Sistemes de virtualització]({{< relref "/U02/07-Sistemas-Virtualizacion-VAL.md" >}}) | Concepte de virtualització, tipus d'hipervisor, Oracle VirtualBox i els seus modes de xarxa. |
| [8. Contenidors]({{< relref "/U02/08-Contenedores-VAL.md" >}}) | Què és Docker, diferències amb les màquines virtuals i conceptes clau: imatges, contenidors, volums i xarxes. |
| [9. Tutorial de Docker]({{< relref "/U02/09-Tutorial-Docker-VAL.md" >}}) | Ordres bàsiques per a descarregar imatges i gestionar contenidors. |
| [10. Bones pràctiques amb Docker]({{< relref "/U02/10-Buenas-Practicas-Docker-VAL.md" >}}) | Construir imatges pròpies, publicar-les en un repositori i construccions en diverses etapes (*multi-stage builds*). |
| [11. WSL]({{< relref "/U02/11-WSL-VAL.md" >}}) | El subsistema de Windows per a Linux: versions, requisits, instal·lació i ordres principals. |

## Entorn de pràctiques

En aquesta unitat instal·laràs sistemes operatius diverses vegades, així que **no ho faces sobre el teu equip real**. Utilitza sempre màquines virtuals:

| Eina | Ús en la unitat |
| --- | --- |
| **Oracle VirtualBox** (o l'hipervisor que s'indique a classe) | Instal·lar Windows 11 i Ubuntu Desktop en màquines virtuals. |
| **Docker** (Docker Desktop en Windows o Docker Engine en Linux) | Apartats 8 a 10. |
| **WSL** (en Windows 11) | Apartat 11 i unitats següents. |

> [!IMPORTANT]
> Per a utilitzar màquines virtuals de 64 bits, Docker Desktop o WSL 2, la **virtualització per maquinari** (Intel VT-x o AMD-V) ha d'estar activada en la configuració UEFI de l'equip. Pots comprovar-ho en l'*Administrador de tasques* de Windows, pestanya *Rendiment* > *CPU*, en la línia *Virtualització*.

> [!TIP]
> Abans de cada canvi important en una màquina virtual (actualitzar, instal·lar un controlador, particionar), fes una **instantània**. Si alguna cosa ix malament, podràs tornar a l'estat anterior en segons. És la tècnica de recuperació més senzilla que hi ha i la utilitzaràs en totes les unitats.
