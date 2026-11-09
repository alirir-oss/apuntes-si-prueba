---
title: "Unidad 4"
weight: 4
translationKey: "u04"
bookCollapseSection: true
---

# Unidad 4. Administración de sistemas operativos y usuarios

En la Unidad 3 aprendiste a moverte por la terminal y a automatizar tareas con scripts. En esta unidad das un paso más y te pones en el papel de **administrador del sistema**: decidir **quién** puede entrar en un equipo, **qué** puede hacer cada persona, **qué programas y servicios** se están ejecutando y **cómo está funcionando** el sistema.

Aunque vuestro perfil es el de desarrolladores, estas tareas forman parte de vuestro día a día: el servidor en el que desplegáis una aplicación necesita un usuario propio con los permisos justos, el servicio que la mantiene en marcha hay que arrancarlo y vigilarlo, y cuando algo falla, la primera pista está en los **registros del sistema**.

Cada apartado se estudia en los dos sistemas operativos más extendidos: **GNU/Linux** (con ejemplos en Ubuntu/Debian) y **Microsoft Windows**, utilizando tanto las herramientas gráficas como la línea de comandos (Bash, CMD y PowerShell).

## Resultado de aprendizaje

Esta unidad trabaja el **RA4** del módulo *Sistemas Informáticos* (0483): **gestiona sistemas operativos utilizando comandos y herramientas gráficas y evaluando las necesidades del sistema**.

| Criterio de evaluación | Dónde se trabaja |
| --- | --- |
| a) Se han configurado cuentas de usuario locales y grupos. | Apartados 1, 2 y 3 |
| b) Se ha asegurado el acceso al sistema mediante el uso de directivas de cuenta y directivas de contraseñas. | Apartado 4 |
| c) Se han identificado, arrancado y detenido servicios y procesos. | Apartado 8 |
| d) Se ha protegido el acceso a la información mediante el uso de permisos locales y listas de control de acceso. | Apartados 5, 6 y 7 |
| e) Se han utilizado comandos para realizar las tareas básicas de configuración y administración del sistema. | Todos los apartados; resumen en el apartado 12.5 |
| f) Se ha monitorizado el sistema. | Apartados 9 y 10 |
| g) Se han instalado y evaluado utilidades para el mantenimiento y optimización del sistema. | Apartado 11 |
| h) Se han evaluado las necesidades del sistema informático en relación con el desarrollo de aplicaciones. | Apartado 12 |

## Contenido de la unidad

**Bloque A: usuarios y grupos**

| Apartado | Qué aprenderás |
| --- | --- |
| [1. Usuarios y grupos en Linux]({{< relref "/U04/U04.01-Usuarios-Grupos-Linux-CAS.md" >}}) | UID y GID, ficheros `/etc/passwd`, `/etc/shadow` y `/etc/group`, creación y gestión de cuentas, `sudo`. |
| [2. Usuarios y grupos en Windows]({{< relref "/U04/U04.02-Usuarios-Grupos-Windows-CAS.md" >}}) | Tipos de cuenta, SID, grupos integrados, UAC, gestión gráfica, con `net user` y con PowerShell. |
| [3. Perfiles de usuario]({{< relref "/U04/U04.03-Perfiles-Usuario-CAS.md" >}}) | Directorio personal, `/etc/skel`, ficheros de inicio de la shell y perfiles de Windows. |
| [4. Directivas de cuenta y de contraseñas]({{< relref "/U04/U04.04-Politicas-Contrasenas-CAS.md" >}}) | Caducidad, complejidad y bloqueo de cuentas en Linux (`chage`, PAM) y en Windows (`secpol.msc`, `net accounts`). |

**Bloque B: protección de la información**

| Apartado | Qué aprenderás |
| --- | --- |
| [5. Permisos en Linux: UGO avanzado y ACL]({{< relref "/U04/U04.05-Permisos-Linux-ACL-CAS.md" >}}) | `chown`, `umask`, SUID, SGID, *sticky bit* y listas de control de acceso (`setfacl`). |
| [6. Permisos NTFS en Windows]({{< relref "/U04/U04.06-Permisos-NTFS-CAS.md" >}}) | Permisos estándar y especiales, herencia, permisos efectivos, `icacls` y `Get-Acl`. |
| [7. Compartición de recursos en red]({{< relref "/U04/U04.07-Comparticion-Recursos-CAS.md" >}}) | Carpetas e impresoras compartidas en Windows y servidor Samba en Linux. |

**Bloque C: procesos, monitorización y mantenimiento**

| Apartado | Qué aprenderás |
| --- | --- |
| [8. Procesos y servicios]({{< relref "/U04/U04.08-Procesos-Servicios-CAS.md" >}}) | Listar, terminar y priorizar procesos; arrancar, detener y habilitar servicios con `systemctl` y PowerShell. |
| [9. Monitorización de recursos]({{< relref "/U04/U04.09-Monitorizacion-CAS.md" >}}) | `top`, `htop`, `free`, `df`, Administrador de tareas, Monitor de recursos y Monitor de rendimiento. |
| [10. Registros del sistema]({{< relref "/U04/U04.10-Registros-Sistema-CAS.md" >}}) | `/var/log`, `journalctl`, `logrotate`, Visor de eventos y `Get-WinEvent`. |
| [11. Mantenimiento y optimización]({{< relref "/U04/U04.11-Mantenimiento-Optimizacion-CAS.md" >}}) | Actualizaciones, limpieza, salud del disco, arranque y evaluación de utilidades. |
| [12. El sistema al servicio del desarrollo]({{< relref "/U04/U04.12-Sistema-Desarrollo-CAS.md" >}}) | Requisitos según el tipo de desarrollo, preparación del equipo, caso práctico integrador y resumen de comandos. |

## Entorno de prácticas

Muchas de las tareas de esta unidad **modifican la configuración de seguridad del sistema**. Por eso, practica siempre en **máquinas virtuales** (Unidad 2) y haz una **instantánea** antes de empezar cada apartado; si algo sale mal, podrás volver atrás en segundos.

| Máquina | Uso en la unidad |
| --- | --- |
| **Ubuntu 26.04 LTS** (escritorio o servidor) o **Debian 13** | Todos los apartados de Linux. Para el servidor Samba (apartado 7) basta con una versión servidor sin entorno gráfico. |
| **Windows 11 Pro o Education** | Todos los apartados de Windows. |
| **Windows Server 2025** (opcional) | Compartición de recursos. En la Unidad 6 se usará para los dominios. |

> [!WARNING]
> La edición **Windows 11 Home** no incluye varias de las herramientas que se usan en esta unidad: *Usuarios y grupos locales* (`lusrmgr.msc`), *Directiva de seguridad local* (`secpol.msc`) ni el *Editor de directivas de grupo local* (`gpedit.msc`). Si en casa tienes la edición Home, usa una máquina virtual con la edición Pro o Education (el centro facilita las licencias educativas) o utiliza los comandos equivalentes (`net user`, `net accounts`, PowerShell), que sí funcionan en todas las ediciones.

Para poner en red las máquinas virtuales (apartado 7), configura sus adaptadores en modo **Red NAT** (*NAT Network*) o **Adaptador puente** en VirtualBox, de forma que todas estén en la misma red y puedan comunicarse entre sí.

> [!TIP]
> Mantén siempre **una segunda sesión de administrador abierta** (otra terminal con `sudo -i` o una consola de PowerShell elevada) mientras cambies contraseñas, directivas o permisos. Si te bloqueas a ti mismo, podrás deshacer el cambio desde esa sesión.
