# Administration système - TP 3

## Auteurs :

*BIARD Gauthier*

*MARION Audran*

*Le 28/02/2020*


***

## Exercice 1. Commandes de base

**1. Quels sont les 5 derniers paquets installés sur votre machine ?**

Nous utilisons la commande suivante :

```bash
grep installed /var/log/dpkg.log | tail -5
```

En réponse, nous obtenons :
```bash
2020-02-28 12:52:42 status installed libxaw7:amd64 2:1.0.13-1
2020-02-28 12:52:42 status installed x11-xerver-utils:amd64 7.7+8
2020-02-28 12:52:42 status installed libc-bin:amd64 2.30-0ubuntu2
2020-02-28 12:52:42 status installed systemd:amd64 242-7ubuntu3.6
2020-02-28 12:52:42 status installed man-db:amd64 2.8.7-3
```

&nbsp;

**2. Utiliser *dpkg* et *apt* pour compter le nombre de paquets installés (ne pas hésiter à consulter le manuel !).
Comment explique-t-on la (petite) différence de comptage ?**

La commande à faire pour dpkg est :
```bash
dpkg --list | grep "^ii" | wc -l
```
Le résultat est : 572
> Nous utilisons ```grep "^ii"``` afin de ne compter que les paquets qui sont installés car il y à 4 lignes au début de la liste qui ne sont que des informations de comment lire la liste.

La commande pour apt est :
```bash
apt list -- installed | wc -l
```
Le résultat est : 573

La différence est due au fait qu'au début de la liste affichée par apt, il y a une ligne : ```En train de lister...```

&nbsp;

**3. Combien de paquets sont disponibles en téléchargement ?**

apt list | wc -l

61599

&nbsp;

**4. Créer un alias “maj” qui met à jour le système**

Il faut d'abord aller dans le fichier .bashrc avec la commande : 
```bash
nano .bashrc
```
Dans ce fichier on ajoute :
```bash
alias maj="sudo apt update & upgrade'
```
Ensuite, on actualise le bash: 
```bash
source ~/.bashrc
```

&nbsp;

**5. A quoi sert le paquet fortunes ? Installez-le.**
