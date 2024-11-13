# I. Utilisateurs

## 1. Liste des users

🌞 **Afficher la ligne du fichier qui concerne votre utilisateur**

- mettez uniquement cette ligne en évidence
- pas `root`, l'autre, que vous avez créé à l'installation
- faites déjà un `cat` du fichier pour voir à quoi ressemble son contenu

```
leobln@testtoto:~$ cat /etc/passwd | grep leobln
leobln:x:1000:1000:leobellina,,,:/home/leobln:/bin/bash
```

🌞 **Afficher la ligne du fichier qui concerne votre utilisateur ET celle de `root` en même temps**

- mettez uniquement ces deux lignes en évidence
- uniquement ces 2 lignes doivent sortir

```
leobln@testtoto:~$ cat /etc/passwd | grep -e leobln -e root
root:x:0:0:root:/root:/bin/bash
leobln:x:1000:1000:leobellina,,,:/home/leobln:/bin/bash
```

🌞 **Afficher la liste des groupes d'utilisateurs de la *machine***

- ptite recherche par vous-mêmes pour trouver quel fichier c'est !
- le même sous tous les OS Linux :)

```
leobln@testtoto:~$ sudo cat /etc/group
[sudo] password for leobln:
root:x:0:
daemon:x:1:
bin:x:2:
sys:x:3:
adm:x:4:
tty:x:5:
disk:x:6:
lp:x:7:
mail:x:8:
news:x:9:
uucp:x:10:
man:x:12:
proxy:x:13:
kmem:x:15:
dialout:x:20:
fax:x:21:
voice:x:22:
cdrom:x:24:leobln
floppy:x:25:leobln
tape:x:26:
sudo:x:27:leobln
audio:x:29:pulse,leobln
dip:x:30:leobln
www-data:x:33:
backup:x:34:
operator:x:37:
list:x:38:
irc:x:39:
src:x:40:
shadow:x:42:
utmp:x:43:
video:x:44:leobln
sasl:x:45:
plugdev:x:46:leobln
staff:x:50:
games:x:60:
users:x:100:leobln
nogroup:x:65534:
systemd-journal:x:999:
systemd-network:x:998:
crontab:x:101:
input:x:102:
sgx:x:103:
kvm:x:104:
render:x:105:
netdev:x:106:leobln
systemd-timesync:x:997:
messagebus:x:107:
_ssh:x:108:
ssl-cert:x:109:
avahi-autoipd:x:110:
bluetooth:x:111:leobln
avahi:x:112:
lpadmin:x:113:leobln
pulse:x:114:
pulse-access:x:115:
scanner:x:116:saned,leobln
saned:x:117:
lightdm:x:118:
polkitd:x:996:
rtkit:x:119:
colord:x:120:
leobln:x:1000:
marmotte:x:1001:
```

🌞 **Afficher la ligne du fichier qui concerne votre utilisateur ET celle de `root` en même temps**

- afficher uniquement le nom d'utilisateur et le chemin vers leur répertoire personnel (celui de votre utilisateur est dans `/home`, celui de `root` c'est `/root`)
- on peut demander à `cut` d'afficher plusieurs colonnes avec `-fx,y` où `x` et `y` sont les deux numéros de colonnes qu'on veut afficher
- mettez uniquement ces deux lignes en évidence

```
leobln@testtoto:~$ sudo cat /etc/group | grep -e leobln -e root
root:x:0:
cdrom:x:24:leobln
floppy:x:25:leobln
sudo:x:27:leobln
audio:x:29:pulse,leobln
dip:x:30:leobln
video:x:44:leobln
plugdev:x:46:leobln
users:x:100:leobln
netdev:x:106:leobln
bluetooth:x:111:leobln
lpadmin:x:113:leobln
scanner:x:116:saned,leobln
leobln:x:1000:
```

## 2. Hash des passwords

Le *hash* des mots de passe des utilisateurs est stocké dans un fichier aussi : le fichier `/etc/shadow`.

🌞 **Afficher la ligne qui contient le hash du mot de passe de votre utilisateur**

- mettez uniquement cette ligne en évidence

```
leobln@testtoto:~$ sudo cat /etc/shadow | grep leobln
leobln:$y$j9T$ksi4HE87G0hCkAvpAVgTG.$56pTU0SvRcTjx8vWE772jNFIg098COelw89ecEvBxoA:20033:0:99999:7:::
```

## 3. Sudo

🌞 **Faites en sorte que votre utilisateur puisse taper n'importe quelle commande `sudo`**

- par défaut sur Debian il existe un groupe d'utilisateurs nommé `sudo`
- tous les membres du groupe `sudo` ont par défaut le droit de taper n'importe quelle comannde `sudo`

- **ajoutez donc votre utilisateur au groupe `sudo` qui existe déjà** afin de pouvopir utiliser `sudo` pleinement
- utilisez une commande `usermod` pour ajouter votre utilisateur au groupe `sudo`

```
leobln@testtoto:~$ sudo usermod leobln
[sudo] password for leobln:
leobln is not in the sudoers file.
leobln@testtoto:~$ groups leobln
leobln : leobln cdrom floppy audio dip video plugdev users netdev bluetooth lpadmin scanner
```

### B. Practice

🌞 **Créer un groupe d'utilisateurs**

- il devra s'appeler `stronk_admins`

🌞 **Créer un utilisateur**

- il devra s'appeler `imbob`
- il devra avoir un mot de passe défini
- il devra appartenir aux groupes `imbob` et `stronk_admins`

🌞 **Prouver que l'utilisateur `imbob` est créé**

- en affichant une seule ligne du fichier `/etc/passwd`

🌞 **Prouver que l'utilisateur `imbob` a un password défini**

- en affichant une seule ligne du fichier `/etc/shadow`

🌞 **Prouver que l'utilisateur `imbob` appartient au groupe `stronk_admins`**

- la liste des groupes et de leurs membres c'est dans `/etc/group`
- affichez une seule ligne

🌞 **Créer un deuxième utilisateur**

- il devra s'appeler `imnotbobsorry`
- il devra avoir un mot de passe défini
- il devra appartenir au groupe `imnotbobsorry` ET `stronk_admins`

🌞 **Modifier la configuration de `sudo` pour que**

- les membres du groupes `stronk_admins` ait le droit de taper des commandes `apt` en tant que `root`
- l'utilisateur `imbob` peut taper n'importe quelle commande en tant que `root`

🌞 **Créer le dossier `/home/goodguy`** (avec une commande)

🌞 **Changer le répertoire personnel de `imbob`**

- avec une commande `usermod`, définissez ce dossier comme le *répertoire personnel* de `imbob`
- prouvez que le changement est effectif en affichant le contenu du fichier `passwd`

> "*Répertoire personnel*" ça se dit "*home directory*" en anglais, on dit souvent juste "*homedir*" pour faire court. Sous Windows, pour rappel, les *homedirs* des utilisateurs sont stockés par défaut dans `C:/Users/<USER>`, pour Linux c'est donc `/home/<USER>` par défaut.

🌞 **Créer le dossier `/home/badguy`**

🌞 **Changer le répertoire personnel de `imnotbobsorry`**

- avec une commande `usermod`, définissez ce dossier `/home/badguy` comme le *répertoire personnel* de `imnotbobsorry`
- prouvez que le changement est effectif en affichant le contenu du fichier `passwd`

> Si t'essaies de te connecter en tant que `imbob` là en tapant la commande `su - imbob` il va sûrement se passer des trucs chelous... En tout cas `imbob` ne pourra pas y créer des fichiers. En effet, tu as sûrement du utiliser les droits de `root` pour créer le dossier, donc actuellement, le *répertoire personnel* de `imbob`, il appartient à `root`... Donc `imbob` n'a aucun droit dans son propre *répertoire personnel*, chelou.

🌞 **Prouver que les permissions du dossier `/home/gooduy` sont incohérentes**

- ça n'appartient pas à l'utilisateur `imbob`
- ce qui est chelou, l'utilisateur il peut se connecter, mais il peut pas créer quoique ce soit dans son propre *répertoire personnel*, genre dans son propre dossier "Mes Documents"

🌞 **Modifier les permissions de `/home/goodguy`**

- le dossier doit appartenir à `imbob`
- pareil pour tout son contenu
- avec une commande `chown` (il faudra mettre options et arguments)

🌞 **Modifier les permissions de `/home/badguy`**

- le dossier doit appartenir à `imnotbobsorry`
- pareil pour tout son contenu

🌞 **Connectez-vous sur l'utilisateur `imbob`**

- il faut utiliser la commande `su - <USER>` pour ouvrir une nouvelle session en tant qu'un utilisateur
  - ça doit sortir aucun message d'erreur particulier
- si tu fais `pwd` tu devrais être dans le dossier `/home/goodguy` tout de suite après connexion (le *répertoire personnel* de `imbob` !)
- si tu fais `sudo echo meow` ou n'importe quelle autre commande avec `sudo`, ça devrait fonctionner

🌞 **Connectez-vous sur l'utilisateur `imnotbobsorry`**

- il faut utiliser la commande `su - <USER>` pour ouvrir une nouvelle session en tant qu'un utilisateur
  - ça doit sortir aucun message d'erreur particulier
- si tu fais `pwd` tu devrais être dans le dossier `/home/badguy` tout de suite après 
- si tu fais `sudo echo meow` ou n'importe quelle autre commande avec `sudo`, ça ne devrait fonctionner PAS fonctionner
  - sauf les commandes `sudo apt...`, essaie un `sudo apt update` pour voir ?
