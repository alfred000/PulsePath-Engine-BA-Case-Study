# 📄 User Stories Détaillées : PulsePath Engine

## 1. Présentation
Ce document détaille les spécifications fonctionnelles sous forme de **User Stories**. Chaque story définit un besoin utilisateur, sa valeur métier et les critères précis permettant de valider sa bonne implémentation (Definition of Done).

---

## 2. US-01 : Recalcul dynamique de l'échéance cible
**Priorité :** Must-have | **Estimation :** 5 SP

*   **Énoncé :** En tant qu'utilisateur, je veux que le système mette à jour ma date d'échéance estimée dès que je saisis mes données du jour, afin de comprendre l'impact immédiat de ma discipline sur mon objectif.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : Le calcul doit impérativement utiliser la règle **RM-VEL-01** (moyenne glissante sur 7 jours).
    *   **CA 2** : Si le déficit cumulé augmente, la date doit avancer ; s'il y a un surplus, la date doit reculer.
    *   **CA 3** : L'interface doit afficher le "Delta" de jours (ex: +2 jours) de manière visuelle (Vert pour une avance, Rouge pour un recul).
    *   **CA 4** : En l'absence de données suffisantes (< 3 jours), le système doit afficher "Calcul en cours..." pour éviter toute donnée erronée.

---

## 3. US-03 : Algorithme de TDEE dynamique (Pas)
**Priorité :** Must-have | **Estimation :** 5 SP

*   **Énoncé :** En tant qu'utilisateur, je veux que ma dépense énergétique soit ajustée en fonction de mon activité réelle (pas), afin d'avoir un bilan calorique net précis chaque jour.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : Le système doit appliquer les paliers d'activité définis dans la règle **RM-MET-01**.
    *   **CA 2** : Si le log de pas est vide, le système doit appliquer par défaut le multiplicateur "Sédentaire" (1.2).
    *   **CA 3** : Le TDEE recalculé doit être visible sur l'interface de saisie pour donner un feedback immédiat à l'utilisateur.

---

## 4. US-04 : Suivi de la cible de Protéines
**Priorité :** Should-have | **Estimation :** 3 SP

*   **Énoncé :** En tant qu'utilisateur, je veux visualiser mon apport protéique par rapport à ma cible personnalisée, afin de m'assurer que je préserve ma masse musculaire pendant ma perte de poids.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : La cible doit être calculée selon le poids cible de l'utilisateur (2g / kg de poids cible).
    *   **CA 2** : Une jauge de progression doit s'afficher en temps réel lors de la saisie.
    *   **CA 3** : Si l'apport est inférieur à 1.5g/kg de poids actuel, une icône d'alerte orange doit apparaître.

---

## 5. US-05 : Tracking de la fenêtre de Jeûne Intermittent
**Priorité :** Should-have | **Estimation :** 5 SP

*   **Énoncé :** En tant qu'utilisateur, je veux enregistrer mes heures de début et de fin de repas, afin de vérifier le respect de mon protocole de jeûne.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : Le système doit calculer la durée entre l'heure du premier repas et celle du dernier.
    *   **CA 2** : Si cette durée est ≤ 8 heures, la journée est validée "Objectif Jeûne Atteint".
    *   **CA 3** : Le système doit empêcher la saisie d'une heure de fin antérieure à l'heure de début sur la même journée civile.

---
## 6. US-06 : Inscription & Connexion Sécurisée 
**Priorité :** Should-have | **Estimation :** 5 SP

*   **Énoncé :**  En tant que nouvel utilisateur, Je veux créer un compte avec un email et un mot de passe sécurisé, puis m'authentifier, Afin de protéger mes données physiologiques personnelles et accéder à mon espace sécurisé.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : Sécurité du stockage (Back-End)
        *   **Given** Un utilisateur qui soumet le formulaire d'inscription.
        *   **When** L'API traite la demande d'inscription.
        *   **Then** Le mot de passe doit être obligatoirement salé et haché avec l'algorithme BCrypt avant l'insertion dans la table `Users` de SQLite.
    *   **CA 2** : Délivrance du Jeton de Session (JWT)
        *    **Given** Un utilisateur enregistré qui fournit des identifiants valides sur la route `/api/auth/login`.
        *    **When** L'authentification réussit.
        *    **Then** Le serveur doit retourner un code `200 OK` contenant un token JWT valide signé contenant le `UserId`.
    *   **CA 3** : Gestion des erreurs de connexion
         *    **Given** Un utilisateur saisissant un email inexistant ou un mot de passe erroné.
         *    **When** La requête de connexion est envoyée.
         *    **Then** L'API doit retourner une erreur standardisée `401 Unauthorized` avec un message générique pour éviter l'énumération de comptes.

---

## US-07 : Initialisation du Profil Métabolique (Should)
**Priorité :** Should-have | **Estimation :** 5 SP

*   **Énoncé :** En tant qu'utilisateur authentifié, Je veux renseigner mon profil biologique (âge, sexe, taille, poids, facteur d'activité), Afin de fournir au système expert les variables nécessaires au calcul de mon métabolisme de base (BMR).

*   **Critères d'Acceptation (CA) :**
   *    **CA 1** : Validation stricte des données d'entrée (Front-End & Back-End)
        *    **Given** L'écran de configuration du profil utilisateur.
        *    **When** L'utilisateur valide le formulaire.
        *    **Then** Les champs doivent respecter les bornes de sécurité suivantes : *Âge [15 - 90 ans]*, *Taille [100 - 250 cm]*, *Poids [40 - 250 kg]*.
        *    **And** Si une valeur est hors bornes, le bouton de validation reste désactivé et l'API rejette un code `400 Bad Request`.
   *   **CA 2** : Persistance et isolation des données
       *    **Given** Une requête valide `POST /api/profile`.
       *    **When** L'API persiste les données dans le modèle `UserProfile` de la base SQLite.
       *    **Then** Les données physiologiques doivent être strictement associées au `UserId` extrait du token JWT de la session courante.

---

## US-08 : Définition d'Objectifs S.M.A.R.T (Should)
**Priorité :** Should-have | **Estimation :** 5 SP

*   **Énoncé :** En tant qu'utilisateur ayant configuré son profil, Je veux définir un objectif de poids et un rythme hebdomadaire de progression, Afin de générer automatiquement ma prescription calorique et mes macros sans mettre ma santé en danger.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : Garde-fou de sécurité métabolique (RM-GOAL-01)
        * **Given** Un utilisateur ciblant une perte de poids agressive.
        * **When** L'utilisateur tente de valider un rythme supérieur à **1% de son poids corporel par semaine** OU un déficit qui abaisse ses calories sous son **BMR de survie**.
        * **Then** Le système bloque la validation.
        * **And** Le système affiche une alerte contextuelle de sécurité et refuse de générer la feuille de route.
    *   **CA 2** : Calcul instantané de la feuille de route
        * **Given** Un objectif physiologiquement validé par le système expert.
        * **When** L'enregistrement de l'objectif est confirmé en base de données.
        * **Then** Le système calcule instantanément et affiche sur le Dashboard les valeurs cibles de la phase.
        * **And** Ces valeurs incluent le **TDEE dynamique**, les **calories cibles journalières**, et la répartition des **macro-nutriments** (RM-MAC-01).

---

## US-09 : Suivi des Progrès & Journal Quotidien (Should)
**Priorité :** Should-have | **Estimation :** 5 SP

*   **Énoncé :** En tant qu'utilisateur engagé dans son programme, Je veux consigner chaque jour mon poids réel, mes apports caloriques et mes pas,Afin de visualiser ma trajectoire de progression et permettre au moteur de rattrapage de s'activer en cas d'écart.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : Règle d'unicité du journal quotidien
        * **Given** Un utilisateur qui a déjà soumis son journal pour le jour J.
        * **When** Il tente d'accéder à nouveau au formulaire de saisie pour la même date.
        * **Then** Le système passe le formulaire en mode édition (`PUT`) au lieu d'une création (`POST`).
        * **And** Cela empêche l'apparition de doublons chronologiques en base SQLite.
    *   **CA 2** : Déclenchement de l'Insight de Trajectoire
        * **Given**Un historique de saisies sur les 3 derniers jours présentant un écart moyen de plus de **15%** par rapport au déficit planifié.
        * **When** L'utilisateur charge son Dashboard Angular.
        * **Then** Le Bloc 3 (Insights) doit afficher une notification dynamique signalant l'activation imminente du protocole de rattrapage (RM-COR-01).
     
