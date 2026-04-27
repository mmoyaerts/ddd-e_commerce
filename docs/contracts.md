# Contrats d’échange entre contexts

---

## Contrat 1 : Commande → Paiement

* **Context source :** ContexteCommande
* **Context cible :** ContextePaiement
* **Type d’échange :** REST (API externe avec ACL)

```json
{
  "commandeId": "CMD-12345",
  "montant": 99.99,
  "devise": "EUR",
  "moyenPaiement": "CARTE_BANCAIRE"
}
```

---

## Contrat 2 : Commande → Stock

* **Context source :** ContexteCommande
* **Context cible :** ContexteStock
* **Type d’échange :** Event (asynchrone)

```json
{
  "eventType": "CommandeValidee",
  "data": {
    "commandeId": "CMD-12345",
    "lignes": [
      {
        "produitId": "PROD-1",
        "quantite": 2
      }
    ]
  }
}
```

---

## Contrat 3 : Stock → Commande

* **Context source :** ContexteStock
* **Context cible :** ContexteCommande
* **Type d’échange :** Event

```json
{
  "eventType": "StockReserve",
  "data": {
    "commandeId": "CMD-12345",
    "statut": "OK"
  }
}
```

---

## Contrat 4 : Commande → PréparationCommande

* **Context source :** ContexteCommande
* **Context cible :** ContextePréparationCommande
* **Type d’échange :** Event

```json
{
  "eventType": "CommandePayee",
  "data": {
    "commandeId": "CMD-12345",
    "lignes": [
      {
        "produitId": "PROD-1",
        "quantite": 2
      }
    ],
    "adresseLivraison": {
      "rue": "10 rue de Paris",
      "ville": "Lyon",
      "codePostal": "69000",
      "pays": "France"
    }
  }
}
```

---

## Contrat 5 : PréparationCommande → Livraison

* **Context source :** ContextePréparationCommande
* **Context cible :** ContexteLivraison
* **Type d’échange :** REST (API interne)

```json
{
  "commandeId": "CMD-12345",
  "colis": {
    "poids": 2.5,
    "dimensions": {
      "longueur": 30,
      "largeur": 20,
      "hauteur": 10
    }
  },
  "adresseLivraison": {
    "rue": "10 rue de Paris",
    "ville": "Lyon",
    "codePostal": "69000",
    "pays": "France"
  }
}
```

---

## Contrat 6 : Livraison → AnalyseEtReporting

* **Context source :** ContexteLivraison
* **Context cible :** ContexteAnalyseEtReporting
* **Type d’échange :** Event

```json
{
  "eventType": "CommandeLivree",
  "data": {
    "commandeId": "CMD-12345",
    "livraisonId": "LIV-45678",
    "dateLivraison": "2026-04-29",
    "delaiHeures": 48
  }
}
```
