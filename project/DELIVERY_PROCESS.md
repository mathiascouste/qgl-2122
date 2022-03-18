
# Consignes de livraison

*Ce document est soumis à évolution*

## Liste des livraisons
### Constitution des équipes
Remplissez le fichier `README.md` avec les informations suivantes :
 - une image représentant le drapeau de votre équipage (chargé depuis un fichier `flag.png` présent à la racine de votre projet)
 - liste des membres de votre équipe
 - le nom de votre équipe
 - le nom de votre bateau

Créez un tag `CREW` contenant le README avant le 20 Janvier 2022 - 18h.

### Livraisons hebdomadaires

Des livraisons auront lieu chaque semaine (hors pauses pédagogiques).
Les détails de ces livraisons sont donnés plus bas.

### Livraison finale
Cette livraison permettra d'évaluer entre autres la qualité de votre code, l'application des principes étudiés en QGL.

Créez un tag `FINAL` contenant la dernière version de votre projet avant le *21 Mai 2022*. (Ce rendu est commun avec le rapport)

### Rapport final
Vous devrez rédiger un rapport résumant vos choix techniques ainsi que l'application des principes vus en cours.

*Les consignes de rédaction du rapport sont présentes dans [ce répertoire](../report).*

Créez un tag `FINAL` contenant votre rapport avant le *21 Mai 2022*. (Ce rendu est commun avec la livraison finale)

## Étapes de chaque livraison hebdomadaire
### Annonce de la livraison
Au moins une semaine avant la date de livraison, seront données les informations suivantes :

 - Nom du tag de livraison.
 - Indication du mode de jeu de la prochaine partie.
 - Des indices sur les caractéristiques de la mer pour la prochaine partie.

### Livraison par les équipes

Créez un tag contenant la version de votre projet à exécuter.

### Exécution du script de récupération
Le code doit être compilable, testable et exécutable avec le script suivant (considérant que le fichier pom.xml doit être à la racine de votre repository) :

    git clone <url_de_votre_repertoire> <id_projet>
    cd <id_projet>
    git checkout <TAG>
    mvn clean package

Votre projet doit respecter les [spécifications techniques](./TECHNICAL_SPECS.md).

### Compte rendu de partie

Les résultats de chaque partie seront disponibles dans [ce répertoire](../championship).
Un sous-répertoire sera créé pour l'épreuve qui vient de se dérouler et contiendra :
 - le résultat de l'épreuve du jour
 - des sous-répertoires pour chaque équipe avec des informations sur les raisons de leur succès/échec ainsi que des logs de la partie.


### Comment rater sa livraison hebdomadaire

 1. Absence du tag de livraison.
 2. Retard du tag de livraison.
 3. Le projet et tests ne compilent pas.
 4. Les tests échouent.
 5. Les [spécifications techniques](./TECHNICAL_SPECS.md) ne sont pas respectées.

