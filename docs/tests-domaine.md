# Scénarios de test de domaine

---

### Invariant 1 : Une commande doit contenir au moins une ligne de commande

#### Scénario 1 – Happy path
Given une commande créée avec au moins une ligne de commande valide  
When le système enregistre la commande  
Then la commande est acceptée et peut continuer son cycle de vie  

#### Scénario 2 – Sad path
Given une commande sans aucune ligne de commande  
When le système tente de créer la commande  
Then le système refuse la création de la commande avec un message indiquant qu’au moins un produit est requis  

---

### Invariant 2 : Une commande payée ne peut plus être modifiée

#### Scénario 1 – Happy path
Given une commande créée mais non encore payée  
When le client modifie une ligne de commande  
Then la modification est acceptée  

#### Scénario 2 – Sad path
Given une commande avec un paiement validé  
When le client tente de modifier la commande  
Then le système refuse la modification pour garantir la cohérence avec le paiement  

---

### Invariant 3 : Le statut de la commande suit un cycle défini

#### Scénario 1 – Happy path
Given une commande avec le statut "créée"  
When le paiement est validé  
Then le statut devient "payée"  

#### Scénario 2 – Sad path
Given une commande avec le statut "créée"  
When le système tente de passer directement à "expédiée"  
Then le système refuse la transition car les étapes intermédiaires ne sont pas respectées  

---

### Invariant 4 : Une livraison est toujours associée à une commande existante

#### Scénario 1 – Happy path
Given une commande valide et préparée  
When une livraison est créée  
Then la livraison est associée à la commande existante  

#### Scénario 2 – Sad path
Given aucune commande existante  
When le système tente de créer une livraison  
Then le système refuse la création de la livraison  

---

### Invariant 5 : Un colis doit avoir un identifiant de suivi unique

#### Scénario 1 – Happy path
Given un colis avec un identifiant de suivi unique  
When le colis est enregistré dans le système  
Then le suivi est accepté et permet le tracking  

#### Scénario 2 – Sad path
Given un colis avec un identifiant de suivi déjà existant  
When le système tente d’enregistrer le colis  
Then le système refuse l’enregistrement pour éviter les conflits de suivi  

---

### Invariant 6 : Le statut de livraison suit un flux cohérent

#### Scénario 1 – Happy path
Given une livraison en statut "expédiée"  
When le colis est en cours d’acheminement  
Then le statut devient "en cours"  

#### Scénario 2 – Sad path
Given une livraison en statut "préparation"  
When le système tente de passer directement à "livrée"  
Then le système refuse la transition car le flux logique n’est pas respecté  