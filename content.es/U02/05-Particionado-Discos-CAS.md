---
title: 5. Particionado de discos
weight: 5
---

# **Particionado de Disco: Conceptos y Tipos**

---

## **1. ¿Qué es el particionado de disco?**
El **particionado de disco** es el proceso de dividir un disco duro (HDD/SSD) en secciones lógicas independientes llamadas **particiones**. Cada partición puede:
- Contener un **sistema de archivos** diferente (NTFS, ext4, FAT32, etc.).
- Almacenar datos o sistemas operativos de forma aislada.
- Ser gestionada como una unidad separada (ej: `C:`, `D:`, `/home`, `/boot`).

**Ventajas:**
- Organizar datos de manera eficiente.
- Instalar múltiples sistemas operativos en un mismo disco.
- Mejorar el rendimiento (ej: separar el sistema de los datos de usuario).
- Facilitar copias de seguridad o recuperaciones.

---

## **2. Tipos de Particiones**

### **2.1. Particiones Primarias**
- **Definición:** Particiones principales que pueden contener directamente un sistema operativo o datos.
- **Límite:**
  - **MBR (Master Boot Record):** Máximo **4 particiones primarias** por disco.
  - **GPT (GUID Partition Table):** Hasta **128 particiones primarias** (sin límite práctico).
- **Uso típico:**
  - En Windows: La partición del sistema (ej: `C:`) suele ser primaria.
  - En Linux: `/boot` o `/` pueden ser primarias.
- **Características:**
  - Se crean directamente en la tabla de particiones del disco.
  - No pueden subdividirse (a menos que se conviertan en extendidas).

---

### **2.2. Partición Extendida**
- **Definición:** Una partición **especial** que actúa como un contenedor para **particiones lógicas**.
- **Límite:**
  - Solo **1 partición extendida** por disco (en MBR).
  - No existe en GPT (allí todas son primarias).
- **Uso típico:**
  - Permite superar el límite de 4 particiones en discos MBR.
  - En Linux, se usa para crear múltiples particiones lógicas (ej: `/home`, `/var`).
- **Características:**
  - No puede almacenar datos directamente; solo contiene particiones lógicas.
  - Ocupa una de las 4 ranuras de particiones primarias en MBR.

---
### **2.3. Particiones Lógicas**
- **Definición:** Particiones creadas **dentro de una partición extendida**.
- **Límite:**
  - En MBR: Ilimitadas en teoría (pero dependen del espacio en la extendida).
  - En GPT: No aplicable (todas son primarias).
- **Uso típico:**
  - En Linux: Particiones como `/home`, `/usr`, o `swap` suelen ser lógicas.
  - En Windows: Menos común, pero puede usarse para unidades de datos (ej: `D:`).
- **Características:**
  - No son accesibles directamente por el BIOS/UEFI (requieren un gestor de arranque como GRUB).
  - Se numeran a partir de **5** en Linux (ej: `/dev/sda5`, `/dev/sda6`).

---
## **3. Esquemas de Particionado**

### **3.1. MBR (Master Boot Record)**
- **Estructura:**
  - Tabla de particiones en los primeros **512 bytes** del disco.
  - Soporta discos hasta **2 TB** (limitación de direccionamiento).
- **Particiones:**
  - Máximo **4 primarias** (o 3 primarias + 1 extendida con múltiples lógicas).
- **Uso:**
  - Equipos con firmware **BIOS** (o UEFI en modo de compatibilidad **CSM**).
  - Windows solo arranca desde un disco MBR en modo BIOS; en modo UEFI necesita GPT.
  - Hoy se sigue usando en discos pequeños, memorias USB y equipos antiguos.

---
### **3.2. GPT (GUID Partition Table)**
- **Estructura:**
  - Usa **GUIDs** (Identificadores Únicos Globales) para particiones.
  - Soporta discos de **hasta 8 ZiB** (prácticamente ilimitado).
- **Particiones:**
  - Hasta **128 particiones primarias** (sin necesidad de extendidas/lógicas).
- **Uso:**
  - Es el esquema de los sistemas modernos y el que utiliza el firmware **UEFI**.
  - **Windows** solo arranca desde un disco GPT en modo UEFI. Windows 11 exige UEFI y, por tanto, GPT en el disco del sistema.
  - **Linux** puede arrancar desde GPT tanto en UEFI como en BIOS *legacy*: en BIOS, GRUB necesita una pequeña partición **BIOS boot** (unos 1 MB, sin sistema de archivos).
  - Cualquier sistema operativo actual puede leer y escribir en discos GPT **de datos**, aunque no arranque desde ellos.
  - Incluye **copia de seguridad** de la tabla de particiones al final del disco y un **MBR protector** al principio, para que las herramientas antiguas no vean el disco como vacío.

---
## **4. Particionado Típico en Windows**

### **4.1. Disco MBR (Legacy BIOS)**

 | Partición | Tipo       | Sistema de Archivos | Uso                                   |
 |-----------|------------|---------------------|---------------------------------------|
 | 1         | Primaria   | NTFS                | Reservada para el sistema (activa)    |
 | 2         | Primaria   | NTFS                | Sistema (C:)                          |
 | 3         | Primaria   | NTFS                | Recuperación (oculta)                 |
 | 4         | Extendida  | -                   | Contenedor                            |
 | 5         | Lógica     | NTFS                | Datos (D:)                            |


- **Notas:**
  - El instalador de Windows crea él mismo la partición **Reservada para el sistema** (unos cientos de MB, según la versión), que contiene los archivos de arranque, y una partición **oculta de recuperación**.
  - La partición desde la que arranca el BIOS (la **activa**) **debe ser primaria**.
  - Como ya hay 3 primarias, para añadir más de una partición de datos hay que usar una **extendida** con particiones **lógicas** dentro.

---
### **4.2. Disco GPT (UEFI)**
 | Partición | Tipo       | Sistema de Archivos | Uso                     |
 |-----------|------------|---------------------|-------------------------|
 | 1         | Primaria   | FAT32               | EFI System Partition (ESP) |
 | 2         | Primaria   | -                   | MSR (Reservada)        |
 | 3         | Primaria   | NTFS                | Sistema (C:)           |
 | 4         | Primaria   | NTFS                | Recuperación (oculta)  |
 | 5         | Primaria   | NTFS                | Datos (D:)             |

- **Notas:**
  - Este es el orden en que el instalador de Windows crea las particiones: **ESP → MSR → Windows → Recuperación**. La partición de datos se añade después.
  - **EFI System Partition (ESP):** contiene los archivos de arranque (obligatoria en UEFI). Suele ocupar entre 100 y 300 MB.
  - **MSR (Microsoft Reserved):** 16 MB reservados para uso interno de Windows. No tiene sistema de archivos y no aparece en el *Administrador de discos*.
  - En GPT no existen extendidas ni lógicas: la palabra «primaria» se usa aquí solo para compararlo con MBR.

---
## **5. Particionado Típico en Linux**

### **5.1. Disco MBR (Legacy BIOS)**
 | Partición | Tipo       | Sistema de Archivos | Punto de Montaje | Uso                     |
 |-----------|------------|---------------------|------------------|-------------------------|
 | 1         | Primaria   | ext4                | /boot            | Kernel y arranque       |
 | 2         | Extendida  | -                   | -                | Contenedor             |
 | 5         | Lógica     | ext4                | /                | Sistema raíz           |
 | 6         | Lógica     | ext4                | /home            | Datos de usuario        |
 | 7         | Lógica     | swap                | -                | Memoria virtual        |

- **Notas:**
  - `/boot` suele ser primaria para evitar problemas con el BIOS.
  - `swap` es opcional (recomendada si no hay suficiente RAM).

---
### **5.2. Disco GPT (UEFI)**
 | Partición | Tipo       | Sistema de Archivos | Punto de Montaje | Uso                     |
 |-----------|------------|---------------------|------------------|-------------------------|
 | 1         | Primaria   | FAT32               | /boot/efi        | EFI System Partition    |
 | 2         | Primaria   | ext4                | /boot            | Kernel                 |
 | 3         | Primaria   | ext4                | /                | Sistema raíz           |
 | 4         | Primaria   | ext4                | /home            | Datos de usuario        |
 | 5         | Primaria   | swap                | -                | Memoria virtual        |

- **Notas:**
  - **ESP (`/boot/efi`):** Obligatoria para UEFI (compartida con Windows en dual-boot).
  - No hay límite de particiones primarias.

---
## **6. Herramientas para Particionar**
 | Sistema   | Herramienta (CLI)       | Herramienta (GUI)          |
 |-----------|--------------------------|----------------------------|
 | Windows   | `diskpart`               | Administrador de discos    |
 | Linux     | `fdisk`, `gdisk`, `parted` | GParted, KDE Partition Manager |

---
