# Entités et Objets Valeur – Vue conceptuelle

Ce document présente une vue conceptuelle de certains éléments clés du domaine
e-commerce & livraison, sans considération technique ou d’implémentation.

---

## Entités

### 1. Commande

La commande représente l’engagement du client d’acheter un ensemble de produits.
Elle constitue un élément central du métier.

#### Attributs

| Attribut | Type métier | Description |
|--------|-------------|-------------|
| IdentifiantCommande | Identifiant | Identifie de manière unique une commande dans le système. |
| LignesCommande | Collection de LigneCommande | Liste des produits commandés avec leur quantité et leur prix. |
| StatutCommande | Statut | Représente l’état métier de la commande (créée, payée, préparée, expédiée, livrée, annulée). |
| DateCreation | Date | Date à laquelle la commande est créée par le client. |
| AdresseLivraison | AdresseLivraison | Adresse choisie par le client pour la livraison de la commande. |
| MontantTotal | Montant | Somme totale à payer pour la commande, calculée à partir des lignes. |

#### Invariants métier

- Une commande ne peut pas être expédiée tant que le paiement n’est pas validé.
- Une commande livrée ne peut plus être annulée.
- Le montant total doit toujours être supérieur à zéro.

---

### 2. Livraison

La livraison représente le processus d’acheminement d’un ou plusieurs colis vers le client final.
Elle est directement liée à l’expérience client.

#### Attributs

| Attribut | Type métier | Description |
|--------|-------------|-------------|
| IdentifiantLivraison | Identifiant | Identifie de manière unique une livraison. |
| Colis | Colis | Colis physique associé à la livraison. |
| StatutLivraison | Statut | État de la livraison (en préparation, en cours, livrée, échouée). |
| Transporteur | Transporteur | Acteur responsable de l’acheminement du colis. |
| DateExpedition | Date | Date à laquelle le colis est remis au transporteur. |
| DateLivraisonPrevue | Date | Date estimée de livraison communiquée au client. |

#### Invariants métier

- Une livraison ne peut concerner qu’un seul client.
- Une livraison livrée ne peut pas repasser à l’état “en cours”.

---

## Objet Valeur

### AdresseLivraison

L’adresse de livraison représente l’emplacement physique où un colis doit être livré.
Elle ne possède pas d’identité propre et est définie uniquement par ses valeurs.

#### Propriétés

- Nom du client
- Rue
- CodePostal
- Ville
- Pays


#### Immuabilité

L’adresse de livraison est un objet valeur immuable car une fois associée à une commande,
elle ne doit plus être modifiée afin de garantir la traçabilité des livraisons.
Toute modification d’adresse entraîne la création d’une nouvelle instance,
ce qui évite les effets de bord et simplifie les règles métier.

