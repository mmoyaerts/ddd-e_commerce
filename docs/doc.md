 Modèle de domaine

Ce document formalise le modèle conceptuel du domaine e-commerce & livraison.
Il présente les principales entités, les objets valeur et leurs relations,
sans considération technique ou d’implémentation.

---

## Entités

| Entité | Description métier | Identifiant métier |
|------|--------------------|-------------------|
| Client | Le client représente une personne utilisant la plateforme e-commerce pour consulter le catalogue, passer des commandes et suivre ses livraisons. Il possède des informations personnelles et peut avoir plusieurs commandes associées. Le client est au centre de l’expérience utilisateur et interagit avec le système tout au long du cycle de vente. | ClientId |
| Commande | La commande représente l’engagement d’achat du client après validation du panier et du paiement. Elle regroupe un ensemble de produits, possède un état métier et déclenche les processus de préparation et de livraison. Elle constitue un élément central du cœur métier. | CommandeId |
| Livraison | La livraison correspond au processus d’acheminement d’un colis vers le client final. Elle permet de suivre l’état de transport, de gérer les incidents et de confirmer la réception. Elle est directement liée à la satisfaction client. | LivraisonId |

---

## Objets Valeur

| Objet Valeur | Description métier | Propriétés principales |
|-------------|--------------------|------------------------|
| AdresseLivraison | Représente l’adresse physique à laquelle une commande doit être livrée. Elle est définie uniquement par ses valeurs et ne possède pas d’identité propre. Toute modification entraîne la création d’une nouvelle instance. | Rue, CodePostal, Ville, Pays, InformationsComplementaires |
| LigneCommande | Représente un produit commandé avec sa quantité et son prix au moment de l’achat. Elle n’existe que dans le contexte d’une commande et n’a pas de cycle de vie propre. | Produit, Quantite, PrixUnitaire |
| StatutCommande | Représente l’état métier d’une commande à un instant donné. Il permet de contrôler les transitions autorisées du cycle de vie de la commande. | Valeur (Créée, Payée, Préparée, Expédiée, Livrée, Annulée) |

---

## Diagramme UML (conceptuel)

Diagramme UML conceptuel représentant les entités, objets valeur et leurs relations.

(voir diagramme.png)


Commande utilise StatutCommande (objet valeur)


Relations :
- Un client peut avoir plusieurs commandes
- Une commande appartient à un seul client
- Une commande est composée de plusieurs lignes de commande
- Une commande possède une adresse de livraison
- Une commande est associée à une livraison
- Le statut de commande est un objet valeur utilisé par la commande