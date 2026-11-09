---
title: "Unitat 4"
weight: 4
translationKey: "u04"
bookCollapseSection: true
---

# Unitat 4. Administració de sistemes operatius i usuaris

En la Unitat 3 vas aprendre a moure't pel terminal i a automatitzar tasques amb scripts. En aquesta unitat fas un pas més i et poses en el paper d'**administrador del sistema**: decidir **qui** pot entrar en un equip, **què** pot fer cada persona, **quins programes i serveis** s'estan executant i **com està funcionant** el sistema.

Encara que el vostre perfil és el de desenvolupadors, aquestes tasques formen part del vostre dia a dia: el servidor en què desplegueu una aplicació necessita un usuari propi amb els permisos justos, el servei que la manté en marxa cal arrancar-lo i vigilar-lo, i quan alguna cosa falla, la primera pista està en els **registres del sistema**.

Cada apartat s'estudia en els dos sistemes operatius més estesos: **GNU/Linux** (amb exemples en Ubuntu/Debian) i **Microsoft Windows**, utilitzant tant les eines gràfiques com la línia d'ordres (Bash, CMD i PowerShell).

## Resultat d'aprenentatge

Aquesta unitat treballa el **RA4** del mòdul *Sistemes Informàtics* (0483): **gestiona sistemes operatius utilitzant ordres i eines gràfiques i avaluant les necessitats del sistema**.

| Criteri d'avaluació | On es treballa |
| --- | --- |
| a) S'han configurat comptes d'usuari locals i grups. | Apartats 1, 2 i 3 |
| b) S'ha assegurat l'accés al sistema mitjançant l'ús de directives de compte i directives de contrasenyes. | Apartat 4 |
| c) S'han identificat, arrancat i aturat serveis i processos. | Apartat 8 |
| d) S'ha protegit l'accés a la informació mitjançant l'ús de permisos locals i llistes de control d'accés. | Apartats 5, 6 i 7 |
| e) S'han utilitzat ordres per a fer les tasques bàsiques de configuració i administració del sistema. | Tots els apartats; resum en l'apartat 12.5 |
| f) S'ha monitorat el sistema. | Apartats 9 i 10 |
| g) S'han instal·lat i avaluat utilitats per al manteniment i l'optimització del sistema. | Apartat 11 |
| h) S'han avaluat les necessitats del sistema informàtic en relació amb el desenvolupament d'aplicacions. | Apartat 12 |

## Contingut de la unitat

**Bloc A: usuaris i grups**

| Apartat | Què aprendràs |
| --- | --- |
| [1. Usuaris i grups en Linux]({{< relref "/U04/U04.01-Usuarios-Grupos-Linux-VAL.md" >}}) | UID i GID, fitxers `/etc/passwd`, `/etc/shadow` i `/etc/group`, creació i gestió de comptes, `sudo`. |
| [2. Usuaris i grups en Windows]({{< relref "/U04/U04.02-Usuarios-Grupos-Windows-VAL.md" >}}) | Tipus de compte, SID, grups integrats, UAC, gestió gràfica, amb `net user` i amb PowerShell. |
| [3. Perfils d'usuari]({{< relref "/U04/U04.03-Perfiles-Usuario-VAL.md" >}}) | Directori personal, `/etc/skel`, fitxers d'inici de la shell i perfils de Windows. |
| [4. Directives de compte i de contrasenyes]({{< relref "/U04/U04.04-Politicas-Contrasenas-VAL.md" >}}) | Caducitat, complexitat i bloqueig de comptes en Linux (`chage`, PAM) i en Windows (`secpol.msc`, `net accounts`). |

**Bloc B: protecció de la informació**

| Apartat | Què aprendràs |
| --- | --- |
| [5. Permisos en Linux: UGO avançat i ACL]({{< relref "/U04/U04.05-Permisos-Linux-ACL-VAL.md" >}}) | `chown`, `umask`, SUID, SGID, *sticky bit* i llistes de control d'accés (`setfacl`). |
| [6. Permisos NTFS en Windows]({{< relref "/U04/U04.06-Permisos-NTFS-VAL.md" >}}) | Permisos estàndard i especials, herència, permisos efectius, `icacls` i `Get-Acl`. |
| [7. Compartició de recursos en xarxa]({{< relref "/U04/U04.07-Comparticion-Recursos-VAL.md" >}}) | Carpetes i impressores compartides en Windows i servidor Samba en Linux. |

**Bloc C: processos, monitoratge i manteniment**

| Apartat | Què aprendràs |
| --- | --- |
| [8. Processos i serveis]({{< relref "/U04/U04.08-Procesos-Servicios-VAL.md" >}}) | Llistar, finalitzar i prioritzar processos; arrancar, aturar i habilitar serveis amb `systemctl` i PowerShell. |
| [9. Monitoratge de recursos]({{< relref "/U04/U04.09-Monitorizacion-VAL.md" >}}) | `top`, `htop`, `free`, `df`, Administrador de tasques, Monitor de recursos i Monitor de rendiment. |
| [10. Registres del sistema]({{< relref "/U04/U04.10-Registros-Sistema-VAL.md" >}}) | `/var/log`, `journalctl`, `logrotate`, Visor d'esdeveniments i `Get-WinEvent`. |
| [11. Manteniment i optimització]({{< relref "/U04/U04.11-Mantenimiento-Optimizacion-VAL.md" >}}) | Actualitzacions, neteja, salut del disc, arrancada i avaluació d'utilitats. |
| [12. El sistema al servei del desenvolupament]({{< relref "/U04/U04.12-Sistema-Desarrollo-VAL.md" >}}) | Requisits segons el tipus de desenvolupament, preparació de l'equip, cas pràctic integrador i resum d'ordres. |

## Entorn de pràctiques

Moltes de les tasques d'aquesta unitat **modifiquen la configuració de seguretat del sistema**. Per això, practica sempre en **màquines virtuals** (Unitat 2) i fes una **instantània** abans de començar cada apartat; si alguna cosa ix malament, podràs tornar arrere en segons.

| Màquina | Ús en la unitat |
| --- | --- |
| **Ubuntu 26.04 LTS** (escriptori o servidor) o **Debian 13** | Tots els apartats de Linux. Per al servidor Samba (apartat 7) n'hi ha prou amb una versió servidor sense entorn gràfic. |
| **Windows 11 Pro o Education** | Tots els apartats de Windows. |
| **Windows Server 2025** (opcional) | Compartició de recursos. En la Unitat 6 s'utilitzarà per als dominis. |

> [!WARNING]
> L'edició **Windows 11 Home** no inclou diverses de les eines que s'utilitzen en aquesta unitat: *Usuaris i grups locals* (`lusrmgr.msc`), *Directiva de seguretat local* (`secpol.msc`) ni l'*Editor de directives de grup local* (`gpedit.msc`). Si a casa tens l'edició Home, utilitza una màquina virtual amb l'edició Pro o Education (el centre facilita les llicències educatives) o utilitza les ordres equivalents (`net user`, `net accounts`, PowerShell), que sí que funcionen en totes les edicions.

Per a connectar en xarxa les màquines virtuals (apartat 7), configura els seus adaptadors en mode **Xarxa NAT** (*NAT Network*) o **Adaptador pont** en VirtualBox, de manera que totes estiguen en la mateixa xarxa i puguen comunicar-se entre si.

> [!TIP]
> Mantín sempre **una segona sessió d'administrador oberta** (un altre terminal amb `sudo -i` o una consola de PowerShell elevada) mentre canvies contrasenyes, directives o permisos. Si et bloqueges a tu mateix, podràs desfer el canvi des d'aquesta sessió.
