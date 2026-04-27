# Scénario complet inter-contextes

## Description narrative détaillée

Un client se connecte à la plateforme e-commerce et ajoute plusieurs produits à son panier. Une fois son panier validé, il confirme sa commande. Le ContexteCommande crée alors une nouvelle Commande avec un statut initial "créée" et vérifie la cohérence des données (lignes de commande, adresse de livraison).

Le ContexteCommande émet ensuite un événement *CommandeValidee* afin de réserver les produits. Le ContexteStock reçoit cet événement et vérifie la disponibilité des articles. Si le stock est suffisant, il réserve les quantités nécessaires et publie un événement *StockReserve*.

À réception de cet événement, le ContexteCommande initie le paiement en appelant le ContextePaiement via une API REST. Si le paiement est accepté, le statut de la commande passe à "payée" et un événement *CommandePayee* est émis.

Le ContextePréparationCommande consomme cet événement et démarre la préparation. Un opérateur collecte les produits, les emballe et marque la commande comme prête. Une fois terminé, un événement *CommandePreparee* est publié.

Le ContexteLivraison reçoit cet événement et crée une livraison associée. Un colis est généré avec un identifiant de suivi unique et confié à un transporteur. Le statut évolue progressivement (expédiée, en cours de livraison).

Une fois la livraison effectuée, le ContexteLivraison publie un événement *CommandeLivree*. Le ContexteCommande met alors à jour le statut final de la commande à "livrée". En parallèle, le ContexteAnalyseEtReporting consomme les événements pour mettre à jour les indicateurs de performance.

Le client reçoit des notifications à chaque étape clé du processus, garantissant une expérience fluide et transparente.

---

## Liste des événements déclenchés (dans l’ordre)

1. CommandeValidee
2. StockReserve
3. CommandePayee
4. CommandePreparee
5. CommandeExpediee
6. CommandeLivree

---

## Rappel des invariants concernés

### Agrégat Commande

* Une commande doit contenir au moins une ligne de commande valide (vérifié à la création).
* Une commande payée ne peut plus être modifiée (après *CommandePayee*).
* Le statut de la commande suit un cycle strict (créée → payée → préparée → expédiée → livrée).

### Agrégat Livraison

* Une livraison est toujours associée à une commande existante (création après *CommandePreparee*).
* Un colis possède un identifiant de suivi unique (généré à la création de la livraison).
* Le statut de livraison suit un flux cohérent (expédiée → en cours → livrée).
