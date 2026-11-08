---
title: "6. Actualitzacions dels Sistemes Operatius"
weight: 6
---

Un SO acabat d'instal·lar no està enllestit: porta mesos de retard en actualitzacions, probablement li falten controladors i encara no té els teus programes. Aquesta pàgina tracta tot el que fas des que arrenca l'instal·lador fins que l'equip queda preparat per a treballar, i com es diagnostica quan alguna cosa no va bé.


## 1. Instal·lació d'un SO client

Un SO **client** és el que s'instal·la en l'equip d'una persona (escriptori o portàtil): Windows 10/11, Ubuntu Desktop, Linux Mint... Es diferencia d'un SO **servidor**, que estudiarem en la pàgina 4.

### 1.1 Windows

Les fases habituals d'una instal·lació **neta** són:

1. **Arrencar des del mitjà** (USB o ISO) en mode UEFI.
2. **Idioma, format d'hora i teclat.**
3. **Clau de producte** (o «no tinc clau», si s'activarà després) i **edició** de Windows.
4. **Acceptar la llicència.**
5. **Tipus d'instal·lació**: *Actualitzar* (conserva els fitxers i les aplicacions d'una versió anterior) o *Personalitzada* (instal·lació neta; és la que et permet triar la partició).
6. **Selecció de la partició**: ací veus el disc i les seves particions; pots eliminar-les, crear-ne de noves o triar l'espai lliure. Windows crearà les particions pròpies que necessite (EFI, MSR, recuperació).
7. **Còpia de fitxers i reinicis automàtics.** No lleves l'USB fins que l'equip es reinicie i comence a carregar des del disc.
8. **Configuració inicial** (OOBE): regió, xarxa, compte d'usuari, privadesa. En algunes edicions i versions, Windows exigeix connexió a Internet i un compte de Microsoft; els detalls canvien amb les versions.
9. **Primer inici de sessió.**

#### Requisits de Windows 11
Segons les especificacions publicades per Microsoft, Windows 11 demana un processador de 64 bits compatible, 4 GB de RAM, 64 GB d'emmagatzematge, microprogramari UEFI amb Secure Boot i un mòdul TPM 2.0. Consulta sempre la pàgina oficial de la versió que instal·les: els requisits poden canviar.


### 1.2 Ubuntu (i altres distribucions d'escriptori)

1. **Arrencar des del mitjà.** La ISO d'Ubuntu ofereix un mode **live**: pots provar tot el sistema des de l'USB sense instal·lar res, cosa útil per a comprovar que el maquinari funciona abans de decidir.
2. **Idioma i teclat.**
3. **Tipus d'instal·lació**, que determina què passa amb el disc:
   - *Esborrar el disc i instal·lar Ubuntu*: el més senzill, però destrueix el que hi haja.
   - *Instal·lar al costat de Windows* (arrencada dual): redimensiona el que hi haja i afegeix Ubuntu.
   - *Instal·lació manual / «una altra cosa»*: tu dissenyes les particions (EFI, `/`, `/home`, swap), com en la pàgina anterior.
4. **Zona horària, compte d'usuari i contrasenya.**
5. **Còpia de fitxers i reinici.** Quan l'instal·lador ho demane, retira el mitjà.

Les opcions exactes de l'instal·lador canvien entre versions, però l'estructura és sempre la mateixa: **idioma → disc → usuari → còpia → reinici**.

### 1.3 Configuració inicial

Quan el sistema arrenca per primera vegada, abans d'utilitzar-lo seriosament, repassa:

| Què | Per què | Windows | Ubuntu |
|---|---|---|---|
| **Nom de l'equip** | Identifica la màquina en la xarxa; ha de ser únic i sense espais | `Rename-Computer -NewName AULA-PC01` (i reiniciar) | `sudo hostnamectl set-hostname aula-pc01` |
| **Compte d'usuari** | Tindre un compte d'ús diari i no treballar sempre com a administrador | *Configuració → Comptes* | `adduser`, o durant la instal·lació |
| **Xarxa** | Comprovar que hi ha connexió i adreça | `ipconfig` | `ip a` |
| **Data, hora i zona** | Necessàries per als certificats i els registres | *Configuració → Hora i idioma* | `timedatectl` |
| **Actualitzacions** | Pedaços de seguretat (secció 2) | Windows Update | `apt update` i `apt upgrade` |

## 2. Actualitzacions

La imatge d'instal·lació pot tindre mesos o anys d'antiguitat. Si no actualitzes, tens un sistema amb vulnerabilitats conegudes des del primer minut. Per això, **el primer que es fa en acabar una instal·lació és actualitzar**.

Hi ha tres tipus d'actualització que convé distingir:

- **De seguretat**: corregeixen vulnerabilitats. Són les més urgents.
- **De qualitat o d'errors**: corregeixen errors sense afegir funcions.
- **De funcionalitats**: afegeixen novetats o canvien la versió principal del SO. Poden requerir més temps i una còpia prèvia.

### 2.1 En Windows

S'actualitza des de **Windows Update** (*Configuració → Windows Update*). Descarrega i instal·la pedaços i, de vegades, controladors, i moltes actualitzacions demanen un **reinici**. Des de PowerShell pots veure les actualitzacions instal·lades:

```powershell
Get-HotFix                   # pedaços instal·lats
winget upgrade               # programes (no només Windows) amb actualització disponible
```

### 2.2 En Ubuntu i Debian

El gestor de paquets `apt` treballa en dos temps, i la confusió entre tots dos és molt freqüent:

```bash
sudo apt update              # actualitza la LLISTA de paquets disponibles (no instal·la res)
sudo apt upgrade             # INSTAL·LA les actualitzacions d'aquesta llista
apt list --upgradable        # mostra què es pot actualitzar
```

Cal executar-los **en aquest ordre**: `update` descarrega les novetats del catàleg i `upgrade` les aplica. Si només fas `upgrade`, instal·les segons un catàleg desactualitzat. Ubuntu també té el **Gestor d'actualitzacions** gràfic i, per defecte, aplica automàticament les actualitzacions de seguretat.


## 3. Instal·lar, desinstal·lar i actualitzar aplicacions

El criteri RA2.h demana saber gestionar les aplicacions. La idea és la mateixa en tots els sistemes (**instal·lar, desinstal·lar, actualitzar**), però canvia l'eina.

### 3.1 En Windows

Les aplicacions s'instal·len de tres maneres:

- **Instal·lador descarregat** (`.exe` o `.msi`) del web del fabricant. Comprova que és el lloc oficial.
- **Microsoft Store**.
- **Gestor de paquets `winget`** (Windows Package Manager), que ve amb les versions actuals de Windows 10 i 11.

```powershell
winget search vlc            # busca paquets
winget install VideoLAN.VLC  # instal·la per l'identificador del paquet
winget upgrade --all         # actualitza tot el que es puga
winget uninstall VideoLAN.VLC
```

Per a **desinstal·lar** de manera gràfica: *Configuració → Aplicacions → Aplicacions instal·lades*. Una desinstal·lació pot deixar restes (configuració en les carpetes de l'usuari) que cal netejar a mà si vols un esborrament complet.

### 3.2 En Ubuntu

La font principal són els **repositoris** de la distribució: servidors que guarden paquets verificats. Amb `apt`:

```bash
apt search vlc               # busca
sudo apt install vlc         # instal·la (amb les seves dependències)
sudo apt remove vlc          # desinstal·la, però conserva la configuració
sudo apt purge vlc           # desinstal·la i també esborra la configuració
sudo apt autoremove          # elimina les dependències que ja no utilitza ningú
```

També hi ha paquets **Snap** (`snap install`) i **Flatpak**, que inclouen les seves dependències, i la instal·lació de fitxers `.deb` descarregats amb `sudo apt install ./fitxer.deb`.

### 3.3 Equivalències

| Acció | Windows | Ubuntu |
|---|---|---|
| Buscar | `winget search` | `apt search` |
| Instal·lar | `winget install` | `sudo apt install` |
| Actualitzar-ho tot | `winget upgrade --all` | `sudo apt update && sudo apt upgrade` |
| Desinstal·lar | `winget uninstall` | `sudo apt remove` / `purge` |


## 4. Controladors de dispositius

Un **controlador** (*driver*) és el programari que permet al SO comunicar-se amb un component concret. Sense ell, el dispositiu pot no funcionar, o funcionar amb un controlador **genèric** que només ofereix les funcions bàsiques: per exemple, una targeta gràfica dedicada que mostra la imatge però sense acceleració 3D.

### 4.1 En Windows

Windows instal·la molts controladors durant la instal·lació o poc després mitjançant Windows Update. Per al maquinari del qual es busca el màxim rendiment (sobretot les **targetes gràfiques**), convé baixar el controlador oficial més recent del web del fabricant (NVIDIA, AMD, Intel...).

La peça central és l'**Administrador de dispositius** (`devmgmt.msc`):

- Un **triangle groc amb un signe d'exclamació** indica un dispositiu amb problemes; si es mostra com a «Dispositiu desconegut», no hi ha controlador.
- En les **Propietats** del dispositiu, la pestanya *Controlador* permet **actualitzar**, **revertir al controlador anterior** (útil quan un controlador nou dona problemes) o **desinstal·lar** el dispositiu perquè Windows el torne a detectar.
- Les propietats mostren un codi d'error (per exemple, el codi 28 significa que no s'han instal·lat els controladors).

```powershell
Get-PnpDevice -PresentOnly | Where-Object { $_.Status -ne 'OK' }   # dispositius amb problemes
driverquery                                                          # controladors carregats
```

### 4.2 En Linux

El **nucli** Linux porta de sèrie controladors de codi obert per a una gran quantitat de maquinari, de manera que sovint tot funciona sense fer res. Per a maquinari específic (una altra vegada, les targetes gràfiques), alguns sistemes, com Ubuntu, tenen un gestor de **«controladors addicionals»** que instal·la controladors propietaris.

Per a diagnosticar:

```bash
lspci                        # maquinari del bus PCI (gràfica, xarxa...)
lspci -k                     # afegeix quin controlador (del nucli) utilitza cada dispositiu
lsusb                        # dispositius USB
ubuntu-drivers devices       # controladors recomanats (Ubuntu)
sudo ubuntu-drivers install  # instal·la els recomanats
```

Si en `lspci -k` un dispositiu no mostra cap línia **Kernel driver in use**, aquest component no té cap controlador carregat.



### 4.3 Mètode de diagnòstic

Quan un dispositiu no funciona, segueix sempre el mateix camí:

1. **Detecta**: apareix en el sistema? (`Administrador de dispositius`; `lspci`/`lsusb`).
2. **Identifica** el model exacte.
3. **Comprova l'estat** del controlador (avís groc; línia *driver in use*).
4. **Actualitza** per Windows Update/gestor de paquets o des del web del fabricant.
5. **Torna enrere** si el problema va començar amb una actualització (revertir el controlador).
6. **Documenta** el que has fet i el resultat.

