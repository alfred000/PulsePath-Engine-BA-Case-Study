# 📄 Cahier de Recette Fonctionnelle : PulsePath Engine

## 1. Introduction
Ce document définit les scénarios de tests nécessaires pour valider la conformité du **Moteur PulsePath**. Chaque test vérifie une règle métier ou une spécification fonctionnelle définie dans les phases de conception.

---

## 2. Stratégie de Test
L'approche de test se concentre sur trois axes :
*   **Exactitude des calculs** : Vérifier que les formules (TDEE, Vélocité) sont appliquées sans erreur.
*   **Robustesse (Edge Cases)** : Tester le comportement du système avec des données limites ou erronées.
*   **Réaction de l'Intelligence (Insights)** : Vérifier que les alertes de coaching se déclenchent selon les bons seuils.

---

## 3. Scénarios de Tests Fonctionnels

### TC-01 : Validation de l'algorithme de Vélocité
*   **Objectif** : Vérifier que la date d'échéance s'ajuste correctement en fonction du bilan énergétique.
*   **Données de test** : Déficit cumulé du jour = 1100 kcal (équivalent à 1/7 de kg).
*   **Procédure** : 
    1. Saisir une journée avec un déficit de 1100 kcal.
    2. Enregistrer la donnée.
*   **Résultat attendu** : La "Date d'Échéance Estimée" doit être avancée de **exactement 1 jour** par rapport au calcul précédent (Règle RM-VEL-01).

### TC-02 : Alerte de Protection Musculaire (Protéines)
*   **Objectif** : Vérifier le déclenchement de l'alerte nutritionnelle.
*   **Données de test** : Utilisateur de 80kg, Saisie de 80g de protéines (soit 1.0g/kg).
*   **Procédure** : Saisir 80g de protéines et valider.
*   **Résultat attendu** : Le système doit afficher une alerte orange : *"Apport protéique insuffisant pour préserver le muscle."* (Règle RM-PRO-01).

### TC-03 : Calcul du TDEE Dynamique (Pas)
*   **Objectif** : Vérifier que le changement d'activité impacte le métabolisme calculé.
*   **Données de test** : Passer de 4 000 pas (Sédentaire) à 12 000 pas (Très Actif).
*   **Résultat attendu** : Le multiplicateur d'activité doit passer de 1.2 à 1.6, augmentant ainsi le TDEE affiché sur le dashboard.

### TC-04 : Contrôle de Saisie (Data Validation)
*   **Objectif** : Empêcher les erreurs de calcul dues à des saisies impossibles.
*   **Données de test** : Saisir "-10" dans le champ Calories.
*   **Résultat attendu** : Le système doit bloquer l'enregistrement, afficher le champ en rouge et proposer un message d'erreur : *"Veuillez saisir un entier positif."*

---

## 4. Matrice de Traçabilité des Tests


| ID Test | ID User Story | ID Règle Métier | Statut |
| :--- | :--- | :--- | :--- |
| **TC-01** | US-01 | RM-VEL-01 | ⚪ À tester |
| **TC-02** | US-04 | RM-PRO-01 | ⚪ À tester |
| **TC-03** | US-03 | RM-MET-01 | ⚪ À tester |
| **TC-04** | SF-01 | - | ⚪ À tester |


