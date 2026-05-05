# 📄 Registre des Risques : PulsePath Engine

## 1. Méthodologie d'Évaluation
Chaque risque est évalué selon deux critères sur une échelle de 1 à 5 :
*   **Probabilité (P)** : La chance que le risque se produise.
*   **Impact (I)** : La sévérité des conséquences sur le projet ou l'utilisateur.
*   **Criticité** : Produit de la Probabilité par l'Impact (P x I).

---

## 2. Tableau de Suivi des Risques


| ID | Description du Risque | Catégorie | P | I | Criticité | Stratégie d'Atténuation (Mitigation) |
| :--- | :--- | :--- | :---: | :---: | :---: | :--- |
| **R-001** | **Biais de saisie** (sous-estimation des calories par l'utilisateur). | Métier | 5 | 4 | 🔴 **20** | Implémenter des aides à la saisie et des alertes si les calories sont < BMR. |
| **R-002** | **Données manquantes** (Journal non rempli) cassant la précision. | Produit | 4 | 5 | 🔴 **20** | Mise en place du **Score d'Intégrité** et rappels personnalisés. |
| **R-003** | **Comportement à risque** (restriction calorique extrême). | Santé | 2 | 5 | 🟠 **10** | Blocage automatique des calculs si le déficit est jugé dangereux. |
| **R-004** | **Baisse de motivation** si la date d'échéance recule. | UX | 3 | 4 | 🟠 **12** | Valoriser les "Victoires Non-Scalaires" (énergie, sommeil) via le dashboard. |
| **R-005** | **Instabilité des données** (poids) due à la rétention d'eau. | Technique | 4 | 2 | 🟡 **8** | Application systématique de la **Moyenne Glissante (SMA)** sur 7 jours. |

---

## 3. Détail des Actions d'Analyse (Focus Business Analyst)

### 🔴 Focus R-001 : Fiabilité de la donnée
En tant que BA, j'ai identifié que la réussite du projet repose sur la qualité de l'input.
*   **Action** : Spécifier une fonction de "Quick-Log" pour réduire la friction de saisie.
*   **Action** : Intégrer une base de données d'aliments fiable pour limiter les erreurs d'estimation.

### 🟠 Focus R-003 : Sécurité de l'utilisateur
La responsabilité éthique est au cœur de l'analyse métier santé.
*   **Action** : Définition d'un "Garde-fou" fonctionnel interdisant de fixer un objectif de perte de poids > 1% du poids de corps par semaine.

---

## 4. Conclusion sur l'Exposition aux Risques
L'exposition majeure est liée à la **volonté de l'utilisateur**. La stratégie de PulsePath Engine n'est pas seulement technique, elle est comportementale : utiliser la **donnée comme levier de motivation** pour réduire les risques d'abandon.
