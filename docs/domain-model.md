# Modèle de domaine

Ce document formalise le modèle conceptuel du domaine e-commerce & livraison.
Il couvre l’ensemble des bounded contexts identifiés et décrit les principales
entités et objets valeur du système.

---

## Entités

| Entité | Description métier | Identifiant métier |
|------|--------------------|-------------------|
| Client | Le client représente une personne utilisant la plateforme e-commerce. Il consulte le catalogue, passe des commandes et suit ses livraisons. Il possède un compte et un historique d’achats. Il est au centre de l’expérience utilisateur et déclenche les principaux flux métier. | ClientId |
| Commande | La commande représente l’engagement d’achat du client. Elle regroupe des produits, possède un statut et déclenche les processus de préparation et de livraison. Elle orchestre les interactions avec les autres contextes (paiement, stock, livraison). Elle constitue la racine d’agrégat principale du système. | CommandeId |
| Livraison | La livraison correspond au processus d’acheminement d’un colis vers le client. Elle permet le suivi logistique, la gestion des incidents et la confirmation de réception. Elle joue un rôle clé dans la satisfaction client. | LivraisonId |
| Produit | Le produit représente un article disponible à la vente dans le catalogue. Il possède un prix, une description et une disponibilité. Il est utilisé dans les commandes et la gestion du stock. | ProduitId |
| Stock | Le stock représente la quantité disponible d’un produit dans un entrepôt. Il est mis à jour lors des commandes et des réapprovisionnements. Il permet d’éviter les ruptures et les surventes. | StockId |
| Entrepot | L’entrepôt est un lieu physique où sont stockés les produits et préparées les commandes. Il centralise les opérations logistiques et la gestion du stock. | EntrepotId |
| Colis | Le colis représente l’ensemble des produits emballés pour une commande. Il est utilisé dans le processus de livraison et possède un suivi. | ColisId |
| Paiement | Le paiement représente la transaction financière associée à une commande. Il valide ou refuse l’achat et influence le cycle de vie de la commande. | PaiementId |

---

## Objets Valeur

| Objet Valeur | Description métier | Propriétés principales |
|-------------|--------------------|------------------------|
| AdresseLivraison | Représente l’adresse de livraison d’une commande. Elle est définie uniquement par ses propriétés et est immuable pour garantir la traçabilité. | Rue, CodePostal, Ville, Pays, InformationsComplementaires |
| LigneCommande | Représente un produit dans une commande avec sa quantité et son prix. Elle n’existe que dans le contexte d’une commande. | Produit, Quantite, PrixUnitaire |
| StatutCommande | Représente l’état d’une commande dans son cycle de vie. Il permet de contrôler les transitions métier. | Créée, Payée, Préparée, Expédiée, Livrée, Annulée |
| StatutLivraison | Représente l’état d’une livraison. Il garantit la cohérence du flux logistique. | Préparation, Expédiée, EnCours, Livrée, Échouée |
| SuiviLivraison | Contient les informations de tracking d’un colis. Il permet de suivre l’évolution de la livraison. | Statut, Date, Localisation |
| StatutPaiement | Représente le résultat d’un paiement. Il influence directement la commande. | Validé, Refusé |
| Quantite | Représente une quantité de produit. Elle est toujours positive et utilisée dans le stock et les lignes de commande. | Valeur |
| Prix | Représente une valeur monétaire associée à un produit ou une commande. | Montant, Devise |

---

## Diagramme UML (conceptuel)

Diagramme UML conceptuel représentant les entités, objets valeur et leurs relations.

(voir diagramme1.png)

---

## Relations principales

- Un client peut avoir plusieurs commandes  
- Une commande contient plusieurs lignes de commande  
- Une ligne de commande référence un produit  
- Une commande possède une adresse de livraison  
- Une commande est associée à un paiement  
- Une commande déclenche une livraison  
- Une livraison est liée à un colis  
- Un produit est géré par un stock dans un entrepôt  
