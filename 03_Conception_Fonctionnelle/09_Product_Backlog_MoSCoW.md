# 📄 Product Backlog & Priorisation MoSCoW

## 1. Présentation
Ce document recense l'ensemble des besoins fonctionnels et techniques identifiés pour le projet **PulsePath Engine**. Chaque élément est priorisé afin de garantir la livraison d'un Produit Minimum Viable (MVP) fonctionnel avant l'ajout de fonctionnalités avancées.

---

## 2. Le Framework de Priorisation (MoSCoW)
*   **Must-have (Indispensable)** : Fonctionnalités vitales. Sans elles, le produit n'a aucune valeur.
*   **Should-have (Important)** : Forte valeur ajoutée, mais non bloquant pour le lancement.
*   **Could-have (Souhaitable)** : Améliorations de confort ou esthétiques (Nice-to-have).
*   **Won't-have (Pour plus tard)** : Fonctionnalités exclues du périmètre actuel du projet.

---

## 3. Product Backlog Priorisé



| ID | Catégorie | Titre de la User Story | Priorité | Valeur Métier | Complexité (SP) |
| :--- | :--- | :--- | :---: | :---: | :---: |
| **US-01** | Calcul | Recalcul dynamique de l'échéance cible (Vélocité) | **Must** | 🔴 Critique | 5 |
| **US-02** | Saisie | Interface de saisie quotidienne (5 piliers santé) | **Must** | 🔴 Critique | 8 |
| **US-03** | Algorithme | Calcul du TDEE dynamique basé sur les pas (Steps) | **Must** | 🟠 Haute | 5 |
| **US-04** | Nutrition | Suivi des Macros-nutriments (Focus Protéines) | **Should** | 🟠 Haute | 3 |
| **US-05** | Nutrition | Module de tracking du Jeûne Intermittent | **Should** | 🟠 Haute | 5 |
| **US-06** | Gamif. | Calcul du Score d'Intégrité (Qualité des logs) | **Could** | 🟡 Moyenne | 2 |
| **US-07** | Coaching | Génération d'Insights prédictifs automatisés | **Could** | 🟡 Moyenne | 8 |
| **US-08** | Technique | Synchronisation API avec montres connectées | **Won't** | 🟠 Haute | 13 |
| **US-09** | UX | Export des rapports de progression en PDF | **Won't** | 🟢 Basse | 3 |

*\*SP : Story Points (Estimation relative de l'effort de développement)*

---

## 4. Stratégie de Release

### 🚀 Release 1.0 (MVP)
Focus exclusif sur les **Must-have**. 
*   **Objectif** : Fournir à l'utilisateur une trajectoire de poids fiable basée sur son métabolisme réel.
*   **Contenu** : US-01, US-02, US-03.

### 📈 Release 1.1 (Optimisation Métabolique)
Intégration des **Should-have**.
*   **Objectif** : Affiner la qualité de la transformation physique (protection musculaire via les macros et le jeûne).
*   **Contenu** : US-04, US-05.

---

## 5. Justification des Choix de Priorisation
Le choix de placer la **Synchronisation API (US-08)** en "Won't-have" est une décision stratégique de Business Analyst. Bien que très demandée par les utilisateurs, sa complexité technique (dépendances API tierces) risquerait de retarder la livraison du cœur de métier (le moteur de calcul de vélocité). La saisie manuelle est privilégiée pour le MVP afin de valider l'algorithme rapidement.

