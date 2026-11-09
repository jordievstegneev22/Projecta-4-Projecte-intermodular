# Tasca 5: Disaster Recovery Plan: pla de còpies

> Enunciat original: <https://github.com/SMX-0226SI/NF2AA2-PlaCopies/>

## Enunciat

## Presentació de l'activitat

### Introducció al cas

"Muntatges i Serveis Tècnics SL" és una petita empresa dedicada a la instal·lació i manteniment d'equips industrials. L'empresa disposa de la següent infraestructura informàtica:

- Servidor de Fitxers (Ubuntu Server): Conté tota la documentació crítica:
  - Documents de Projectes: Plànols, especificacions tècniques (300 GB, creixement moderat).
  - Bases de Dades (Comptabilitat i Clients): Crítiques i d'ús diari (20 GB, canvi constant).
  - Carpetes Personals dels Usuaris: Per a la feina diària (100 GB).

- 10 Equips Clients (Windows 11): Els usuaris treballen majoritàriament amb fitxers del servidor, però alguns tècnics guarden de forma temporal informes i altres arxius importants a la carpeta Documents.
- Connexió a Internet: Fibra òptica de 600 Mbps (simètrica).

Un anàlisi dels requisits de recuperació permet determinar les necessitats específiques per a la gestió de les còpies de seguretat:

- Temps de Recuperació (RTO): Les dades de Comptabilitat/Clients han d'estar disponibles en menys de 4 hores.
- Pèrdua de Dades Admesa (RPO): Es pot admetre una pèrdua màxima de 24 hores per a la majoria de dades, però les dades de Comptabilitat/Clients no poden perdre més de 4 hores de treball.
- Retenció: Cal guardar les dades amb un historial d'almenys un mes.

### Durada de l'activitat

La durada estimada de l'activitat és de 3 hores.

### Objectius de l'activitat

L’objectiu d’aquesta activitat és treballar els aspectes relatius a la política de còpies de seguretat mitjançant treball cooperatiu.

### Competències treballades

a) Determinar la logística associada a les operacions d’instal·lació, configuració i manteniment de sistemes microinformàtics, interpretant-ne la documentació tècnica associada i organitzant els recursos necessaris.

j) Elaborar documentació tècnica i administrativa del sistema, complint les normes i reglamentació del sector, per al seu manteniment i l’assistència al client.

### Resultats d'aprenentatge i criteris d'avaluació

RA2. Gestiona dispositius d'emmagatzematge descrivint els procediments efectuats i aplicant tècniques per assegurar la integritat de la informació.

2.5 Selecciona estratègies per a la realització de còpies de seguretat.

2.6 Té en compte la freqüència i l'esquema de rotació.

2.7 Realitza còpies de seguretat amb diferents estratègies.

### Continguts

2.4 Còpies de seguretat i imatges de suport. Mitjans d'emmagatzematge.

### Capacitats clau

- Autonomia
- Organització del treball
- Relació interpersonal
- Responsabilitat
- Treball en equip

### Semàfor ús de la IA

🟠 Aquesta activitat permet un ús parcial o restringit.

- Permès per com a eina de suport en la millora de la redacció dels informes, cerca preliminar d'informació, estructuració d'idees o explicació de conceptes teòrics complexos.
- Condicions: Cal processar, entendre i validar sempre els resultats rebuts. **Està totalment prohibit copiar l'enunciat d'un exercici directament al xat de la IA i enganxar la resposta generada** per al lliurament final sense treball propi ni anàlisi crítica.

## Realització pràctica

### Fase 1: Treball individual

De forma individual, heu de donar resposta a les següents preguntes basant-se en el cas pràctic:

1. Què copiar? (Priorització): Quines són les dades més crítiques del servidor? Cal fer còpia dels 10 equips clients? Justifica-ho.

2. Periodicitat i Tipus de Còpia: Proposa un calendari bàsic per a la setmana (Diari/Setmanal/Mensual) i quin tipus de còpia aplicaràs (Completa, Diferencial, Incremental) per a les dades crítiques.

3. Mitjans i Ubicació: Quin tipus de mitjà de còpia utilitzaries (Discs durs externs, NAS, Cloud, Cintes)? On s'hauria de guardar físicament la còpia més recent (Regla 3-2-1).

### Fase 2: Treball en grup

1. Discussió i Consens: Comparen les seves respostes individuals (Fase 1).

2. Elaboració d'una Proposta Unificada: Heu de consensuar i dissenyar el vostre propi Esquema 3-2-1 de Còpies (3 còpies, 2 mitjans, 1 fora de lloc) basat en els requisits del cas.

    | Element             | Proposta Unificada | Justificació |
    |---------            |------------------  |--------------|
    | Dades crítiques     |                    |              |
    |Periodicitat         |                    |              |
    |Tipus de còpia       |                    |              |
    |Mitjà 1 (local)      |                    |              |
    |Mitjà 2 (extern)     |                    |              |

3. Elaboració de la proposta final:

    - Dades objecte de la còpia de seguretat: Quines dades es copien i amb quina freqüència, indicant clarament les crítiques i les no crítiques.
    - Cronograma setmanal detallat.

    | Dia                 | Dades              | Tipus de còpia | Mitjà |
    |---------            |------------------  |--------------  |-------|
    |Dilluns              |                    |                |       |
    |Dimarts              |                    |                |       |
    |...                  |                    |                |       |
    |Diumenge             |                    |                |       |

4. Elecció dels mitjans i ubicació (Regla 3-2-1)

   - Mitjà 1 (Local): Quin mitjà concret (p. ex., Disc dur USB, NAS) s'utilitza.
   - Mitjà 2 (Extern): Quin mitjà (p. ex., Cloud, LTO) i el proveïdor proposat (p. ex., Azure, Google Cloud, servei local).
   - Ubicació Fora de Lloc: On es guarda la còpia externa (física o lògica) i qui és el responsable de la seva gestió.

5. Estratègia de recuperació

   Com es garanteix que les dades de Comptabilitat/Clients compleixen amb el requisit de RPO (4 hores) i RTO (4 hores).

## Què cal lliurar?

- Document amb les respostes individuals (Fase 1).
- Document amb la proposta unificada (Fase 2).
- Document amb la proposta final, incloent el cronograma setmanal i l'estratègia de recuperació.

## Materials i recursos

- Material propi del mòdul. NF2.AA3 Còpies de seguretat.

- INCIBE. Copias de seguridad. Una guía de aproximación para el empresario (PDF). [Enllaç](https://www.incibe.es/sites/default/files/contenidos/guias/guia-copias-de-seguridad.pdf)

- Xataka. *Backup 3 - 2 - 1, el método definitivo para mantener a salvo tus datos* (Youtube) 2017. [Enllaç](https://youtu.be/PM_M4Iz6I4o?si=F7DRyDDTZE3hjWn8)
