# Tasca 7: Monitorització de servidors Linux

> Enunciat original: <https://github.com/SMX-SOX/activitats/blob/main/nf1-Linux/UD5/AA2-monitoritzaci%C3%B3.md>

## Enunciat

## Presentació de l'activitat

### Durada de l'activitat

La durada prevista de l'activitat és de 4 hores a classe.

### Objectius específics de l'activitat

- Conèixer eines de gestió gràfica de servidors Linux.
- Instal·lar i configurar eines de gestió de servidors Linux.
- Presentar les conclusions d'una comparativa entre diferents eines de gestió de servidors Linux.

### Competències treballades

h) Mantenir sistemes microinformàtics i xarxes locals, substituint-ne, actualitzant-ne i ajustant-ne els components, per assegurar el rendiment del sistema en condicions de qualitat i seguretat.

### Resultats d'aprenentatge i criteris d'avaluació

RA5. Realitza tasques de monitorització i ús del sistema operatiu en xarxa, descrivint les eines utilitzades i identificant-ne les principals incidències

- 5.1 Descriu les característiques dels programes de monitoratge.
- 5.2 Identifica problemes de rendiment en els dispositius d'emmagatzematge.
- 5.3 Observa l'activitat del sistema operatiu en xarxa a partir de les traces generades pel propi sistema.
- 5.4 Executar tasques de manteniment del programari del servidor (apt, neteja i gestió de serveis).
- 5.6 Interpretar la configuració i l'estat de la xarxa del sistema operatiu.

### Continguts

- 5.Monitoratge i ús del sistema operatiu en xarxa

### Capacitats clau treballades

- Autonomia
- Organització del treball
- Resolució de problemes
- Responsabilitat

### Semàfor ús de la IA

🟠 Aquesta activitat permet un ús parcial o restringit.

Permès per com a eina de suport en la millora de la redacció dels informes, cerca preliminar d'informació, estructuració d'idees o explicació de conceptes teòrics complexos.

Condicions: Cal processar, entendre i validar sempre els resultats rebuts. Està totalment prohibit copiar l'enunciat d'un exercici directament al xat de la IA i enganxar la resposta generada per al lliurament final sense treball propi ni anàlisi crítica.

## Enunciat de l'activitat

### Requeriments previs

Necessiteu el servidor Ubuntu en funcionament i amb el gestor web instal·lat a l'activitat anterior (Cockpit, Webmin o Ajenti).

### Part 1. Característiques de les eines de monitoratge

1. Recol·lecció d'informació del sistema:

   - Executa les comandes `dmidecode`, `lshw` i `lscpu` per obtenir informació del maquinari del sistema. Documenta els resultats obtinguts.

2. Comparativa d'eines de monitoratge en temps real:

   - Prova les comandes de text `top` i `htop` per examinar l'ús de CPU i memòria RAM.
   - Executa `free -h` i `vmstat` per analitzar l'estat de la memòria i de l'espai d'intercanvi (swap). Explica els resultats obtinguts i les diferències entre les dues eines.
   - Instal·la i prova l'eina `btop`. Documenta les diferències amb les eines anteriors.
   - Accedeix al gestor web instal·lat a l'activitat anterior i documenta les funcionalitats de monitoratge que ofereix. Compara-les amb les eines de línia d'ordres.

3. Anàlisi de la configuració de xarxa:

   - Mostra la configuració de xarxa del sistema utilitzant la comanda `ip addr`.
   - Utilitza la comanda `ss` per llistar els sockets actius, la taula de connexions i els ports en escolta al servidor.
   - Identifica els ports que estan escoltant a la xarxa i documenta els resultats.

### Part 2. Monitorització i diagnòstic d'emmagatzematge

1. Anàlisi de l'espai de disc:

   - Executa `df -h` per veure l'espai utilitzat i disponible en els sistemes de fitxers muntats.
   - Utilitza `du -sh /var/log` per comprovar quant espai ocupa el directori de registres del sistema.
   - Instal·la i prova l'eina `ncdu` per analitzar l'ús de l'espai de disc. Observa com et permet explorar els directoris i identificar quins fitxers o carpetes ocupen més espai.

2. Diagnòstic de rendiment de I/O (lectura i escriptura):

   - Executa `iostat -xz 1` per mostrar els resultats, refrescant cada segon i amagant dispositius inactius. Observa les següents mètriques:
       - `r/s` i `w/s`: Nombre de lectures i escriptures per segon.
       - `rkB/s` i `wkB/s`: Quantitat de dades llegides i escrites per segon (en KB).
       - `%util`: Percentatge d'utilització del dispositiu.
       - `await`: Temps mitjà d'espera per a les operacions d'I/O.

### Part 3. Logs del sistema

1. Consulta i filtre de logs amb journalctl:

    - Examina els esdeveniments emmagatzemats a `/var/log` mitjançant el dimoni journalctl. Utilitza la comanda `journalctl`.
    - Executa `journalctl -u ssh` per veure exclusivament les traces del servei SSH.
    - Executa `journalctl -f` per veure l'arribada de registres en temps real.
    - Realitza un intent de connexió SSH fallit des d'un altre equip i localitza la línia de registre generada.
    - Filtra els logs del sistema per prioritat d'error utilitzant:
      - `journalctl -p err` per veure només els errors.

2. Consulta des del gestor web:

    - Accedeix al gestor web instal·lat a l'activitat anterior i localitza la secció de logs del sistema. Documenta les funcionalitats que ofereix per a la consulta i filtratge de logs.
    - Mostra els logs corresponents a l'autenticació d'usuaris i identifica els intents de connexió fallits.

### Part 4. Automatització de tasques de manteniment

1. Podem automatitzar tasques, com podem ser la neteja de fitxers temporals, la gestió de serveis o l'actualització del sistema. Per això, utilitzarem el programador de tasques cron.

    - Configura una tasca programada al fitxer de crontab de l'administrador (sudo crontab -e) perquè cada dia a les 20:00 hores s'executi la comanda `apt update && apt upgrade -y` per actualitzar el sistema. Documenta els passos seguits i comprova que la tasca s'ha afegit correctament.

## Què cal lliurar?

Cada alumne ha de lliurar un Dossier Tècnic que inclogui la documentació de totes les parts de l'activitat, amb captures de pantalla i explicacions dels resultats obtinguts.El dossier ha d'estar ben estructurat i redactat.

Aquest dossier estarà en format **Markdown** i es trobarà a la carpeta corresponent a la tasca en el vostre repositori del projecte.

## Material de suport

- Material propi del mòdul. [NF1.UD5. Administració avançada](https://github.com/SMX-SOX/Materials/blob/main/NF1_SOX_Linux/UD5-Administraci%C3%B3_Avan%C3%A7ada/README.md)

- Arsys. [Cron jobs: una Guía Completa](https://www.arsys.es/blog/cron-jobs-una-guia-completa/)
