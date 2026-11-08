---
title: 6. Actualizaciones de los Sistemas Operativos
weight: 6
---

Un SO recién instalado no está terminado: tiene meses de retraso en actualizaciones, probablemente le faltan controladores y todavía no tiene tus programas. Esta página cubre todo lo que haces desde que arranca el instalador hasta que el equipo queda listo para trabajar, y cómo se diagnostica cuando algo no va bien.


## 1. Instalación de un SO cliente

Un SO **cliente** es el que se instala en el equipo de una persona (escritorio o portátil): Windows 10/11, Ubuntu Desktop, Linux Mint... Se diferencia de un SO **servidor**, que estudiaremos en la página 4.

### 1.1 Windows

Las fases habituales de una instalación **limpia** son:

1. **Arrancar desde el medio** (USB o ISO) en modo UEFI.
2. **Idioma, formato de hora y teclado.**
3. **Clave de producto** (o «no tengo clave», si se activará después) y **edición** de Windows.
4. **Aceptar la licencia.**
5. **Tipo de instalación**: *Actualizar* (conserva archivos y aplicaciones de una versión anterior) o *Personalizada* (instalación limpia; es la que te deja elegir la partición).
6. **Selección de la partición**: aquí ves el disco y sus particiones; puedes eliminarlas, crear otras nuevas o elegir el espacio libre. Windows creará las particiones propias que necesite (EFI, MSR, recuperación).
7. **Copia de archivos y reinicios automáticos.** No quites el USB hasta que el equipo reinicie y empiece a cargar desde el disco.
8. **Configuración inicial** (OOBE): región, red, cuenta de usuario, privacidad. En algunas ediciones y versiones Windows exige conexión a Internet y una cuenta de Microsoft; los detalles cambian con las versiones.
9. **Primer inicio de sesión.**

#### Requisitos de Windows 11
Según las especificaciones publicadas por Microsoft, Windows 11 pide un procesador de 64 bits compatible, 4 GB de RAM, 64 GB de almacenamiento, firmware UEFI con Secure Boot y un módulo TPM 2.0. Consulta siempre la página oficial de la versión que instalas: los requisitos pueden cambiar.


### 1.2 Ubuntu (y otras distribuciones de escritorio)

1. **Arrancar desde el medio.** La ISO de Ubuntu ofrece un modo **live**: puedes probar todo el sistema desde el USB sin instalar nada, útil para comprobar que el hardware funciona antes de decidir.
2. **Idioma y teclado.**
3. **Tipo de instalación**, que determina qué pasa con el disco:
   - *Borrar el disco e instalar Ubuntu*: lo más simple, pero destruye lo que haya.
   - *Instalar junto a Windows* (dual boot): redimensiona lo que haya y añade Ubuntu.
   - *Instalación manual / «otra cosa»*: tú diseñas las particiones (EFI, `/`, `/home`, swap), como en la página anterior.
4. **Zona horaria, cuenta de usuario y contraseña.**
5. **Copia de archivos y reinicio.** Cuando el instalador lo pide, retira el medio.

Las opciones exactas del instalador cambian entre versiones, pero la estructura es siempre la misma: **idioma → disco → usuario → copia → reinicio**.

### 1.3 Configuración inicial

Cuando el sistema arranca por primera vez, antes de usarlo en serio, repasa:

| Qué | Por qué | Windows | Ubuntu |
|---|---|---|---|
| **Nombre del equipo** | Identifica la máquina en la red; debe ser único y sin espacios | `Rename-Computer -NewName AULA-PC01` (y reiniciar) | `sudo hostnamectl set-hostname aula-pc01` |
| **Cuenta de usuario** | Tener una cuenta de uso diario y no trabajar siempre como administrador | *Configuración → Cuentas* | `adduser`, o durante la instalación |
| **Red** | Comprobar que hay conexión y dirección | `ipconfig` | `ip a` |
| **Fecha, hora y zona** | Necesarias para certificados y registros | *Configuración → Hora e idioma* | `timedatectl` |
| **Actualizaciones** | Parches de seguridad (sección 2) | Windows Update | `apt update` y `apt upgrade` |

## 2. Actualizaciones

La imagen de instalación puede tener meses o años de antigüedad. Si no actualizas, tienes un sistema con vulnerabilidades conocidas desde el primer minuto. Por eso, **lo primero que se hace al acabar una instalación es actualizar**.

Hay tres tipos de actualización que conviene distinguir:

- **De seguridad**: corrigen vulnerabilidades. Son las más urgentes.
- **De calidad o errores**: corrigen fallos sin añadir funciones.
- **De funcionalidades**: añaden novedades o cambian la versión principal del SO. Pueden requerir más tiempo y una copia previa.

### 2.1 En Windows

Se actualiza desde **Windows Update** (*Configuración → Windows Update*). Descarga e instala parches y, a veces, controladores, y muchas piden un **reinicio**. Desde PowerShell puedes ver las actualizaciones instaladas:

```powershell
Get-HotFix                   # parches instalados
winget upgrade               # programas (no solo Windows) con actualización disponible
```

### 2.2 En Ubuntu y Debian

El gestor de paquetes `apt` trabaja en dos tiempos, y la confusión entre los dos es muy frecuente:

```bash
sudo apt update              # actualiza la LISTA de paquetes disponibles (no instala nada)
sudo apt upgrade             # INSTALA las actualizaciones de esa lista
apt list --upgradable        # muestra qué se puede actualizar
```

Hay que ejecutarlos **en ese orden**: `update` descarga las novedades del catálogo, y `upgrade` las aplica. Si solo haces `upgrade`, instalas según un catálogo desactualizado. Ubuntu también tiene el **Gestor de actualizaciones** gráfico y, por defecto, aplica automáticamente las actualizaciones de seguridad.


## 3. Instalar, desinstalar y actualizar aplicaciones

El criterio RA2.h pide saber gestionar las aplicaciones. La idea es la misma en todos los sistemas (**instalar, desinstalar, actualizar**), pero cambia la herramienta.

### 3.1 En Windows

Las aplicaciones se instalan de tres maneras:

- **Instalador descargado** (`.exe` o `.msi`) de la web del fabricante. Comprueba que es el sitio oficial.
- **Microsoft Store**.
- **Gestor de paquetes `winget`** (Windows Package Manager), que viene con las versiones actuales de Windows 10 y 11.

```powershell
winget search vlc            # busca paquetes
winget install VideoLAN.VLC  # instala por el identificador del paquete
winget upgrade --all         # actualiza todo lo que se pueda
winget uninstall VideoLAN.VLC
```

Para **desinstalar** de manera gráfica: *Configuración → Aplicaciones → Aplicaciones instaladas*. Una desinstalación puede dejar restos (configuración en las carpetas del usuario) que hay que limpiar a mano si quieres un borrado completo.

### 3.2 En Ubuntu

La fuente principal son los **repositorios** de la distribución: servidores que guardan paquetes verificados. Con `apt`:

```bash
apt search vlc               # busca
sudo apt install vlc         # instala (con sus dependencias)
sudo apt remove vlc          # desinstala, pero conserva la configuración
sudo apt purge vlc           # desinstala y borra también la configuración
sudo apt autoremove          # elimina dependencias que ya no usa nadie
```

También existen paquetes **Snap** (`snap install`) y **Flatpak**, que incluyen sus dependencias, y la instalación de archivos `.deb` descargados con `sudo apt install ./archivo.deb`.

### 3.3 Equivalencias

| Acción | Windows | Ubuntu |
|---|---|---|
| Buscar | `winget search` | `apt search` |
| Instalar | `winget install` | `sudo apt install` |
| Actualizar todo | `winget upgrade --all` | `sudo apt update && sudo apt upgrade` |
| Desinstalar | `winget uninstall` | `sudo apt remove` / `purge` |


## 4. Controladores de dispositivos

Un **controlador** (*driver*) es el software que permite al SO comunicarse con un componente concreto. Sin él, el dispositivo puede no funcionar, o funcionar con un controlador **genérico** que solo ofrece las funciones básicas: una tarjeta gráfica dedicada que muestra la imagen pero sin aceleración 3D, por ejemplo.

### 4.1 En Windows

Windows instala muchos controladores durante la instalación o poco después mediante Windows Update. Para hardware que busca el máximo rendimiento (sobre todo **tarjetas gráficas**), conviene bajar el controlador oficial más reciente desde la web del fabricante (NVIDIA, AMD, Intel...).

La pieza central es el **Administrador de dispositivos** (`devmgmt.msc`):

- Un **triángulo amarillo con un signo de exclamación** indica un dispositivo con problema; si se muestra como «Dispositivo desconocido», no hay controlador.
- En las **Propiedades** del dispositivo, la ficha *Controlador* permite **actualizar**, **revertir al controlador anterior** (útil cuando un controlador nuevo da problemas) o **desinstalar** el dispositivo para que Windows lo vuelva a detectar.
- Las propiedades muestran un código de error (por ejemplo, el código 28 significa que no se han instalado los controladores).

```powershell
Get-PnpDevice -PresentOnly | Where-Object { $_.Status -ne 'OK' }   # dispositivos con problemas
driverquery                                                          # controladores cargados
```

### 4.2 En Linux

El **núcleo** Linux lleva de serie controladores de código abierto para una gran cantidad de hardware, de modo que a menudo todo funciona sin hacer nada. Para hardware específico (otra vez, tarjetas gráficas) algunos sistemas, como Ubuntu, tienen un gestor de **«controladores adicionales»** que instala controladores propietarios.

Para diagnosticar:

```bash
lspci                        # hardware de bus PCI (gráfica, red...)
lspci -k                     # añade qué controlador (núcleo) usa cada dispositivo
lsusb                        # dispositivos USB
ubuntu-drivers devices       # controladores recomendados (Ubuntu)
sudo ubuntu-drivers install  # instala los recomendados
```

Si en `lspci -k` un dispositivo no muestra ninguna línea **Kernel driver in use**, ese componente no tiene controlador cargado.



### 4.3 Método de diagnóstico

Cuando un dispositivo no funciona, sigue siempre el mismo camino:

1. **Detecta**: ¿aparece en el sistema? (`Administrador de dispositivos`; `lspci`/`lsusb`).
2. **Identifica** el modelo exacto.
3. **Comprueba el estado** del controlador (aviso amarillo; línea *driver in use*).
4. **Actualiza** por Windows Update/gestor de paquetes o desde la web del fabricante.
5. **Vuelve atrás** si el problema empezó con una actualización (revertir controlador).
6. **Documenta** lo que has hecho y el resultado.

