---
title: 11. WSL
weight: 11
---

# **Windows Subsystem for Linux (WSL)**


## **1. ¿Qué es WSL?**
**Windows Subsystem for Linux (WSL)** es una **capacidad de Windows** que permite ejecutar un **entorno Linux completo** directamente en Windows sin necesidad de una máquina virtual tradicional, un *dual boot* o un servidor remoto. Fue desarrollado por Microsoft en colaboración con la comunidad de Linux para ofrecer una experiencia de desarrollo integrada y eficiente.

### **Características principales**
- **Integración nativa**: Ejecuta binarios de Linux directamente en Windows, con un rendimiento cercano al nativo.
- **Sin sobrecarga de máquina virtual**: A diferencia de soluciones como VirtualBox o VMware, WSL no requiere una VM completa, lo que reduce el consumo de recursos.
- **Acceso al sistema de archivos de Windows**: Los archivos de Linux y Windows son accesibles desde ambos entornos.
- **Soporte para GUI y GPU**: WSL 2 permite ejecutar aplicaciones gráficas de Linux (GUI) y aprovechar la aceleración por hardware (GPU) para tareas como *machine learning* o desarrollo gráfico.
- **Compatibilidad con herramientas de desarrollo**: Ideal para desarrolladores que necesitan herramientas como `git`, `docker`, `python`, `node.js`, etc., en un entorno Linux.

---

## **2. Versiones de WSL**
Existen dos versiones de WSL, cada una con sus propias características:


Comparativa entre WSL 1 y WSL 2


| **Aspecto**               | **WSL 1**                                      | **WSL 2**                                      |
|---------------------------|------------------------------------------------|------------------------------------------------|
| **Arquitectura**          | Capa de traducción de llamadas del sistema.   | Máquina virtual ligera con kernel Linux real. |
| **Rendimiento**           | Bueno para operaciones de archivos.           | Mejor rendimiento general (especialmente para operaciones de sistema de archivos y red). |
| **Compatibilidad**        | 100% compatible con llamadas del sistema.     | Compatible con la mayoría de aplicaciones, pero puede haber diferencias en llamadas del sistema. |
| **Soporte para GUI**      | No soporta aplicaciones gráficas.             | Soporta aplicaciones GUI de Linux.             |
| **Soporte para Docker**   | Requiere configuración adicional.              | Integración nativa con Docker Desktop.         |
| **Consumo de recursos**   | Bajo.                                          | Mayor (por la VM ligera).                      |
| **Tiempo de inicio**      | Rápido.                                        | Más lento (por la inicialización de la VM).    |
| **Kernel**                | Usa el kernel de Windows.                      | Usa un kernel Linux real.                      |

---
---

## **3. Requisitos para WSL en Windows 11**
- **Versión de Windows**: Windows 11 (versión 22000 o superior).
- **Arquitectura**: x64, ARM64 o x86 (WSL 2 solo está disponible para x64 y ARM64).
- **Virtualización**: Debe estar habilitada en la BIOS/UEFI (tecnología **VT-x** para Intel o **AMD-V** para AMD).
- **Espacio en disco**: Al menos 1 GB para la instalación de WSL y la distribución de Linux.

---

## **4. Instalación de WSL en Windows 11**

### **4.1 Habilitar WSL**
1. **Abrir PowerShell como administrador** y ejecutar:
```powershell
wsl --install
```
- Este comando instalará WSL 2 y la distribución de Linux por defecto (Ubuntu).

2. **Reiniciar el equipo** para aplicar los cambios.

3. **Verificar la instalación**:
```powershell
wsl --list --verbose
```
- Muestra las distribuciones de Linux instaladas y su versión de WSL.

---

### **4.2 Instalar una distribución de Linux**
1. **Desde Microsoft Store**:
- Abrir la tienda y buscar la distribución deseada (ej. Ubuntu, Debian, Kali Linux, Fedora, etc.).
- Hacer clic en **"Obtener"** para descargarla e instalarla.

2. **Desde PowerShell**:
- Listar las distribuciones disponibles:
```powershell
wsl --list --online
```
- Instalar una distribución específica (ej. Debian):
```powershell
wsl --install -d Debian
```

3. **Iniciar la distribución**:
- Buscar la distribución instalada en el menú de inicio (ej. "Ubuntu") y abrirla.
- Se abrirá una terminal de Linux donde se deberá configurar el usuario y contraseña.

---

### **4.3 Configurar WSL 2 como versión predeterminada (ya no es necesario)**
1. **Verificar la versión de WSL**:
```powershell
wsl --set-default-version 2
```
- Esto establece WSL 2 como la versión predeterminada para nuevas distribuciones.

2. **Convertir una distribución existente a WSL 2**:
```powershell
wsl --set-version [NombreDistribución] 2
```
- Ejemplo:
```powershell
wsl --set-version Ubuntu 2
```

---

## **5. Configuración Avanzada de WSL**

### **5.1 Configurar el kernel de WSL 2**
- WSL 2 utiliza un kernel Linux personalizado por Microsoft. Para actualizarlo manualmente:
1. Descargar el kernel desde el repositorio oficial de Microsoft:
[WSL 2 Kernel](https://github.com/microsoft/WSL2-Linux-Kernel).
2. Reemplazar el archivo `kernel` en la carpeta de instalación de WSL.

---

### **5.2 Limitar el uso de recursos**
Es posible limitar el uso de CPU, memoria RAM y espacio en disco para WSL 2 editando el archivo `.wslconfig` en el directorio de usuario de Windows (`C:\Users\[TuUsuario]\.wslconfig`).

**Ejemplo de `.wslconfig`**:
```ini
[wsl2]
memory=4GB    # Limita la memoria RAM a 4 GB
processors=2   # Limita el uso a 2 núcleos de CPU
swap=1GB       # Limita el espacio de swap a 1 GB
swapFile=C:\\temp\\wsl-swap.vhdx  # Ubicación personalizada del archivo de swap
```

- **Aplicar los cambios**:
- Reiniciar WSL:
```powershell
wsl --shutdown
```

---

### **5.3 Acceder a archivos de Windows desde Linux**
- Los archivos de Windows están montados automáticamente en `/mnt/` dentro de WSL.
  - Ejemplo: `C:\Users` está disponible en `/mnt/c/Users/`.
- **Permisos**: Los archivos creados desde Linux pueden tener problemas de permisos en Windows. Para solucionarlo, se puede usar el comando:
```bash
sudo umount /mnt/c
sudo mount -t drvfs C: /mnt/c -o uid=$(id -u),gid=$(id -g)
```

---

### **5.4 Integrar WSL con Docker**
1. **Instalar Docker Desktop** desde [Docker Official Website](https://www.docker.com/products/docker-desktop/).
2. **Habilitar la integración con WSL**:
- Abrir Docker Desktop.
- Ir a **Settings > Resources > WSL Integration**.
- Seleccionar la distribución de WSL que se desea integrar.
3. **Verificar la integración**:
- Desde la terminal de WSL, ejecutar:
```bash
docker --version
```
- Si está configurado correctamente, mostrará la versión de Docker.

---

### **5.5 Configurar una GUI para WSL 2**
1. **Instalar un servidor X en Windows**:
- Opciones populares:
- [VcXsrv](https://sourceforge.net/projects/vcxsrv/)
- [X410](https://x410.dev/)
- [GWSL](https://www.microsoft.com/store/productId/9NL6KD1H33V3) (para Windows 11).
2. **Configurar la variable de entorno `DISPLAY` en WSL**:
- Editar el archivo `~/.bashrc` o `~/.zshrc` y añadir:
```bash
export DISPLAY=$(grep -m 1 nameserver /etc/resolv.conf | awk '{print $2}'):0.0
export LIBGL_ALWAYS_INDIRECT=1
```
- Recargar el archivo:
```bash
source ~/.bashrc
```
3. **Probar con una aplicación gráfica**:
- Instalar una aplicación gráfica en WSL (ej. `x11-apps` en Ubuntu):
```bash
sudo apt update && sudo apt install x11-apps
```
- Ejecutar:
```bash
xeyes
```
- Debería aparecer una ventana con los ojos de X11.

---

## **6. Comandos Principales de WSL**

### **6.1 Comandos en PowerShell (Windows)**

Comandos de WSL en PowerShell


| **Comando**                              | **Descripción**                                                                                     |
|------------------------------------------|-----------------------------------------------------------------------------------------------------|
| `wsl --install`                          | Instala WSL y la distribución por defecto (Ubuntu).                                                |
| `wsl --list --verbose`                   | Lista las distribuciones instaladas y su versión de WSL.                                           |
| `wsl --list --online`                    | Lista las distribuciones disponibles para instalar.                                              |
| `wsl --install -d [Distribución]`        | Instala una distribución específica de Linux.                                                     |
| `wsl --unregister [Distribución]`        | Desinstala una distribución de WSL.                                                               |
| `wsl --set-default [Distribución]`      | Establece la distribución predeterminada.                                                         |
| `wsl --set-version [Distribución] [Versión]` | Cambia la versión de WSL (1 o 2) para una distribución.                                           |
| `wsl --set-default-version [Versión]`   | Establece la versión predeterminada de WSL para nuevas distribuciones.                             |
| `wsl --shutdown`                         | Apaga todas las distribuciones de WSL.                                                             |
| `wsl --update`                           | Actualiza el kernel de WSL 2.                                                                       |
| `wsl -l -v`                              | Lista las distribuciones instaladas con detalles.                                                  |
| `wsl -t [Distribución]`                  | Detiene una distribución específica.                                                              |
| `wsl -d [Distribución]`                  | Inicia una distribución específica.                                                               |

---

### **6.2 Comandos dentro de WSL (Linux)**

Comandos útiles dentro de WSL


| **Comando**                              | **Descripción**                                                                                     |
|------------------------------------------|-----------------------------------------------------------------------------------------------------|
| `sudo apt update && sudo apt upgrade -y` | Actualiza los paquetes en distribuciones basadas en Debian/Ubuntu.                                  |
| `sudo dnf update -y`                     | Actualiza los paquetes en distribuciones basadas en Fedora.                                         |
| `wslpath [RutaWindows]`                  | Convierte una ruta de Windows a una ruta de WSL (ej. `wslpath "C:\Users"` → `/mnt/c/Users`).        |
| `explorer.exe .`                         | Abre el explorador de archivos de Windows en la carpeta actual de WSL.                              |
| `code .`                                 | Abre Visual Studio Code en la carpeta actual de WSL (requiere VS Code con la extensión *Remote - WSL*). |
| `ping [IP/Dominio]`                      | Prueba la conectividad de red desde WSL.                                                           |
| `ip addr`                                | Muestra la dirección IP de la interfaz de red de WSL.                                              |
| `cat /etc/os-release`                    | Muestra información sobre la distribución de Linux instalada.                                     |
| `df -h`                                  | Muestra el uso del espacio en disco en WSL.                                                        |
| `free -h`                                | Muestra el uso de memoria RAM en WSL.                                                              |
| `htop`                                   | Muestra un monitor de procesos interactivo (requiere instalación: `sudo apt install htop`).       |
