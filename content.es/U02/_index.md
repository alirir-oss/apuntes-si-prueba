---
title: "Unidad 2"
weight: 2
translationKey: "u02"
bookCollapseSection: true
---

# Unidad 2. Instalación de sistemas operativos

El **sistema operativo** es el programa que hace de intermediario entre el hardware que estudiaste en la Unidad 1 y las aplicaciones que vais a desarrollar. Cuando vuestro programa abre un fichero, reserva memoria o se conecta a la red, en realidad se lo está pidiendo al sistema operativo. Por eso, entender cómo funciona y saber instalarlo y configurarlo es la base de todo lo que haréis en el resto del módulo.

Esta unidad tiene tres partes:

1. **Qué es un sistema operativo**: su arquitectura, sus componentes y los tipos de licencia con los que se distribuye.
2. **Preparar e instalar un sistema**: sistemas de archivos, particionado de discos, instalación de Windows y Linux, actualizaciones, aplicaciones y controladores.
3. **Virtualización y contenedores**: máquinas virtuales, Docker y WSL, las herramientas que usaréis a diario para probar sistemas y desplegar aplicaciones sin riesgo para vuestro equipo.

## Resultado de aprendizaje

Esta unidad trabaja el **RA2** del módulo *Sistemas Informáticos* (0483): **instala sistemas operativos planificando el proceso e interpretando documentación técnica**.

| Criterio de evaluación | Dónde se trabaja |
| --- | --- |
| a) Se han identificado los elementos funcionales de un sistema informático. | Apartados 1 y 2 |
| b) Se han analizado las características, funciones y arquitectura de un sistema operativo. | Apartados 1 y 2 |
| c) Se han comparado sistemas operativos en base a sus requisitos, características, campos de aplicación y licencias de uso. | Apartados 3 y 6 |
| d) Se ha planificado el proceso de la instalación de sistemas operativos. | Apartados 4, 5 y 6 |
| e) Se han instalado y actualizado sistemas operativos libres y propietarios. | Apartado 6 |
| f) Se han aplicado técnicas de actualización y recuperación del sistema. | Apartado 6 (actualizaciones). Las técnicas de recuperación están pendientes de ampliar. |
| g) Se han utilizado tecnologías de virtualización para instalar y probar sistemas operativos. | Apartados 7 a 11 |
| h) Se han instalado, desinstalado y actualizado aplicaciones. | Apartado 6 |
| i) Se han documentado los procesos realizados. | Prácticas de la unidad (memorias de instalación) |

## Contenido de la unidad

**Bloque A: el sistema operativo**

| Apartado | Qué aprenderás |
| --- | --- |
| [1. Arquitectura de sistemas operativos]({{< relref "/U02/01-Arquitectura-SO-CAS.md" >}}) | Qué es un sistema operativo, cómo ha evolucionado y qué tipos de arquitectura existen (monolítica, micronúcleo, híbrida…). |
| [2. Elementos de un sistema operativo]({{< relref "/U02/02-Elementos-SO-CAS.md" >}}) | Funciones principales, modos de operación del procesador y componentes clave: gestión de procesos, memoria, ficheros y dispositivos. |
| [3. Licencias de los sistemas operativos]({{< relref "/U02/03-Licencias-SO-CAS.md" >}}) | Software libre y propietario, tipos de licencia y criterios para comparar sistemas operativos. |

**Bloque B: preparar e instalar el sistema**

| Apartado | Qué aprenderás |
| --- | --- |
| [4. Sistemas de archivos]({{< relref "/U02/04-Sistemas-Archivos-CAS.md" >}}) | FAT32, NTFS y ext4: características, comparativa y cuándo usar cada uno. |
| [5. Particionado de discos]({{< relref "/U02/05-Particionado-Discos-CAS.md" >}}) | Particiones primarias, extendidas y lógicas, esquemas MBR y GPT, y particionado típico en Windows y Linux. |
| [6. Instalación y actualizaciones]({{< relref "/U02/06-Actualizaciones-Sistemas-CAS.md" >}}) | Instalación de un sistema cliente (Windows y Ubuntu), configuración inicial, actualizaciones, aplicaciones y controladores de dispositivos. |

**Bloque C: virtualización y contenedores**

| Apartado | Qué aprenderás |
| --- | --- |
| [7. Sistemas de virtualización]({{< relref "/U02/07-Sistemas-Virtualizacion-CAS.md" >}}) | Concepto de virtualización, tipos de hipervisor, Oracle VirtualBox y sus modos de red. |
| [8. Contenedores]({{< relref "/U02/08-Contenedores-CAS.md" >}}) | Qué es Docker, diferencias con las máquinas virtuales y conceptos clave: imágenes, contenedores, volúmenes y redes. |
| [9. Tutorial de Docker]({{< relref "/U02/09-Tutorial-Docker-CAS.md" >}}) | Órdenes básicas para descargar imágenes y gestionar contenedores. |
| [10. Buenas prácticas con Docker]({{< relref "/U02/10-Buenas-Practicas-Docker-CAS.md" >}}) | Construir imágenes propias, publicarlas en un repositorio y compilaciones en varias etapas (*multi-stage builds*). |
| [11. WSL]({{< relref "/U02/10-WSL-CAS.md" >}}) | El subsistema de Windows para Linux: versiones, requisitos, instalación y órdenes principales. |

## Entorno de prácticas

En esta unidad vas a instalar sistemas operativos varias veces, así que **no lo hagas sobre tu equipo real**. Usa siempre máquinas virtuales:

| Herramienta | Uso en la unidad |
| --- | --- |
| **Oracle VirtualBox** (o el hipervisor que se indique en clase) | Instalar Windows 11 y Ubuntu Desktop en máquinas virtuales. |
| **Docker** (Docker Desktop en Windows o Docker Engine en Linux) | Apartados 8 a 10. |
| **WSL** (en Windows 11) | Apartado 11 y siguientes unidades. |

> [!IMPORTANT]
> Para usar máquinas virtuales de 64 bits, Docker Desktop o WSL 2, la **virtualización por hardware** (Intel VT-x o AMD-V) debe estar activada en la configuración UEFI del equipo. Puedes comprobarlo en el *Administrador de tareas* de Windows, pestaña *Rendimiento* > *CPU*, en la línea *Virtualización*.

> [!TIP]
> Antes de cada cambio importante en una máquina virtual (actualizar, instalar un controlador, particionar), haz una **instantánea**. Si algo sale mal, podrás volver al estado anterior en segundos. Es la técnica de recuperación más sencilla que existe y la usarás en todas las unidades.
