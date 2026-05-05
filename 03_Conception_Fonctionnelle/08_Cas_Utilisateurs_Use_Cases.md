# 📄 Cas d’Utilisation (Use Cases) : PulsePath Engine

## 1. Présentation
Les cas d’utilisation ci-dessous décrivent les interactions majeures entre l’**Utilisateur** et le **Moteur PulsePath**. Ils se concentrent sur les besoins fonctionnels nécessaires pour maintenir la "Source Unique de Vérité" (le journal quotidien) et générer les indicateurs de "Vélocité".

---

## 2. UC-01 : Saisir les métriques quotidiennes (Cœur du système)
*   **Acteur principal :** Utilisateur
*   **Objectif :** Enregistrer les données physiologiques et nutritionnelles pour mettre à jour la trajectoire.
*   **Pré-conditions :** L'utilisateur est authentifié ; les données de profil (poids cible, âge, taille) sont renseignées.
*   **Scénario nominal (Success Scenario) :**
    1. L'utilisateur sélectionne "Ajouter une entrée au journal".
    2. L'utilisateur saisit : **Poids**, **Calories**, **Macros (Protéines)**, **Heures de sommeil** et **Durée du jeûne**.
    3. Le système valide que les entrées sont dans des plages réalistes (ex: Calories > 500).
    4. Le système calcule le **Score d'Intégrité**.
    5. Le système enregistre l'entrée et déclenche automatiquement le recalcul de la vélocité.
*   **Post-conditions :** Les données sont sauvegardées et l'historique est mis à jour.
*   **Exceptions :**
    *   *Données invalides :* Le système affiche un message d'erreur et bloque l'enregistrement si les valeurs sont aberrantes (ex: Poids < 30kg).

---

## 3. UC-02 : Consulter la trajectoire dynamique (Analyse prédictive)
*   **Acteur principal :** Utilisateur
*   **Objectif :** Visualiser l'impact du comportement récent sur la date d'échéance finale.
*   **Pré-conditions :** Au moins 3 jours de saisies consécutives sont présents en base de données.
*   **Scénario nominal :**
    1. L'utilisateur navigue vers le tableau de bord "Trajectoire".
    2. Le système calcule la moyenne glissante du bilan énergétique net sur les 7 derniers jours.
    3. Le système applique la règle **RM-VEL-01** pour générer la nouvelle **Date d'Échéance Estimée (DEE)**.
    4. Le système affiche graphiquement le "Delta" (jours gagnés ou perdus) par rapport à la DEE initiale.
*   **Post-conditions :** L'utilisateur visualise clairement sa progression réelle au-delà du simple chiffre sur la balance.

---

## 4. UC-03 : Recevoir un "Coach Insight" automatisé
*   **Acteur principal :** Système (Automatisé)
*   **Objectif :** Fournir un conseil stratégique basé sur les corrélations de données.
*   **Scénario nominal :**
    1. Le système détecte une corrélation (ex: Sommeil < 7h entraînant une hausse calorique le lendemain).
    2. Le système génère une notification contextuelle personnalisée.
    3. L'utilisateur consulte l'insight et reçoit une recommandation actionnable (ex: "Améliorez votre sommeil pour stabiliser votre faim").
*   **Post-conditions :** L'utilisateur est éduqué sur l'interdépendance de ses métriques de santé.

---

## 5. Matrice de Priorité des Cas d'Utilisation

| ID | Nom du Cas d'Utilisation | Importance Métier | Complexité |
| :--- | :--- | :--- | :--- |
| **UC-01** | Saisie des métriques | 🔴 Critique | Basse |
| **UC-02** | Consultation Trajectoire | 🔴 Critique | Haute |
| **UC-03** | Coach Insights | 🟠 Haute | Moyenne |
