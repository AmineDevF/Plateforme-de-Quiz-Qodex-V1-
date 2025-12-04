# Mini Plateforme de Quiz – Version Sécurisée

## 1. Contexte Général
Objectif principal : créer une mini plateforme de quiz sécurisée où un enseignant peut créer des quiz organisés par catégories et où les étudiants peuvent y répondre.

### Améliorations apportées
- CRUD complet pour toutes les entités
- Sécurité renforcée (sessions, CSRF, XSS, SQL injection)
- Validation et sanitization des données
- Hashage sécurisé des mots de passe
- Gestion des erreurs et logs

---

## 2. Diagramme de Classes

### Classes principales
- User
- Category
- Quiz
- Question
- Result

### Attributs des classes

#### User
- id
- nom
- email
- password_hash
- role (enseignant ou etudiant)
- created_at
- deleted_at (Bonus)

#### Category
- id
- nom
- description
- created_by (enseignant_id)
- created_at
- updated_at

#### Quiz
- id
- titre
- description
- categorie_id
- enseignant_id
- created_at
- updated_at
- is_active

#### Question
- id
- quiz_id
- question
- option1
- option2
- option3
- option4
- correct_option
- created_at

#### Result
- id
- quiz_id
- etudiant_id
- score
- total_questions
- completed_at

---

## 3. Fonctionnalités (Planning sur 7 jours)

### Jours 1–2 : Authentification Sécurisée
- Inscription : validation email, mot de passe, hashage, CSRF, email unique
- Connexion : vérification credentials, régénération ID session, mise à jour last_login
- Déconnexion : destruction complète de la session

### Jour 3 : CRUD Catégories (Enseignant)
- Create : nom + description, validation, sanitization
- Read : afficher les catégories
- Update : vérifier propriétaire
- Delete : vérifier quiz associés, soft delete optionnel

### Jour 4 : CRUD Quiz (Enseignant)
- Create : titre, description, catégorie, minimum 1 question
- Read : liste des quiz, détail, filtres
- Update : modifier infos, ajouter/modifier/supprimer questions
- Delete : suppression quiz + questions, gestion résultats

### Jour 5 : CRUD Questions (Enseignant)
- Create : question + 4 options + réponse correcte
- Read : afficher questions d’un quiz
- Update : texte/options
- Delete : suppression

### Jour 6 : Passage Quiz & Résultats (Étudiant)
Étudiant :
- Voir catégories
- Voir quiz par catégorie
- Passer un quiz
- Soumettre réponses (CSRF)
- Voir score immédiat
- Voir historique personnel

Enseignant :
- Voir résultats de ses quiz
- Filtrer par quiz ou par étudiant
- Statistiques simples

### Jour 7 : Finalisation et Tests
- Revue sécurité
- Tests de validation et permissions
- Documentation
- Optimisation du code

---

## 4. User Stories avec Critères de Sécurité

### Enseignant

#### US1 – Créer une catégorie
Critères de sécurité :
- Session active et rôle enseignant
- Sanitization des champs
- Token CSRF
- Requêtes préparées

#### US2 – Créer un quiz
Critères de sécurité :
- Vérification rôle enseignant
- Validation que la catégorie existe
- Sanitization de tous les champs
- Minimum une question obligatoire
- Token CSRF

#### US3 – Modifier ou supprimer un quiz
Critères de sécurité :
- Vérification propriétaire
- Confirmation suppression
- Token CSRF
- Gestion en cascade des questions

#### US4 – Consulter les résultats
Critères de sécurité :
- Accès limité aux quiz de l’enseignant
- Aucun mot de passe exposé
- Pagination (Bonus)

### Étudiant

#### US5 – Voir les catégories
Critères de sécurité :
- Session active
- Affichage uniquement des quiz actifs

#### US6 – Passer un quiz
Critères de sécurité :
- Token CSRF
- Validation quiz actif
- Vérification que toutes les réponses sont présentes
- Enregistrement sécurisé du résultat
- Résultat non modifiable

#### US7 – Voir ses résultats
Critères de sécurité :
- Accès uniquement à ses propres résultats
- Je veux voir l'historique de mes scores
- Pas d'accès aux résultats des autres

