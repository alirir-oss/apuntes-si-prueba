---
title: "Unitat 3"
weight: 3
translationKey: "u03"
---

# Unitat 3. Automatització de tasques i *shell scripting*

Com a futurs desenvolupadors, desplegareu les vostres aplicacions en servidors, contenidors i *pipelines* d'integració contínua. En quasi tots aquests entorns **no hi ha interfície gràfica**: es treballa mitjançant un **terminal** i de xicotets programes anomenats **scripts** que automatitzen les tasques repetitives, com les còpies de seguretat, la creació d'usuaris o la vigilància del sistema.

Aquesta unitat té tres parts:

1. **El terminal**: moure's pel sistema de fitxers, manipular fitxers i text, i gestionar l'emmagatzematge i les còpies de seguretat.
2. **Programació de scripts en Bash**: variables, condicions, bucles, funcions, paràmetres i programació de tasques amb `cron`.
3. ***Scripting* en PowerShell**: la sintaxi equivalent en Windows i el Programador de tasques.

## Resultat d'aprenentatge

Aquesta unitat treballa el **RA3** del mòdul *Sistemes Informàtics* (0483): **gestiona la informació del sistema identificant les estructures d'emmagatzematge i aplicant mesures per a assegurar la integritat de les dades**.

| Criteri d'avaluació | On es treballa |
| --- | --- |
| a) S'han comparat sistemes d'arxius. | Apartat 4.1 |
| b) S'ha identificat l'estructura i la funció dels directoris del sistema operatiu. | Apartat 1.5 |
| c) S'han utilitzat eines en entorn gràfic i ordres per a localitzar informació en el sistema d'arxius. | Apartats 2.6, 3.4 i 4.3 |
| d) S'han creat diferents tipus de particions i unitats lògiques. | Apartat 4.2 |
| e) S'han fet i restaurat còpies de seguretat. | Apartats 4.4 i 11, exercicis 11.2 i 13 |
| f) S'han planificat i automatitzat tasques. | Apartats 5 a 13 |
| g) S'han instal·lat i avaluat utilitats relacionades amb la gestió d'informació. | Apartats 4.3 i 4.4 |

## Contingut de la unitat

**Bloc A: el terminal**

| Apartat | Què aprendràs |
| --- | --- |
| [1. Introducció a la shell]({{< relref "/U03/U03.01-Introduccion-Shell-VAL.md" >}}) | Què és una shell, sintaxi de les ordres, ajuda, estructura de directoris, rutes i comodins. |
| [2. Fitxers, directoris i permisos]({{< relref "/U03/U03.02-Ficheros-Permisos-VAL.md" >}}) | Crear, copiar, moure, buscar fitxers i donar permís d'execució. |
| [3. Redireccions, canonades i filtres]({{< relref "/U03/U03.03-Redirecciones-Filtros-VAL.md" >}}) | Combinar ordres i processar text. |
| [4. Emmagatzematge i còpies de seguretat]({{< relref "/U03/U03.04-Almacenamiento-Copias-VAL.md" >}}) | Sistemes d'arxius, particions, volums lògics, localització d'informació i còpies. |

**Bloc B: *scripting* en Bash**

| Apartat | Què aprendràs |
| --- | --- |
| [5. Primers scripts]({{< relref "/U03/U03.05-Primeros-Scripts-VAL.md" >}}) | Crear, executar i depurar un script. |
| [6. Variables, paràmetres i E/S]({{< relref "/U03/U03.06-Variables-Parametros-VAL.md" >}}) | Tipus de dades, operadors, arguments i lectura del teclat. |
| [7. Condicions]({{< relref "/U03/U03.07-Condiciones-VAL.md" >}}) | Prendre decisions amb `if`, `case` i `[[ ]]`. |
| [8. Bucles]({{< relref "/U03/U03.08-Bucles-VAL.md" >}}) | Repetir accions amb `for`, `while`, `until` i menús. |
| [9. Funcions]({{< relref "/U03/U03.09-Funciones-VAL.md" >}}) | Organitzar el codi en blocs reutilitzables. |
| [10. Arrays i cadenes]({{< relref "/U03/U03.10-Arrays-Cadenas-VAL.md" >}}) | Treballar amb llistes de dades i manipular text. |
| [11. Automatització de tasques]({{< relref "/U03/U03.11-Automatizacion-VAL.md" >}}) | `cron`, `at`, *timers* de systemd i el Programador de tasques de Windows. |
| [12. Exercicis resolts]({{< relref "/U03/U03.12-Ejercicios-Resueltos-VAL.md" >}}) | Problemes integradors: còpies, creació massiva d'usuaris, monitoratge… |

**Bloc C: *scripting* en Windows**

| Apartat | Què aprendràs |
| --- | --- |
| [13. Scripting en PowerShell]({{< relref "/U03/U03.13-Powershell-VAL.md" >}}) | Sintaxi de PowerShell, paràmetres, cmdlets d'ús habitual i exercicis equivalents als de Bash. |

## Entorn de pràctiques

Per a seguir la unitat necessites una shell Bash i, per al bloc C, un equip amb Windows. Per a Bash, qualsevol d'aquestes opcions és vàlida (les vam veure en la Unitat 2):

- **WSL** en Windows, amb una distribució Ubuntu o Debian.
- Una **màquina virtual** amb Linux (imprescindible per a practicar amb particions i discos).
- Un **contenidor Docker** d'un sol ús, ideal per a practicar sense por de trencar res:

```bash
# Arranca un contenidor Ubuntu interactiu que s'esborra en eixir (--rm)
docker run -it --rm ubuntu:latest bash
```

> [!TIP]
> Practica **sempre** escrivint les ordres tu mateix en lloc de copiar-les i apegar-les. La memòria muscular forma part de l'aprenentatge i t'ajudarà a detectar errors de sintaxi.
