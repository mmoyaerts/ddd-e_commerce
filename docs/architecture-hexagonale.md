# Architecture hexagonale

L’architecture hexagonale permet de séparer clairement le cœur métier
des aspects techniques. Elle repose sur une organisation en trois couches :
Domain, Application et Adapters, afin de garantir un système évolutif,
maintenable et centré sur le métier.

---

## Description des couches

### Domain

La couche Domain contient le cœur métier du système. Elle regroupe les entités,
objets valeur, agrégats et invariants définis dans le modèle de domaine.
Elle exprime les règles métier en utilisant le langage ubiquitaire et ne dépend
d’aucune technologie. Cette couche est stable et évolue uniquement en fonction
des besoins métier. Dans notre cas, elle inclut des concepts comme Commande,
Livraison, LigneCommande, StatutCommande et leurs invariants associés.

---

### Application

La couche Application orchestre les cas d’usage du système. Elle ne contient pas
de logique métier complexe mais coordonne les interactions entre les entités du
domaine. Elle appelle les agrégats, applique des règles transverses et définit
les ports (interfaces) pour interagir avec l’extérieur. Par exemple, un service
applicatif peut gérer le cas d’usage "PasserCommande" en validant le panier,
déclenchant le paiement et initiant la préparation.

---

### Adapters

La couche Adapters correspond à l’infrastructure et aux interfaces externes.
Elle implémente les ports définis dans la couche Application. Cela inclut les
API REST, les bases de données, les systèmes de paiement ou encore les systèmes
de messagerie. Cette couche traduit les requêtes techniques en actions métier
et inversement. Par exemple, un adapter REST transforme une requête HTTP en
commande métier, et un adapter de persistence sauvegarde une commande en base.

---

## Exemple de flux (commande → réponse)

Un client envoie une requête HTTP pour passer une commande via un endpoint REST.
L’adapter REST reçoit la requête et la transforme en commande métier exploitable
par la couche Application. Le service applicatif "PasserCommande" est appelé et
orchestre les étapes nécessaires : validation du panier, création de la commande
et vérification des invariants métier.

Le service applicatif appelle ensuite le domaine pour créer l’agrégat Commande,
qui applique ses règles métier (ex : au moins une ligne de commande, statut
cohérent). Une fois la commande validée, le service applicatif utilise un port
de repository pour demander sa sauvegarde.

Ce port est implémenté par un adapter de persistence (ex : base de données).
Après confirmation de la sauvegarde, le résultat remonte jusqu’à l’adapter REST,
qui construit une réponse JSON contenant l’identifiant de la commande et son
statut. Cette réponse est ensuite renvoyée au client.

---

## Schéma

      [ Client / UI ]
             |
             v
    ---------------------
    |   REST Adapter    |
    ---------------------
             |
             v
    ---------------------
    |   Application     |
    | (Use Cases)       |
    ---------------------
             |
    ---------------------
    |      Domain       |
    | (Commande, etc.)  |
    ---------------------
             |
    ---------------------
    |   Repository Port |
    ---------------------
             |
    ---------------------
    |  DB Adapter       |
    ---------------------