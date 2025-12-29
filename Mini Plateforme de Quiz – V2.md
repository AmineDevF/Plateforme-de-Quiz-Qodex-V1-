# Mini Plateforme de Quiz – V2  
## Student Interface (Main Objective)

---

## 1. Contexte Général (V2 – Étudiant Centré)

Dans cette version **V2**, l’interface **Étudiant** devient l’objectif principal du projet.

Les étudiants n’ont **aucun droit de création**.  
Ils interagissent uniquement avec des quiz **déjà créés par les enseignants dans la V1**.

L’étudiant peut :

- Explorer les catégories disponibles  
- Consulter les quiz actifs  
- Passer un quiz  
- Soumettre ses réponses de manière sécurisée  
- Voir son score immédiatement  
- Consulter son historique personnel  

Toute l’application repose sur :

- La Programmation Orientée Objet (OOP)  
- Une architecture claire et maintenable  
- Des règles de sécurité applicative strictes  

---


---

## 3. Modèle Objet – Classes OOP (Focus Étudiant)

### User (Étudiant uniquement)

Représente un utilisateur étudiant authentifié.

- id  
- nom  
- email  
- password_hash  
- role (valeur fixe : `student`)  
- created_at  

---

### Category

Représente une catégorie de quiz.

- id  
- nom  
- description  

Lecture seule pour l’étudiant.

---

### Quiz

Représente un quiz disponible.

- id  
- titre  
- description  
- categorie_id  
- is_active  
- created_at  

Seuls les quiz actifs sont visibles.

---

### Question

Représente une question d’un quiz.

- id  
- quiz_id  
- question  
- option1  
- option2  
- option3  
- option4  
- correct_option  

La bonne réponse n’est jamais exposée côté client.

---

### Result

Représente le résultat final d’un quiz.

- id  
- quiz_id  
- student_id  
- score  
- total_questions  
- completed_at  

Les résultats sont en lecture seule.

---

### Attempt (bonus)

Représente une tentative de passage de quiz.

- id  
- quiz_id  
- student_id  
- started_at  
- completed_at  
- is_finished  

Cette classe permet de :

- Limiter le nombre de tentatives  
- Bloquer la re-soumission  
- Suivre l’état du quiz (en cours / terminé)

---

## 4. Fonctionnalités Étudiant (V2)



### Navigation Étudiant

- Accéder au dashboard  
- Voir toutes les catégories  
- Voir les quiz par catégorie  
- Démarrer un quiz  
- Répondre aux questions  
- Soumettre le quiz  
- Voir le score  
- Consulter l’historique personnel  

---

### Passage de Quiz

- Une seule tentative par quiz (ou configurable)  
- Toutes les questions doivent être répondues  
- Calcul du score côté serveur  
- Résultat non modifiable  
- Tentative automatiquement clôturée après soumission  

---

## 5. Sécurité (Obligatoire)

Toutes les fonctionnalités doivent respecter les règles suivantes :

- Protection CSRF sur tous les formulaires  
- Utilisation exclusive de requêtes préparées (PDO)  
- Validation et sanitization des entrées utilisateur  
- Protection contre :  
  - SQL Injection  
  - XSS  
  - Session Hijacking  
  - Accès non autorisé aux données  
- Vérification systématique du rôle étudiant :  
  - `Security::checkStudent();`  
- Interdiction totale d’accès aux résultats d’autres étudiants  

---

## 6. User Stories – Étudiant (V2)

### US1 – Voir les catégories

En tant qu’étudiant, je veux voir la liste des catégories afin de choisir un quiz.

Contraintes de sécurité :

- Session valide  
- Affichage uniquement des quiz actifs  

---

### US2 – Voir les quiz d’une catégorie

En tant qu’étudiant, je veux voir les quiz disponibles pour une catégorie donnée.

Contraintes de sécurité :

- Vérification de l’ID  
- Quiz actif uniquement  

---

### US3 – Passer un quiz

En tant qu’étudiant, je veux répondre aux questions d’un quiz et soumettre mes réponses.

Contraintes de sécurité :

- Token CSRF valide  
- Quiz actif  
- Validation de toutes les réponses  
- Une seule tentative active  

---

### US4 – Voir mon score

En tant qu’étudiant, je veux voir mon score immédiatement après la soumission.

Contraintes de sécurité :

- Résultat lié à mon `user_id`  
- Aucune modification possible  

---

### US5 – Historique personnel

En tant qu’étudiant, je veux consulter l’historique de mes quiz passés.

Contraintes de sécurité :

- Accès strictement personnel  
- Aucune donnée d’autres étudiants  

---

## 7. Objectifs Pédagogiques (Durée : 1 semaine)

- Appliquer la programmation orientée objet en PHP  
- Comprendre la séparation entre :  
  - logique métier  
  - sécurité  
  - affichage  
- Manipuler :  
  - relations SQL  
  - sessions PHP  
  - formulaires sécurisés  
- Se rapprocher d’un mini-projet réel de type SaaS  

---

## 8. Bonus (Optionnel)

- Timer de quiz  
- Pagination des quiz  
- Blocage définitif des re-tentatives  
- Classe `ScoreService`  
- Statistiques personnelles simples  
