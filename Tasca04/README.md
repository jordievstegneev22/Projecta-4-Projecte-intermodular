# Tasca 4: Gestió emmagatzematge redundant amb LVM

## 1. Presentació

Som tècnics de la consultora **EverPia**. Un **bufet d'advocats** ens ha contractat perquè treballa amb dades molt sensibles i el seu sistema d'emmagatzematge actual pot fallar i perdre informació.

Hem dissenyat una solució amb **LVM** a Linux (Zorin OS) que compleix tres objectius:

- **Alta disponibilitat:** les dades continuen accessibles encara que falli un disc.
- **Redundància:** les dades es guarden duplicades i es poden recuperar si s'esborren.
- **Escalabilitat:** es pot ampliar l'espai quan faci falta, sense perdre dades.

Aquesta guia explica pas a pas com ho hem fet, perquè qualsevol persona ho pugui repetir.

---

## 2. Conceptes bàsics de LVM

**LVM** (Logical Volume Manager) és una eina de Linux que permet gestionar els discos de forma flexible. En lloc de treballar directament amb particions fixes, LVM ajunta els discos i després en fa "trossos" a mida.

LVM té tres nivells:

- **PV (Physical Volume):** és un disc físic (o una partició) preparat per fer-lo servir amb LVM. Per exemple, `/dev/sdb`.
- **VG (Volume Group):** és un "magatzem" que ajunta l'espai de diversos PV. Per exemple, `vg1` ajunta `sdb` i `sdc`.
- **LV (Logical Volume):** és un tros d'espai que agafem del VG i que fem servir com si fos un disc. Hi posem format i el muntem en una carpeta.

Resumint: **discos (PV) → magatzem (VG) → volums (LV)**.

---

## 3. Preparació

Fem servir una màquina virtual amb **Zorin OS** a **VirtualBox**. Abans d'engegar-la, hi afegim **2 discos de 10 GB**. Dins de Linux es diran `sdb` i `sdc`.

Instal·lem el paquet `lvm2`, que conté les comandes de LVM, i mirem els discos que té la màquina:

```bash
sudo apt install lvm2 -y
lsblk
```

![Instal·lació de lvm2 i discos amb lsblk](img/01_lsblk_inicial.png)

---

## 4. Configuració inicial

### 4.1. Crear els volums físics (PV)

Preparem els dos discos perquè LVM els pugui fer servir i comprovem que s'han creat:

```bash
sudo pvcreate /dev/sdb
sudo pvcreate /dev/sdc
sudo pvs
```

![Volums físics creats amb pvs](img/02_pvs.png)

### 4.2. Crear el grup de volums (VG)

Ajuntem els dos discos en un grup que es diu `vg1` i en comprovem l'espai:

```bash
sudo vgcreate vg1 /dev/sdb /dev/sdc
sudo vgs
```

![Grup de volums vg1 amb vgs](img/03_vgs.png)

### 4.3. Crear el volum lògic (LV)

Creem un volum lògic de 4 GB que es diu `lv1` i li donem format `ext4` perquè s'hi puguin guardar fitxers:

```bash
sudo lvcreate -L 4G -n lv1 vg1
sudo mkfs.ext4 /dev/vg1/lv1
```

Creem la carpeta on el muntarem i comprovem que el volum existeix:

```bash
sudo mkdir -p /mnt/lv1
sudo lvs
```

![Volum lògic lv1 amb lvs](img/04_lvs.png)

---

## 5. Muntatge automàtic amb /etc/fstab

Volem que el volum es munti sol cada vegada que s'engega l'ordinador. Per això l'afegim al fitxer `/etc/fstab`.

Obrim el fitxer amb l'editor `nano`:

```bash
sudo nano /etc/fstab
```

Al final del fitxer afegim aquesta línia i el desem (`Ctrl+O`, `Enter`, `Ctrl+X`):

```text
/dev/vg1/lv1 /mnt/lv1 ext4 defaults 0 0
```

![Línia afegida a /etc/fstab](img/05_fstab.png)

Muntem tot el que hi ha a `fstab` i comprovem que `lv1` surt muntat a `/mnt/lv1`:

```bash
sudo mount -av
df -h
```

![Muntatge amb mount -av i df -h](img/06_mount_df.png)

Finalment **reiniciem la màquina** i tornem a fer `df -h` per comprovar que el volum s'ha muntat sol.

![df -h després del reinici](img/07_df_reinici.png)

---

## 6. Alta disponibilitat: volum en mirall

Un **mirall** (RAID 1) guarda les mateixes dades en dos discos alhora. Si un disc es trenca, les dades continuen a l'altre.

Creem un volum de 4 GB en mirall (`-m 1` vol dir "una còpia extra") i mirem a quins discos està:

```bash
sudo lvcreate -L 4G -m 1 -n lvm_mirror vg1
sudo lvs -a -o name,copy_percent,devices vg1
```

Aquí es veu que el volum `lvm_mirror` fa servir **`sdb` i `sdc` alhora**. Si fem `lsblk`, també es veu: cada disc té una part que es diu `rimage_0` i `rimage_1`, que són les dues còpies del mirall.

![Volum en mirall amb lvs -a](img/08_mirall_lvs.png)

Li donem format, creem la carpeta `/mnt/mirror` i l'hi muntem:

```bash
sudo mkfs.ext4 /dev/vg1/lvm_mirror
sudo mkdir -p /mnt/mirror
sudo mount /dev/vg1/lvm_mirror /mnt/mirror
```

I hi guardem un fitxer de prova:

```bash
echo "dades del bufet" | sudo tee /mnt/mirror/prova.txt
```

![Fitxer de prova al mirall](img/09_mirall_prova.png)

---

## 7. Instantànies (snapshots)

Un **snapshot** és com una **"foto"** d'un volum en un moment concret. Si després s'esborren o es fan malbé les dades, podem tornar a l'estat de la foto.

### 7.1. Afegir dos discos nous

Amb la **màquina apagada**, afegim a VirtualBox **2 discos més de 10 GB**. Dins de Linux es diran `sdd` i `sde`.

Els preparem per a LVM, els afegim al grup `vg1` i comprovem que el grup ara és més gran:

```bash
sudo pvcreate /dev/sdd /dev/sde
sudo vgextend vg1 /dev/sdd /dev/sde
sudo vgs
```

![vg1 ampliat amb sdd i sde](img/10_vgextend.png)

### 7.2. Crear el volum de dades

Creem un volum de 5 GB que es diu `lv_dades`, **només al disc `sdd`**, i li donem format:

```bash
sudo lvcreate -L 5G -n lv_dades vg1 /dev/sdd
sudo mkfs.ext4 /dev/vg1/lv_dades
```

El muntem a la carpeta `/mnt/lv_dades`:

```bash
sudo mkdir -p /mnt/lv_dades
sudo mount /dev/vg1/lv_dades /mnt/lv_dades
```

![Volum lv_dades creat i muntat](img/11_lv_dades.png)

### 7.3. Afegir fitxers de prova

Descarreguem tres imatges d'internet al volum perquè faci de "dades del bufet":

```bash
sudo wget -O /mnt/lv_dades/imagen1.jpg https://picsum.photos/800
sudo wget -O /mnt/lv_dades/imagen2.jpg https://picsum.photos/800
sudo wget -O /mnt/lv_dades/imagen3.jpg https://picsum.photos/800
```

Comprovem que les imatges hi són:

```bash
ls -l /mnt/lv_dades
```

![Imatges a lv_dades](img/12_imatges.png)

### 7.4. Crear el snapshot

Fem la "foto" de `lv_dades`. El snapshot ocupa 2 GB i el guardem al disc `sde`, que és un disc diferent del de les dades:

```bash
sudo lvcreate -L 2G -s -n lv_snapshot /dev/vg1/lv_dades /dev/sde
sudo lvs
```

![Snapshot lv_snapshot creat](img/13_snapshot.png)

---

## 8. Simular un desastre i restaurar

### 8.1. Esborrar les dades

Simulem que algú esborra per error tots els fitxers del volum i mirem què queda:

```bash
sudo rm /mnt/lv_dades/*
ls -l /mnt/lv_dades
```

Les imatges han desaparegut. Només queda la carpeta **`lost+found`**: és normal, la crea `ext4` automàticament i `rm` sense `-r` no esborra carpetes.

![Dades esborrades](img/14_desastre.png)

### 8.2. Restaurar des del snapshot

Desmuntem el volum i fusionem el snapshot amb el volum original. Així el volum torna a l'estat de la "foto":

```bash
sudo umount /mnt/lv_dades
sudo lvconvert --merge /dev/vg1/lv_snapshot
```

El tornem a muntar i comprovem que les imatges tornen a ser-hi:

```bash
sudo mount /dev/vg1/lv_dades /mnt/lv_dades
ls -l /mnt/lv_dades
```

![Imatges recuperades](img/15_restaurat.png)

Després de la fusió, el **snapshot desapareix**: s'ha "gastat" per restaurar les dades. Si en volem un altre, l'hem de tornar a crear.

---

## 9. Escalabilitat: ampliar el volum

LVM permet fer més gran un volum quan es queda petit, sense perdre les dades.

Primer mirem quant ocupa ara `lv_dades` i quant espai lliure té el grup (columna **VFree**):

```bash
df -h /mnt/lv_dades
sudo vgs
```

![Mida abans d'ampliar i espai lliure del VG](img/16_abans_ampliar.png)

Desmuntem el volum i li donem tot l'espai lliure que queda al grup:

```bash
sudo umount /mnt/lv_dades
sudo lvextend -l +100%FREE /dev/vg1/lv_dades
```

Revisem el sistema de fitxers i l'ampliem perquè ocupi tot el volum nou:

```bash
sudo e2fsck -f /dev/vg1/lv_dades
sudo resize2fs /dev/vg1/lv_dades
```

![lvextend, e2fsck i resize2fs](img/17_ampliar.png)

El tornem a muntar i comprovem que ara és més gran i que les imatges continuen allà:

```bash
sudo mount /dev/vg1/lv_dades /mnt/lv_dades
df -h /mnt/lv_dades
ls -l /mnt/lv_dades
```

![Mida després d'ampliar i dades intactes](img/18_despres_ampliar.png)

---

## 10. Problemes trobats i solucions

| Problema | Causa | Solució |
|----------|-------|---------|
| En muntar `lv_dades` sortia l'error **"superbloque incorrecto"**. | El volum encara no tenia format. | Donar-li format amb `sudo mkfs.ext4 /dev/vg1/lv_dades` abans de muntar-lo. |
| `cp ~/Downloads/*` donava error. | No hi havia cap imatge a la carpeta Downloads. | Descarregar les imatges directament amb `wget`. |
| `wget` donava l'error **"opción incorrecta"**. | Havia escrit un **zero** (`-0`) en lloc de la **lletra O** (`-O`). | Escriure `-O` amb la lletra O majúscula. |

---

## 11. Conclusió per al client

Amb aquesta solució, el bufet d'advocats té les seves dades molt més protegides:

- **El mirall** protegeix davant la **fallada d'un disc**: les dades estan duplicades en dos discos i, si un es trenca, l'altre continua funcionant.
- **Els snapshots** permeten **recuperar dades esborrades o danyades**, tornant a l'estat d'una "foto" anterior.
- **LVM** permet **ampliar l'espai** afegint discos nous, **sense perdre dades** i sense haver de reinstal·lar res.

**Recomanació per al bufet:** guardar els documents dels clients en un volum en **mirall**, fer **snapshots periòdics** (per exemple, cada dia abans de tancar) en un disc diferent, i deixar espai lliure al grup de volums per poder créixer quan calgui. Tot i això, LVM no substitueix les **còpies de seguretat**: cal tenir també còpies fora del servidor.
