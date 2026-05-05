# 📄 Registre des Anomalies et Suivi des Bugs : PulsePath Engine

## 1. Présentation
Ce document recense les écarts constatés entre le comportement attendu (défini dans les spécifications fonctionnelles) et le comportement réel observé lors de la phase de recette (Phase 4).

## 2. Cycle de Vie d'une Anomalie
1.  **Détection** : Identifiée lors de l'exécution du Cahier de Recette.
2.  **Qualification** : Analyse de la sévérité (Bloquante, Majeure, Mineure) par le Business Analyst.
3.  **Correction** : Prise en charge par l'équipe technique.
4.  **Clôture** : Re-test effectué par le BA pour valider la résolution.

---

## 3. État du Suivi des Anomalies



| ID Bug | Titre de l'anomalie | Sévérité | Statut | Origine (ID Test) | Commentaire du Business Analyst |
| :--- | :--- | :--- | :---: | :---: | :--- |
| **BUG-01** | Erreur d'arrondi sur la Date d'Échéance | **Mineure** | ✅ Corrigé | TC-01 | La date affichait des décimales (ex: 14.2 Mai). Corrigé pour afficher l'entier supérieur. |
| **BUG-02** | Calcul TDEE erroné si Pas > 20 000 | **Majeure** | ✅ Corrigé | TC-03 | Le multiplicateur plafonnait à 1.6 sans prendre en compte le surplus d'activité extrême. |
| **BUG-03** | Plantage système sur saisie de texte | **Bloquante** | 🛠️ En cours | TC-04 | L'application crash si l'utilisateur saisit des lettres dans le champ 'Poids'. |
| **BUG-04** | Alerte Protéines illisible en Mode Sombre | **Moyenne** | 📅 Planifié | TC-02 | Problème de contraste (orange sur noir). Reporté à la prochaine itération UI. |

---

## 4. Analyse de la Qualité (Bilan Recette)
*   **Total Anomalies** : 4
*   **Résolues** : 2
*   **En cours** : 1
*   **Reportées** : 1

**Note du BA** : Le système est stable pour les fonctions de calcul critiques. L'anomalie **BUG-03** est la seule priorité absolue avant le déploiement de la version bêta pour garantir l'intégrité de la base de données.
