# Scénarios d’intégration

## Scénario : De la commande client à la livraison

Un client se connecte à la plateforme e-commerce via une application web et consulte le catalogue de produits. Il ajoute plusieurs articles à son panier puis valide sa commande. Une requête est envoyée au système qui déclenche la création d’une Commande dans le ContexteCommande.

Le système vérifie la disponibilité des produits en interrogeant le ContexteStock. Si les produits sont disponibles, ils sont réservés et la commande est confirmée. Ensuite, le ContexteCommande appelle le ContextePaiement via une API REST pour effectuer la transaction.

Une fois le paiement validé, un événement *CommandePayée* est émis. Cet événement déclenche le processus de préparation dans le ContextePréparationCommande. Un préparateur récupère les produits, les emballe et marque la commande comme préparée.

À la fin de la préparation, un événement *CommandePréparée* est publié. Le ContexteLivraison reçoit cet événement et crée une Livraison associée à la commande. Un colis est généré avec un numéro de suivi unique et transmis au transporteur.

Le transporteur met à jour le statut de la livraison (expédiée, en cours, livrée) via des événements ou des appels API. Ces informations sont propagées au ContexteCommande pour maintenir la cohérence globale du statut.

Le client reçoit des notifications à chaque étape clé (commande validée, expédition, livraison). Une fois la livraison effectuée, la commande est marquée comme livrée. Les événements sont également envoyés au ContexteAnalyseEtReporting pour alimenter les tableaux de bord.

---

# Schéma de séquence

(voir diagramme_test.png)
