# Context Map

## Relations et patterns

| Contexte source             | Contexte cible              | Pattern de relation  | Justification                                                                                                                                                                                                                                                                                                                                                      |
| --------------------------- | --------------------------- | -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| ContexteCommande            | ContextePaiement            | Anticorruption Layer | Le paiement est un service externe ou générique qui ne doit pas impacter le modèle métier central. Une ACL protège le Core Domain en traduisant les concepts de paiement (transaction, statut) vers le langage métier interne (Commande payée, refusée). Cela évite le couplage direct avec un fournisseur externe et permet de changer facilement de prestataire. |
| ContexteCommande            | ContexteStock               | Partnership          | Les deux contextes collaborent étroitement pour vérifier et réserver les stocks lors de la validation d’une commande. Les règles métier sont partagées et doivent rester cohérentes. Un partenariat fort est nécessaire car toute incohérence entre stock et commande impacte directement l’expérience client.                                                     |
| ContexteCommande            | ContextePréparationCommande | Customer/Supplier    | Le ContexteCommande fournit les informations nécessaires à la préparation (articles, quantités, priorité, adresse). Le ContextePréparationCommande dépend de ces données pour exécuter son travail opérationnel. Commande agit donc comme fournisseur et préparation comme client.                                                                                 |
| ContextePréparationCommande | ContexteLivraison           | Customer/Supplier    | Une fois la commande préparée et emballée, les informations du colis sont transmises au ContexteLivraison. Ce dernier dépend entièrement des données fournies pour organiser l’expédition. La relation est clairement orientée fournisseur vers client.                                                                                                            |
| ContexteCommande            | ContexteLivraison           | Partnership          | Le suivi global d’une commande dépend directement de son état de livraison. Les deux contextes doivent partager des informations fiables et synchronisées pour garantir la traçabilité. Une collaboration étroite est nécessaire pour maintenir la cohérence des statuts.                                                                                          |
| ContexteCommande            | ContexteAnalyseEtReporting  | Conformist           | Le ContexteAnalyseEtReporting consomme les données produites par les autres contextes sans influencer leur modèle. Il s’adapte aux structures existantes et exploite les événements métier. Ce contexte se conforme donc au modèle du Core Domain.                                                                                                                 |
| ContexteLivraison           | ContexteAnalyseEtReporting  | Conformist           | Les données de livraison (délais, incidents, statuts) sont exploitées pour produire des indicateurs. Le reporting n’impose aucune règle métier aux autres contextes et s’adapte à leurs structures. Il agit comme consommateur conforme.                                                                                                                           |

## Intégrations techniques envisagées

### 1. API REST — Commande / Paiement

* **Type :** API REST externe avec Anticorruption Layer
* **Bounded Contexts :** ContexteCommande, ContextePaiement
* **Cas d’usage :** Lorsqu’un client valide une commande, le ContexteCommande appelle le service de paiement externe. L’ACL traduit la réponse du fournisseur en événements métier internes comme *CommandePayée* ou *PaiementRefusé*. Cela protège le cœur métier contre les changements de fournisseur.

### 2. Événements métier via broker (Kafka/RabbitMQ)

* **Type :** Architecture événementielle (event-driven)
* **Bounded Contexts :** Commande, Stock, PréparationCommande, Livraison, Reporting
* **Cas d’usage :** Publication d’événements tels que *CommandeValidée*, *StockRéservé*, *CommandePréparée*, *ColisExpédié*, *CommandeLivrée*. Chaque contexte réagit de manière asynchrone, ce qui réduit le couplage et améliore la scalabilité du système.

### 3. API interne — Préparation / Livraison

* **Type :** API REST interne
* **Bounded Contexts :** ContextePréparationCommande, ContexteLivraison
* **Cas d’usage :** Une fois la commande préparée, le ContextePréparationCommande appelle le ContexteLivraison pour créer l’expédition. Les informations transmises incluent le poids, l’adresse, le transporteur et les dimensions. Le contexte livraison retourne un numéro de suivi.

### 4. Event Streaming vers Reporting

* **Type :** Event streaming / pipeline de données
* **Bounded Contexts :** Tous les Core BCs → ContexteAnalyseEtReporting
* **Cas d’usage :** Le reporting reçoit l’ensemble des événements métier pour construire des tableaux de bord en temps réel. Les indicateurs incluent le nombre de commandes, les délais moyens de livraison, les retards et les incidents. Cela permet à la direction de piloter l’activité sans impacter les systèmes opérationnels.
