# Tasca 2: Solucions

## Part 1: Bones pràctiques en commits

Creem el `README.md` i el fitxer `00-installer-config.yaml` i fem els commits seguint la convenció de missatges (`docs:` i `feat:`):

```bash
git add README.md
git commit -m "docs: inicialitza la repositori de prova per tasca 2"

git add 00-installer-config.yaml
git commit -m "feat: afegeix configuració de xarxa amb DHCP a enps03"
```

**Explicació:** Docs és documentació i feat és una funcionalitat nova. Els missatges segueixen el format "tipus: descripció".

---

## Part 2: L'historial del repositori

```bash
git log
git log --oneline
git show HEAD
```

![Sortida de git log](img/01.png)

![Sortida de git log --oneline](img/02.png)

![Historial del repositori a Git Graph](img/03.png)

![Sortida de git show HEAD](img/04.png)

![Darrer commit a Git Graph](img/05.png)

**Explicació:** Git log mostra el hash, l'autor, el correu, la data i el missatge de cada commit. git log --oneline només mostra el hash curt i el missatge. HEAD apunta al darrer commit.

---

## Part 3: Recuperar el working directory

```bash
git status
git restore 00-installer-config.yaml
git status
```

![git status amb el fitxer modificat](img/06.png)

![git restore i git status net](img/07.png)

**Explicació:** Git restore desfà els canvis del working directory que encara no tenen commit. Després el git status surt net.

---

## Part 4: Recuperar commits anteriors

### Els tres commits nous

```bash
git commit -am "feat: configura IP estàtica a enps03"
git commit -am "feat: afegeix interfície enps08 amb DHCP"
git commit -am "fix: canvia el servidor DNS de enps03 a 8.8.8.8"
git log --oneline
```

![Els 3 commits i git log --oneline](img/08.png)

### Revert

```bash
git revert --no-commit HEAD~3..HEAD
git commit -m "revert: torna a la configuració DHCP inicial de enps03"
```

![Revert a Git Graph](img/09.png)

### Explorar un commit anterior

![Explorar el commit d'enps08](img/10.png)

### Reset

```bash
git log --oneline
git reset --hard HEAD~1
git status
git log --oneline
```

![Abans del reset](img/11.png)

![Fitxer després del reset](img/12.png)

![git status i git log després del reset](img/13.png)

**Explicació:** El revert no esborra commits, en crea un de nou que desfà els canvis. El reset sí que esborra el commit, per això no es recomana.
