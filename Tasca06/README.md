# Tasca 6: Administració avançada de servidors Linux

> Enunciat original: <https://github.com/SMX-SOX/activitats/blob/main/nf1-Linux/UD5/AA1-gestors_web.md>

## Enunciat

## Presentació de l'activitat

A **Núvol Clar Serveis**, fins ara, el seu equip tècnic ha gestionat els servidors principalment des de la línia d'ordres, i busca eines gràfiques que en facilitin la supervisió i les tasques habituals sense perdre de vista la seguretat i les necessitats de l'organització.

En aquesta activitat, treballareu com a equip d'assessorament tècnic. Cada membre instal·larà i provarà un gestor gràfic en un servidor Ubuntu, documentarà el procés i explorarà algunes de les seves funcions. Després posareu en comú els resultats, comparareu les solucions i presentareu a **Núvol Clar Serveis** una recomanació justificada sobre quina eina s'ajusta millor al seu entorn.

### Durada de l'activitat

La durada prevista de l'activitat:

- 4 hores a SOX (inclou explicació inicial).
- 4 hores al Projecte Intermodular (posada en comú, elaboració de la presentació i exposició final).

### Objectius específics de l'activitat

- Conèixer eines de gestió gràfica de servidors Linux.
- Instal·lar i configurar eines de gestió de servidors Linux.
- Presentar les conclusions d'una comparativa entre diferents eines de gestió de servidors Linux.

### Competències treballades

h) Mantenir sistemes microinformàtics i xarxes locals, substituint-ne, actualitzant-ne i ajustant-ne els components, per assegurar el rendiment del sistema en condicions de qualitat i seguretat.

l) Assessorar i assistir al client, canalitzant a un nivell superior els supòsits que ho requereixin per trobar solucions adequades a les necessitats d’aquest.

### Resultats d'aprenentatge i criteris d'avaluació

0224.RA 5. Realitza tasques de monitorització i ús del sistema operatiu en xarxa, descrivint les eines utilitzades i identificant-ne les principals incidències

- 5.6 Interpreta la informació de configuració del sistema operatiu en xarxa.

1713.RA5. Transmet informació amb claredat, de manera ordenada i estructurada.

- 5.1 Manté una actitud ordenada i metòdica en la transmissió de la informació.
- 5.2 Transmet informació verbal tant horitzontal com verticalment.
- 5.3 Transmet informació entre els membres del grup utilitzant mitjans informàtics.
- 5.4 Coneix els termes tècnics en altres llengües que siguin estàndards del sector.

### Continguts

- Gestió gràfica de servidors Linux.

### Capacitats clau treballades

- Autonomia
- Organització del treball
- Treball en equip
- Resolució de problemes
- Responsabilitat
- Relació interpersonal

### Semàfor ús de la IA

🟠 Aquesta activitat permet un ús parcial o restringit.

Permès per com a eina de suport en la millora de la redacció dels informes, cerca preliminar d'informació, estructuració d'idees o explicació de conceptes teòrics complexos.

Condicions: Cal processar, entendre i validar sempre els resultats rebuts. Està totalment prohibit copiar l'enunciat d'un exercici directament al xat de la IA i enganxar la resposta generada per al lliurament final sense treball propi ni anàlisi crítica.

## Enunciat de l'activitat

### Requeriments previs

Aquesta activitat es realitzarà en equips de dos-tres membres. Cadascun dels membres de l'equip haurà de tenir accés a un servidor Linux (Ubuntu Server) instal·lat i configurat de l'activitat anterior. Cas que sigui necessari, torneu a desplegar el servidor a partir del fitxer `.OVA` que vau generar.

### Fase 1: individual

Cada alumne treballa amb el seu servidor Linux (Ubuntu Server). Cada alumne tria un dels gestors gràfics disponibles. Els gestors gràfics mínims a instal·lar i configurar són:

- Cockpit
- Webmin
- Ajenti (pels grups de tres alumnes)

#### Documentació de la instal·lació

- Instal·lar el gestor assignat mitjançant repositoris o l'script oficial.

- Accedir a la interfície web des del navegador del PC amfitrió usant l'adreça de la interfície `host-only` del servidor Linux i el port per defecte de l'eina (ex. Cockpit: 9090, Webmin: 10000, Ajenti: 8000).

Redactar un guia bàsica amb e següent contingut:

- Comandes utilitzades durant la instal·lació.
- Incidències trobades i com s'han resolt.
- Captures de pantalla del procés d'instal·lació i de l'accés inicial.
- Breu explicació del funcionament de l'eina i de les funcionalitats disponibles. Proveu un parell de funcionalitats de les més rellevants: crear un usuari o un grup, canviar la contrasenya d'un usuari, etc. Incloure captures de pantalla i explicació del procés.

#### Fase 2: Elaboració de la comparativa i exposició final

En primer lloc els membres de l'equip han de compartir les seves experiències amb els diferents gestors gràfics. A continuació, elaboraran una presentació amb les conclusions de la comparativa i una recomanació pel client final. Es recomana seguir la següent estructura per la presentació:

1. Diapositiva inicial amb el nom de l'activitat, nom dels membres de l'equip i data.
2. Diapostives (2-4) amb un resum de la instal·lació i característiques de cada gestor gràfic.
3. Diapositives (2-4) amb demostració dels casos d'ús realitzats per cadascun.
4. Taula comparativa entre les diferents solucions. La taula pot ser com la que es mostra d'exemple:

    | Característica            | Cockpit | Webmin | Ajenti |
    |----------------           |---------|--------|--------|
    | Dificultat d'instal·lació |         |        |        |
    | Depedències necessàries   |         |        |        |
    | Port accés web            |         |        |        |
    | Facilitat d'ús            |         |        |        |
    | Funcionalitats            |         |        |        |

5. Conclusió i recomanació final pel client

### Què cal lliurar?

- Al repositori de lliurament del projecte intermodular, dins la carpeta per l'activitat incloure els següent elements:

  - Fitxer README.md amb l'enunciat de l'activitat.
  - Les guies individual de cada membre de l'equip. A cada guia ha de constar el nom de l'alumne que ha fet la feina.
  - Enllaç a la presentació final amb les conclusions de la comparativa i la recomanació final pel client que podeu posar al final del fitxer `README.md`.

## Material de suport

- Material propi del mòdul. [NF1.UD5. Administració avançada](https://github.com/SMX-SOX/Materials/blob/main/NF1_SOX_Linux/UD5-Administraci%C3%B3_Avan%C3%A7ada/README.md)

- [Webmin](https://www.webmin.com/)

- [Cockpit](https://cockpit-project.org/)

- [Ajenti](https://ajenti.org/)
