# Perceptron multi-couches


## Travail à faire

Vous avez les modules pré-compilés et les exécutables
dans les sous-répertoires:
- bin/dbg et lib/dbg, pour les versions lentes avec plein d'informations sur l'exécution du programme
- bin/prof et lib/prof, pour les versions normales, plus rapides mais avec un minimum d'informations sur l'exécution du programme.


### Méthode de travail

La méthode de travail est la suivante:
1. au départ:
   - copiez les modules compilés avec lesquels vous préférez travailler dans lib
   - compilez une première fois les programme.
2. pour écrire un nouveau module (par exemple, il est conseillé de commencer avec le module matrices):
   - créez une branche matrices. Toutes les modifications sur ce module seront faites dans cette branche.
   - supprimez le module lib/matrices.o
   - créez un module src/matrices.c
   - modifiez ce module et recompilez jusqu'à ce que la compilation réussise (il faut écrire au moins un squelette pour chaque fonction)
   - sauvegardez cette première version avec git
   - ensuite, il faut exécuter les programmes d'apprentissage et d'évaluation sans qu'il n'y ait de fautes (avec make test)
   - C'est bien d'avoir des sauvegardes régulières dès que vous avez progressé.
   - Enfin, il faut vérifier que le réseau apprends correctement, avec un faible écart-type à la fin.
   - lorsque vous êtes satisfait du résultat, vous fusionnez la branche matrices avec la branche main.

Comme vous êtes plusieurs dans le groupe, chaque membre peut commencer
à développer un module différent. Les développements peuvent être
faits en parallèle tant que chacun modifie bien la branche de son
module. Faîtes attention quand vous fusionnez la branche d'un module
avec la branche main, car à partir de là tous les autres membres du
groupe devront utiliser votre version.

### Modules de base

Certains modules sont plus faciles ou plus court que d'autres:

Le module ```utils``` est essentiellement le module/la librairie
d'entrées-sorties faites en TP, sauf qu'il y a un paramètre
supplémentaire, le fichier ```FILE * f``` dans lequel il faut écrire.

Le module matrice peut-être écrit dès qu'on aura vu les tableaux et
l'allocation dynamique. La structure des matrices est imposée dans
```include/matrices_struct.h```, et sera vue en TP.

Le module ```matrices_accesseurs``` est presque plus simple, une fois
qu'on a fait le module ```matrices```: il s'agit juste de cacher la
structure matrice en écrivant des fonctions qui opèrent sur la
structure. Le principe de programmation derrière est que tous les
modules qui utilisent ```include/matrices_struct.h``` doivent être
changés si on modifie cette structure, alors que ceux qui utilisent
```matrices_accesseurs``` n'ont pas besoin d'être changés.

Le module ```lire_db``` est relativement facile: il faut lire un
fichier dans le format tests/fonction_lineaire.csv. Dans la structure
de ```include/lire_db.h```, le record est souvent traduit en français
par enregistrement. Il s'agit juste de compter combien il y a de lignes
de données à traiter.


Il est demandé de faire ces 3 modules d'ici la fin du projet, avec une
personne différente pour chaque module.

### Modules avancés

Le module ```matrices_operation``` n'est pas trivial car certaines
opérations (celles avec onearg, two args, etc.) prennent une fonction
en argument. C'est la mode actuelle, il faut s'y faire ! Notez, si
vous décidez de vous y mettre, que si on enlève les logs et les
assert, chaque fonction fait 4 lignes, déclaration des variables
comprises et double boucle for comprise. Mais sinon, rien de
difficile. Notez qu'il n'y a pas de "malloc.h" dans ce module: toutes
les matrices sont censées exister. La rétro-propagation et la mise à
jour des coefficients sont plus compliquées, mais il faudra surtout
expliquer, dans les commentaires et le rapport, les formules que vous
avez utilisées pour les calculs d'erreur.

Le module ```activation``` est un peu plus compliqué au départ, car on
utilise des structures qui contiennent des fonctions. Mais au final,
il y a essentiellement plein de petites fonctions (et leur dérivée)
sur les float à écrire, et une grosse fonction qui retourne une
structure avec les bonnes fonctions à l'intérieur.

Le module ```reseau``` commence à être difficile, car il faut
comprendre le fichier ```include/couches_struct.h```: chaque structure
couche contient un ensemble de fonctions. Il faut appeler ces
fonctions, ainsi que les fonctions spécifiques pour chaque type de
couche, pour implémenter le module ```reseau```. Pour le reste, les
fonctions sont courtes (moins de 10 lignes à chaque fois une fois les
logs et assertions enlevées), sauf la lecture et l'écriture d'un
réseau dans un fichier, qui sont longues (et pas très intéressantes,
c'est juste qu'il y a plein de cas à traiter).

Le module ```specification``` crée un tableau de spécifications de
couche en interagissant avec l'utilisateur. Il crée ensuite un réseau
à partir de toutes ces couches. C'est presque facile après avoir fait
le module ```reseau```

Implémenter un de ces modules est déjà bien. Deux ou trois, c'est très
bien, mais pas forcément optimal: on va être attentifs à la qualité du
code que vous écrirez, aux commentaires, et à un petit rapport de 2 à
5 pages qui décrira ce que vous avez fait. Faire tout ça très bien (y
compris l'utilisation de git) est suffisant pour avoir une note au
dela de 16. Aprè, ce sont des points de bonus, limités à 20 (et la
satisfaction du travail accompli). 


### Modules `au dela du raisonnable'

Le module ```couches``` et les modules implémentant des couches
spécifiques sont a priori trop difficiles pour des débutants. Tous les
`concepts' de C auront été vus en décembre, mais il faut du temps pour
les digérer. Contactez directement votre enseignant si vous en êtes
là, pour au moins savoir s'il vaut mieux les aborder ou améliorer ce
que vous avez déjà.


### Tests

Vous avez dans tests/lineaire.c un petit programme qui génère un log à
partir d'une fonction. N'hésitez pas à le modifier si vous voulez
apprendre des fonctions pus compliquées. Les fichiers
tests/input... permettent de ne pas à avoir à tout taper à la main
quand le programme nous demande quelque chose.

## valgrind

Valgrind est un outil permettant de diagnostiquer les problèmes
d'utilisation de la mémoire. On l'utilise avec la commande:

    valgrind --leak-check=full --show-leak-kinds=all bin/apprentissage tests/fonction_lineaire.csv

Il nous renseigne sur toutes les erreurs de gestion de la mémoire
qu'il peut trouver. Il marche surtout avec les versions de débugage
(qui gardent le numéro de ligne et le fichier d'origine de l'opération
qui pose problème).

On peut l'installer avec:

     sudo apt install valgrind

