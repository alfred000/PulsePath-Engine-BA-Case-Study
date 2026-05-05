# 📄 Spécifications Fonctionnelles : PulsePath Engine

## 1. Introduction
Ce document décrit les capacités fonctionnelles du système **PulsePath Engine**, les contrôles de validation des données et les règles d'interface utilisateur (UI). Il sert de référence pour le développement et la validation de la solution.

---

## 2. Architecture Fonctionnelle
La solution est structurée autour de trois modules interdépendants :
1.  **Module de Collecte (Journal)** : Interface de saisie quotidienne des 5 piliers de données.
2.  **Moteur de Calcul (Core Engine)** : Traitement des algorithmes de TDEE, de vélocité et de composition corporelle.
3.  **Module de Restitution (Dashboard)** : Visualisation de la trajectoire et diffusion des alertes de coaching.

---

## 3. Spécifications des Données (Dictionnaire de Données)


| Champ | Type | Plage de Valeurs / Format | Unité | Obligatoire |
| :--- | :--- | :--- | :--- | :---: |
| **Poids_Jour** | Décimal | 30.0 à 300.0 | kg | OUI |
| **Calories_In** | Entier | 500 à 10 000 | kcal | OUI |
| **Proteines_In** | Entier | 20 à 500 | g | NON |
| **Sommeil_Heures**| Décimal | 0.0 à 24.0 | h | OUI |
| **Pas_Steps** | Entier | 0 à 100 000 | pas | OUI |
| **Debut_Jeune** | Heure | hh:mm | - | NON |

---

## 4. Spécifications des Fonctions & Comportements

### SF-01 : Validation de l'Intégrité de la Saisie
*   **Description** : Empêcher le traitement de données incohérentes ou incomplètes.
*   **Comportement** : Le bouton "Enregistrer" reste désactivé tant que les champs obligatoires (Poids, Calories, Sommeil, Pas) ne sont pas remplis.
*   **Règle de Gestion** : Afficher un indicateur visuel (pastille rouge/verte) pour chaque métrique saisie.

### SF-02 : Algorithme de Lissage de la Trajectoire
*   **Description** : Stabiliser l'affichage de la date d'échéance pour éviter les fluctuations émotionnelles dues à la rétention d'eau.
*   **Comportement** : Le système doit utiliser une **moyenne glissante (SMA)** sur les 7 derniers jours pour calculer la vélocité. 
*   **Règle de Gestion** : Si l'historique est < 3 jours, masquer le graphique de trajectoire.

### SF-03 : Hiérarchie des Alertes (Coaching Insights)
*   **Description** : Prioriser les messages affichés à l'utilisateur.
*   **Règle de Gestion** : 
    1. **Niveau 1 (Rouge)** : Alertes de sécurité (ex: Calories chroniquement trop basses).
    2. **Niveau 2 (Orange)** : Alertes nutritionnelles (ex: Manque de protéines).
    3. **Niveau 3 (Bleu)** : Conseils d'optimisation (ex: Corrélation sommeil/faim).

---

## 5. Spécifications de l'Interface Utilisateur (UI)

*   **Code Couleur Sémantique** :
    *   **Vert** : Progression conforme à la vélocité cible.
    *   **Orange** : Écart modéré détecté (action requise).
    *   **Rouge** : Écart critique ou donnée manquante bloquante.
*   **Navigation** : Accès direct au "Quick Log" depuis l'écran d'accueil pour minimiser la friction (moins de 3 clics pour enregistrer la journée).

---

## 6. Contraintes Techniques Impactant le Métier
*   **Disponibilité Hors-ligne** : L'utilisateur doit pouvoir saisir ses logs sans connexion internet ; la synchronisation et le recalcul de la vélocité se font lors de la reconnexion.

