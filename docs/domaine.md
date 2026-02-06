# Scénario choisi

**E-commerce & livraison**

# Contexte métier

Le secteur du e-commerce connaît une croissance rapide (+36% en 2022 => https://www.francenum.gouv.fr/magazine-du-numerique/e-commerce-en-france-chiffres-cles-et-5-tendances-fortes), avec des entreprises qui doivent gérer des volumes importants de commandes, des stocks en temps réel et des livraisons rapides. Les plateformes e-commerce proposent une large variété de produits et doivent assurer une expérience utilisateur fluide depuis la navigation jusqu’à la réception du colis.

L’organisation type comprend plusieurs équipes : direction commerciale, gestion logistique, préparation de commandes et service client. Les enjeux principaux sont la satisfaction client, la réduction des délais de livraison, la gestion efficace des stocks et la coordination entre les différents acteurs de la chaîne logistique.

Le système doit également répondre à des contraintes fortes : gestion des pics de commandes, suivi précis des livraisons, intégration avec des transporteurs externes, sécurité des paiements et traçabilité complète des opérations.

La satisfaction utilisateur est une priorité car les avis pèsent énormément sur l'image de l'entreprise et la confiance des potentiels clients.

# Rôles utilisateurs

| Rôle                    | Type         | Description                                                                                                                                                             |
|-------------------------|--------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Responsable e-commerce  | Direction    | Supervise l’activité commerciale, analyse les ventes et optimise les performances. Il définit les stratégies de livraison, de prix et de satisfaction client.           |
| Préparateur de commande | Opérationnel | Travaille dans l’entrepôt et prépare les commandes clients. Il vérifie les produits, emballe les colis et les transmet au transporteur.                               |
| Transporteur            | Opérationnel  | Assure l’acheminement des colis depuis l’entrepôt jusqu’au client final. Il gère la livraison, le suivi des colis, les délais et le traitement des incidents éventuels. |
| Client                  | Client final | Navigue sur la plateforme, passe commande et choisit un mode de livraison. Il suit son colis et peut contacter le support en cas de problème.                         |



# Problématiques métier

* Gestion fiable des stocks en temps réel pour éviter les ruptures ou surventes.
* Optimisation du processus de préparation et d’expédition des commandes.
* Suivi précis des livraisons et gestion des retards ou incidents.
* Coordination entre plateforme e-commerce, entrepôt et transporteurs.
* Amélioration de l’expérience client (commande, paiement, livraison, retours).

# Scénario fil rouge

Un client se connecte à la plateforme e-commerce et parcourt le catalogue de produits. Il ajoute plusieurs articles à son panier et vérifie leur disponibilité en stock. Une fois son panier validé, il choisit une adresse de livraison et un mode de transport (standard ou express), puis procède au paiement sécurisé.

Le système enregistre la commande et envoie une notification à l’entrepôt. Un préparateur de commande reçoit la liste des articles à préparer. Il récupère les produits dans le stock, vérifie leur conformité et les emballe dans un colis. Une fois la commande prête, elle est marquée comme “expédiée” et transmise au transporteur.

Le transporteur prend en charge le colis et met à jour les informations de suivi. Le client reçoit une notification avec un lien de tracking pour suivre la livraison en temps réel. En cas d’absence lors de la livraison, un second passage ou un dépôt en point relais est proposé.

Une fois le colis livré, le système confirme la réception et invite le client à évaluer son expérience. Les données de vente et de livraison sont ensuite analysées par le responsable e-commerce pour améliorer les performances logistiques et commerciales.
