# 📄 Glossaire Métier : PulsePath Engine

## 1. Objectif du Document
Ce glossaire définit les concepts clés et les indicateurs utilisés dans le projet **PulsePath Engine**. L'objectif est de garantir une « Source Unique de Vérité » (Single Source of Truth) pour tous les contributeurs, du marketing au développement.

---

## 2. Métriques de Performance et Santé


| Terme | Définition | Unité / Formule |
| :--- | :--- | :--- |
| **BMR (Métabolisme de Base)** | Quantité minimale d'énergie nécessaire au corps pour survivre au repos. | kcal (Mifflin-St Jeor) |
| **TDEE (Dépense Énergétique Totale)** | Quantité totale de calories brûlées sur 24h, activité physique incluse. | `BMR × Facteur d'Activité` |
| **Vélocité Santé** | Vitesse de progression réelle vers l'objectif, lissée sur 7 jours. | jours / semaine |
| **Score d'Intégrité** | Indicateur de qualité et de régularité de la saisie des données par l'utilisateur. | 0 à 100 % |
| **Bilan Énergétique Net** | Différence entre l'apport calorique et la dépense totale réelle. | `Calories In - TDEE` |

---

## 3. Terminologie Nutritionnelle & Lifestyle

*   **Fenêtre Alimentaire (Feeding Window)** : Intervalle de temps durant lequel la consommation de calories est autorisée dans le cadre d'un jeûne intermittent (ex: 8 heures).
*   **Risque Catabolique** : État d'alerte où le corps risque de dégrader ses propres muscles suite à un manque de protéines pendant un déficit calorique.
*   **Refeed (Recharge)** : Augmentation contrôlée de l'apport en glucides pour restaurer les niveaux d'énergie et réguler les hormones métaboliques.
*   **Victoire Non-Scalaire (Non-Scale Victory)** : Progrès mesurables qui ne concernent pas le poids (ex: amélioration du sommeil, tour de taille, énergie).

---

## 4. Concepts de Trajectoire

*   **Date d'Échéance Estimée (DEE)** : Date calculée par l'algorithme à laquelle l'utilisateur devrait atteindre son poids cible si sa vélocité actuelle se maintient.
*   **Impact Trajectoire** : Nombre de jours gagnés ou perdus sur la DEE suite aux actions des dernières 24 heures.
*   **Moyenne Glissante (SMA - 7 jours)** : Méthode de calcul utilisée pour lisser les fluctuations quotidiennes (poids, eau) et dégager une tendance réelle.

---
## 5. Métriques Biométriques Ajoutées
*   **IMG (Indice de Masse Grasse)** : Estimation de la proportion de tissu adipeux dans le corps, calculée via la formule de Deurenberg. Elle catégorise l'utilisateur (Normal, Trop bas, Excès de masse grasse).
*   **Plage de Poids Bien-être** : Intervalle de poids médicalement sain, calculé sur une base d'IMC allant de 18.5 à 24.9.
*   **Calories de Maintenance** : Énergie nécessaire pour maintenir le poids actuel sans variation, équivalente au TDEE initial de l'utilisateur.

## 6. Dictionnaire des Macro-nutriments
*   **Protéines (1g = 4 kcal)** : Macro-nutriment prioritaire pour la protection de la masse maigre. Fixé à 30% du budget énergétique en phase de perte de gras.
*   **Glucides (1g = 4 kcal)** : Source d'énergie principale, ajustée de manière flexible selon le niveau d'activité et l'objectif.
*   **Lipides (1g = 9 kcal)** : Acides gras essentiels à la régulation hormonale. Maintenus à un seuil stable de 25% à 30% des calories totales.

---

## 7. Acronymes utilisés
*   **MVP** : Produit Minimum Viable (version simplifiée du produit).
*   **KPI** : Indicateur Clé de Performance.
*   **SME** : Expert Métier (Subject Matter Expert).
*   **UI/UX** : Interface Utilisateur / Expérience Utilisateur.

