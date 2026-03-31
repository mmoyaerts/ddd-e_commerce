# Agrégats et invariants

## Agrégat : Commande

**Racine de l’agrégat :** Commande

**Entités / Objets de Valeur internes :**

* LigneCommande
* AdresseLivraison (VO)
* StatutCommande (VO)
* Paiement (référence externe)

### Invariants

| Invariant                                                 | Description métier                                                                                                                                                                                                                                     | Conséquence si non respecté                                    |
| --------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------- |
| Une commande doit contenir au moins une ligne de commande | Une commande ne peut exister sans produit associé. Chaque commande doit contenir au minimum une LigneCommande valide avec un produit et une quantité. Cela garantit que la commande représente une intention d’achat réelle.                                        | Commandes vides, incohérences métier, erreurs de facturation.  |
| Une commande payée ne peut plus être modifiée             | Une fois le paiement validé, la commande devient immuable pour garantir la cohérence avec la transaction financière. Toute modification nécessiterait un remboursement ou une nouvelle commande. Cela protège l’intégrité financière et contractuelle.              | Risque de fraude, incohérence entre paiement et contenu livré. |
| Le statut de la commande suit un cycle défini             | Une commande doit respecter un enchaînement logique de statuts (créée → payée → préparée → expédiée → livrée). Il est interdit de sauter des étapes ou de revenir en arrière sans processus spécifique (ex : retour). Cela garantit la traçabilité du cycle de vie. | Perte de traçabilité, erreurs de suivi, confusion client.      |

---

## Agrégat : Livraison

**Racine de l’agrégat :** Livraison

**Entités / Objets de Valeur internes :**

* Colis
* AdresseLivraison (VO)
* SuiviLivraison (VO)
* StatutLivraison (VO)
* Transporteur (référence externe)

### Invariants

| Invariant                                                    | Description métier                                                                                                                                                                                                              | Conséquence si non respecté                                                      |
| ------------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------- |
| Une livraison est toujours associée à une commande existante | Une livraison ne peut être créée que pour une commande valide et préparée. Cela garantit que chaque expédition correspond à une transaction réelle. La cohérence entre commande et livraison est essentielle pour le suivi.                  | Colis orphelins, erreurs logistiques, perte de traçabilité.                      |
| Un colis doit avoir un identifiant de suivi unique           | Chaque colis expédié doit être identifiable de manière unique via un numéro de suivi. Cela permet au client et au système de suivre son évolution en temps réel. L’unicité est essentielle pour éviter toute confusion.                      | Impossible de suivre les colis, erreurs de livraison, support client inefficace. |
| Le statut de livraison suit un flux cohérent                 | Une livraison doit suivre un cycle logique (en préparation → expédiée → en cours → livrée ou échouée). Les transitions doivent être contrôlées et cohérentes avec les événements réels. Cela garantit une information fiable pour le client. | Informations erronées, mauvaise expérience client, perte de confiance.           |

---

## Schéma UML des agrégats

```
+---------------------+
|      Commande       |  <-- Racine
+---------------------+
| idCommande          |
| statut              |
| adresseLivraison    |
+----------+----------+
           |
           | contient
           v
   +-------------------+
   | LigneCommande     |
   +-------------------+
   | produit           |
   | quantité          |
   | prix              |
   +-------------------+

-----------------------------------------

+---------------------+
|     Livraison       |  <-- Racine
+---------------------+
| idLivraison         |
| statut              |
| suivi               |
+----------+----------+
           |
           | contient
           v
      +-----------+
      |  Colis    |
      +-----------+
      | tracking  |
      +-----------+
```
