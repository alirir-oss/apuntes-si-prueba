---
title: "Unidad 1"
weight: 1
translationKey: "u01"
bookCollapseSection: true
---

# Unidad 1. Arquitectura de equipos y componentes hardware

Como desarrolladores, vuestras aplicaciones se ejecutarán sobre un **hardware** concreto: el portátil con el que programáis, el servidor donde se despliegan o el móvil donde se instalan. Conocer qué hay dentro de un equipo os permitirá entender por qué una compilación es lenta, qué ordenador necesitáis para trabajar con emuladores y máquinas virtuales, o por qué una aplicación consume demasiada memoria.

En esta unidad se recorren los componentes de un ordenador, uno por uno: qué función cumple cada uno, cómo se conectan entre sí, qué gamas y características existen y cómo elegirlos con criterio según el uso que se le vaya a dar al equipo. La unidad termina con el **proceso de arranque**: lo que ocurre desde que se pulsa el botón de encendido hasta que el sistema operativo está listo.

## Resultado de aprendizaje

Esta unidad trabaja el **RA1** del módulo *Sistemas Informáticos* (0483): **evalúa sistemas informáticos, identificando sus componentes y características**.

| Criterio de evaluación | Dónde se trabaja |
| --- | --- |
| a) Se han reconocido los componentes físicos de un sistema informático y sus mecanismos de interconexión. | Apartados 1 a 8 |
| b) Se han clasificado los tipos de memorias, señalando sus características e identificando sus prestaciones y la función que desarrollan en el conjunto del sistema. | Apartados 4 y 8 |
| c) Se ha verificado el proceso de puesta en marcha de un equipo. | Apartado 10 |
| d) Se han clasificado, instalado y configurado diferentes tipos de dispositivos periféricos. | Apartados 7 y 9 |
| e) Se han identificado los tipos de redes y sistemas de comunicación. | Apartado 7 (tarjetas de red y estándares Wi-Fi). Se amplía en la Unidad 5. |
| f) Se han identificado los componentes de una red informática. | Apartado 7 (introducción). Se amplía en la Unidad 5. |
| g) Se han interpretado mapas físicos y lógicos de una red informática. | Pendiente de desarrollar. |
| h) Se han reconocido las normas de seguridad y prevención de riesgos laborales en el uso de los sistemas informáticos. | Pendiente de desarrollar. |

## Contenido de la unidad

**Bloque A: el núcleo del equipo**

| Apartado | Qué aprenderás |
| --- | --- |
| [1. Placas base]({{< relref "/U01/U01.01-Placas-Base-CAS.md" >}}) | Arquitectura de un ordenador, formatos de placa base, *sockets*, *chipset*, ranuras y conectores. |
| [2. Procesadores]({{< relref "/U01/U01.02-Procesadores-CAS.md" >}}) | Intel frente a AMD, gamas, generaciones, cómo interpretar el modelo y arquitectura híbrida (P-core y E-core). |
| [3. Disipadores]({{< relref "/U01/U01.03-Disipadores-CAS.md" >}}) | Refrigeración pasiva, activa y líquida, y cuándo conviene cada una. |
| [4. RAM]({{< relref "/U01/U01.04-RAM-CAS.md" >}}) | SRAM y DRAM, jerarquía de memorias, generaciones DDR, formatos y perfiles XMP y EXPO. |
| [5. Fuentes de alimentación]({{< relref "/U01/U01.05-FAlimentacion-CAS.md" >}}) | Estándares, tipos de cableado, certificaciones y cálculo de la potencia necesaria. |

**Bloque B: tarjetas, almacenamiento y periféricos**

| Apartado | Qué aprenderás |
| --- | --- |
| [6. Tarjetas gráficas]({{< relref "/U01/U01.06-Graficas-CAS.md" >}}) | Fabricantes, componentes y características de una GPU, y su relación con la fuente de alimentación. |
| [7. Tarjetas de red y multimedia]({{< relref "/U01/U01.07-TRed-CAS.md" >}}) | Tarjetas de red internas y externas, estándares Wi-Fi, tarjetas de sonido, de puertos y controladoras de discos. |
| [8. Almacenamiento]({{< relref "/U01/U01.08-Almacenamiento-CAS.md" >}}) | Discos HDD y SSD, tipos de SSD según controlador e interfaz, velocidades y características. |
| [9. Monitores y cables]({{< relref "/U01/U01.09-Monitores-Cables-CAS.md" >}}) | Tamaño, resolución, tasa de refresco, tipos de panel y cables de vídeo (HDMI y DisplayPort). |

**Bloque C: puesta en marcha**

| Apartado | Qué aprenderás |
| --- | --- |
| [10. Proceso de arranque de un sistema operativo]({{< relref "/U01/U01.10-Carga-Sistema-CAS.md" >}}) | Fases del arranque, BIOS frente a UEFI, arranque de Linux con GRUB y de Windows, y problemas habituales. |

## Cómo trabajar la unidad

Esta unidad es más teórica que las siguientes, pero se entiende mucho mejor si se relaciona con equipos reales:

- **Mira tu propio equipo.** En Windows, el *Administrador de tareas* (pestaña *Rendimiento*) y la herramienta `msinfo32` muestran el procesador, la memoria y los discos instalados. En Linux, órdenes como `lscpu`, `lsblk` y `free -h` ofrecen la misma información.
- **Consulta las especificaciones oficiales** de los fabricantes (Intel, AMD, NVIDIA, fabricantes de placas base) cuando tengas dudas sobre un modelo concreto.
- **Compara configuraciones** en tiendas en línea: las actividades de cada apartado te pedirán elegir componentes para un uso determinado y justificar la elección.

> [!TIP]
> El mercado del hardware cambia cada pocos meses. Más que memorizar modelos concretos, céntrate en **entender las características** (núcleos, frecuencia, tipo de memoria, interfaz del disco…) que te permitirán comparar cualquier modelo nuevo.

> [!CAUTION]
> Antes de abrir un ordenador, **apágalo y desconéctalo de la corriente**, y descarga la electricidad estática de tu cuerpo tocando una superficie metálica sin pintar o utilizando una pulsera antiestática. Manipula los componentes por los bordes, sin tocar los contactos.
