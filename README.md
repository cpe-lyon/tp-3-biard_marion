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

Le paquet *fortunes* est un paquet permettant d'afficher des épigrammes, connues sous le nom de cookies fortune, (des petits messages, des citations, des proverbes) choisies aléatoirement à partir de fichiers fortune.

Pour installer fortune il faut taper l'instruction suivante :
```bash
sudo apt install fortune
```

&nbsp;

**6. Quels paquets proposent de jouer au sudoku ?**

> Les paquets proposant de jouer au sudoku sont :
- xenial
- bionic
- disco
- eoan
- focal

&nbsp;

**7. Lister les derniers paquets installés explicitement avec la commande apt install**

Nous exécutons la commande suivante :
```bash
apt list --manual-installed
```
En retour, nous obtenons :
```bash
En train de lister...Fait
base-passwd/eoan,now 3.5.46 amd64 [installé]
bash/eoan,now 5.0-4ubuntu1 amd64 [installé]
cloud-init/eoan-updates,now 19.4-33-gbb4131a2-Oubuntu1~19.10.1 all [installé]

```
![manual installed](https://github.com/cpe-lyon/tp-3-biard_marion/blob/master/manual-installed.png)

&nbsp;

***

&nbsp;

### Exercice 2

**1. A partir de quel paquet est installée la commande ls ?**

La commande ls est installée à partir du parquet : *GNU coreutils*.

Pour savoir cela, il faut tout d'abord taper la commande :
```bash
which -a ls
```
qui nous affiche le chemin absolu de la commande *ls*.

> Dans notre cas, il existe deux commande *ls*, ce qui signifie que nous aurons deux retours.

Une fois que nous avons le chemin de la commande qui nous intéresse, ici */bin/ls*, nous exécutons la commande :
```bash
dpkg -S /bin/ls
```
Cette commande nous retourne donc que la commande *ls* fait parti du package *coreutils*.

&nbsp;

**2. Comment obtenir cette information en une seule commande, pour n’importe quel programme (indice : la réponse est dans le poly de cours 2, dans la liste des commandes utiles) ?**

Afin d'obtenir cette information en une seule commande pour n'importe quel programme, il faut utiliser la commande :
```bash
which -a programme | xargs dpkg -S 2>/dev/null
```
où nous pouvons remplacer *programme* par le programme dont nous souhaitons connaitre le package d'installation

> **xargs** permet de récuperer les résultats de la première commande et de les passer en arguments de la deuxième.
> **2>/dev/null** permet de supprimer les erreurs de résultats.

&nbsp;

**3. Utilisez la réponse à pour écrire un script appelé origine-commande (sans l’extension.sh) prenant en argument le nom d’une commande, et indiquant quel paquet l’a installée.**

Le script à écrire est le suivant :
```bash
#!/bin/bash

echo $(which -a $1 | xargs dpkg -S 2>/dev/null)
```

&nbsp;

***

&nbsp;

### Exercice 3

**Ecrire une commande qui affiche “INSTALLÉ” ou “NON INSTALLÉ” selon le nom et le statut du package
spécifié dans cette commande.**

Nous avons préféré le faire à l'aide d'un script :
```bash
#!/bin/bash

dpkg -s $1 &>/dev/null

if [ $? -eq 0 ]; then
  echo "INSTALLE"
else
  echo "NON INSTALLE"
fi
```

Nous pouvons également le faire à l'aide d'une ligne de commande :
```bash
A REMPLIS
```

&nbsp;

***

&nbsp;

### Exercice 4

**Lister les programmes livrés avec coreutils. A quoi sert la commande ’[’ et comment afficher ce qu’elle
retourne ?**

&nbsp;

***

&nbsp;

### Exercice 5 : aptitude

**Installez le paquet emacs à l’aide de la version graphique d’aptitude**

&nbsp;

***

&nbsp;

### Exercice 6 : Installation d’un paquet par PPA

**1. Installer la version Oracle de Java (avec l’ajout des PPA)**

```bash
sudo add-apt-repository ppa:linuxuprising/java
sudo apt update
sudo apt install oracle-java11-installer
```

**2. Vérifiez qu’un nouveau fichier a été créé dans /etc/apt/sources.list.d. Que contient-il ?**

&nbsp;

***

&nbsp;

### Exercice 7. Création de dépôt personnalisé

**Dans cet exercice, vous allez créer vos propres paquets et dépôts, ce qui vous permettra de gérer les
programmes que vous écrivez comme s’ils provenaient de dépôts officiels.**

##### Création d’un paquet Debian avec dpkg-deb

**1. Dans le dossier scripts créé lors du TP 2, créez un sous-dossier origine-commande où vous créerez un
sous-dossier DEBIAN, ainsi que l’arborescence usr/local/bin où vous placerez le script écrit à l’exercice
2**

**2. Dans le dossier DEBIAN, créez un fichier control avec les champs suivants :

**Package: origine-commande #nom du paquet**

**Version: 0.1 #numéro de version**

**Maintainer: Foo Bar #votre nom**

**Architecture: all #les architectures cibles de notre paquet (i386, amd64...)**

**Description: Cherche l'origine d'une commande**

**Section: utils #notre programme est un utilitaire**

**Priority: optional #ce n'est pas un paquet indispendable**

**3. Revenez dans le dossier parent de origine-commande (normalement, c’est votre $HOME) et tapez la
commande suivante pour construire le paquet :**

```bash
dpkg-deb --build origine-commande
```

**Félicitations ! Vous avez créé votre propre paquet !**

#### Création du dépôt personnel avec reprepro

**1. Dans votre dossier personnel, commencez par créer un dossier repo-cpe. Ce sera la racine de votre
dépôt**

**2. Ajoutez-y deux sous-dossiers : conf (qui contiendra la configuration du dépôt) et packages (qui contiendra
nos paquets)**

**3. Dans conf, créez le fichier distributions suivant :**

**Origin: Un nom, une URL, ou tout texte expliquant la provenance du dépôt**

**Label: Nom du dépôt**

**// Suite: stable**

**Codename: cosmic #le nom de la distribution cible**

**Architectures: i386 amd64 #(architectures cibles)**

**Components: universe #(correspond à notre cas)**

**Description: Une description du dépôt**

**4. Dans le dossier repo-cpe, générez l’arborescence du dépôt avec la commande
reprepro -b . export**

**5. Copiez le paquet origine-commande.deb créé précédemment dans le dossier packages du dépôt, puis,
à la racine du dépôt, exécutez la commande**

```
reprepro -b . includedeb cosmic origine-commande.deb
```

**afin que votre paquet soit inscrit dans le dépôt.**

**6. Il faut à présent indiquer à apt qu’il existe un nouveau dépôt dans lequel il peut trouver des logiciels.
Pour cela, créez (avec sudo) dans le dossier /etc/apt/sources.list.d le fichier repo-cpe.list
contenant :**

```
deb file:/home/VOTRE_NOM/repo-cpe cosmic multiverse
```

**(cette ligne reprend la configuration du dépôt, elle est à adapter au besoin)**

**7. Lancez la commande sudo apt update.**

**Féliciations ! Votre dépôt est désormais pris en compte ! ... Enfin, pas tout à fait... Si vous regardez
la sortie d’apt update, il est précidé que le dépôt ne peut être pris en compte car il n’est pas signé.
La signature permet de vérifier qu’un paquet provient bien du bon dépôt. On doit donc signer notre
dépôt.**

#### Signature du dépôt avec GPG

**GPG est la version GNU du protocole PGP (Pretty Good Privacy), qui permet d’échanger des données de
manière sécurisée. Ce système repose sur la notion de clés de chiffrement asymétriques (une clé publique et
une clé privée)**

**1. Commencez par créer une nouvelle paire de clés avec la commande**

```
gpg --gen-key
```

**Attention ! N’oubliez pas votre passphrase !!!**

**2. Ajoutez à la configuration du dépôt (fichier distributions la ligne suivante :
SignWith: yes**

**3. Ajoutez la clé à votre dépôt :**

```
reprepro --ask-passphrase -b . export
```

**Attention ! Cette méthode n’est pas sécurisée et obsolète ; dans un contexte professionnel, on utiliserait
plutot un gpg-agent.**

**4. Ajoutez votre clé publique à votre dépôt avec la commande**

```
gpg --export -a "auteur" > public.key
```

**5. Enfin, ajoutez cette clé à la liste des clés fiables connues de apt :**

```
sudo apt-key add public.key
```

**Félicitations ! La configuration est (enfin) terminée ! Vérifiez que vous pouvez installer votre paquet comme
n’importe quel autre paquet.**

### Exercice 8 :  Installation d’un logiciel à partir du code source

**Lorsqu’un logiciel n’est disponible ni dans les dépôts officiels, ni dans un PPA, ou encore parce qu’on
souhaite n’installer qu’une partie de ses fonctionnalités, on peut se tourner vers la compilation du code
source.
Malheureusement, cette installation ”à la main” fait qu’on ne propose pas des bénéfices de la gestion de
paquets apportée par dpkg ou apt. Heureusement, il est possible de transformer un logiciel installé ”à la
main” en un paquet, et de le gérer ensuite avec apt ; c’est ce que permet par exemple checkinstall.**

**1. Commencez par cloner le dépôt git suivant :**

```
git clone https://github.com/jubalh/nudoku
```

**Ceci permet de récupérer en local le code source du logiciel nudoku.**

**2. Rendez vous dans le dossier nudoku qui vient d’être créé et lancez la commande autoreconf -i (ainsi
que spécifié dans le fichier README.md). A vous d’installer les éventuels paquets manquants (un
peu d’aide : pour résoudre le problème de la macro AM_GNU_GETTEXT manquante, installez le paquet
gettext). Relancez la commande `autoreconf -i` après chaque paquet installé jusqu’à ce
qu’elle se termine sans erreur.**

**3. Exécutez le script configure**

**4. Normalement, à cette étape on exécute la commande make install, qui procède à la compilation
proprement dite et à l’installation (copie des fichiers compilés dans leur dossier de destination). Mais
dans notre cas, on va demander à checkinstall de s’en charger et de créer un paquet au format .deb :
sudo checkinstall
Le logiciel est à présent installé (exécutez nudoku pour vous en assurer) ; on peut vérifier par exemple
avec aptitude qu’il provient bien du paquet qu’on a créé avec checkinstall.**
