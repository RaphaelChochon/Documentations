## Mémo Git et GitHub
###### Dernière modif tuto : 04/06/2016

### 0 - Liens utiles
* [Tutos vidéos Grafikart](https://www.grafikart.fr/formations/git) : Excellents tutos vidéos couvrant une grosse partie des fonctions de git
<br>
* [Tutos vidéos Grafikart - Déployer comme un pro avec git](https://www.grafikart.fr/blog/deployer-site-git) : Pour le déploiement entre environnement de dev et de prod sur deux serveurs différents (tuto qui a servi de base pour le déploiement dev/prod de Geonature au PNM)

### 1 - Installation
##### Environnement : Partie local sur Windows et distant principalement sur Debian 8

L'installation de git sur Windows est de loin la plus foireuse de toutes. Il va nous falloir installer git en utilisant l'executable fournit sur [git-scm.com](https://git-scm.com/download/win). Lors de l'installation il faudra bien faire attention à cocher l'option afin d'obtenir les commande UNIX dans le PATH de notre système. Cette option nous permettra par la suite de profiter de certaines commandes linux sans avoir à utiliser un terminal spécial.
<br>
Après installation et pour les futures commandes à lancer, on utilisera le shell de git (git bash dans windows) :
<br>
![Git Bash dans Windows](img/git_bash.jpg)

### 2 - Configuration générale

Première chose à faire après installation propre sur son poste de travail :
```
	git config --global user.name "MON_NOM"
	git config --global user.email "MON_EMAIL_RATTACHÉ_A_MON_COMPTE_GITHUB"
	git config --global color.ui auto
	git config --global core.autocrlf true
```
* Les deux premiers paramètres permettent d’identifier l’auteur des commits avec son nom et son adresse mail (cette dernière doit être identique à celle rensignée sur son compte GitHub perso).
* Le troisième permet d’activer la colorisation de la sortie en ligne de commande, cependant elle ne parait plus nécessaire, puisque ce serait maintenant le comportement par défaut à l’installation (pour une installation sur environnement Windows uniquement).
* La quatrième permet de convertir les retours-chariots pour le système UNIX (donc nécessaire uniquement sous environnement Windows)

<br><br>

Si à l’installation nous avons choisi d’importer dans le git bash les commandes de bases unix, alors nous pouvons ensuite nous en servir comme dans Linux pour se déplacer par exemple dans l’arborescence de nos projets :
```
	cd /c/Mercantour/WORKSPACE
```

### 3 - Initialisation d'un projet
Pour initialiser un nouveau projet, ou même un projet commencé de longue date mais non "gité", rien de plus simple
On peut créer le répertoire, ou bien si le projet est déjà existant s'y déplacer :
```
	mkdir mon_projet
	cd mon_projet
	git init
```
Création du répertoire du projet, puis initialisation de Git à l’intérieur de celui-ci afin de commencer à tracker toutes modifications.

### 4 - Commandes de bases utiles
Liste et détail de quelques commandes que j'utilise assez régulièrement, et d'autres moins souvent et qui requiert justement un mémo :


##### Commandes généralistes


```
	git --help
```
Une commande qu'on oublie trop souvent quand on ne se rappel plus le nom ou la fonction d'une commande

***

```
	git init
```
Va initialiser git dans le répertoire courant

***

```
	git clone [url ou chemin vers le dossier si en local]
```
Télécharge un projet et tout son historique de versions


##### Effectuer des modifications...


```
	git status
```
Cette commande va nous retourner l’état des fichiers que track Git, afin de savoir s’il faut que nous fassions un commit. Ci-dessous, deux fichiers ont été modifiées :
<br>
![git status](img/status.jpg)

***

```
	git add .
```
Permet d'ajouter TOUS les fichiers au suivi de version de git (A effectuer dés que l'on modifie ou ajoute un fichier dans notre répertoire de travail). Sans ça, les fichiers ne seront pas inclus aux éventuels futurs commits.
On peut remplacer le ``.`` par le nom exact du ou des fichiers que l'on veut ajouter (Cela peut par exemple permettre de décomposer nos commits).

***

```
	git commit -m "Commit initial - Ici on entre le texte que l'on veut"
```
Permet de commiter nos fichiers et donc d'effectuer un instantané de notre répertoire de travail, de façon permanente dans l'historique de version de git.
A noter que pour être effectué, il faut avoir au préalable ajouté des fichiers au suivi avec ``git add`` OU ajouter le paramètre ``-a`` juste avant le ``-m``.

***

```
	git diff
```
Montre les modifications non indexées sur les fichiers

***

```
	git diff --staged
```
Montre les différences de fichier entre la version indexée et la dernière version

***

```
	git commit -m "Mon commentaire initial"
	git add fichier_oublie
	git commit --amend
```
Si nous voulons rectifier le dernier commit fait car nous avons oublié d’y ajouter un fichier modifié nous pouvons le faire avec la commande ``git commit --amend``
Cela nous ouvre alors le dernier commit avec l’éditeur de texte par défaut (vi). Nous pouvons alors le modifier puis à nouveau le soumettre.

***

```
	git reset [fichier]
```
Enleve le fichier de l'index, mais conserve son contenu

***


##### Supprimer un commit


Si après avoir fait un commit nous nous rendons compte que nous avons fait une erreur ou omis de spécifier un changement, nous pouvons aussi annuler ce commit en le supprimant, et en utilisant une commande spécifique. Cette commande permet aussi de supprimer plusieurs commit à la volée.
:boom: :bangbang: <span style="color: #fb4141">**Cependant, il faut faire attention à ne supprimer que des commentaires qui n’ont pas encore été envoyés vers un serveur distant (push) où d’autres personnes sont susceptibles de travailler dessus.**</span> :bangbang: :boom:

```
	git reset [le SHA-1 abrégé du commentaire précédent celui (ou ceux) que nous voulons supprimer]
```

Afin de récupérer le SHA-1 abrégé nous pouvons lancer la commande suivante :

```
	git log –-abbrev-commit
```

Ou de façon plus simple en utilisant « ungit », le SHA-1 abrégé est ici directement présenté :
<br>
![Ungit SHA-1 abrégé](img/SHA-1.jpg)

***

:boom: :bangbang: <span style="color: #fb4141">**ATTENTION**</span> :bangbang: :boom:
```
	git reset --hard [le SHA-1 abrégé du commentaire précédent celui (ou ceux) que nous voulons supprimer]
```
<span style="color: #fb4141">**Supprime tout l'historique et les modifications effectuées après le commit spécifié**</span>


##### Les branches


<br>
![Git et son système de branche](img/git_branch.png)

```
	git branch
```
Liste toutes les branches locales dans le dépôt courant

***

```
	git branch [nom-de-branche]
```
Crée une nouvelle branche

***

```
	git checkout [nom-de-branche]
```
Bascule sur la branche spécifiée et met à jour le répertoire de travail (et ça c'est magique !)

***

```
	git branch -d [nom-de-branche]
```
Supprime la branche spécifiée
Si la branche est "pleine" ou n'a pas été fusionnée avec une autre, alors il faudra utiliser ``git branch -D [nom-de-branche]``
**Attention on perdra toutes les modifications qui y ont été effectuées !**

***

```
	git merge [nom-de-branche]
```
Fusionne dans la branche courante l'historique de la branche spécifiée

***


##### Changements au niveau des noms de fichiers


:point_right: Commandes de base en console Linux, mais avec Git :point_left:

```
	git rm [fichier]
```
Supprime le fichier du répertoire de travail et met à jour l'index

***

```
	git rm --cached [fichier]
```
Supprime le fichier du système de suivi de version mais le préserve localement.
A comparer avec ``git reset [fichier]`` ?! Pas encore testé !

***

```
	git mv [fichier-nom] [fichier-nouveau-nom]
```
Renomme le fichier et prépare le changement pour un commit

***


##### Changements au niveau des noms de fichiers


```
	git log
```
Montre l'historique des versions pour la branche courante

***

```
	git log --follow [fichier]
```
Montre l'historique des versions, y compris les actions de renommage, pour le fichier spécifié

***


```
	git diff [premiere-branche]...[deuxieme-branche]
```
Montre les différences de contenu entre deux branches

***

```
	git show [commit]
```
Montre les modifications de métadonnées et de contenu inclues dans le commit spécifié

***


##### Branches distantes, synchronisations, etc...


```
	git fetch [nom-de-depot]
```
Récupère tout l'historique du dépôt nommé
@xavier-pnm à creuser pour récupérer l'historique de [Geonature](https://github.com/PnEcrins/GeoNature)

***

```
	git merge [nom-de-depot]/[branche]
```
Fusionne la branche du dépôt dans la branche locale courante

***

```
	git push [alias] [branche]
```
Envoie tous les commits de la branche locale vers GitHub

***

```
	git pull [alias] [branche]
```
Récupère tout l'historique du dépôt nommé et incorpore les modifications

***


##### Résoudre des conflits

:soon:





<br><br><br><br>
###### Source
* [Doc officielle](https://github.com/github/training-kit/blob/master/downloads/fr/github-git-cheat-sheet.md)
<br>
