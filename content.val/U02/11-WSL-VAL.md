---
title: "11. WSL"
weight: 11
---

# **Windows Subsystem for Linux (WSL)**


## **1. Què és WSL?**
**Windows Subsystem for Linux (WSL)** és una **funcionalitat de Windows** que permet executar un **entorn Linux complet** directament en Windows sense necessitat d'una màquina virtual tradicional, d'una arrencada dual (*dual boot*) o d'un servidor remot. El va desenvolupar Microsoft en col·laboració amb la comunitat de Linux per a oferir una experiència de desenvolupament integrada i eficient.

### **Característiques principals**
- **Integració nativa**: Executa binaris de Linux directament en Windows, amb un rendiment pròxim al natiu.
- **Sense la sobrecàrrega d'una màquina virtual**: A diferència de solucions com VirtualBox o VMware, WSL no requereix una VM completa, cosa que redueix el consum de recursos.
- **Accés al sistema de fitxers de Windows**: Els fitxers de Linux i de Windows són accessibles des dels dos entorns.
- **Suport per a GUI i GPU**: WSL 2 permet executar aplicacions gràfiques de Linux (GUI) i aprofitar l'acceleració per maquinari (GPU) per a tasques com l'aprenentatge automàtic (*machine learning*) o el desenvolupament gràfic.
- **Compatibilitat amb eines de desenvolupament**: Ideal per a desenvolupadors que necessiten eines com `git`, `docker`, `python`, `node.js`, etc., en un entorn Linux.

---

## **2. Versions de WSL**
Hi ha dues versions de WSL, cadascuna amb les seves pròpies característiques:


Comparativa entre WSL 1 i WSL 2


| **Aspecte**               | **WSL 1**                                      | **WSL 2**                                      |
|---------------------------|------------------------------------------------|------------------------------------------------|
| **Arquitectura**          | Capa de traducció de crides al sistema.   | Màquina virtual lleugera amb un kernel Linux real. |
| **Rendiment**           | Bo per a operacions amb fitxers.           | Millor rendiment general (especialment en operacions del sistema de fitxers i de xarxa). |
| **Compatibilitat**        | 100% compatible amb les crides al sistema.     | Compatible amb la majoria d'aplicacions, però hi pot haver diferències en les crides al sistema. |
| **Suport per a GUI**      | No admet aplicacions gràfiques.             | Admet aplicacions GUI de Linux.             |
| **Suport per a Docker**   | Requereix configuració addicional.              | Integració nativa amb Docker Desktop.         |
| **Consum de recursos**   | Baix.                                          | Més alt (per la VM lleugera).                      |
| **Temps d'inici**      | Ràpid.                                        | Més lent (per la inicialització de la VM).    |
| **Kernel**                | Utilitza el kernel de Windows.                      | Utilitza un kernel Linux real.                      |

---
---

## **3. Requisits per a WSL en Windows 11**
- **Versió de Windows**: Windows 11 (versió 22000 o superior).
- **Arquitectura**: x64, ARM64 o x86 (WSL 2 només està disponible per a x64 i ARM64).
- **Virtualització**: Ha d'estar habilitada en la BIOS/UEFI (tecnologia **VT-x** per a Intel o **AMD-V** per a AMD).
- **Espai en disc**: Almenys 1 GB per a la instal·lació de WSL i de la distribució de Linux.

---

## **4. Instal·lació de WSL en Windows 11**

### **4.1 Habilitar WSL**
1. **Obrir PowerShell com a administrador** i executar:
```powershell
wsl --install
```
- Aquesta ordre instal·larà WSL 2 i la distribució de Linux per defecte (Ubuntu).

2. **Reiniciar l'equip** per a aplicar els canvis.

3. **Verificar la instal·lació**:
```powershell
wsl --list --verbose
```
- Mostra les distribucions de Linux instal·lades i la seva versió de WSL.

---

### **4.2 Instal·lar una distribució de Linux**
1. **Des de Microsoft Store**:
- Obrir la botiga i buscar la distribució desitjada (p. ex. Ubuntu, Debian, Kali Linux, Fedora, etc.).
- Fer clic en **"Obtindre"** per a descarregar-la i instal·lar-la.

2. **Des de PowerShell**:
- Llistar les distribucions disponibles:
```powershell
wsl --list --online
```
- Instal·lar una distribució específica (p. ex. Debian):
```powershell
wsl --install -d Debian
```

3. **Iniciar la distribució**:
- Buscar la distribució instal·lada en el menú d'inici (p. ex. "Ubuntu") i obrir-la.
- S'obrirà un terminal de Linux on caldrà configurar l'usuari i la contrasenya.

---

### **4.3 Configurar WSL 2 com a versió predeterminada (ja no és necessari)**
1. **Establir la versió de WSL**:
```powershell
wsl --set-default-version 2
```
- Això estableix WSL 2 com a versió predeterminada per a les distribucions noves.

2. **Convertir una distribució existent a WSL 2**:
```powershell
wsl --set-version [NomDistribució] 2
```
- Exemple:
```powershell
wsl --set-version Ubuntu 2
```

---

## **5. Configuració Avançada de WSL**

### **5.1 Configurar el kernel de WSL 2**
- WSL 2 utilitza un kernel Linux personalitzat per Microsoft. Per a actualitzar-lo manualment:
1. Descarregar el kernel des del repositori oficial de Microsoft:
[WSL 2 Kernel](https://github.com/microsoft/WSL2-Linux-Kernel).
2. Substituir el fitxer `kernel` en la carpeta d'instal·lació de WSL.

---

### **5.2 Limitar l'ús de recursos**
Es pot limitar l'ús de CPU, de memòria RAM i d'espai en disc de WSL 2 editant el fitxer `.wslconfig` en el directori d'usuari de Windows (`C:\Users\[ElTeuUsuari]\.wslconfig`).

**Exemple de `.wslconfig`**:
```ini
[wsl2]
memory=4GB    # Limita la memòria RAM a 4 GB
processors=2   # Limita l'ús a 2 nuclis de CPU
swap=1GB       # Limita l'espai de swap a 1 GB
swapFile=C:\\temp\\wsl-swap.vhdx  # Ubicació personalitzada del fitxer de swap
```

- **Aplicar els canvis**:
- Reiniciar WSL:
```powershell
wsl --shutdown
```

---

### **5.3 Accedir als fitxers de Windows des de Linux**
- Els fitxers de Windows es munten automàticament en `/mnt/` dins de WSL.
  - Exemple: `C:\Users` està disponible en `/mnt/c/Users/`.
- **Permisos**: Els fitxers creats des de Linux poden tindre problemes de permisos en Windows. Per a solucionar-ho, es pot utilitzar l'ordre:
```bash
sudo umount /mnt/c
sudo mount -t drvfs C: /mnt/c -o uid=$(id -u),gid=$(id -g)
```

---

### **5.4 Integrar WSL amb Docker**
1. **Instal·lar Docker Desktop** des del [web oficial de Docker](https://www.docker.com/products/docker-desktop/).
2. **Habilitar la integració amb WSL**:
- Obrir Docker Desktop.
- Anar a **Settings > Resources > WSL Integration**.
- Seleccionar la distribució de WSL que es vol integrar.
3. **Verificar la integració**:
- Des del terminal de WSL, executar:
```bash
docker --version
```
- Si està configurat correctament, mostrarà la versió de Docker.

---

### **5.5 Configurar una GUI per a WSL 2**
1. **Instal·lar un servidor X en Windows**:
- Opcions populars:
- [VcXsrv](https://sourceforge.net/projects/vcxsrv/)
- [X410](https://x410.dev/)
- [GWSL](https://www.microsoft.com/store/productId/9NL6KD1H33V3) (per a Windows 11).
2. **Configurar la variable d'entorn `DISPLAY` en WSL**:
- Editar el fitxer `~/.bashrc` o `~/.zshrc` i afegir:
```bash
export DISPLAY=$(grep -m 1 nameserver /etc/resolv.conf | awk '{print $2}'):0.0
export LIBGL_ALWAYS_INDIRECT=1
```
- Tornar a carregar el fitxer:
```bash
source ~/.bashrc
```
3. **Provar-ho amb una aplicació gràfica**:
- Instal·lar una aplicació gràfica en WSL (p. ex. `x11-apps` en Ubuntu):
```bash
sudo apt update && sudo apt install x11-apps
```
- Executar:
```bash
xeyes
```
- Hauria d'aparéixer una finestra amb els ulls d'X11.

---

## **6. Ordres Principals de WSL**

### **6.1 Ordres en PowerShell (Windows)**

Ordres de WSL en PowerShell


| **Ordre**                              | **Descripció**                                                                                     |
|------------------------------------------|-----------------------------------------------------------------------------------------------------|
| `wsl --install`                          | Instal·la WSL i la distribució per defecte (Ubuntu).                                                |
| `wsl --list --verbose`                   | Llista les distribucions instal·lades i la seva versió de WSL.                                           |
| `wsl --list --online`                    | Llista les distribucions disponibles per a instal·lar.                                              |
| `wsl --install -d [Distribució]`        | Instal·la una distribució específica de Linux.                                                     |
| `wsl --unregister [Distribució]`        | Desinstal·la una distribució de WSL.                                                               |
| `wsl --set-default [Distribució]`      | Estableix la distribució predeterminada.                                                         |
| `wsl --set-version [Distribució] [Versió]` | Canvia la versió de WSL (1 o 2) d'una distribució.                                           |
| `wsl --set-default-version [Versió]`   | Estableix la versió predeterminada de WSL per a les distribucions noves.                             |
| `wsl --shutdown`                         | Apaga totes les distribucions de WSL.                                                             |
| `wsl --update`                           | Actualitza el kernel de WSL 2.                                                                       |
| `wsl -l -v`                              | Llista les distribucions instal·lades amb detalls.                                                  |
| `wsl -t [Distribució]`                  | Atura una distribució específica.                                                              |
| `wsl -d [Distribució]`                  | Inicia una distribució específica.                                                               |

---

### **6.2 Ordres dins de WSL (Linux)**

Ordres útils dins de WSL


| **Ordre**                              | **Descripció**                                                                                     |
|------------------------------------------|-----------------------------------------------------------------------------------------------------|
| `sudo apt update && sudo apt upgrade -y` | Actualitza els paquets en distribucions basades en Debian/Ubuntu.                                  |
| `sudo dnf update -y`                     | Actualitza els paquets en distribucions basades en Fedora.                                         |
| `wslpath [RutaWindows]`                  | Converteix una ruta de Windows en una ruta de WSL (p. ex. `wslpath "C:\Users"` → `/mnt/c/Users`).        |
| `explorer.exe .`                         | Obri l'explorador de fitxers de Windows en la carpeta actual de WSL.                              |
| `code .`                                 | Obri Visual Studio Code en la carpeta actual de WSL (requereix VS Code amb l'extensió *Remote - WSL*). |
| `ping [IP/Domini]`                      | Prova la connectivitat de xarxa des de WSL.                                                           |
| `ip addr`                                | Mostra l'adreça IP de la interfície de xarxa de WSL.                                              |
| `cat /etc/os-release`                    | Mostra informació sobre la distribució de Linux instal·lada.                                     |
| `df -h`                                  | Mostra l'ús de l'espai en disc en WSL.                                                        |
| `free -h`                                | Mostra l'ús de memòria RAM en WSL.                                                              |
| `htop`                                   | Mostra un monitor de processos interactiu (requereix instal·lació: `sudo apt install htop`).       |
