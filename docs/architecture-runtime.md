
# Architecture runtime

Le système est composé de plusieurs services interconnectés exécutés via Docker Compose.

Les principaux services sont :
- api-commande : expose les endpoints REST du ContexteCommande
- db-commande : base de données associée au ContexteCommande
- broker : système de messaging (Kafka ou RabbitMQ)

Les services communiquent entre eux :
- via REST pour les appels synchrones (ex : paiement)
- via le broker pour les événements métier (ex : CommandeConfirmée)

Chaque service est isolé dans un conteneur mais partage un réseau Docker commun.


## Exemple docker-compose

```yaml
version: '3.8'

services:

  api-commande:
    image: commande-service
    ports:
      - "8080:8080"
    environment:
      - DB_HOST=db-commande
      - BROKER_HOST=broker
    depends_on:
      - db-commande
      - broker

  db-commande:
    image: postgres
    environment:
      - POSTGRES_DB=ecommerce
      - POSTGRES_USER=me
      - POSTGRES_PASSWORD=test

  broker:
    image: rabbitmq
    ports:
      - "5672:5672"