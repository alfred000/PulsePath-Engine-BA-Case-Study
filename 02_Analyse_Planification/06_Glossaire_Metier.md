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
## 8. Niveaux d'Activité Initiaux (Diagnostic)
*   **Mode de vie sédentaire (Coeff. 1.2)** : Activité quotidienne limitée au repos et aux déplacements bureautiques minimaux.
*   **Activité légère (Coeff. 1.375)** : Station debout prolongée ou marche légère régulière au cours de la journée.
*   **Activité modérée (Coeff. 1.55)** : Mode de vie actif, marche fréquente et entraînements physiques légers.
*   **Activité élevée (Coeff. 1.725)** : Métier à forte contrainte physique ou entraînements sportifs quotidiens de haute intensité.
---
## 9. Indicateurs de Progression et d'Évolution (KPIs)
*   **Compteur de Jours Cumulés** : Nombre total de jours écoulés depuis la date exacte de la fixation de l'objectif S.M.A.R.T. Ce compteur s'incrémente continuellement, quel que soit le comportement alimentaire.
*   **Perte Totale (kg)** : Écart absolu entre le *Poids de départ* enregistré lors du diagnostic et le *Poids du jour*.
*   **Progrès Global (%)** : Pourcentage de réalisation de l'objectif de perte de poids. Formule : `(Poids_Départ - Poids_Jour) / (Poids_Départ - Poids_Cible) * 100`.
*   **Surplus Métabolique Temporaire (Gel des KPIs)** : État logique déclenché lorsque le poids du jour est supérieur ou égal au poids de départ. Le système fige la perte et le progrès à 0 pour éviter les calculs négatifs absurdes, tout en laissant le compteur de jours s'incrémenter.
---
## 10. Planification Sportive
*   **Fréquence Sportive Programmée** : Nombre de séances d'activité physique ciblées par l'utilisateur lors de la planification (ex: 4 séances/semaine).
*   **Volume Hebdomadaire Validé** : Somme cumulée des séances réelles déclarées dans le journal quotidien au cours de la semaine glissante en cours.
---
## 11. Concepts de Modélisation Prédictive et Correction
*   **Moteur de Rattrapage Intelligent (Course-Correction Engine)** : Algorithme prescriptif qui s'active en cas de déviation par rapport à la trajectoire initiale. Il recalcule les cibles à court terme pour ramener l'utilisateur sur sa trajectoire de référence.
*   **Méthode d'Optimisation sous Contraintes (Recherche de Racines)** : Logique mathématique simulant l'impact de différents scénarios d'ajustement afin de trouver le point d'équilibre exact annulant l'écart de jours accumulés, tout en respectant les limites biologiques de l'utilisateur.
*   **Horizon Glissant de Rattrapage** : Fenêtre temporelle fixe (définie à 7 jours dans le MVP) sur laquelle le surplus calorique accumulé doit être lissé et éliminé.

---

## 12. Leviers Fonctionnels d'Ajustement
*   **Levier Nutritionnel (Alimentation)** : Réduction temporaire et calibrée du budget calorique quotidien pour absorber une partie de l'écart énergétique.
*   **Levier d'Activité (Pas / Cardio LISS)** : Augmentation compensatoire du volume d'activité physique (Cardio à basse intensité / Low-Intensity Steady State) pour forcer la dépense énergétique journalière.
*   **Hard Guardrail (Barrière Physiologique)** : Limite logicielle inviolable empêchant l'algorithme de proposer des cibles dangereuses (ex: apport inférieur au BMR ou volume d'activité supérieur à 18 000 pas/jour).

---

## 13. Acronymes utilisés
*   **MVP** : Produit Minimum Viable (version simplifiée du produit).
*   **KPI** : Indicateur Clé de Performance.
*   **SME** : Expert Métier (Subject Matter Expert).
*   **UI/UX** : Interface Utilisateur / Expérience Utilisateur.

