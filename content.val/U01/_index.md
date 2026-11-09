---
title: "Unitat 1"
weight: 1
translationKey: "u01"
bookCollapseSection: true
---

# Unitat 1. Arquitectura d'equips i components maquinari

Com a desenvolupadors, les vostres aplicacions s'executaran sobre un **maquinari** concret: el portàtil amb què programeu, el servidor on es despleguen o el mòbil on s'instal·len. Conéixer què hi ha dins d'un equip vos permetrà entendre per què una compilació és lenta, quin ordinador necessiteu per a treballar amb emuladors i màquines virtuals, o per què una aplicació consumix massa memòria.

En aquesta unitat es recorren els components d'un ordinador, un per un: quina funció complix cadascun, com es connecten entre si, quines gammes i característiques hi ha i com triar-los amb criteri segons l'ús que se li vaja a donar a l'equip. La unitat acaba amb el **procés d'arrancada**: el que ocorre des que es prem el botó d'encesa fins que el sistema operatiu està llest.

## Resultat d'aprenentatge

Aquesta unitat treballa el **RA1** del mòdul *Sistemes Informàtics* (0483): **avalua sistemes informàtics, identificant-ne els components i les característiques**.

| Criteri d'avaluació | On es treballa |
| --- | --- |
| a) S'han reconegut els components físics d'un sistema informàtic i els seus mecanismes d'interconnexió. | Apartats 1 a 8 |
| b) S'han classificat els tipus de memòries, assenyalant-ne les característiques i identificant-ne les prestacions i la funció que exercixen en el conjunt del sistema. | Apartats 4 i 8 |
| c) S'ha verificat el procés de posada en marxa d'un equip. | Apartat 10 |
| d) S'han classificat, instal·lat i configurat diferents tipus de dispositius perifèrics. | Apartats 7 i 9 |
| e) S'han identificat els tipus de xarxes i sistemes de comunicació. | Apartat 7 (targetes de xarxa i estàndards Wi-Fi). S'amplia en la Unitat 5. |
| f) S'han identificat els components d'una xarxa informàtica. | Apartat 7 (introducció). S'amplia en la Unitat 5. |
| g) S'han interpretat mapes físics i lògics d'una xarxa informàtica. | Pendent de desenvolupar. |
| h) S'han reconegut les normes de seguretat i prevenció de riscos laborals en l'ús dels sistemes informàtics. | Pendent de desenvolupar. |

## Contingut de la unitat

**Bloc A: el nucli de l'equip**

| Apartat | Què aprendràs |
| --- | --- |
| [1. Plaques base]({{< relref "/U01/U01.01-Placas-Base-VAL.md" >}}) | Arquitectura d'un ordinador, formats de placa base, *sockets*, *chipset*, ranures i connectors. |
| [2. Processadors]({{< relref "/U01/U01.02-Procesadores-VAL.md" >}}) | Intel enfront d'AMD, gammes, generacions i com interpretar el model del processador. |
| [3. Dissipadors]({{< relref "/U01/U01.03-Disipadores-VAL.md" >}}) | Refrigeració passiva, activa i líquida, i quan convé cadascuna. |
| [4. RAM]({{< relref "/U01/U01.04-RAM-VAL.md" >}}) | SRAM i DRAM, jerarquia de memòries, generacions DDR, formats i perfils XMP i EXPO. |
| [5. Fonts d'alimentació]({{< relref "/U01/U01.05-FAlimentacion-VAL.md" >}}) | Estàndards, tipus de cablejat, certificacions i càlcul de la potència necessària. |

**Bloc B: targetes, emmagatzematge i perifèrics**

| Apartat | Què aprendràs |
| --- | --- |
| [6. Targetes gràfiques]({{< relref "/U01/U01.06-Graficas-VAL.md" >}}) | Fabricants, components i característiques d'una GPU, i la seua relació amb la font d'alimentació. |
| [7. Targetes de xarxa i multimèdia]({{< relref "/U01/U01.07-TRed-VAL.md" >}}) | Targetes de xarxa internes i externes, estàndards Wi-Fi, targetes de so, de ports i controladores de discos. |
| [8. Emmagatzematge]({{< relref "/U01/U01.08-Almacenamiento-VAL.md" >}}) | Discos HDD i SSD, tipus de SSD segons el controlador i la interfície, velocitats i característiques. |
| [9. Monitors i cables]({{< relref "/U01/U01.09-Monitores-Cables-VAL.md" >}}) | Mida, resolució, freqüència de refresc, tipus de panell i cables de vídeo (HDMI i DisplayPort). |

**Bloc C: posada en marxa**

| Apartat | Què aprendràs |
| --- | --- |
| [10. Procés d'arrancada d'un sistema operatiu]({{< relref "/U01/U01.10-Carga-Sistema-VAL.md" >}}) | Fases de l'arrancada, BIOS enfront d'UEFI, arrancada de Linux amb GRUB i de Windows, i problemes habituals. |

## Com treballar la unitat

Aquesta unitat és més teòrica que les següents, però s'entén molt millor si es relaciona amb equips reals:

- **Mira el teu propi equip.** En Windows, l'*Administrador de tasques* (pestanya *Rendiment*) i l'eina `msinfo32` mostren el processador, la memòria i els discos instal·lats. En Linux, ordres com `lscpu`, `lsblk` i `free -h` oferixen la mateixa informació.
- **Consulta les especificacions oficials** dels fabricants (Intel, AMD, NVIDIA, fabricants de plaques base) quan tingues dubtes sobre un model concret.
- **Compara configuracions** en botigues en línia: les activitats de cada apartat et demanaran triar components per a un ús determinat i justificar l'elecció.

> [!TIP]
> El mercat del maquinari canvia cada pocs mesos. Més que memoritzar models concrets, centra't a **entendre les característiques** (nuclis, freqüència, tipus de memòria, interfície del disc…) que et permetran comparar qualsevol model nou.

> [!CAUTION]
> Abans d'obrir un ordinador, **apaga'l i desconnecta'l del corrent**, i descarrega l'electricitat estàtica del teu cos tocant una superfície metàl·lica sense pintar o utilitzant una polsera antiestàtica. Manipula els components per les vores, sense tocar els contactes.
