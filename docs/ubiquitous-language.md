# Ubiquitous Language

Ce document définit le langage métier commun utilisé par les acteurs du domaine e-commerce.
Les termes sont exprimés en **PascalCase** et partagés entre les équipes métier et techniques.
Certains termes sont **contextuels** et prennent leur sens principal dans un Bounded Context donné.

---

## Glossaire métier

| Terme (PascalCase) | Définition métier | Exemple concret |
|--------------------|------------------|-----------------|
| Client | Personne qui utilise la plateforme e-commerce pour consulter des produits et passer commande. | Marie crée un compte et passe une commande. |
| CompteClient | Espace personnel du client contenant ses informations et son historique. | Paul se connecte à son CompteClient. |
| CatalogueProduit | Ensemble des produits proposés à la vente sur la plateforme. | Le CatalogueProduit affiche des smartphones. |
| Produit | Article vendu sur la plateforme avec un prix et une référence unique. | Un casque audio est un Produit. |
| TypeProduit | Catégorie permettant de regrouper des produits similaires. | Le TypeProduit “Casque” regroupe plusieurs modèles. |
| Panier | Espace temporaire contenant les produits sélectionnés avant commande. | Le Panier contient deux articles. |
| LignePanier | Élément du panier correspondant à un produit et une quantité. | Une LignePanier de 2 t-shirts. |
| Commande | Ensemble des produits validés par le client après paiement. | La Commande #1024 est créée. |
| LigneCommande | Détail d’un produit spécifique dans une commande. | Une LigneCommande pour un casque audio. |
| StatutCommande | État d’avancement d’une commande (créée, payée, préparée, expédiée, livrée, annulée). | Le StatutCommande passe à “Expédiée”. |
| Paiement | Transaction financière liée à une commande. | Le Paiement par carte est validé. |
| StatutPaiement | État du paiement (validé, refusé). | Le StatutPaiement est “Refusé”. |
| Stock | Quantité disponible d’un produit dans l’entrepôt. | Le Stock passe de 10 à 9. |
| Entrepot | Lieu physique de stockage et de préparation des commandes. | L’Entrepot de Lyon prépare les colis. |
| PreparationCommande | Processus logistique de préparation des commandes. | La PreparationCommande est lancée. |
| PreparateurCommande | Employé chargé de préparer les commandes. | Le PreparateurCommande emballe le colis. |
| Colis | Ensemble des produits emballés prêts à être expédiés. | Le Colis est remis au transporteur. |
| Livraison | Processus d’acheminement du colis vers le client. | La Livraison est en cours. |
| SuiviLivraison | Informations permettant de suivre l’état de la livraison. | Le SuiviLivraison indique “Livré”. |
| AdresseLivraison | Adresse choisie par le client pour recevoir le colis. | AdresseLivraison à Lyon. |
| RetardLivraison | Dépassement du délai de livraison prévu. | Un RetardLivraison de 24h. |
| RetourProduit | Processus de retour d’un produit par le client. | RetourProduit pour article défectueux. |
| Notification | Message envoyé suite à un événement métier. | Notification de livraison envoyée. |
| ResponsableEcommerce | Acteur supervisant les performances commerciales et logistiques. | Le ResponsableEcommerce analyse les ventes. |

---

## Termes par contexte

| Terme | Contexte principal | Commentaire (facultatif) |
|------|--------------------|--------------------------|
| Commande | ContexteCommande | Agrégat racine représentant le cycle de vie d’une commande. |
| LigneCommande | ContexteCommande | Élément interne à la commande. |
| StatutCommande | ContexteCommande | Représente l’état métier global de la commande. |
| Panier | ContexteCommande | Utilisé avant la création de la commande. |
| Paiement | ContextePaiement | Interaction avec un système de paiement externe. |
| StatutPaiement | ContextePaiement | Résultat d’une transaction financière. |
| Stock | ContexteStock | Quantité disponible et réservée des produits. |
| Entrepot | ContexteStock | Lieu physique lié à la gestion des stocks. |
| PreparationCommande | ContextePréparationCommande | Processus interne de préparation logistique. |
| PreparateurCommande | ContextePréparationCommande | Acteur opérationnel de l’entrepôt. |
| Colis | ContexteLivraison | Support physique de la livraison. |
| Livraison | ContexteLivraison | Processus d’acheminement du colis. |
| SuiviLivraison | ContexteLivraison | Informations exposées au client. |
| RetardLivraison | ContexteLivraison | Incident logistique géré par le transport. |
| RetourProduit | ContexteCommande | Extension du cycle de vie de la commande. |
| Notification | Generic | Service technique transverse déclenché par événements. |
| CatalogueProduit | Supporting | Support à la vente, non différenciant. |
| Produit | Supporting | Entité métier partagée mais secondaire. |
| ResponsableEcommerce | ContexteAnalyseEtReporting | Consommateur des indicateurs de performance. |

