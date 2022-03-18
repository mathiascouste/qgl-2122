



# Spécifications techniques

*Ce document est soumis à évolution*

## Structure du projet
Votre projet doit conserver la structure telle que donnée dans le [template](https://github.com/mathiascouste/qgl-template).

Lorsque la commande `mvn clean package` est exécutée, un fichier jar nommé `<teamId>-0.1-SNAPSHOT.jar` doit être présent dans le répertoire `player/target`.

## Librairie externes
L'utilisation de librairies externes de manière non-maîtrisée peut entraîner des erreurs lors de l'exécution.
Si une erreur arrive suite à l'utilisation d'une librairie non incluse de base dans le template, votre livraison échoue.
Il vous est donc fortement recommandé de ne pas ajouter de librairies dans votre projet.

## Cockpit.java
Votre projet doit obligatoirement contenir une classe nommée `Cockpit` telle que donnée dans le [template](https://github.com/mathiascouste/qgl-template/blob/master/player/src/main/java/fr/unice/polytech/si3/qgl/teamid/Cockpit.java).
Cette classe doit être positionnée dans le package `fr.unice.polytech.si3.qgl.<teamid>`.

La classe Cockpit doit contenir les méthodes suivantes :
### InitGame

    public void initGame(String game);

Cette méthode sera invoquée avec en paramètre une instance de String contenant un JSON au format `initGame` (voir définition plus bas ou [exemple](./examples/initGame.json)).

### NextRound

    public String nextRound(String round);
    

Cette méthode sera invoquée avec en paramètre une instance de String contenant un JSON au format `nextRound` (voir définition plus bas ou [exemple](./examples/nextRound.json)).

Cette méthode doit retourner une instance de String contenant un JSON au format `actions` (voir définition plus bas ou [exemple](./examples/actions.json)).

Les actions retournées sont vérifiées puis exécutées. Si l'action n'est pas valide (voir [règles du jeu](./README.md) ou description JSON plus bas) alors elle sera ignorée.

### GetLogs


    public List<String> getLogs();
    
Cette méthode est invoquée à la fin de la partie.
Elle vous permets de générer vos propres logs.
Vous pouvez retourner des chaînes de caractères dans le format de votre choix.

**Seule limite :** vous ne pouvez retourner qu'un maximum de 100 Strings, et aucune ne doit dépasser les 200 caractères.

Ces logs vous seront donnés dans le rapport d'exécution de la partie.

## Interfaces JSON
### InitGame

| Propriétés | Type |
|--|--|
| goal | #RegattaGoal OU #BattleGoal |
| ship | #Bateau |
| sailors | #Marin[] |
| shipCount | integer |

### RegattaGoal

| Propriétés | Type |
|--|--|
| mode | "REGATTA" |
| checkpoints | #Checkpoint[] |

### BattleGoal

| Propriétés | Type |
|--|--|
| mode | "BATTLE" |

### Checkpoint

| Propriétés | Type |
|--|--|
| position | #Position |
| shape | #Circle OU #Rectangle OU #Polygone |

### Position

| Propriétés | Type |
|--|--|
| x | double |
| y | double |
| orientation | double |

### Circle

| Propriétés | Type |
|--|--|
| type | "circle" |
| radius | double |

### Rectangle

| Propriétés | Type |
|--|--|
| type | "rectangle" |
| width | double |
| height | double |
| orientation | double |

### Polygone

| Propriétés | Type |
|--|--|
| type | "polygon" |
| orientation | double |
| vertices | Point[] |

Les sommets du polygone sont positionnés par rapport à son centre.
Par exemple, un carré de coté 2 représenté tel un polygone aura pour sommets : [1;1], [-1;1], [-1;-1], [1;-1]

### Point

| Propriétés | Type |
|--|--|
| x | double |
| y | double |

### Bateau

| Propriétés | Type |
|--|--|
| type | "ship" |
| life | integer |
| position | #Position |
| name | string |
| deck | #Deck |
| entities | (#Rame OU #Voile OU #Gouvernail OU #Vigie OU #Canon)[] |
| shape | #Circle OU #Rectangle OU #Polygone |

### Deck

| Propriétés | Type |
|--|--|
| width | integer |
| length | integer |

### Rame

| Propriétés | Type |
|--|--|
| type | "oar" |
| x | integer |
| y | integer |

### Voile

| Propriétés | Type |
|--|--|
| type | "sail" |
| x | integer |
| y | integer |
| openned | boolean |

### Gouvernail

| Propriétés | Type |
|--|--|
| type | "rudder" |
| x | integer |
| y | integer |

### Vigie

| Propriétés | Type |
|--|--|
| type | "watch" |
| x | integer |
| y | integer |

### Canon

| Propriétés | Type |
|--|--|
| type | "canon" |
| x | integer |
| y | integer |
| loaded | boolean |
| angle | double |

### Marin

| Propriétés | Type |
|--|--|
| id | integer |
| x | integer |
| y | integer |
| name | string |

### NextRound

| Propriétés | Type |
|--|--|
| ship | #Bateau |
| wind | #Vent |
| visibleEntities | (#Courant OU #AutreBateau OU #Recif)[] |

### Vent

| Propriétés | Type |
|--|--|
| orientation | double |
| strength | double |

### Courant

| Propriétés | Type |
|--|--|
| type | "stream" |
| position | #Position |
| shape | #Circle OU #Rectangle OU #Polygone |
| strength | double |

### AutreBateau

| Propriétés | Type |
|--|--|
| type | "ship" |
| position | #Position |
| shape | #Circle OU #Rectangle OU #Polygone |
| life | integer |

### Recif

| Propriétés | Type |
|--|--|
| type | "reef" |
| position | #Position |
| shape | #Circle OU #Rectangle OU #Polygone |

### Actions

(#MOVING OU #LIFT_SAIL OU #LOWER_SAIL OU #TURN OU #OAR OU #USE_WATCH OU #AIM OU #FIRE OU #RELOAD)[]

### MOVING

| Propriétés | Type | Contraintes |
|--|--|--|
| sailorId | integer | |
| type | "MOVING" | |
| xdistance | integer | absolute(xdistance) + absolute(ydistance) <= 5 |
| ydistance | integer | absolute(xdistance) + absolute(ydistance) <= 5 |

### LIFT_SAIL

| Propriétés | Type |
|--|--|
| sailorId | integer |
| type | "LIFT_SAIL" |

### LOWER_SAIL

| Propriétés | Type |
|--|--|
| sailorId | integer |
| type | "LOWER_SAIL" |

### TURN

| Propriétés | Type | Contraintes |
|--|--|--|
| sailorId | integer | N/A |
| type | "TURN" | N/A |
| rotation | double | -PI/4 <= rotation <= PI/4 |

### OAR

| Propriétés | Type |
|--|--|
| sailorId | integer |
| type | "OAR" |

### USE_WATCH

| Propriétés | Type |
|--|--|
| sailorId | integer |
| type | "USE_WATCH" |

### AIM

| Propriétés | Type | Contraintes |
|--|--|--|
| sailorId | integer | N/A |
| type | "AIM" | N/A |
| angle | double | -PI/4 <= rotation <= PI/4 |

### FIRE

| Propriétés | Type | Contraintes |
|--|--|--|
| sailorId | integer | N/A |
| type | "FIRE" | N/A |

### RELOAD

| Propriétés | Type | Contraintes |
|--|--|--|
| sailorId | integer | N/A |
| type | "RELOAD" | N/A |

