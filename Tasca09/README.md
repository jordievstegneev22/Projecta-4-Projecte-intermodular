# Tasca 9: Instal·lant un servidor de noms

> Enunciat original: <https://github.com/SMX-ProjecteIntermodular/Projecte4/blob/main/activitats/Tasca09-DNS.md>

## Enunciat

## Presentació de l'activitat

Després de l’experiència de formació, els nostres clients de `Digicore` estan satisfets amb la nostra feina i ens encarreguen la implantació des de zero dels seus serveis de DNS interns.

Actualment, l’agència fa servir adreces IP per accedir als seus servidors de desenvolupament, bases de dades i eines de gestió interna. Aquest mètode dificulta el treball diari:

- Usabilitat deficient: els empleats han de memoritzar o buscar constantment adreces IP, com ara 192.168.10.25.
- Manteniment feixuc: quan un servidor canvia d’IP, cal revisar les aplicacions que hi accedeixen directament amb aquesta adreça.
- Identificació dels serveis: els noms permeten reconèixer més fàcilment quin recurs es vol utilitzar.

La nostra missió és implementar un sistema de noms de domini (DNS) intern perquè els equips puguin localitzar els serveis de l’agència mitjançant noms fàcils de recordar.

### El vostre repte

Utilitzareu BIND9 per instal·lar i configurar un servidor DNS primari en un sistema Linux. Creareu una zona directa, per resoldre noms a adreces IP, i una zona inversa, per consultar el nom associat a una IP.

Per fer la prova de concepte, usareu el domini digicore-XX.test, on XX serà el vostre número de llista. A més, cada servidor allotjarà una zona secundària del domini d’un company o companya de l’equip.

**Pas previ**: prepareu un Ubuntu Server amb 4 GB de RAM i 20 GB de disc. La màquina virtual tindrà una interfície en adaptador pont i una segona en host-only. El professorat indicarà els paràmetres de xarxa a l’inici del repte. Instal·leu BIND9 i el servei SSH per administrar el servidor i copiar els fitxers de configuració al vostre repositori de GitHub.

### Durada de l'activitat

La durada prevista de l’activitat és de 8 hores a classe. Caldrà dedicar el temps de treball autònom necessari a repassar conceptes i completar la documentació.

### Objectius específics de l'activitat

- Instal·lar i configurar un servidor DNS amb BIND9 que resolgui noms interns i consultes externes, utilitzant zones directa i inversa, registres coherents i un reenviador.
- Coordinar-se amb l’equip per configurar zones primàries i secundàries, autoritzar transferències i comprovar que les còpies de zona es poden consultar.
- Verificar el funcionament des del client, diagnosticar incidències de manera sistemàtica i documentar la configuració, les proves i les solucions amb evidències pròpies.

### Competències treballades

f) Instal·lar, configurar i mantenir serveis multiusuari, aplicacions i dispositius compartits en un entorn de xarxa local, atenent les necessitats i els requeriments especificats.

g) Realitzar proves funcionals en sistemes microinformàtics i xarxes locals, localitzant i diagnosticant disfuncions per comprovar i ajustar-ne el funcionament.

### Resultats d'aprenentatge i criteris d'avaluació

0227RA2. Instal·la serveis de resolució de noms, descrivint-ne les característiques i aplicacions.

#### Criteris d'avaluació

2.4 Instal·la un servei jeràrquic de resolució de noms.
2.5 Prepara el servei per a emmagatzemar les respostes procedents de servidors de xarxes públiques i servir-les als equips de la xarxa local.
2.6 Afegeix registres de noms corresponents a una zona nova, amb opcions relatives a servidors de correu i àlies.
2.7 Treballa en grup per realitzar transferències de zona entre dos o més servidors.
2.8 Comprova el funcionament correcte del servidor.

### Continguts

C2. Instal·lació de serveis de resolució de noms:

    - 2.1 Sistemes de noms plans i jeràrquics.
    - 2.2 Zones primàries i secundàries. Transferències de zona. Tipus de registres.

### Capacitats clau treballades

- Autonomia
- Organització del treball
- Treball en equip
- Innovació
- Resolució de problemes
- Responsabilitat
- Relació interpersonal

### Semàfor d’ús de la IA

🟠 L’ús de la IA està permès parcialment o amb restriccions.

Se’n permet l’ús com a eina de suport per a millorar la redacció dels informes, fer cerques preliminars d’informació, estructurar idees o explicar conceptes teòrics complexos.

**Condicions**: Cal processar, entendre i validar sempre els resultats rebuts. Està totalment prohibit copiar l'enunciat d'un exercici directament al xat de la IA i enganxar la resposta generada per al lliurament final sense treball propi ni anàlisi crítica.

Cal entendre i validar les propostes en les vostres màquines. No es permet lliurar configuracions, explicacions o captures alienes com si fossin pròpies, ni copiar l’enunciat al xat i presentar-ne la resposta sense treball propi ni anàlisi crítica. Si utilitzeu IA, indiqueu al README per a què i com heu comprovat el resultat.

Durant l’avaluació pràctica individual, l’ús de la IA no està permès. Es manté com a únic material de suport el full manuscrit indicat.

## Enunciat de l'activitat

### Requeriments previs

Cal disposar del servidor Ubuntu Server instal·lat i configurat a l’activitat anterior. Si és necessari, torneu-lo a desplegar a partir del fitxer .OVA que vau generar. Reviseu els recursos i els adaptadors indicats a l’apartat El vostre repte.

Necessitareu un client Zorin o equivalent, permisos d’administració, accés a Internet i al repositori de GitHub del Projecte Intermodular. Tingueu disponible l’eina `dig` al client i al servidor; si no hi està instal·lada, instal·leu-la mitjançant el paquet `dnsutils`.

Abans de començar, reviseu IP, màscara, porta d’enllaç, DNS, adaptadors virtuals, ordres bàsiques de Linux, edició de fitxers i permisos. Comproveu la connectivitat per IP entre client i servidor i l’accés per SSH.

### Explicacions

Cada alumne o alumna configurarà el seu servidor i el seu domini `digicore-XX.test`. La transferència de zones es farà amb els companys de l’equip del Projecte Intermodular. Cadascú ha de poder explicar la seva configuració i demostrar-ne el funcionament.

Abans de modificar fitxers, anoteu les interfícies, les IP, la màscara, la porta d’enllaç, el domini i el servidor amb què fareu la transferència. Manteniu estable la IP del servidor segons les indicacions del professorat. El client i els servidors dels companys s’han de poder comunicar per la xarxa en pont; la interfície host-only correspon a una xarxa diferent.

Seguiu les accions de l’apartat anterior i documenteu cada fase mentre la feu. Conserveu una còpia dels fitxers abans de modificar-los i expliqueu la funció dels canvis. L’objectiu és comprendre per què cada consulta retorna una resposta determinada.

Abans de reiniciar el servei, consulteu l’ajuda de `named-checkconf` i `named-checkzone` i utilitzeu-les per validar la configuració i les zones. Si alguna cosa falla, reviseu primer l’adreçament i la connectivitat; després, el DNS configurat al client, l’estat del servei, els missatges del registre i els fitxers de zona.

A les proves amb `dig`, identifiqueu el servidor consultat, el nom o la IP demanats, el tipus de registre i la resposta. Diferencieu una consulta enviada explícitament al vostre DNS d’una consulta que utilitza la configuració habitual del client. Una pàgina oberta al navegador, per si sola, no demostra que hagi respost el vostre servidor.

Per a les transferències, reviseu l’autorització per IP, la comunicació pel port TCP 53 i el número de sèrie SOA. Investigueu on ha de desar BIND9 les zones secundàries a Ubuntu i quins permisos necessita. Comproveu una actualització de la zona al primari i la seva arribada al secundari.

Documenteu les incidències amb aquesta seqüència: símptoma observat, hipòtesi, prova realitzada, resultat, correcció i comprovació final. Si no apareix cap incidència, expliqueu les verificacions preventives que heu fet. Les captures han de mostrar evidències reals i anar acompanyades d’una interpretació breu.

### Què cal lliurar, on i com?

Cada alumne o alumna lliurarà a Moodle l’enllaç a la seva carpeta de treball dins del repositori de GitHub del Projecte Intermodular. Utilitzeu la carpeta `tasca09` i assegureu-vos que el professorat hi tingui accés. El termini serà el que s’indiqui a Moodle.

La carpeta ha d’incloure:

- **README.md** amb identificació de l’autor i l’equip, objectiu, esquema o taula d’adreçament, domini, configuració aplicada, proves, incidències i conclusions. Expliqueu també com configurar el DNS al client i fer una consulta bàsica.
- **config/** amb les còpies finals de named.conf.options, named.conf.local i els fitxers de les zones primàries. Documenteu la declaració de la zona secundària i la seva procedència. Els fitxers han de coincidir amb la configuració comprovada.
- **captures/** amb imatges llegibles i referenciades des del README. Cal mostrar l’estat del servei, les validacions, el DNS del client, una consulta externa i les consultes A, CNAME i PTR. Incloeu també proves de la transferència i de l’actualització de la zona secundària.
- **Una taula de comprovacions al README** amb la consulta o ordre executada, el resultat esperat, el resultat obtingut i la conclusió. A les conclusions, indiqueu què heu après i què revisaríeu o milloraríeu.
- **Fonts consultades** i, si escau, una breu declaració de l’ús de la IA. Identifiqueu el company o companya amb qui heu fet la transferència i la vostra aportació al treball compartit.

Publiqueu només els fitxers necessaris per reproduir i entendre la pràctica. No inclogueu contrasenyes, claus privades SSH ni fitxers amb secrets. Comproveu que les imatges i els enllaços del `README.md` es visualitzen correctament abans de lliurar-lo.

## Material de suport

1. Apunts i presentacions de DNS de la RA2, guia d’Ubuntu Server i materials de repàs de TCP/IP disponibles a Moodle.

1. Ajuda local de les eines: man dig, man named-checkconf i man named-checkzone.

1. Ubuntu Server Documentation · [Domain Name Service (DNS)](https://ubuntu.com/server/docs/how-to/networking/install-dns/)

1. BIND9 Documentation. [3. Configurations and Zone Files](https://bind9.readthedocs.io/en/stable/chapter3.html)

1. XTEC. [Mòduls professionals de SMX a partir del curs 2024–2025 [docx]](https://xtec.gencat.cat/web/.content/curriculum/professionals/fp/titolsloe/infcomunicacions/mp_IC10_sistemes_microinformatics_xarxes_20240424.docx)
