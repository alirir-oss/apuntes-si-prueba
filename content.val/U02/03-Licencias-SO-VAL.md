---
title: "3. Llicències dels Sistemes Operatius"
weight: 3
---

## 1. Llicències i com comparar sistemes operatius

El criteri RA2.c de la unitat et demana **comparar sistemes operatius** segons els requisits, les característiques, el camp d'aplicació i la llicència. La llicència és la part que més es descuida i la que més problemes dona.

### 1.1 Tipus de programari segons la llicència

| Tipus | Què permet | Exemples |
|---|---|---|
| **Programari lliure** | Executar-lo per a qualsevol ús, estudiar-ne el codi, modificar-lo i redistribuir-lo (les «quatre llibertats») | Linux, FreeBSD, LibreOffice |
| **Codi obert** (*open source*) | Codi font accessible amb una llicència que permet utilitzar-lo i modificar-lo; a la pràctica se solapa amb el lliure | Android (base), Chromium |
| **Propietari** | El fabricant conserva el control del codi; l'usuari només té dret d'ús, segons la llicència (EULA) | Windows, macOS |
| **Freeware** | Gratuït, però amb el codi tancat | Alguns controladors i utilitats |
| **Shareware / de prova** | Es pot provar de franc durant un temps o amb funcions limitades i després cal pagar | Versions d'avaluació |

Dins del programari lliure hi ha dues famílies de llicències: les **copyleft** (com la **GPL**, que exigeix que les obres derivades mantinguen la mateixa llibertat; el nucli Linux és GPL) i les **permissives** (com **MIT** o **BSD**, que permeten incloure el codi en productes tancats).


### 1.2 Llicències de Windows: de què depén el que pagues

En l'àmbit professional, Windows es pot adquirir de diferents maneres:

- **OEM**: ve preinstal·lada en un equip nou (o es compra per a muntar-lo) i queda vinculada a aquest maquinari.
- **Retail** (comercial): es compra solta i, en general, es pot transferir a un altre equip.
- **Per volum**: per a empreses i centres educatius, amb una sola clau o contracte per a molts equips.

A més, Windows Server té les seves pròpies regles de llicència (per nuclis de processador i amb llicències d'accés de client). No cal que les memoritzes: l'important és saber que **abans de desplegar** un SO propietari cal comprovar que la llicència cobreix el nombre d'equips i l'ús previst. Per a practicar hi ha **versions d'avaluació** gratuïtes i temporals, que el fabricant publica per a fer proves.

### 1.3 Taula comparativa de sistemes actuals

| Sistema | Llicència | Camp d'aplicació habitual | Observacions |
|---|---|---|---|
| Windows 10/11 | Propietària | Escriptori i portàtil, ofimàtica, jocs | Té uns requisits mínims de maquinari que has de consultar per a cada versió |
| Windows Server | Propietària | Servidors d'empresa | Es tracta en la pàgina 4 |
| Ubuntu, Debian, Fedora... | Lliure (GPL i altres) | Escriptori, servidors, desenvolupament | Moltes distribucions, un mateix nucli |
| macOS | Propietària | Escriptori d'Apple | Només en maquinari d'Apple |
| Android | Lliure en la base, amb components propietaris | Mòbils i tauletes | Basat en el nucli Linux |
