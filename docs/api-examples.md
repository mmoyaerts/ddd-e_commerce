# Exemples d’API REST

## Endpoint 1

Méthode : POST  
URL : /commandes  

Description  
Permet de créer une Commande à partir d’un Panier validé.  
Ce endpoint correspond au cas d’usage PasserCommande du ContexteCommande.

Exemple de requête (JSON)

```json
{
  "clientId": "CL-001",
  "lignesCommande": [
    {
      "produitId": "PRD-100",
      "quantite": 2,
      "prixUnitaire": 49.99
    },
    {
      "produitId": "PRD-200",
      "quantite": 1,
      "prixUnitaire": 19.99
    }
  ],
  "adresseLivraison": {
    "rue": "10 rue de Paris",
    "codePostal": "75001",
    "ville": "Paris",
    "pays": "France"
  }
}
```

## Endpoint 2

Méthode : GET  
URL : /commandes/{commandeId}  

Description  
Permet de consulter une Commande ainsi que son état et les informations de Livraison associées.

Exemple de requête (JSON)

```json
{}

{
  "commandeId": "CMD-1234",
  "statutCommande": "EXPEDIEE",
  "montantTotal": 119.97,
  "adresseLivraison": {
    "rue": "10 rue de Paris",
    "codePostal": "75001",
    "ville": "Paris",
    "pays": "France"
  },
  "livraison": {
    "livraisonId": "LIV-789",
    "statutLivraison": "EN_COURS",
    "suiviLivraison": {
      "statut": "En cours de livraison",
      "date": "2026-03-31",
      "localisation": "Centre de tri Paris"
    }
  }
}

```