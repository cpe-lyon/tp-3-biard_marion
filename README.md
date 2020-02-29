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

**2. Utiliser *dpkg* et *apt* pour compter le nombre de paquets installés (ne pas hésiter à consulter le manuel !).
Comment explique-t-on la (petite) différence de comptage ?**

La commande à faire pour dpkg est :
```bash
dpkg --list | wc -l
```
