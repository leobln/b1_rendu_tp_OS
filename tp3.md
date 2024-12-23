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
leobln@testtoto:/etc$ su - root
Password:
root@testtoto:~# usermod -a -G sudo leobln
```

### B. Practice

🌞 **Créer un groupe d'utilisateurs**

- il devra s'appeler `stronk_admins`

```
leobln@testtoto:~$ sudo groupadd stronk_admins
```

🌞 **Créer un utilisateur**

- il devra s'appeler `imbob`
- il devra avoir un mot de passe défini
- il devra appartenir aux groupes `imbob` et `stronk_admins`

```
leobln@testtoto:~$ sudo useradd imbob
leobln@testtoto:~$ sudo passwd imbob
New password:
Retype new password:
passwd: password updated successfully
leobln@testtoto:~$ sudo usermod -aG stronk_admins imbob
```

🌞 **Prouver que l'utilisateur `imbob` est créé**

- en affichant une seule ligne du fichier `/etc/passwd`

```
leobln@testtoto:~$ cat /etc/passwd | grep imbob
imbob:x:1002:1003::/home/imbob:/bin/sh
```

🌞 **Prouver que l'utilisateur `imbob` a un password défini**

- en affichant une seule ligne du fichier `/etc/shadow`

```
leobln@testtoto:~$ sudo cat /etc/shadow | grep imbob
imbob:$y$j9T$bggqsGoPhaAyt0h34Ru3D.$CnOHzo3Pq5zu1iKys2pZsUCqzu5jEdwHF4.GuYF/EX/:20040:0:99999:7:::
```

🌞 **Prouver que l'utilisateur `imbob` appartient au groupe `stronk_admins`**

- la liste des groupes et de leurs membres c'est dans `/etc/group`
- affichez une seule ligne

```
leobln@testtoto:~$ sudo cat /etc/group | grep stronk_admins
stronk_admins:x:1002:imbob
```

🌞 **Créer un deuxième utilisateur**

- il devra s'appeler `imnotbobsorry`
- il devra avoir un mot de passe défini
- il devra appartenir au groupe `imnotbobsorry` ET `stronk_admins`

```
leobln@testtoto:~$ sudo useradd imnotbobsorry
leobln@testtoto:~$ sudo passwd imnotbobsorry
New password:
Retype new password:
passwd: password updated successfully
leobln@testtoto:~$ sudo usermod -aG stronk_admins imnotbobsorry
```

🌞 **Modifier la configuration de `sudo` pour que**

- les membres du groupes `stronk_admins` ait le droit de taper des commandes `apt` en tant que `root`
- l'utilisateur `imbob` peut taper n'importe quelle commande en tant que `root`

```
leobln@testtoto:~$ sudo visudo
%stronk_admins ALL=(ALL) NOPASSWD: /usr/bin/apt, /usr/bin/apt-get
```

🌞 **Créer le dossier `/home/goodguy`** (avec une commande)

```
leobln@testtoto:~$ sudo mkdir /home/goodguy
```

🌞 **Changer le répertoire personnel de `imbob`**

- avec une commande `usermod`, définissez ce dossier comme le *répertoire personnel* de `imbob`
- prouvez que le changement est effectif en affichant le contenu du fichier `passwd`

```
leobln@testtoto:~$ sudo usermod -d /home/goodguy -m imbob
[sudo] password for leobln:
usermod: directory /home/goodguy exists
leobln@testtoto:~$ grep imbob /etc/passwd
imbob:x:1002:1003::/home/goodguy:/bin/sh
leobln@testtoto:~$ su - imbob
Password:
$ pwd
/home/goodguy
```

🌞 **Créer le dossier `/home/badguy`**

🌞 **Changer le répertoire personnel de `imnotbobsorry`**

- avec une commande `usermod`, définissez ce dossier `/home/badguy` comme le *répertoire personnel* de `imnotbobsorry`
- prouvez que le changement est effectif en affichant le contenu du fichier `passwd`

```
leobln@testtoto:~$ sudo mkdir /home/badguy
leobln@testtoto:~$ sudo usermod -d /home/badguy -m imnotbobsorry
usermod: directory /home/badguy exists
leobln@testtoto:~$  grep imnotbobsorry /etc/passwd
imnotbobsorry:x:1003:1004::/home/badguy:/bin/sh
leobln@testtoto:~$ su - imnotbobsorry
Password:
$ pwd
/home/badguy
```

🌞 **Prouver que les permissions du dossier `/home/gooduy` sont incohérentes**

- ça n'appartient pas à l'utilisateur `imbob`
- ce qui est chelou, l'utilisateur il peut se connecter, mais il peut pas créer quoique ce soit dans son propre *répertoire personnel*, genre dans son propre dossier "Mes Documents"

```
leobln@testtoto:~$ ls -ld /home/goodguy
drwxr-xr-x 2 root root 4096 Nov 15 16:18 /home/goodguy
leobln@testtoto:~$ su - imbob
Password:
$ mkdir toto
mkdir: cannot create directory ‘toto’: Permission denied
``` 

🌞 **Modifier les permissions de `/home/goodguy`**

- le dossier doit appartenir à `imbob`
- pareil pour tout son contenu
- avec une commande `chown` (il faudra mettre options et arguments)

```
leobln@testtoto:~$ sudo chown imbob:imbob /home/goodguy
leobln@testtoto:~$ ls -ld /home/goodguy/
drwxr-xr-x 2 imbob imbob 4096 Nov 15 16:18 /home/goodguy/
leobln@testtoto:~$ su - imbob
Password:
$ mkdir toto
(pas de message d ereure)
$ 
```

🌞 **Modifier les permissions de `/home/badguy`**

- le dossier doit appartenir à `imnotbobsorry`
- pareil pour tout son contenu

```
leobln@testtoto:~$ sudo chown imnotbobsorry:imnotbobsorry /home/badguy
leobln@testtoto:~$ ls -ld /home/badguy/
drwxr-xr-x 2 imnotbobsorry imnotbobsorry 4096 Nov 15 16:33 /home/badguy/
leobln@testtoto:~$ su - imnotbobsorry
Password:
$ mkdir toto2
(pas de message d ereure)
$
```

🌞 **Connectez-vous sur l'utilisateur `imbob`**

- il faut utiliser la commande `su - <USER>` pour ouvrir une nouvelle session en tant qu'un utilisateur
  - ça doit sortir aucun message d'erreur particulier
- si tu fais `pwd` tu devrais être dans le dossier `/home/goodguy` tout de suite après connexion (le *répertoire personnel* de `imbob` !)
- si tu fais `sudo echo meow` ou n'importe quelle autre commande avec `sudo`, ça devrait fonctionner

```
leobln@testtoto:~$ su - imbob
Password:
$ pwd
/home/goodguy
$ sudo echo meow
meow
```

🌞 **Connectez-vous sur l'utilisateur `imnotbobsorry`**

- il faut utiliser la commande `su - <USER>` pour ouvrir une nouvelle session en tant qu'un utilisateur
  - ça doit sortir aucun message d'erreur particulier
- si tu fais `pwd` tu devrais être dans le dossier `/home/badguy` tout de suite après 
- si tu fais `sudo echo meow` ou n'importe quelle autre commande avec `sudo`, ça ne devrait fonctionner PAS fonctionner
  - sauf les commandes `sudo apt...`, essaie un `sudo apt update` pour voir ?
  
```
leobln@testtoto:~$ su - imnotbobsorry
Password:
$ pwd
/home/badguy
$ sudo echo meow
[sudo] password for imnotbobsorry:
Sorry, user imnotbobsorry is not allowed to execute '/usr/bin/echo meow' as root on testtoto.toto.
$ sudo apt update
Get:1 http://security.debian.org/debian-security bookworm-security InRelease [48.0 kB]
Hit:2 http://deb.debian.org/debian bookworm InRelease
Get:3 http://deb.debian.org/debian bookworm-updates InRelease [55.4 kB]
Get:4 http://security.debian.org/debian-security bookworm-security/main Sources [124 kB]
Fetched 228 kB in 2s (107 kB/s)
Reading package lists... Done
Building dependency tree... Done
Reading state information... Done
66 packages can be upgraded. Run 'apt list --upgradable' to see them.
```

# II. Processes


🌞 **Affichez les processus `bash`**

- une commande `ps` puis vous filtrez la sortie pour afficher que les `bash`

```
leobln@testtoto:~$ ps -aux | grep bash
leobln      4930  0.0  0.2   9792  5752 pts/1    Ss   16:27   0:00 -bash
leobln      5430  0.0  0.1   6332  2096 pts/1    S+   17:07   0:00 grep bash
```

🌞 **Affichez tous les processus lancés par votre utilisateur**

- uniquement ceux qui sont lancés par votre utilisateur, pas ceux lancés par `root` ou autres

```
leobln@testtoto:~$ ps aux | grep leobln
```

🌞 **Affichez le top 5 des processus qui utilisent le plus de RAM**

- je sais pas si j'ai besoin de préciser pourquoi c'est utile de savoir ça
- si t'as plus de *RAM*, t'aimes bien savoir qui te la mange !
- uniquement 5 lignes doivent s'afficher et elles ne contiennent QUE le nom du processus et la *RAM* utilisée

```
leobln@testtoto:~$ ps -eo %mem,comm --sort=-%mem | head -n 6
%MEM COMMAND
 5.4 lightdm-gtk-gre
 5.4 Xorg
 4.9 xfwm4
 3.7 Xorg
 2.9 xfdesktop
```

🌞 **Affichez le *PID* du processus du service SSH**

- le nom du *programme* c'est `sshd`
- vous ne devez afficher qu'une seule ligne car un seul *programme* est lancé quand vous démarrez le service SSH
- toutes les autres lignes qui s'affichent sont les *processus* lancés pour gérer vos sessions SSH actuellement en cours
- il existe donc un seul *processus* SSH en cours d'exécution qui est le *parent* de tous les autres *processus* SSH (qui sont ses *enfants*)

```
leobln@testtoto:~$ ps -C sshd -o pid= | head -n 1
    616
```

🌞 **Affichez le nom du processus avec l'identifiant le plus petit**

- votre *commande* doit afficher le processus qui a le plus petit identifiant sans le connaître à l'avance
  - l'identifiant d'un processus c'est son *PID* (*Process IDentifier*)
  - sans connaître ni son nom ni son *PID* à l'avance, proposer une *commande* qui n'affiche que lui
  - si on trie la liste par *PID*, suffit d'afficher que la première ligne, non ?...
- **une seule ligne doit être affichée**
- la *commande* doit tout le temps fonctionner quoi !

```
leobln@testtoto:~$ ps -e --sort=pid -o comm= | head -n 1
systemd
```

## 2. Parent, enfant, et meurtre

🌞 **Déterminer le *PID* de votre shell actuel**

- quand on ouvre un terminal sous Linux, généralement, le shell c'est `bash`
- donc déterminez le *PID* du *processus* `bash` dans lequel vous tapez des *commandes*
- n'affichez qu'une seule ligne

```
leobln@testtoto:~$ echo $$
4930
```

🌞 **Déterminer le *PPID* de votre shell actuel**

- le *PPID* c'est pour *Parent PID* : l'identifiant du *processus* parent
- avec une *commande* `ps` et des *options* usuelles, l'info va sortir
- n'affichez qu'une seule ligne

```
leobln@testtoto:~$ ps -o ppid= -p $$
   4926
```

🌞 **Déterminer le nom de ce *processus***

- donc, votre `bash` est l'enfant d'un *processus* : lequel ?
- vous venez de repérer son PID juste avant, facile de repérer son nom maintenant
- n'affichez qu'une seule ligne

```
leobln@testtoto:~$ ps -o comm= -p $(ps -o ppid= -p $$)
sshd
```

🌞 **Lancer un *processus* `sleep 9999` en tâche de fond**

- avec le caractère `&` comme au TP précédent
- déterminer avec une *commande* `ps` son PID et son PPID
  - vous n'afficherez qu'une seule ligne
- vous devriez constater que son PPID, c'est votre `bash`

```
leobln@testtoto:~$ sleep 9999 &
[1] 5528
leobln@testtoto:~$ ps -o pid,ppid -p $!
    PID    PPID
   5528    4930
```

🌞 **Fermez votre session SSH**

- genre complètement déconnectez-vous de vos sessions SSH
- puis reconnecte-toi avec une nouvelle connexion SSH
- est-ce que le *processus* `sleep` lancé en tâche de fond s'exécute toujours ?
- prouver que oui ou non en une seule *commande*

```
leobln@testtoto:~$ ps aux | grep 'sleep 9999'
leobln      5528  0.0  0.0   5464   872 ?        S    17:53   0:00 sleep 9999
``` 
# III. Services



🌞 **S'assurer que le service `ssh` est démarré**

```
leobln@testtoto:~$ systemctl status
● testtoto
    State: running
    Units: 272 loaded (incl. loaded aliases)
     Jobs: 0 queued
   Failed: 0 units
    Since: Wed 2024-11-20 08:20:56 CET; 1h 26min ago
  systemd: 252.31-1~deb12u1
   CGroup: /
           ├─init.scope
           │ └─1 /lib/systemd/systemd --system --deserialize=16
           ├─system.slice
           │ ├─ModemManager.service
           │ │ └─444 /usr/sbin/ModemManager
           │ ├─NetworkManager.service
           │ │ └─430 /usr/sbin/NetworkManager --no-daemon
           │ ├─avahi-daemon.service
           │ │ ├─388 "avahi-daemon: running [testtoto.local]"
           │ │ └─412 "avahi-daemon: chroot helper"
           │ ├─colord.service
           │ │ └─941 /usr/libexec/colord
           │ ├─cron.service
           │ │ └─391 /usr/sbin/cron -f
           │ ├─cups-browsed.service
           │ │ └─613 /usr/sbin/cups-browsed
           │ ├─cups.service
           │ │ └─599 /usr/sbin/cupsd -l
           │ ├─dbus.service
           │ │ └─392 /usr/bin/dbus-daemon --system --address=systemd: --nof>
           │ ├─lightdm.service
           │ │ ├─ 602 /usr/sbin/lightdm
           │ │ ├─ 614 /usr/lib/xorg/Xorg :0 -seat seat0 -auth /var/run/ligh>
           │ │ ├─1160 /usr/lib/xorg/Xorg :1 -seat seat0 -auth /var/run/ligh>
           │ │ └─1291 lightdm --session-child 17 29
           │ ├─networking.service
           │ │ └─520 dhclient -4 -v -i -pf /run/dhclient.enp0s3.pid -lf /va>
           │ ├─polkit.service
           │ │ └─403 /usr/lib/polkit-1/polkitd --no-debug
           │ ├─rtkit-daemon.service
           │ │ └─651 /usr/libexec/rtkit-daemon
           │ ├─ssh.service
lines 1-40

```

🌞 **Isolez la ligne qui indique le nom du programme lancé**

```
leobln@testtoto:~$ systemctl status | grep "Main PID"
             │ │ └─31939 grep "Main PID"
```

🌞 **Déterminer le port sur lequel écoute le service SSH**

```
leobln@testtoto:~$ sudo ss -lnpt | grep sshd
LISTEN 0      128          0.0.0.0:22        0.0.0.0:*    users:(("sshd",pid=609,fd=3))
LISTEN 0      128             [::]:22           [::]:*    users:(("sshd",pid=609,fd=4))
```

🌞 **Consulter les logs du service SSH**

```
leobln@testtoto:~$ sudo journalctl -u ssh.service
```

## 3. Modification du service

🌞 **Identifier le fichier de configuration du serveur SSH**

```
leobln@testtoto:~/work$ ls -l /etc/ssh/sshd_config
-rw-r--r-- 1 root root 3223 Jun 22 21:38 /etc/ssh/sshd_config
```

🌞 **Modifier le fichier de conf**

```
leobln@testtoto:~/work$ echo $RANDOM
20340
leobln@testtoto:~/work$ cat /etc/ssh/sshd_config | grep "Port"
#Port 20340
```

🌞 **Redémarrer le service**

```
leobln@testtoto:~/work$ sudo systemctl restart ssh
```

🌞 **Effectuer une connexion SSH sur le nouveau port**

```
leobln@testtoto:~/work$ ssh -p 20340 leobln@192.168.74.3
leobln@192.168.74.3's password:
Linux testtoto 6.1.0-26-amd64 #1 SMP PREEMPT_DYNAMIC Debian 6.1.112-1 (2024-09-30) x86_64

The programs included with the Debian GNU/Linux system are free software;
the exact distribution terms for each program are described in the
individual files in /usr/share/doc/*/copyright.

Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
permitted by applicable law.
Last login: Wed Nov 20 11:22:44 2024 from 192.168.74.3
```

🌞 **Trouver le fichier `ssh.service`**

```
leobln@testtoto:~$ sudo find /etc/systemd /lib/systemd -name "ssh.service"
/etc/systemd/system/multi-user.target.wants/ssh.service
/lib/systemd/system/ssh.service
leobln@testtoto:~$ cat /lib/systemd/system/ssh.service
[Unit]
Description=OpenBSD Secure Shell server
Documentation=man:sshd(8) man:sshd_config(5)
After=network.target nss-user-lookup.target auditd.service
ConditionPathExists=!/etc/ssh/sshd_not_to_be_run

[Service]
EnvironmentFile=-/etc/default/ssh
ExecStartPre=/usr/sbin/sshd -t
ExecStart=/usr/sbin/sshd -D $SSHD_OPTS
ExecReload=/usr/sbin/sshd -t
ExecReload=/bin/kill -HUP $MAINPID
KillMode=process
Restart=on-failure
RestartPreventExitStatus=255
Type=notify
RuntimeDirectory=sshd
RuntimeDirectoryMode=0755

[Install]
WantedBy=multi-user.target
Alias=sshd.service
```

🌞 **Déterminer quel est le programme lancé**

```
leobln@testtoto:~$ grep "ExecStart=" /lib/systemd/system/ssh.service
ExecStart=/usr/sbin/sshd -D $SSHD_OPTS
```

## 4. Créez votre propre service

🌞 **Déterminer le dossier qui contient la commande `python3`**

```
leobln@testtoto:~$ which python3
/usr/bin/python3
```

🌞 **Créez un fichier `/etc/systemd/system/meow_web.service`**

```
leobln@testtoto:~$ sudo nano /etc/systemd/system/meow_web.service
```

🌞 **Indiquez à l'OS que vous avez modifié les *services***

```
leobln@testtoto:~$ sudo systemctl daemon-reload

```

🌞 **Démarrez votre service**

```
leobln@testtoto:~$ sudo systemctl start meow_web.service
```

🌞 **Assurez-vous que le service `meow_web` est actif**

```
leobln@testtoto:~$ sudo systemctl status meow_web.service
```

🌞 **Déterminer le PID du *processus* Python en cours d'exécution**

```
leobln@testtoto:~$ ps -eo pid,user,command | grep python
   1047 leobln   /usr/bin/python3 /usr/share/system-config-printer/applet.py
   1356 leobln   python3 -m http.server 8888
```

🌞 **Prouvez que le *programme* écoute derrière le port 8888**

```
leobln@testtoto:~$ sudo ss -lnpt | grep ':8888' | grep python
LISTEN 0      5            0.0.0.0:8888      0.0.0.0:*    users:(("python3",pid=1356,fd=3))
```

🌞 **Faire en sote que le *service* se lance automatiquement au démarrage de la machine**

```
leobln@testtoto:~$ sudo systemctl enable meow_web.service
Created symlink '/etc/systemd/system/multi-user.target.wants/meow_web.service' → '/etc/systemd/system/meow_web.service'.
```



