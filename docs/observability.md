# Observabilité

## Correlation ID

Le Correlation ID est un identifiant unique généré pour chaque requête entrante dans le système.  
Il est créé au niveau de l’adapter REST dès la réception d’une requête client (par exemple lors d’un POST /commandes).  
Cet identifiant est ensuite propagé à travers toutes les couches : Application, Domaine, et Adapters (persistence, paiement, messaging).  
Il est également transmis dans les appels externes (ex : API de paiement) et inclus dans les événements publiés sur le broker.  
Le Correlation ID permet de tracer une requête de bout en bout, facilitant le debugging, le monitoring et l’analyse des incidents.

---

## Métriques métier

| Nom de la métrique | Description | Type |
|-------------------|------------|------|
| commandes_creees_total | Nombre total de Commandes créées dans le système | counter |
| commandes_annulees_total | Nombre total de Commandes annulées (ex : paiement refusé) | counter |
| delai_livraison_moyen | Temps moyen entre la création d’une Commande et sa Livraison | histogram |

---

## Logging structuré

Exemple de log JSON :

```json
{
  "timestamp": "2026-04-27T10:15:30Z",
  "level": "INFO",
  "correlationId": "corr-123456",
  "service": "ContexteCommande",
  "action": "PasserCommande",
  "commandeId": "CMD-1234",
  "statutCommande": "CREEE",
  "message": "Commande créée avec succès"
}