---
title: 3. Licencias de los Sistemas Operativos
weight: 3
---

## 1. Licencias y cómo comparar sistemas operativos

El criterio RA2.c de la unidad te pide **comparar sistemas operativos** por requisitos, características, campo de aplicación y licencia. La licencia es la parte que más se descuida y más problemas da.

### 1.1 Tipos de software según la licencia

| Tipo | Qué permite | Ejemplos |
|---|---|---|
| **Software libre** | Ejecutarlo para cualquier uso, estudiar su código, modificarlo y redistribuirlo (las «cuatro libertades») | Linux, FreeBSD, LibreOffice |
| **Código abierto** (*open source*) | Código fuente accesible con licencia que permite usarlo y modificarlo; en la práctica se solapa con el libre | Android (base), Chromium |
| **Propietario** | El fabricante conserva el control del código; el usuario solo tiene derecho de uso, según la licencia (EULA) | Windows, macOS |
| **Freeware** | Gratuito, pero con el código cerrado | Algunos controladores y utilidades |
| **Shareware / de prueba** | Se puede probar gratis un tiempo o con funciones limitadas y luego hay que pagar | Versiones de evaluación |

Dentro del software libre hay dos familias de licencias: las **copyleft** (como la **GPL**, que exige que las obras derivadas mantengan la misma libertad; el núcleo Linux es GPL) y las **permisivas** (como **MIT** o **BSD**, que permiten incluir el código en productos cerrados).


### 1.2 Licencias de Windows: de qué depende lo que pagas

En el ámbito profesional, Windows se puede adquirir de maneras distintas:

- **OEM**: viene preinstalada en un equipo nuevo (o se compra para montarlo) y queda vinculada a ese hardware.
- **Retail** (comercial): se compra suelta y, en general, se puede transferir a otro equipo.
- **Por volumen**: para empresas y centros educativos, con una sola clave o contrato para muchos equipos.

Además, Windows Server se licencia con reglas propias (por núcleos de procesador y con licencias de acceso de cliente). No hace falta que las memorices: lo importante es saber que **antes de desplegar** un SO propietario hay que comprobar que la licencia cubre el número de equipos y el uso previsto. Para practicar existen **versiones de evaluación** gratuitas y temporales, que el fabricante publica para pruebas.

### 1.3 Tabla comparativa de sistemas actuales

| Sistema | Licencia | Campo de aplicación habitual | Observaciones |
|---|---|---|---|
| Windows 10/11 | Propietaria | Escritorio y portátil, ofimática, juegos | Tiene requisitos mínimos de hardware que debes consultar por versión |
| Windows Server | Propietaria | Servidores de empresa | Se trata en la página 4 |
| Ubuntu, Debian, Fedora... | Libre (GPL y otras) | Escritorio, servidores, desarrollo | Muchas distribuciones, un mismo núcleo |
| macOS | Propietaria | Escritorio de Apple | Solo en hardware de Apple |
| Android | Libre en la base, con componentes propietarios | Móviles y tabletas | Basado en el núcleo Linux |
