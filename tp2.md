

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