---
title: "5. Particionament de discs"
weight: 5
---

# **Particionament de Disc: Conceptes i Tipus**

---

## **1. Què és el particionament de disc?**
El **particionament de disc** és el procés de dividir un disc dur (HDD/SSD) en seccions lògiques independents anomenades **particions**. Cada partició pot:
- Contindre un **sistema de fitxers** diferent (NTFS, ext4, FAT32, etc.).
- Emmagatzemar dades o sistemes operatius de manera aïllada.
- Ser gestionada com una unitat separada (p. ex.: `C:`, `D:`, `/home`, `/boot`).

**Avantatges:**
- Organitzar les dades de manera eficient.
- Instal·lar múltiples sistemes operatius en un mateix disc.
- Millorar el rendiment (p. ex.: separar el sistema de les dades d'usuari).
- Facilitar les còpies de seguretat o les recuperacions.

---

## **2. Tipus de Particions**

### **2.1. Particions Primàries**
- **Definició:** Particions principals que poden contindre directament un sistema operatiu o dades.
- **Límit:**
  - **MBR (Master Boot Record):** Màxim **4 particions primàries** per disc.
  - **GPT (GUID Partition Table):** Fins a **128 particions primàries** (sense límit pràctic).
- **Ús típic:**
  - En Windows: La partició del sistema (p. ex.: `C:`) sol ser primària.
  - En Linux: `/boot` o `/` poden ser primàries.
- **Característiques:**
  - Es creen directament en la taula de particions del disc.
  - No es poden subdividir (llevat que es convertisquen en extendides).

---

### **2.2. Partició Extendida**
- **Definició:** Una partició **especial** que actua com a contenidor de **particions lògiques**.
- **Límit:**
  - Només **1 partició extendida** per disc (en MBR).
  - No existeix en GPT (allí totes són primàries).
- **Ús típic:**
  - Permet superar el límit de 4 particions en discs MBR.
  - En Linux, s'utilitza per a crear múltiples particions lògiques (p. ex.: `/home`, `/var`).
- **Característiques:**
  - No pot emmagatzemar dades directament; només conté particions lògiques.
  - Ocupa una de les 4 posicions de particions primàries en MBR.

---
### **2.3. Particions Lògiques**
- **Definició:** Particions creades **dins d'una partició extendida**.
- **Límit:**
  - En MBR: Il·limitades en teoria (però depenen de l'espai de l'extendida).
  - En GPT: No s'aplica (totes són primàries).
- **Ús típic:**
  - En Linux: Particions com `/home`, `/usr` o `swap` solen ser lògiques.
  - En Windows: Menys comú, però es pot utilitzar per a unitats de dades (p. ex.: `D:`).
- **Característiques:**
  - No són accessibles directament per la BIOS/UEFI (requereixen un gestor d'arrencada com GRUB).
  - En Linux es numeren a partir del **5** (p. ex.: `/dev/sda5`, `/dev/sda6`).

---
## **3. Esquemes de Particionament**

### **3.1. MBR (Master Boot Record)**
- **Estructura:**
  - Taula de particions en els primers **512 bytes** del disc.
  - Admet discs de fins a **2 TB** (limitació d'adreçament).
- **Particions:**
  - Màxim **4 primàries** (o 3 primàries + 1 extendida amb múltiples lògiques).
- **Ús:**
  - Equips amb microprogramari **BIOS** (o UEFI en mode de compatibilitat **CSM**).
  - Windows només arrenca des d'un disc MBR en mode BIOS; en mode UEFI necessita GPT.
  - Hui encara s'utilitza en discs xicotets, memòries USB i equips antics.

---
### **3.2. GPT (GUID Partition Table)**
- **Estructura:**
  - Utilitza **GUID** (identificadors únics globals) per a les particions.
  - Admet discs de **fins a 8 ZiB** (pràcticament il·limitat).
- **Particions:**
  - Fins a **128 particions primàries** (sense necessitat d'extendides/lògiques).
- **Ús:**
  - És l'esquema dels sistemes moderns i el que utilitza el microprogramari **UEFI**.
  - **Windows** només arrenca des d'un disc GPT en mode UEFI. Windows 11 exigeix UEFI i, per tant, GPT en el disc del sistema.
  - **Linux** pot arrencar des de GPT tant en UEFI com en BIOS *legacy*: en BIOS, GRUB necessita una xicoteta partició **BIOS boot** (aproximadament 1 MB, sense sistema de fitxers).
  - Qualsevol sistema operatiu actual pot llegir i escriure en discs GPT **de dades**, encara que no arrenque des d'ells.
  - Inclou una **còpia de seguretat** de la taula de particions al final del disc i un **MBR protector** al principi, perquè les eines antigues no vegen el disc com a buit.

---
## **4. Particionament Típic en Windows**

### **4.1. Disc MBR (Legacy BIOS)**

 | Partició | Tipus       | Sistema de Fitxers | Ús                                   |
 |-----------|------------|---------------------|---------------------------------------|
 | 1         | Primària   | NTFS                | Reservada per al sistema (activa)    |
 | 2         | Primària   | NTFS                | Sistema (C:)                          |
 | 3         | Primària   | NTFS                | Recuperació (oculta)                 |
 | 4         | Extendida  | -                   | Contenidor                            |
 | 5         | Lògica     | NTFS                | Dades (D:)                            |


- **Notes:**
  - L'instal·lador de Windows crea ell mateix la partició **Reservada per al sistema** (uns quants centenars de MB, segons la versió), que conté els fitxers d'arrencada, i una partició **oculta de recuperació**.
  - La partició des de la qual arrenca la BIOS (l'**activa**) **ha de ser primària**.
  - Com que ja hi ha 3 primàries, per a afegir més d'una partició de dades cal utilitzar una **extendida** amb particions **lògiques** a dins.

---
### **4.2. Disc GPT (UEFI)**
 | Partició | Tipus       | Sistema de Fitxers | Ús                     |
 |-----------|------------|---------------------|-------------------------|
 | 1         | Primària   | FAT32               | EFI System Partition (ESP) |
 | 2         | Primària   | -                   | MSR (Reservada)        |
 | 3         | Primària   | NTFS                | Sistema (C:)           |
 | 4         | Primària   | NTFS                | Recuperació (oculta)  |
 | 5         | Primària   | NTFS                | Dades (D:)             |

- **Notes:**
  - Aquest és l'ordre en què l'instal·lador de Windows crea les particions: **ESP → MSR → Windows → Recuperació**. La partició de dades s'afegeix després.
  - **EFI System Partition (ESP):** conté els fitxers d'arrencada (obligatòria en UEFI). Sol ocupar entre 100 i 300 MB.
  - **MSR (Microsoft Reserved):** 16 MB reservats per a ús intern de Windows. No té sistema de fitxers i no apareix en l'*Administrador de discos*.
  - En GPT no existeixen extendides ni lògiques: la paraula «primària» s'utilitza ací només per a comparar-ho amb MBR.

---
## **5. Particionament Típic en Linux**

### **5.1. Disc MBR (Legacy BIOS)**
 | Partició | Tipus       | Sistema de Fitxers | Punt de Muntatge | Ús                     |
 |-----------|------------|---------------------|------------------|-------------------------|
 | 1         | Primària   | ext4                | /boot            | Kernel i arrencada       |
 | 2         | Extendida  | -                   | -                | Contenidor             |
 | 5         | Lògica     | ext4                | /                | Sistema arrel           |
 | 6         | Lògica     | ext4                | /home            | Dades d'usuari        |
 | 7         | Lògica     | swap                | -                | Memòria virtual        |

- **Notes:**
  - `/boot` sol ser primària per a evitar problemes amb la BIOS.
  - `swap` és opcional (recomanada si no hi ha prou RAM).

---
### **5.2. Disc GPT (UEFI)**
 | Partició | Tipus       | Sistema de Fitxers | Punt de Muntatge | Ús                     |
 |-----------|------------|---------------------|------------------|-------------------------|
 | 1         | Primària   | FAT32               | /boot/efi        | EFI System Partition    |
 | 2         | Primària   | ext4                | /boot            | Kernel                 |
 | 3         | Primària   | ext4                | /                | Sistema arrel           |
 | 4         | Primària   | ext4                | /home            | Dades d'usuari        |
 | 5         | Primària   | swap                | -                | Memòria virtual        |

- **Notes:**
  - **ESP (`/boot/efi`):** Obligatòria per a UEFI (compartida amb Windows en arrencada dual).
  - No hi ha límit de particions primàries.

---
## **6. Eines per a Particionar**
 | Sistema   | Eina (CLI)       | Eina (GUI)          |
 |-----------|--------------------------|----------------------------|
 | Windows   | `diskpart`               | Administrador de discos    |
 | Linux     | `fdisk`, `gdisk`, `parted` | GParted, KDE Partition Manager |

---
