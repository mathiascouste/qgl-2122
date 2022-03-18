

# Comprendre l'arbitre

*Ce document est soumis à évolution*

## Déroulement d'une partie
### Initialisation de la partie
Au début de son exécution, l'arbitre charge la partie puis l'ensemble des Cockpits des équipes.
La fonction `initGame` de chaque équipe est alors invoquée.
### Exécution d'un tour
Chaque tour s'exécute de la manière suivante :

 1. Ré-initialisation des éléments de chaque bateau (*ex: rame utilisée redevient disponible*) et équipage (*ex : marin s'étant déjà déplacé au tour précédent récupéré sa capacité à se déplacer*).
 2. La fonction `nextRound` de chaque équipe est invoquée.
 3. Les actions des marins sont contrôlées puis exécutées.
 4. Exécution du déplacement de chaque bateau.
 5. Retrait des bateaux détruits.
 6. Vérification des conditions de victoire ou de fin de partie.

## Calcul des éléments visibles
Un élément est visible s'il remplit les 2 critères suivants:

 1. Il est différent de votre propre bateau
 2. Sa forme entre en collision avec votre champ de vision (cercle imaginaire dont le centre est la position de votre bateau et donc le rayon est votre vision (1000 par défaut / 5000 si vigie activée)).

## Exécutions des actions
À chaque tour, chaque matelot ne peut faire qu’une seule action.
Seule exception le déplacement. En effet, un marin peut se déplacer et exécuter une action.

Avant d’effectuer la moindre action, l’arbitre vérifie les conditions d’activation de cette dernière. En cas de non validité de l’action cette dernière est ignorée.

Les actions sont exécutées par l’arbitre dans l’ordre suivant :
 1. Déplacement de marin
 2. Ramage
 3. Hissage de voile
 4. Affalage de voile
 5. Actionnage du gouvernail
 6. Monter la garde
 7. Orienter canon
 8. Charger canon
 9. Tirer au canon
 
## Déplacements
Un tour dans le jeu représente une trentaine de seconde dans la vraie vie.

### Calcul de la vitesse linéaire
La vitesse linéaire est la combinaison des vitesses (vecteurs) suivantes :

#### Vitesse des rames :

-   Direction : direction du bateau
-   Valeur : 165 x nombre de rames active / nombre total de rames
#### Vitesse du vent:

-   Direction : direction du bateau
-   Valeur : (nombre de voiles ouvertes / nombre de voiles) x force du vent x cosinus(angle entre la direction du vent et la direction du bateau)
    

#### Vitesse du courant :

Uniquement si le bateau est en contact avec un ou plusieurs courant.
-   Direction: direction du courant
-   Valeur: force du courant
    
### Calcul de la rotation du bateau
La rotation est la somme des vitesses angulaires suivantes:

#### Rotation par les rames :
La rotation issue des rames est comprise entre -PI/2 et PI/2.

Cet angle est découpé en n+1 parts (n étant égal au nombre total de rames).

Exemple : un bateau possédant 4 rames (2 de chaque côté) peut avoir les vitesses radiales suivantes: -PI/2, -PI/4, 0, PI/4, et PI/2.

Chaque rame actionnée d’un côté du bateau tend à l’orienter vers le côté opposé.

*Exemple :*

| Rames actives à bâbord | Rames actives à tribord | Rotation |
|--|--|--|
| 0 | 0 | 0 |
| 0 | 1 | PI/4 |
| 0 | 2 | PI/2 |
| 1 | 0 | -PI/4 |
| 1 | 1 | 0 |
| 1 | 2 | PI/4 |
| 2 | 0 | -PI/2 |
| 2 | 1 | -PI/4 |
| 2 | 2 | 0 |

#### Rotation par le gouvernail :

La rotation induite par le gouvernail est de zéro s’il n’est pas utilisé.
Si le gouvernail est utilisé, la rotation induite correspond à l’angle du gouvernail (donné en paramètre de l’action d’activation).  
Si aucun actionnage du gouvernail n'est envoyé pendant le tour, l’angle est de zéro.

### Étape par étape
La simulation de déplacement s'effectue en N étapes.
A chacune de ses sous-étapes, le bateau avance de sa vitesse divisée par N.
Il en va de même pour sa rotation.
À chaque sous-étape, les collisions sont contrôlées. S'il y a collision avec un élément solide de la mer (bateau ou récif) alors le déplacement de cette sous-étape est annulé.


Prenons un exemple :

    Position initiale : [0;0]
    Orientation initiale : 0
    N = 10
    Vitesse linéaire = 100
    Rotation = 1

Étape 1 :
On applique 10% (1/N) du déplacement linéaire dans le sens du bateau.
On applique 10% (1/N) de la rotation.

    Position : [10;0]
    Orientation : 0.1

Étape 2 :
On applique 10% (1/N) du déplacement linéaire dans le sens du bateau.
On applique 10% (1/N) de la rotation.

    Position : [19.95;0.10]
    Orientation : 0.2

Étape 3 :
On applique 10% (1/N) du déplacement linéaire dans le sens du bateau.
On applique 10% (1/N) de la rotation.

    Position : [29.75;2.99]
    Orientation : 0.3

Étape 4 :
On applique 10% (1/N) du déplacement linéaire dans le sens du bateau.
On applique 10% (1/N) de la rotation.

    Position : [39.30;5.94]
    Orientation : 0.4

Étape 5 :
On applique 10% (1/N) du déplacement linéaire dans le sens du bateau.
On applique 10% (1/N) de la rotation.

    Position : [48.51;9.83]
    Orientation : 0.5

Étape 6 :
On applique 10% (1/N) du déplacement linéaire dans le sens du bateau.
On applique 10% (1/N) de la rotation.

    Position : [57.29;14.63]
    Orientation : 0.6

Étape 7 :
On applique 10% (1/N) du déplacement linéaire dans le sens du bateau.
On applique 10% (1/N) de la rotation.

    Position : [65.54;20.28]
    Orientation : 0.7

Étape 8 :
On applique 10% (1/N) du déplacement linéaire dans le sens du bateau.
On applique 10% (1/N) de la rotation.

    Position : [73.19;26.72]
    Orientation : 0.8

Étape 9 :
On applique 10% (1/N) du déplacement linéaire dans le sens du bateau.
On applique 10% (1/N) de la rotation.

    Position : [80.16;33.89]
    Orientation : 0.9

Étape 10 :
On applique 10% (1/N) du déplacement linéaire dans le sens du bateau.
On applique 10% (1/N) de la rotation.

    Position : [86.38;41.72]
    Orientation : 1.0
/!\ Collision détectée ! On reviens à la position précédente.

    Position : [80.16;33.89]
    Orientation : 0.9

*(Dans cet exemple, des approximations ont été faites dans les calculs)*

## Dégâts de collision

En cas de collision entre deux objets dans la mer, ces derniers subissent des dégâts.
Le nombre de points de dégâts est calculé de la manière suivante:

    totalDegats = |vrp| x |vrp| / 20
    Où vrp = vitesse relative entre les deux objets
    Où |v| = norme du vecteur v

**Exemples :**
Si deux bateaux allant à une vitesse 100 se percutent en face à face, ils subissent alors 200 x 200 / 20 = 2000 points de dégâts.

Si un bateau allant à une vitesse de 100 rattrape et percute un bateau allant à une vitesse de 70 dans la même direction, ils subissent alors 30 x 30 / 20 = 45 points de dégâts.

Si bateau circulant vers le Nord à une vitesse de 150 percute un bateau allant vers l'Est à une vitesse de 100, ils subissent alors (sqrt(100 x 100 + 100 x 100)) x (sqrt(100 x 100 + 100 x 100)) / 20 = 1000 points de dégâts.

## Utiliser ses canons
### Orientation du canon
L'orientation du canon dans le repère de la mer dépend de deux facteurs :
 - son orientation de référence qui dépend de sa position (bâbord ou tribord) : le canon est perpendiculaire à la coque.
 - son angle de visée : qui change en fonction de l'action "viser" (AIM).

Lorsque l'action "viser" est faite, le canon prend l'angle donné en paramètre auquel est ajouté un petit peu d'aléatoire (plus ou moins un aléatoire tiré entre 0 et PI/40).

### Tirer
Lorsque l'action "tirer" est faite sur un canon chargé, le tir est effectué.
Les tirs sont effectués avant les déplacements des bateaux.

Le boulet de canon parcourt 500 mètres en touchant TOUS les bateaux sur sa trajectoire.
La trajectoire du boulet commence depuis le centre du bateau.
Les dégâts infligés sont égaux à : 500 - (la distance entre votre bateau et le bateau visé).

Les boulets peuvent passer au dessus des récifs et des courants sans être impactés.
Le vent n'a pas d'effet sur la trajectoire du boulet.

**Exemples :**
Votre bateau est orienté vers le Nord (orientation dans la mer = 0).
Votre bateau dispose de deux canons "B" et "T" : un à bâbord ("B") et un à tribord ("T").

Par défaut :
 - le canon B sera orienté par défaut vers l'Ouest (orientation dans la mer = PI/2)
 - le canon T sera orienté par défaut vers l'Est (orientation dans la mer = -PI/2)

Si l'action viser est donnée au deux canons avec pour angle PI/4 :
 - le canon B sera orienté le Sud-Ouest (orientation dans la mer = 3xPI/4)
 - le canon T sera orienté le Nord-Est (orientation dans la mer = -PI/4)


