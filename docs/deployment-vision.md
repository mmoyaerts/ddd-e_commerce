# Vision de déploiement

Le système est décomposé en plusieurs services, chacun correspondant à un Bounded Context.
Chaque contexte est déployé comme un microservice indépendant afin de garantir une
meilleure scalabilité et isolation.

Les principaux services conteneurisés sont :
- ContexteCommande
- ContextePaiement
- ContexteStock
- ContextePréparationCommande
- ContexteLivraison
- ContexteAnalyseEtReporting

Chaque service est encapsulé dans un conteneur Docker avec sa propre configuration.
Les services exposent des API REST pour les interactions synchrones et utilisent
un broker de messages (Kafka ou RabbitMQ) pour les communications asynchrones.

Chaque service possède ses variables d’environnement :
- URL des dépendances (DB, broker, APIs externes)
- Configuration métier (timeouts, retries)
- Credentials sécurisés

Les bases de données sont également conteneurisées et isolées par service
afin de respecter l’indépendance des bounded contexts.

L’ensemble du système est conçu pour être déployé sur une infrastructure locale via Docker.

---

## Dockerfile

```dockerfile

FROM openjdk:17

WORKDIR /app

COPY target/commande-service.jar app.jar

ENV SPRING_PROFILES_ACTIVE=prod
ENV DB_HOST=database
ENV BROKER_HOST=broker

EXPOSE 8080

CMD ["java", "-jar", "app.jar"]
```