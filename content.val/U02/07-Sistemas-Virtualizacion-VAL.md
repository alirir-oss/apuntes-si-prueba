---
title: "7. Sistemes de Virtualització"
weight: 7
---

## 1. Concepte de Virtualització

En informàtica, la **virtualització** és el procés de crear una representació basada en programari (virtual) d'un recurs tecnològic, com ara un servidor, un dispositiu d'emmagatzematge, una xarxa o fins i tot un sistema operatiu complet. L'objectiu és executar múltiples sistemes operatius i aplicacions de manera aïllada però simultània sobre un únic conjunt de maquinari físic.

Una **Màquina Virtual (VM)** és la unitat fonamental de la virtualització. És un entorn computacional completament aïllat que emula l'arquitectura d'un ordinador físic. Des del punt de vista del programari que s'executa a dins, una VM no es pot distingir d'una màquina real: té la seva pròpia **CPU virtual, memòria RAM, disc dur, targeta de xarxa** i altres perifèrics. El sistema operatiu que s'instal·la dins de la VM s'anomena "sistema operatiu convidat" (*Guest OS*), i s'executa sobre el sistema operatiu de la màquina física, conegut com a "sistema operatiu amfitrió" (*Host OS*).

---

## 2. L'Hipervisor 

L'element que permet que tot això funcione és un programari anomenat **hipervisor** o **Monitor de Màquina Virtual (VMM)**. L'hipervisor és una capa de programari que se situa entre el maquinari físic i les màquines virtuals. Les seves dues funcions principals són:

1.  **Abstracció del Maquinari**: L'hipervisor presenta a cada VM un conjunt de maquinari virtual estandarditzat, independentment del maquinari físic real de l'amfitrió.
2.  **Gestió de Recursos**: Actua com un "director", que assigna i gestiona de manera dinàmica els recursos físics de l'amfitrió (cicles de CPU, blocs de memòria RAM, accés a disc i a xarxa) entre les diferents VM que estan en execució.

Hi ha dos tipus principals d'hipervisors:

* **Tipus 1 (Bare-Metal)**: S'executa directament sobre el maquinari de l'amfitrió i actua com un sistema operatiu molt lleuger. És l'estàndard en entorns empresarials i de servidors (p. ex. VMware ESXi, Microsoft Hyper-V, Proxmox).
* **Tipus 2 (Hosted)**: S'executa com una aplicació més sobre un sistema operatiu amfitrió convencional (Windows, macOS, Linux). És ideal per a entorns de desenvolupament i de proves.

---

## 3. Oracle VM VirtualBox

**VirtualBox** és un **hipervisor de Tipus 2**, potent, gratuït i de codi obert. Com que és una aplicació que instal·les en el teu sistema operatiu, et permet crear i executar múltiples màquines virtuals de manera senzilla. És una eina increïblement popular entre desenvolupadors, administradors de sistemes i estudiants per a:

* Provar programari en diferents sistemes operatius sense necessitat de maquinari addicional.
* Crear entorns de desenvolupament aïllats i reproduïbles.
* Simular arquitectures de xarxa complexes en un únic ordinador.
* Analitzar programari maliciós en un entorn segur i aïllat (*sandbox*).

---

## 4. Modes de Xarxa en VirtualBox

La configuració de la xarxa és un dels aspectes més potents i flexibles de VirtualBox. Triar el mode correcte és crucial segons el que necessites fer. A continuació, es detallen els tipus més importants amb exemples concrets.

### **Mode NAT (Network Address Translation)**

* **Com funciona?**: Aquest és el mode per defecte. VirtualBox actua com un router domèstic per a la teva VM. La VM comparteix l'adreça IP de la teva màquina amfitriona per a accedir a l'exterior. La VM pot accedir a Internet, però els dispositius de la xarxa externa no poden iniciar una connexió directa cap a la VM.
* **Analogia**: Imagina que la teva VM està en una oficina amb un únic número de telèfon públic (la IP del teu amfitrió). Des de dins, pots telefonar a qualsevol número exterior (navegar per Internet), però si algú de fora vol telefonar-te directament, no pot; hauria de passar per una centraleta complexa (redirecció de ports).
* **Cas d'Ús Real**: **Navegació i actualitzacions bàsiques.** És perfecte quan només necessites que la teva VM tinga accés a Internet per a descarregar paquets, actualitzar el programari o navegar per la web, sense necessitat que actue com un servidor accessible des de fora.

### **Mode Adaptador Pont (Bridged Adapter)**

* **Com funciona?**: La VM es connecta directament a la mateixa xarxa física que la teva màquina amfitriona. Obté la seva pròpia adreça IP del router de la xarxa (via DHCP), com si fora un altre ordinador físic connectat al mateix commutador (switch) o a la mateixa xarxa Wi-Fi.
* **Analogia**: Ara la teva VM és un element més de la teva xarxa local. És com si hagueres connectat un portàtil nou directament al teu router. Els altres dispositius de la xarxa la veuen i poden comunicar-se amb ella directament.
* **Cas d'Ús Real**: **Executar un servidor accessible en la xarxa local.** Ideal si dins de la VM tens en marxa un servidor web, una base de dades o qualsevol servei que haja de ser accessible des d'altres ordinadors de casa o de l'oficina. Per exemple, desenvolupar una aplicació web en la VM i provar-la des del teu mòbil, que està en la mateixa xarxa Wi-Fi.

### **Mode Xarxa Interna (Internal Network)**

* **Com funciona?**: Crea una xarxa de programari completament aïllada i privada. Les VM que configures en la mateixa Xarxa Interna poden comunicar-se entre si, però no tenen connexió ni amb la màquina amfitriona ni amb el món exterior (Internet).
* **Analogia**: És com crear una xarxa LAN privada. Has connectat diverses màquines a un commutador (switch) que no està connectat a res més. Només es veuen entre elles.
* **Cas d'Ús Real**: **Simulació de sistemes distribuïts i seguretat.** Pots simular una arquitectura multicapa (p. ex. una VM amb un servidor web que es comunica amb una altra VM amb una base de dades) en què la base de dades mai no ha d'estar exposada. També és el mode ideal per a analitzar programari maliciós de manera segura, ja que la VM infectada no pot comunicar-se amb l'exterior.

### **Mode Adaptador Només Amfitrió (Host-Only Adapter)**

* **Com funciona?**: És un híbrid. Crea una xarxa privada que inclou la màquina amfitriona i una o més VM. Les VM poden comunicar-se entre si i amb l'amfitrió, però no tenen accés a la xarxa externa a través d'aquest adaptador.
* **Analogia**: És com si hagueres connectat el teu ordinador (amfitrió) i una o més VM amb un cable de xarxa directe entre ells, i hagueres creat una xicoteta xarxa privada.
* **Cas d'Ús Real**: **Desenvolupament client-servidor.** Vols executar un servidor (p. ex. una base de dades) en la VM de manera segura, però necessites accedir-hi des de les eines de desenvolupament instal·lades en la teva màquina amfitriona. Amb aquest mode, el teu PC amfitrió pot connectar-se a la IP de la VM, però la VM continua aïllada d'Internet, cosa que augmenta la seguretat.

---

## 5. Ecosistema d'Hipervisors

Encara que VirtualBox és una eina fantàstica, forma part d'un ecosistema molt més ampli de tecnologies de virtualització. Comprendre les diferències entre els diferents hipervisors et permetrà triar l'eina adequada per a cada tasca, ja siga per a desenvolupar en local o per a desplegar una infraestructura a gran escala.

### **Principals Hipervisors del Mercat**

A continuació, analitzem els actors més importants del sector, les seves característiques i per a què són més adequats.

#### **VMware vSphere / ESXi** 

* **Tipus**: Hipervisor de Tipus 1.
* **Descripció**: VMware és el **líder indiscutible del mercat de la virtualització empresarial**. ESXi és l'hipervisor que s'instal·la en el servidor físic, i vSphere és el paquet de gestió que ho controla tot. És conegut per la seva robustesa, el seu enorme ecosistema i un conjunt de característiques molt madur.
* **Sistema Operatiu**: ESXi *és* el sistema operatiu. S'instal·la directament sobre el maquinari del servidor.
* **Ideal Per a**: Centres de dades corporatius, aplicacions de missió crítica, núvols privats i híbrids, i qualsevol entorn que requerisca la màxima fiabilitat i funcionalitats avançades com la migració en calent sense interrupcions (`vMotion`).
* **Model de Llicència**: VMware ofereix una **versió gratuïta d'ESXi** amb funcionalitats bàsiques. El veritable potencial es desbloqueja amb les **llicències de pagament de vSphere**, que habiliten la gestió centralitzada a través de **vCenter Server** i característiques avançades com l'alta disponibilitat i el balanceig de càrregues.

#### **Microsoft Hyper-V** 

* **Tipus**: Hipervisor de Tipus 1.
* **Descripció**: És la resposta de Microsoft a VMware i el seu principal competidor. El seu avantatge més gran és la **profunda integració amb l'ecosistema de Microsoft**. Si una organització ja utilitza Windows Server, afegir el rol d'Hyper-V és un pas natural.
* **Sistema Operatiu**: Ve integrat com un "rol" en **Windows Server**. També hi ha una versió gratuïta i sense interfície gràfica anomenada **Hyper-V Server**. Encara que s'instal·la sobre Windows, tècnicament s'executa en un nivell inferior al del sistema operatiu principal i es comporta com un veritable Tipus 1.
* **Ideal Per a**: Empreses que ja tenen una forta inversió en tecnologies de Microsoft. És excel·lent per a virtualitzar càrregues de treball de Windows i s'integra perfectament amb serveis en el núvol com Microsoft Azure.
* **Model de Llicència**: L'hipervisor en si és **gratuït** (inclòs en les llicències de Windows Server). Els costos s'associen a les llicències del sistema operatiu convidat (Windows) i al paquet de gestió avançada **System Center Virtual Machine Manager (SCVMM)**.

#### **KVM (Kernel-based Virtual Machine)** 

* **Tipus**: Hipervisor de Tipus 1.
* **Descripció**: KVM no és un programa, sinó una **funcionalitat integrada en el nucli (kernel) de Linux**. Això converteix qualsevol distribució moderna de Linux en un potent hipervisor de Tipus 1. És la base de molts dels núvols més grans del món (inclosos Google Cloud i OpenStack) gràcies al seu rendiment, la seva flexibilitat i la seva naturalesa de codi obert.
* **Sistema Operatiu**: S'executa en qualsevol distribució de **Linux** (Ubuntu, CentOS, Debian, etc.).
* **Ideal Per a**: Entorns de núvol, centres de dades basats en Linux, i per a qualsevol que busque una solució de virtualització de codi obert, potent i sense costos de llicència. Solucions com **Proxmox VE** empaqueten KVM amb una interfície web molt fàcil d'utilitzar, cosa que l'ha fet molt popular.
* **Model de Llicència**: **Totalment gratuït i de codi obert**. El model de negoci al voltant de KVM es basa a oferir plataformes de gestió i suport tècnic de pagament, com és el cas de **Red Hat Virtualization**.

#### **Xen / Citrix Hypervisor** 

* **Tipus**: Hipervisor de Tipus 1.
* **Descripció**: Xen és un altre potent **hipervisor de codi obert**, conegut per la seva arquitectura d'alta seguretat (utilitza un domini de control o *Dom0* per a gestionar el sistema). Va ser pioner en la virtualització per al núvol (Amazon Web Services el va utilitzar com a base durant molts anys). **Citrix Hypervisor** (abans XenServer) és la distribució comercial mantinguda per Citrix.
* **Sistema Operatiu**: S'instal·la directament sobre el maquinari.
* **Ideal Per a**: Infraestructura d'escriptoris virtuals (VDI), proveïdors de serveis en el núvol i entorns on l'aïllament i la seguretat són crítics.
* **Model de Llicència**: El projecte **Xen és gratuït i de codi obert**. Citrix Hypervisor té una edició gratuïta i edicions de pagament que afegeixen suport i funcionalitats empresarials.

#### **Proxmox VE (Virtual Environment)**

* **Tipus**: Hipervisor de Tipus 1.
* **Descripció**: Proxmox VE és una **plataforma de virtualització de codi obert** que combina l'hipervisor KVM per a màquines virtuals i LXC per a contenidors. És conegut per la seva **flexibilitat, facilitat d'ús i orientació a la virtualització empresarial**, tot integrat en una solució unificada. Inclou eines de gestió web i suport per a emmagatzematge definit per programari (SDS), xarxes avançades i clústers (*clustering*).
* **Sistema Operatiu**: S'instal·la sobre un sistema operatiu base (Debian Linux), però funciona com un hipervisor independent una vegada desplegat.
* **Ideal Per a**: Empreses que busquen una alternativa de codi obert a VMware o Hyper-V, entorns de núvol privat, i usuaris que necessiten gestionar tant màquines virtuals com contenidors en una mateixa plataforma. També és popular en entorns de laboratori i de desenvolupament.
* **Model de Llicència**: Proxmox VE és **gratuït i de codi obert** (amb la llicència AGPLv3). Ofereix una **subscripció de pagament** per a accedir a repositoris d'actualitzacions estables, suport tècnic i funcionalitats addicionals com les còpies de seguretat integrades.


### **Per Què Pagar per un Hipervisor?**

Si hi ha opcions tan potents i gratuïtes com KVM o les versions gratuïtes d'ESXi i Hyper-V, per què les empreses paguen grans quantitats per les llicències? La resposta no està en l'hipervisor bàsic, sinó en l'**ecosistema de gestió i les funcionalitats avançades** que l'envolten:

1.  **Gestió Centralitzada**: Eines com VMware vCenter o Microsoft SCVMM permeten gestionar centenars de servidors físics i milers de màquines virtuals des d'una única consola.
2.  **Alta Disponibilitat (High Availability - HA)**: Si un servidor físic falla, les VM que s'hi executaven es reinicien **automàticament** en un altre servidor del clúster, i es minimitza el temps d'inactivitat.
3.  **Migració en Calent (Live Migration)**: Permet moure una màquina virtual en ple funcionament d'un servidor físic a un altre **sense cap interrupció del servei**. Això és fonamental per a fer el manteniment del maquinari sense aturar les aplicacions. (P. ex.: `vMotion` en VMware).
4.  **Balanceig de Càrregues Dinàmic (DRS)**: El sistema monitora constantment l'ús de recursos i mou automàticament les VM entre els servidors físics per a optimitzar el rendiment i evitar colls de botella.
5.  **Suport Tècnic Professional**: Les llicències de pagament inclouen accés a suport tècnic 24/7, una cosa indispensable per a qualsevol negoci que depenga de la seva infraestructura virtual.

En resum, es paga per la intel·ligència que transforma un conjunt de servidors virtualitzats en un centre de dades elàstic, resilient i fàcil d'administrar.
