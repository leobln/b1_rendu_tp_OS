

# 1. Fichiers

## A. Find me

🌞 **Trouver le chemin vers le répertoire personnel de votre utilisateur**

- si tu te connectes en tant que l'utilisateur `toto`, il y a un dossier qui est réservé à `toto`
- sur un OS pour un PC de bureau, c'est dans ce dossier qu'on trouve `Mes Documents/`, `Mes Images/` etc.
- la liste des utilisateurs et de leurs infos est dans le fichier `/etc/passwd`

```
leobln@testtoto:~$ echo $HOME  
/home/leobln
```
🌞 **Vérifier les permissions du répertoire personnel de votre utilisateurs**

- vérifier que le propriétaire c'est bien votre utilisateur
- on peut voir le pes permission d'un fichier ou dossier en faisant `ls -l <FICHIER>`

```
leobln@testtoto:~$ ls -l /home/leobln/
total 36
drwxr-xr-x 2 leobln leobln 4096 Nov  6 11:51 Desktop
drwxr-xr-x 2 leobln leobln 4096 Nov  6 11:51 Documents
drwxr-xr-x 2 leobln leobln 4096 Nov  6 11:51 Downloads
drwxr-xr-x 5 leobln leobln 4096 Nov  7 12:47 gameshell
drwxr-xr-x 2 leobln leobln 4096 Nov  6 11:51 Music
drwxr-xr-x 2 leobln leobln 4096 Nov  6 11:51 Pictures
drwxr-xr-x 2 leobln leobln 4096 Nov  6 11:51 Public
drwxr-xr-x 2 leobln leobln 4096 Nov  6 11:51 Templates
drwxr-xr-x 2 leobln leobln 4096 Nov  6 11:51 Videos
```
🌞 **Trouver le chemin du fichier de configuration du serveur SSH**

- idem ici, sous Linux, il existe un dossier qui est dédié à stocker tous les fichiers de configuration
- avec Gameshell, vous avez un peu appris à utiliser la commande `find`, elle peut faire le taff ici vu que je vous donne le nom du fichier !
  - le fichier s'appelle `sshd_config`
  - sous Linux (comme sur MacOS) le dossier racine c'est `/` (l'équivalent de `C:/` sous Windows)

```
leobln@testtoto:~$ sudo find / -name "sshd_config"  
[sudo] password for leobln:  
/etc/ssh/sshd_config  
/usr/share/openssh/sshd_config 
``` 

# 2. Users

## A. Nouveau user

🌞 **Créer un nouvel utilisateur**

- il doit s'appeler `marmotte`
- son password doit être `chocolat`
- son répertoire personnel doit être le dossier `/home/papier_alu/`
- il faut utiliser :
  - la commande `useradd` pour créer un utilisateur avec des infos particulières
  - il faut ajouter une option pour définir le répertoire personnel
  - éventuellement utiliser la commande `passwd` après création pour définir un password
  - éventuellement utiliser la commande `usermod` pour modifier les infos du user après création
  - éventuellement utiliser la commande `userdel` pour supprimer l'utilisateur et repartir de zéro :)

```
leobln@testtoto:~$ sudo useradd -m marmotte -d /home/papier_alu  
leobln@testtoto:~$ sudo passwd marmotte  
New password:  
Retype new password:  
passwd: password updated successfully  
```

## B. Infos enregistrées par le système

🌞 **Prouver que cet utilisateur a été créé**

- en affichant le contenu d'un fichier
- sous Linux, il existe un fichier qui contient la liste des utilisateurs ainsi que des infos sur eux (comme le chemin vers le répertoire personnel)
- utilisez une syntaxe `cat fichier | grep marmotte` pour n'afficher que la ligne qui concerne notre utilisateur `marmotte`

```
leobln@testtoto:~$ cat /etc/passwd | grep marmotte  
marmotte:x:1001:1001::/home/papier_alu:/bin/sh
```
🌞 **Déterminer le *hash* du password de l'utilisateur `marmotte`**

- là encore, sous Linux, il existe un fichier qui liste les hashes des mots de passe de tous les utilisateurs
- encore une syntaxe `cat fichier | grep xxx` pour le compte-rendu

```
leobln@testtoto:~$ sudo cat /etc/shadow | grep marmotte  
marmotte:$y$j9T$zdi1HRFtuAk2FWFz1qRYA.$4tYVymP6hL/JJYUpXP6GeAseed.qpHAF86pUpgEp2y2:20039:0:99999:7:::
```

## D. Connexion sur le nouvel utilisateur

🌞 **Tapez une commande pour vous déconnecter : fermer votre session utilisateur**

- pas de shutdown ou reboot hein, juste fermer la session
- attention, cette commande peut varier suivant l'OS utilisé, ou la façon dont vous vous connectez à la machine (SSH ou non)

```
leobln@testtoto:~$ exit
logout
Connection to 192.168.74.2 closed.
PS C:\Users\leote>
```

🌞 **Assurez-vous que vous pouvez vous connecter en tant que l'utilisateur `marmotte`**

- une fois connecté sur l'utilisateur `marmotte`, essayez de faire un `ls` dans le répertoire personnel de votre premier utilisateur
- assurez-vous que vous mangez un beau `Permission denied` : vous avez pas le droit de regarder dans les répertoires qui vous concernent pas

```
leobln@testtoto:~$ su - marmotte  
Password:  
$ ls /home/leobln  
ls: cannot open directory '/home/leobln':   Permission denied    
```
> **On verra en détails la gestion des droits très vite.** 

# III. Programmes et paquets


## A. Run then kill

🌞 **Lancer un processus `sleep`**

- il doit dormir 1000 secondes
- ouvrez un deuxième terminal, pendant que le premier est occupé par le `sleep`
- dans ce deuxième terminal, déterminer le PID du processus `sleep`
- il existe une commande qui permet de lister les processus en cours d'exécution (un gestionnaire des tâches quoi) : `ps`
- syntaxe `commande | grep xxx` pour afficher uniquement la ligne du `sleep`
  - appelez-moi si besoin d'aide sur cette syntaxe !

## terminal 1
```
leobln@testtoto:~$ sleep 1000  
kill 2535
```
## terminal 2
```
leobln@testtoto:~$ ps aux | grep sleep
leobln      2535  0.0  0.0   5464   888 pts/0    S+   12:45   0:00 sleep 1000
leobln      2537  0.0  0.1   6332  2112 pts/1    S+   12:45   0:00 grep sleep
```

🌞 **Terminez le processus `sleep` depuis le deuxième terminal**

- utilisez la commande `kill` pour arrêter le processus `sleep`

> Utiliser la commande `kill` revient à appuyer sur la croix rouge en haut d'une fenêtre pour fermer un programme.

![Kill it](./img/killit.jpg)

## B. Tâche de fond

🌞 **Lancer un nouveau processus `sleep`, mais en tâche de fond**

- il doit dormir 1000 secondes
- pour lancer une commande en tâche de fond, il faut ajouter `&` à la fin de la commande
- ainsi, on garde notre terminal actif pendant que le programme s'exécute

🌞 **Visualisez la commande en tâche de fond**

- il existe une commande pour lister les processus qu'on a lancé en tâche de fond
- en utilisant cette commande, récupérez le PID du processus sleep

### cela repond aux deux question precedente
```
leobln@testtoto:~$ sleep 1000 &
[1] 2602
leobln@testtoto:~$ ps aux | grep sleep
leobln      2602  0.0  0.0   5464   908 pts/2    S    07:59   0:00 sleep 1000
leobln      2604  0.0  0.1   6332  2048 pts/2    S+   08:00   0:00 grep sleep
leobln@testtoto:~$ kill 2602
leobln@testtoto:~$ ps aux | grep sleep
leobln      2609  0.0  0.1   6332  2088 pts/2    S+   08:00   0:00 grep sleep
[1]+  Terminated              sleep 1000
```

## C. Find paths

🌞 **Trouver le chemin où est stocké le programme `sleep`**

- avec une commande `find`

```
leobln@testtoto:~$ sudo find / -name "sleep"
[sudo] password for leobln:
/usr/lib/klibc/bin/sleep
/usr/bin/sleep
find: ‘/run/user/1000/doc’: Permission denied
```

🌞 Tant qu'on est à chercher des chemins : **trouver les chemins vers tous les fichiers qui s'appellent `.bashrc`**

- utilisez la commande `find` encore

```
leobln@testtoto:~$ sudo find / -name ".bashrc"
/etc/skel/.bashrc
/root/.bashrc
/home/papier_alu/.bashrc
/home/leobln/.bashrc
/home/leobln/gameshell/gameshell.1/World/.bashrc
/home/leobln/gameshell/gameshell.2/World/.bashrc
/home/leobln/gameshell/gameshell/World/.bashrc
/home/marmotte/.bashrc
find: ‘/run/user/1000/doc’: Permission denied
```

🌞 Vérifier que

les commandes sleep, ssh, et ping sont bien des programmes stockés dans l'un des dossiers listés dans votre PATH

```
leobln@testtoto:~$ echo $PATH
/usr/local/bin:/usr/bin:/bin:/usr/local/games:/usr/games
leobln@testtoto:~$ which sleep
/usr/bin/sleep
leobln@testtoto:~$ which ssh
/usr/bin/ssh
leobln@testtoto:~$ which ping
/usr/bin/ping
```

# 2. Paquets

🌞 **Installer le paquet `firefox`**

- c'est juste pour faire pratiquer
- si vous avez choisi un OS sans interface graphique, inutile de télécharger Firefox
  - sans interface graphique, vous pouvez installer le paquet `git` pour remplacer

```
sudo apt install firefox -y
sudo apt install git -y
```


🌞 **Utiliser une commande pour lancer Firefox**

- comme d'hab, une commande, c'est un programme hein
- déterminer le chemin vers le programme `firefox`

```
leobln@testtoto:~$ which firefox
/usr/bin/firefox
```

🌞 **Mais aussi déterminer...**

- l'adresse `http` ou `https` des serveurs où vous téléchargez des paquets
- une commande `apt install` ou `dnf install` permet juste de faire un téléchargement HTTP
- ma question c'est donc : sur quelle(s) URL(s) tu te connectes pour télécharger des paquets
- il existe un dossier qui contient la liste des URLs consultées quand vous demandez un téléchargement de paquets
- c'est un sous-dossier de `/etc/`, et ça dépend du gestionnaire de paquets que tu utilises ! (avec Debian, le gestionnaire de paquets, c'est `apt`)

# IV. Poupée russe

🌞 **Récupérer le fichier `meow`**

- [il est dispo là](./meow) (juste à côté de ce README.md)
- téléchargez-le à l'aide d'une commande

```
leobln@testtoto:~/Downloads$ wget https://gitlab.com/it4lik/b1-os/-/raw/main/tp/2/meow
```

🌞 **Trouver le dossier `dawa/`**

- le fichier `meow` récupéré est une archive compresséePour finir de vous exercer avec le terminal, je vous ai préparé une poupée russe :D

➜ **De mon côté, pour préparer l'exercice :**

- j'ai créé un dossier `dawa/`, qui a plein de sous-dossiers, sous-fichiers, etc
  - y'a des fichiers particuliers, les autres c'est juste du random
- je l'ai archivé/compressé plusieurs fois
- j'ai volontairement supprimé les extensions des fichiers compressés
  - comme ça tu peux pas savoir à l'aide du nom si c'est un `.zip` ou un autre format

➜ **Le but de l'exercice pour vous :**

- je vous file le fichier que j'ai compressé/archivé plusieurs fois, que vous téléchargez dans la VM
- vous devez :
- utilisez la commande `file /path/vers/le/fichier` pour déterminer le type du fichier
- renommez-le fichier correctement (si c'est une archive compressée ZIP, il faut ajouter `.zip` à son nom)
- extraire l'archive avec une commande
- répétez ces opérations jusqu'à trouver le dossier `dawa/`

```
leobln@testtoto:~/Downloads$ ls
meow
leobln@testtoto:~/Downloads$ file meow
meow: Zip archive data, at least v2.0 to extract, compression method=deflate
leobln@testtoto:~/Downloads$ mv meow meow.zip
leobln@testtoto:~/Downloads$ ls
meow.zip
leobln@testtoto:~/Downloads$ unzip meow.zip
Archive:  meow.zip
  inflating: meow
leobln@testtoto:~/Downloads$ ls
meow  meow.zip
leobln@testtoto:~/Downloads$ file meow
meow: XZ compressed data, checksum CRC64
leobln@testtoto:~/Downloads$ mv meow meow.xz
leobln@testtoto:~/Downloads$ ls
meow.xz  meow.zip
leobln@testtoto:~/Downloads$ unxz meow.xz
leobln@testtoto:~/Downloads$ ls
meow  meow.zip
leobln@testtoto:~/Downloads$ file meow
meow: bzip2 compressed data, block size = 900k
leobln@testtoto:~/Downloads$ mv meow meow.bz2
leobln@testtoto:~/Downloads$ ls
meow.bz2  meow.zip
leobln@testtoto:~/Downloads$ bunzip2 meow.bz2
leobln@testtoto:~/Downloads$ ls
meow  meow.zip
leobln@testtoto:~/Downloads$ file meow
meow: RAR archive data, v5
leobln@testtoto:~/Downloads$ mv meow meow.rar
leobln@testtoto:~/Downloads$ ls
meow.rar  meow.zip
leobln@testtoto:~/Downloads$ unrar meow.rar

unrar-free 0.1.3  Copyright (C) 2004  Ben Asselstine, Jeroen Dekkers


Extracting from /home/leobln/Downloads/meow.rar

Extracting  meow                                                      OK

All OK
leobln@testtoto:~/Downloads$ ls
meow  meow.rar  meow.zip
leobln@testtoto:~/Downloads$ rm meow.rar meow.zip
leobln@testtoto:~/Downloads$ ls
meow
leobln@testtoto:~/Downloads$ file meow
meow: gzip compressed data, from Unix, original size modulo 2^32 145049600 gzip compressed data, reserved method, has CRC, extra field, has comment, from FAT filesystem (MS-DOS, OS/2, NT), original size modulo 2^32 145049600
leobln@testtoto:~/Downloads$ gunzip meow.gz
leobln@testtoto:~/Downloads$ ls
meow
leobln@testtoto:~/Downloads$ file meow
meow: POSIX tar archive (GNU)
leobln@testtoto:~/Downloads$ tar -xf meow
leobln@testtoto:~/Downloads$ file meow
meow: POSIX tar archive (GNU)
leobln@testtoto:~/Downloads$ ls
dawa  meow
leobln@testtoto:~/Downloads$ rm meow
leobln@testtoto:~/Downloads$ ls
dawa
```

🌞 **Dans le dossier `dawa/`, déterminer le chemin vers**

- le seul fichier de 15Mo
- le seul fichier qui ne contient que des `7`
- le seul fichier qui est nommé `cookie`
- le seul fichier caché (un fichier caché c'est juste un fichier dont le nom commence par un `.`)
- le seul fichier qui date de 2014
- le seul fichier qui a 5 dossiers-parents
  - je pense que vous avez vu que la structure c'est 50 `folderX`, chacun contient 50 dossiers `X`, et chacun contient 50 `fileX`
  - bon bah là y'a un fichier qui est contenu dans `folderX/X/X/X/X/` et c'est le seul qui 5 dossiers parents comme ça

```
leobln@testtoto:~/Downloads$ find dawa/ -type f -size 15728640c
dawa/folder31/19/file39
leobln@testtoto:~/Downloads$ grep -l -r '^[7]*$' dawa/
dawa/folder43/38/file41
dawa/folder31/19/file39
leobln@testtoto:~/Downloads$ find dawa/ -type f -name "cookie"
dawa/folder14/25/cookie
leobln@testtoto:~/Downloads$ find dawa/ -type f -name ".*"
dawa/folder32/14/.hidden_file
leobln@testtoto:~/Downloads$ find dawa/ -type f -newermt 2014-01-01 ! -newermt 2015-01-01
dawa/folder36/40/file43
leobln@testtoto:~/Downloads$ find dawa/ -type f -mindepth 6 -maxdepth 6
dawa/folder37/45/23/43/54/file43
```


