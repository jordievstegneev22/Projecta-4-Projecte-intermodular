# Tasca 2: Solucions

## Part 1: Bones pràctiques en commits

Un **commit** és com una "foto" del projecte en un moment concret. Cada vegada que fem un commit, Git guarda com estan els fitxers i hi afegeix un missatge que explica què hem canviat. Així després podem veure l'historial i tornar enrere si cal.

Per això és important que els missatges siguin clars. Fem servir el format **"tipus: descripció"**, que fa que tothom entengui ràpidament què fa cada commit:

- `docs:` canvis a la documentació (per exemple, el README).
- `feat:` una funcionalitat nova (per exemple, una configuració nova).
- `fix:` la correcció d'un error.

Creem el `README.md` i el fitxer `00-installer-config.yaml` i fem els commits seguint la convenció de missatges (`docs:` i `feat:`):

Primer afegim el `README.md` a l'àrea de preparació (staging) i fem el commit de tipus `docs`, perquè és documentació:

```bash
git add README.md
git commit -m "docs: inicialitza la repositori de prova per tasca 2"
```

Després afegim el fitxer de configuració de xarxa i fem el commit de tipus `feat`, perquè és una funcionalitat nova (la xarxa amb DHCP a `enps03`):

```bash
git add 00-installer-config.yaml
git commit -m "feat: afegeix configuració de xarxa amb DHCP a enps03"
```

**Explicació:** `docs` és documentació i `feat` és una funcionalitat nova. Els missatges segueixen el format "tipus: descripció".

---

## Part 2: L'historial del repositori

En aquesta part mirem l'**historial** del repositori, és a dir, la llista de tots els commits que hem fet. Serveix per saber qui ha fet cada canvi, quan l'ha fet i per què.

La comanda `git log` mostra tots els commits amb tota la informació:

```bash
git log
```

![Sortida de git log](img/01.png)

Aquí es veuen els dos commits. De cada un surt el **hash** (el codi llarg que identifica el commit), l'**autor**, el **correu**, la **data** i el **missatge**. Al primer surt `HEAD -> main`, que vol dir que és el commit on som ara.

La comanda `git log --oneline` mostra el mateix historial però resumit, una línia per commit:

```bash
git log --oneline
```

![Sortida de git log --oneline](img/02.png)

Aquí es veu l'historial en format curt: només el **hash curt** (7 caràcters) i el missatge. És més fàcil de llegir quan hi ha molts commits.

**Git Graph** és una extensió de VS Code que mostra l'historial de forma gràfica, amb una línia que uneix els commits.

![Historial del repositori a Git Graph](img/03.png)

Aquí es veu el mateix historial a Git Graph: cada punt és un commit, amb la data, l'autor i el hash. L'etiqueta `main` marca on és la nostra branca i `origin/main` marca on és la còpia de GitHub.

La comanda `git show HEAD` mostra el darrer commit i els canvis que s'hi van fer. **HEAD** és un punter que indica el commit on estem ara (normalment, el darrer).

```bash
git show HEAD
```

![Sortida de git show HEAD](img/04.png)

Aquí es veu la informació del darrer commit (el `feat`) i, a sota, els canvis: surt `new file mode` perquè en aquest commit es va crear el fitxer `00-installer-config.yaml`.

![Darrer commit a Git Graph](img/05.png)

Aquí es veu el darrer commit obert a Git Graph. Surten els detalls (hash, commit pare, autor, data i missatge) i, a la dreta, el fitxer que es va canviar: `00-installer-config.yaml`.

**Explicació:** `git log` mostra el hash, l'autor, el correu, la data i el missatge de cada commit. `git log --oneline` només mostra el hash curt i el missatge. `HEAD` apunta al darrer commit.

---

## Part 3: Recuperar el working directory

El **working directory** és la carpeta on treballem, amb els fitxers tal com els tenim ara a l'editor. Si modifiquem un fitxer i encara no hem fet commit, aquests canvis només són aquí. En aquesta part veiem com descartar uns canvis que hem fet per error i tornar el fitxer com estava al darrer commit.

Primer modifiquem el fitxer `00-installer-config.yaml` (canviem `enps03` per `eth0`) i, sense fer commit, mirem l'estat del repositori amb `git status`:

```bash
git status
```

![git status amb el fitxer modificat](img/06.png)

Aquí es veu que el fitxer surt amb una **M** a VS Code perquè està modificat. A la terminal, `git status` el mostra en vermell com a `modified`, dins de "Changes not staged for commit": és un canvi que encara no està guardat en cap commit.

Amb `git restore` descartem els canvis i el fitxer torna a quedar igual que al darrer commit:

```bash
git restore 00-installer-config.yaml
```

Per comprovar que tot torna a estar bé, tornem a mirar l'estat:

```bash
git status
```

![git restore i git status net](img/07.png)

Aquí es veu que després del `git restore` el `git status` diu `nothing to commit, working tree clean`. Vol dir que ja no hi ha cap canvi pendent i el fitxer torna a ser com al darrer commit.

**Explicació:** `git restore` desfà els canvis del working directory que encara no tenen commit. Després el `git status` surt net.

---

## Part 4: Recuperar commits anteriors

A la Part 3 hem desfet canvis que encara no tenien commit. Però, i si ja hem fet commits i ens adonem que no funcionen? En aquesta part fem tres commits nous i després practiquem dues maneres de tornar enrere: **revert** i **reset**.

### Els tres commits nous

Fem tres canvis al fitxer `00-installer-config.yaml`, cadascun amb el seu commit. La opció `-a` afegeix els fitxers modificats sense haver de fer `git add`.

El primer commit canvia `enps03` de DHCP a **IP fixa** (`192.168.1.100/24`, amb porta d'enllaç i DNS `192.168.1.1`):

```bash
git commit -am "feat: configura IP estàtica a enps03"
```

El segon commit afegeix una segona interfície, **`enps08`**, que agafa la IP per DHCP:

```bash
git commit -am "feat: afegeix interfície enps08 amb DHCP"
```

El tercer commit canvia el servidor **DNS** de `enps03` a `8.8.8.8`. És de tipus `fix` perquè corregeix una configuració que no funcionava:

```bash
git commit -am "fix: canvia el servidor DNS de enps03 a 8.8.8.8"
```

Després mirem l'historial per comprovar que hi són tots:

```bash
git log --oneline
```

![Els 3 commits i git log --oneline](img/08.png)

Aquí es veuen els tres commits nous a sobre dels anteriors. El de més amunt (`fix: canvia el servidor DNS...`) és el darrer i és on apunta `HEAD`.

### Revert

Ara volem tornar a la configuració inicial (DHCP a `enps03`). La comanda `git revert --no-commit HEAD~3..HEAD` desfà els **3 últims commits de cop**. `HEAD~3..HEAD` vol dir "els tres commits des de HEAD cap enrere", i `--no-commit` fa que Git prepari els canvis però no faci el commit encara:

```bash
git revert --no-commit HEAD~3..HEAD
```

Després fem un sol commit amb tots els canvis desfets:

```bash
git commit -m "revert: torna a la configuració DHCP inicial de enps03"
```

El revert **no esborra** els commits antics. Crea un commit nou que fa el contrari dels canvis. Així l'historial es manté complet i sempre podem veure què va passar.

![Revert a Git Graph](img/09.png)

Aquí es veu a Git Graph el commit nou `revert: torna a la configuració DHCP inicial de enps03` a dalt de tot. Els commits anteriors continuen a l'historial, no s'ha esborrat cap.

### Explorar un commit anterior

A Git Graph fem clic sobre un commit i després sobre el fitxer `00-installer-config.yaml`. S'obre una pestanya nova a l'editor que compara el fitxer abans i després d'aquell commit. Les línies en **verd** són les que s'hi han afegit i les línies en **vermell** són les que s'hi han eliminat.

![Explorar el commit d'enps08](img/10.png)

Aquí es veu el commit que afegeix la interfície `enps08`. A l'esquerra hi ha el fitxer abans del commit i a la dreta, després. Les línies 13 i 14 (`enps08:` i `dhcp4: true`) surten en verd perquè són les que s'hi van afegir. No hi ha cap línia en vermell perquè en aquest commit no es va esborrar res.

### Reset

Finalment provem `git reset`. Primer mirem l'historial per saber quin és el darrer commit:

```bash
git log --oneline
```

![Abans del reset](img/11.png)

Aquí es veu l'historial abans del reset: el darrer commit és el `revert` (`c3355db`), i `HEAD` apunta a ell.

La comanda `git reset --hard HEAD~1` **esborra el darrer commit**. `HEAD~1` vol dir "el commit d'abans del darrer", i `--hard` fa que el fitxer també torni a com estava en aquell commit:

```bash
git reset --hard HEAD~1
```

![Fitxer després del reset](img/12.png)

Aquí es veu el fitxer després del reset. Com que hem esborrat el commit del revert, el fitxer torna a tenir la IP fixa i el **DNS `8.8.8.8`**, igual que al commit `fix`.

Comprovem l'estat i l'historial:

```bash
git status
git log --oneline
```

![git status i git log després del reset](img/13.png)

Aquí es veu que el `git status` surt net i que a `git log --oneline` el commit del `revert` ja no hi és. Ara el darrer commit torna a ser `fix: canvia el servidor DNS de enps03 a 8.8.8.8`.

El reset **no es recomana** perquè esborra commits de l'historial. Si el projecte és compartit i algú altre ja tenia aquests commits, es poden crear conflictes i perdre feina.

**Explicació:** El revert no esborra commits, en crea un de nou que desfà els canvis. El reset sí que esborra el commit, per això no es recomana.

---

## Conclusió

- **`git restore`** desfà canvis del working directory que encara **no tenen commit**.
- **`git revert`** desfà commits creant **un commit nou**, sense esborrar res de l'historial. És la manera segura de tornar enrere.
- **`git reset --hard`** **esborra commits** de l'historial. És útil però perillós, sobretot en projectes compartits.
