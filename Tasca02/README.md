# Tasca 2: Git, viatjant pel temps

> Enunciat original: <https://github.com/SMX-ProjecteIntermodular/Projecte4/blob/main/activitats/Tasca02-Git.md>

## Enunciat

## Presentació de l'activitat

### Durada de l'activitat

La durada prevista de l'activitat és 2 hores a classe.

### Objectius específics de l'activitat

- Gestionar un repositori mitjançant Git i GitHub.
- Generació de documentació en format Markdown.

### Competències treballades

m) Organitzar i desenvolupar el treball assignat mantenint unes relacions professionals adequades en l’entorn de treball.

### Resultats d'aprenentatge i criteris d'avaluació

- 1713.4. Realitza el seguiment de l'execució de les activitats plantejades, verificant que es compleix amb la planificació.
- 1713.5. Transmet informació amb claredat, de manera ordenada i estructurada.

#### Criteris d'avaluació

4.6 Defineix i elabora la documentació necessària per a l'avaluació de les activitats i del projecte en el seu conjunt.

5.1 Manté una actitud ordenada i metòdica en la transmissió de la informació.

5.4 Coneix els termes tècnics en altres llengües que siguin estàndards del sector.

### Continguts

- Control de versions.

### Capacitats clau treballades

- Autonomia
- Organització del treball
- Resolució de problemes
- Responsabilitat

### Semàfor ús de la IA

🟠 Aquesta activitat permet un ús parcial o restringit.

Permès per com a eina de suport en la millora de la redacció dels informes, cerca preliminar d'informació, estructuració d'idees o explicació de conceptes teòrics complexos.

**Condicions**: Cal processar, entendre i validar sempre els resultats rebuts. Està totalment prohibit copiar l'enunciat d'un exercici directament al xat de la IA i enganxar la resposta generada per al lliurament final sense treball propi ni anàlisi crítica.

## Enunciat de l'activitat

### Requeriments previs

- Compte de GitHub actiu
- Git instal·lat al vostre ordinador
- Editor VS Code
- Identitat de git configurada al repositori local:

  ```bash
    git config user.name "Nom Cognoms"
    git config user.email el_teu_correu@mataro.epiaedu.cat
  ```

### Descripció de l'activitat

En aquesta activitat treballarem un aspecte fonamental de control de versions: l'historial de versions. Veurem com podem viatjar en el temps dins del nostre repositori i recuperar versions anteriors dels fitxers.

Per fer aquesta activitat, creareu un repositori **public** anomenat `projecte4-tasca02`. En aquest repositori realitzareu els exercicis pràctics, que documentareu en un fitxer `solucions.md` dins la carpeta `tasca02` del vostre repositori de projecte4.

#### Part 1: Bones pràctiques en la documentació de commits

Tot i que ja esteu acostumats a fer commits, és important que aquests estiguin ben documentats. Per això, en aquesta part de l'activitat, revisarem els vostres últims commits i els millorarem si cal.

**Exercici pràctic**: crear commits estructurats

1. Crea un fitxer anomenat README.md a dins el repositori amb una breu descripció de la funcionalitat del repositori.
1. Fes el teu primer commit seguint la convenció de missatges de commit que hem vist abans:

   `"docs: inicialitza la repositori de prova per tasca 2"`

1. Crea un fitxer de configuració de la xarxa anomenat `00-installer-config.yaml` amb el següent contingut:

   ```yaml
   network:
     version: 2
     renderer: networkd
     ethernets:
       enps03:
         dhcp4: true
   ```

  i fes un commit following les bones pràctiques, tenint clar que és del tipus `feat`.

#### Part 2: L'historial del repositori

Com a profesional, sovint hauràs de revisar l'historial del repositori per saber qui va fer un canvi determinat o quin dia es va modificar una configuració concreta.

1. Observa l'historial del teu repositori amb l'historial detallat, identificant la informació disponible per cada commit, executant a la terminal la comanda:

  ```bash
  git log
  ```

1. Compara el resultat amb l'historial resumit, executant a la terminal la comanda:

  ```bash
  git log --oneline
  ```

1. Usa l'eina gràfica de l'editor VS Code per veure l'historial del repositori i comparar-lo amb el que has vist a la terminal.

1. Inspecciona els canvis exactes del darrer commit amb la comanda:

  ```bash
  git show HEAD
  ```

  Observa també la mateixa informació amb l'eina gràfica de l'editor VS Code, seleccionant el darrer commit.

  > **Nota**: `HEAD` és un punter que apunta al darrer commit del teu repositori, ja que l'historial funciona com una pila, on el darrer commit és el primer que es mostra.

#### Part 3: Recuperar versions anteriors del working directory

Imagina que estàs editant el fitxer `00-installer-config.yaml` i, per error, esborres informació crucial o comets una erada de tecleig. Com que encara no has fet git commit, pots descartar els canvis del Working Directory i tornar el fitxer a l'estat del darrer commit.

1. Simularem un error humà editant el fitxer `00-installer-config.yaml`:

  ```yaml
  network:
    version: 2
    renderer: networkd
    ethernets:
      eth0:
        dhcp4: true
  ```

1. Observa l'estat del repositori. A l'editor VS Code, veuràs que el fitxer interficies.yaml apareix amb una `M`, indicant que ha estat modificat. A la terminal, executa la comanda al terminal per comprovar com t'indica aquesta situació:

  ```bash
  git status
  ```

1. Desfem els canvis del fitxer interficies.yaml i tornem-lo a l'estat del darrer commit. Això es pot fer gràficament a l'editor VS Code, fent clic amb el botó dret sobre el fitxer i seleccionant l'opció "Discard changes". També es pot fer des de la terminal amb la comanda:

  ```bash
  git restore 00-installer-config.yaml
  ```
  
  Una vegada fet això, comprova que el fitxer ha tornat a l'estat del darrer commit.

Pots comprovar que l'estat del repositori és correcte amb la comanda `git status` o mirant l'eina gràfica de l'editor VS Code:el Working Directory, l'àrea temporal (staging area) i l'historial del repositori estan correctament sincronitzats.

#### Part 4: Recuperar versions anteriors (commits) del repositori

A la part anterior, git ens ha estat útil per descartar els canvis en els que estàvem treballant, però què passa si volem recuperar l'estat del projecte en un commit anterior? Exemple, hem estat treballant en una idea, fent els commits corresponents, però ens adonem que no funciona i volem tornar a l'estat on sí funcionava. Com podem retrocedir en el temps?

1. Editarem l'arxiu `00-installer-config.yaml` i modificarem el valor de `dhcp4` a `false` i afegirem una un configuració estàtica a la interfície `enps03`:

  ```yaml
  network:
    version: 2
    renderer: networkd
    ethernets:
      enps03:
        dhcp4: false
        addresses: [192.168.1.100/24]
        routes:
          - to: default
            via: 192.168.1.1
        nameservers:
          addresses: [192.168.1.1]
  ```

  Genereu un commit amb un missatge clar i concís, seguint les bones pràctiques que hem vist abans.

1. Editem de nou l'arxiu, ara per configurar una segona interfície `enps08` en mode DHCP:

  ```yaml
  network:
    version: 2
    renderer: networkd
    ethernets:
      enps03:
        dhcp4: false
        addresses: [192.168.1.100/24]
        routes:
          - to: default
            via: 192.168.1.1
        nameservers:
          addresses: [192.168.1.1]
      enps08:
        dhcp4: true
  ```
  
  De nou, feu un commit amb un missatge clar i concís, seguint les bones pràctiques que hem vist abans.

1. Per últim, incorporem canviem els servidor DNS a la configuració de la interfície `enps03`, perquè la configuració inicial no funcionava correctament:

  ```yaml
  network:
    version: 2
    renderer: networkd
    ethernets:
      enps03:
        dhcp4: false
        addresses: [192.168.1.100/24]
        routes:
          - to: default
            via: 192.168.1.1
        nameservers:
          addresses: [8.8.8.8]
  ```

  Fent un darrer commit, descriptiu.

Revisa ara l'historial del repositori amb la comanda `git log --oneline` o amb l'eina gràfica per veure tot l'historial de commits realitzats.

1. Anem a documentar un exemple de `revert` d'un commit:

   - Volem tornar a l'estat del segon commit (on s'ha creat l'arxiu `00-installer-config.yaml` amb la configuració dhcp de la interfície `enps03`). Pots fer-ho amb la comanda o amb l'eina gràfica.
   - Mostra amb l'eina `git graph` l'historial del repositori i comprova que s'ha creat un nou commit amb la informació de revert.

1. Explorar un commit anterior:

   - Usant l'eina gràfica exploreu el quart commit.
   - Documenta com s'obre a l'editor una pestanya amb l'estat de l'arxiu en aquell momment, mostrant els canvis (verd insercions i en vermell eliminacions) respecte al commit anterior.

1. Finalment, tot i que no és un procediment recomanat, anem a eliminar el darrer commit (`git reset`)

   - Mostra l'historial actual amb la comanda `git log --oneline`.
   - Elimina el darrer commit bé sigui amb la comanda `git reset --hard HEAD~1` o amb l'eina gràfica.
   - Observa com ha quedat l'arxiu a l'editor.
   - Indica l'estat del repositori amb la comanda `git status` i mostra l'historial amb la comanda `git log --oneline`.

### Què cal lliurar, on i com?

Documentació de l'activitat en format Markdown a la carpeta `tasca02` del vostre repositori del projecte 4. La carpeta ha d'incloure:

- Un fitxer `README.md` amb una breu descripció de la tasca, resum de l'enunciat, etc.
- Carpeta `img` amb les captures de pantalla que consideris rellevants.
- Un fitxer `solucions.md` amb les solucions dels exercicis realitzats.
Aquesta arxiu ha d'incloure els diferents exercicis realitzats, incorporant les accions realitzades i les captures de pantalla o sortides de la terminal que consideris rellevants.

## Material de suport

- Material propi.[Guia 1. Iniciació a Git, GitHub i Visual Studio Code](https://github.com/SMX-ProjecteIntermodular/Guies/blob/main/git-github/01-iniciacio-git-github-vscode.md)

- Material propi. [Guia 2. Treballant en el repositori amb Git](https://github.com/SMX-ProjecteIntermodular/Guies/blob/main/git-github/02-comandes-git.md)

- Material propi. [Guia 3. Markdown per treballar amb repositoris](https://github.com/SMX-ProjecteIntermodular/Guies/blob/main/git-github/03-markdown.md)

- Material propi. [Guia 4. Creació de repositoris a partir de plantilla](https://github.com/SMX-ProjecteIntermodular/Guies/blob/main/git-github/04-crear-repositoris.md)

- Material propi. [Guia 5. Resolució de conflictes amb Git](https://github.com/SMX-ProjecteIntermodular/Guies/blob/main/git-github/05-resolucio-conflictes.md)

- Material propi. [Guia 6. Gestió historial amb Git](https://github.com/SMX-ProjecteIntermodular/Guies/blob/main/git-github/06-gestio-historial.md)

- GitHub. [Documentación de GitHub](https://docs.github.com/es)
