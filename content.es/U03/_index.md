---
title: "Unidad 3"
weight: 3
translationKey: "u03"
bookCollapseSection: true
---

# Unidad 3. Automatización de tareas y *shell scripting*

Como futuros desarrolladores, vais a desplegar vuestras aplicaciones en servidores, contenedores y *pipelines* de integración continua. En casi todos esos entornos **no hay interfaz gráfica**: se trabaja a través de una **terminal** y de pequeños programas llamados **scripts** que automatizan las tareas repetitivas, como las copias de seguridad, la creación de usuarios o la vigilancia del sistema.

Esta unidad tiene tres partes:

1. **La terminal**: moverse por el sistema de ficheros, manipular ficheros y texto, y gestionar el almacenamiento y las copias de seguridad.
2. **Programación de scripts en Bash**: variables, condiciones, bucles, funciones, parámetros y programación de tareas con `cron`.
3. ***Scripting* en PowerShell**: la sintaxis equivalente en Windows y el Programador de tareas.

## Resultado de aprendizaje

Esta unidad trabaja el **RA3** del módulo *Sistemas Informáticos* (0483): **gestiona la información del sistema identificando las estructuras de almacenamiento y aplicando medidas para asegurar la integridad de los datos**.

| Criterio de evaluación | Dónde se trabaja |
| --- | --- |
| a) Se han comparado sistemas de archivos. | Apartado 4.1 |
| b) Se ha identificado la estructura y función de los directorios del sistema operativo. | Apartado 1.5 |
| c) Se han utilizado herramientas en entorno gráfico y comandos para localizar información en el sistema de archivos. | Apartados 2.6, 3.4 y 4.3 |
| d) Se han creado diferentes tipos de particiones y unidades lógicas. | Apartado 4.2 |
| e) Se han realizado y restaurado copias de seguridad. | Apartados 4.4 y 11, ejercicios 11.2 y 13 |
| f) Se han planificado y automatizado tareas. | Apartados 5 a 13 |
| g) Se han instalado y evaluado utilidades relacionadas con la gestión de información. | Apartados 4.3 y 4.4 |

## Contenido de la unidad

**Bloque A: la terminal**

| Apartado | Qué aprenderás |
| --- | --- |
| [1. Introducción a la shell]({{< relref "/U03/U03.01-Introduccion-Shell-CAS.md" >}}) | Qué es una shell, sintaxis de los comandos, ayuda, estructura de directorios, rutas y comodines. |
| [2. Ficheros, directorios y permisos]({{< relref "/U03/U03.02-Ficheros-Permisos-CAS.md" >}}) | Crear, copiar, mover, buscar ficheros y dar permiso de ejecución. |
| [3. Redirecciones, tuberías y filtros]({{< relref "/U03/U03.03-Redirecciones-Filtros-CAS.md" >}}) | Combinar comandos y procesar texto. |
| [4. Almacenamiento y copias de seguridad]({{< relref "/U03/U03.04-Almacenamiento-Copias-CAS.md" >}}) | Sistemas de archivos, particiones, volúmenes lógicos, localización de información y copias. |

**Bloque B: *scripting* en Bash**

| Apartado | Qué aprenderás |
| --- | --- |
| [5. Primeros scripts]({{< relref "/U03/U03.05-Primeros-Scripts-CAS.md" >}}) | Crear, ejecutar y depurar un script. |
| [6. Variables, parámetros y E/S]({{< relref "/U03/U03.06-Variables-Parametros-CAS.md" >}}) | Tipos de datos, operadores, argumentos y lectura del teclado. |
| [7. Condiciones]({{< relref "/U03/U03.07-Condiciones-CAS.md" >}}) | Tomar decisiones con `if`, `case` y `[[ ]]`. |
| [8. Bucles]({{< relref "/U03/U03.08-Bucles-CAS.md" >}}) | Repetir acciones con `for`, `while`, `until` y menús. |
| [9. Funciones]({{< relref "/U03/U03.09-Funciones-CAS.md" >}}) | Organizar el código en bloques reutilizables. |
| [10. Arrays y cadenas]({{< relref "/U03/U03.10-Arrays-Cadenas-CAS.md" >}}) | Trabajar con listas de datos y manipular texto. |
| [11. Automatización de tareas]({{< relref "/U03/U03.11-Automatizacion-CAS.md" >}}) | `cron`, `at`, *timers* de systemd y el Programador de tareas de Windows. |
| [12. Ejercicios resueltos]({{< relref "/U03/U03.12-Ejercicios-Resueltos-CAS.md" >}}) | Problemas integradores: copias, creación masiva de usuarios, monitorización… |

**Bloque C: *scripting* en Windows**

| Apartado | Qué aprenderás |
| --- | --- |
| [13. Scripting en PowerShell]({{< relref "/U03/U03.13-Powershell-CAS.md" >}}) | Sintaxis de PowerShell, parámetros, cmdlets de uso habitual y ejercicios equivalentes a los de Bash. |

## Entorno de prácticas

Para seguir la unidad necesitas una shell Bash y, para el bloque C, un equipo con Windows. Para Bash, cualquiera de estas opciones vale (las vimos en la Unidad 2):

- **WSL** en Windows, con una distribución Ubuntu o Debian.
- Una **máquina virtual** con Linux (imprescindible para practicar con particiones y discos).
- Un **contenedor Docker** desechable, ideal para practicar sin miedo a romper nada:

```bash
# Arranca un contenedor Ubuntu interactivo que se borra al salir (--rm)
docker run -it --rm ubuntu:latest bash
```

> [!TIP]
> Practica **siempre** escribiendo los comandos tú mismo en lugar de copiarlos y pegarlos. La memoria muscular es parte del aprendizaje y te ayudará a detectar errores de sintaxis.
