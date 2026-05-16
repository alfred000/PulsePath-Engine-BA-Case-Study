# 📄 Règles Métier et Logique de Calcul : PulsePath Engine

## 1. Présentation
Ce document répertorie les algorithmes de santé et les logiques décisionnelles qui régissent le moteur de calcul **PulsePath**. Ces règles permettent de transformer des données brutes en indicateurs de coaching fiables.

---

## 2. Règle Métabolique
### 2.1. Règle RM-MET-01 : Calcul du TDEE Dynamique
Le **TDEE (Total Daily Energy Expenditure)** représente la dépense énergétique totale réelle sur 24h.

### A. Calcul du Métabolisme de Base (BMR)
Utilisation de l'équation de **Mifflin-St Jeor**, reconnue comme la plus fiable pour la gestion du poids :
*   **Hommes** : `(10 × poids_kg) + (6.25 × taille_cm) - (5 × âge) + 5`
*   **Femmes** : `(10 × poids_kg) + (6.25 × taille_cm) - (5 × âge) - 161`

### B. Ajustement du Facteur d'Activité (Pas)
Le système remplace les multiplicateurs fixes par un calcul basé sur le `steps_log` quotidien :
*   **Sédentaire (< 5 000 pas)** : Multiplicateur = `1.2`
*   **Actif (5 000 - 10 000 pas)** : Multiplicateur = `1.4`
*   **Très Actif (> 10 000 pas)** : Multiplicateur = `1.6`

**Formule Finale** : `TDEE = BMR × Facteur_Activité`

### 2.2. RM-MET-02 : Facteur d'Activité Initial (Profil)
Calcul de la maintenance théorique de départ avant l'activation des trackers de pas :
*   `Choix = 1` $\rightarrow$ Multiplicateur = `1.2`
*   `Choix = 2` $\rightarrow$ Multiplicateur = `1.375`
*   `Choix = 3` $\rightarrow$ Multiplicateur = `1.55`
*   `Choix = 4` $\rightarrow$ Multiplicateur = `1.725`

---
## 3. Règle RM-VEL-01 : Calcul de la Vélocité et de l'Échéance
L'algorithme de vélocité projette la date de réussite en fonction du comportement récent.

*   **Équivalent Énergétique** : 1 kg de masse grasse = **7 700 kcal**.
*   **Moyenne Glissante (SMA)** : Balance_Hebdo = `Moyenne(Bilan_Energétique_Net_7j)`
*   **Vélocité Hebdomadaire (kg)** : `(Balance_Hebdo × 7) / 7700`
*   **Nouvelle Date d'Échéance** : `Date_Jour + ((Poids_Actuel - Poids_Cible) / Vélocité_Hebdomadaire × 7)`

---

## 4. Règle RM-PRO-01 : Protection de la Masse Musculaire
Règle de sécurité pour éviter la dénutrition ou la fonte musculaire (Sarcopénie) :
*   **Seuil Alerte** : Si `Protéines_In < (Poids_Actuel × 1.5g)`, déclencher l'insight : *"Apport protéique insuffisant pour préserver le muscle."*
*   **Seuil Optimal** : Si `Protéines_In >= (Poids_Actuel × 2g)`, marquer l'objectif comme *"Optimal"*.

---

## 5. Règle RM-GAM-01 : Bonus d'Intégrité
Calcul de la fiabilité de la donnée pour encourager la rétention (Gamification) :
*   **Poids + Calories** : 50% du score (Données critiques).
*   **Pas + Sommeil** : 30% du score.
*   **Macros + Jeûne** : 20% du score.
*   **Conséquence** : Si `Score_Intégrité < 100%`, la vélocité est calculée mais marquée comme *"Estimation à faible fiabilité"*.

---

## 6. Règle RM-FAST-01 : Validation du Jeûne
*   **Condition de Succès** : `(Heure_Dernier_Repas - Heure_Premier_Repas) ≤ 8h`.
*   **Ajustement de la Faim** : Si `Sommeil < 7h` ET `Jeûne = Échec`, générer l'insight : *"Le manque de sommeil a impacté votre régulation hormonale."*

---
## 7. Règle RM-IMC-01 : Catégorisation et Recommandation
*   **Formule** : $IMC = Poids_{kg} / (Taille_{m})^2$
*   **Logique décisionnelle d'interprétation** :
    *   Si IMC < 18.5 : Catégorie = "Maigreur" $\rightarrow$ Bloquer l'objectif "Perte".
    *   Si 18.5 $\le$ IMC $\le$ 24.9 : Catégorie = "Normal" $\rightarrow$ Recommandation = "Maintien ou Recomposition".
    *   Si IMC $\ge$ 25 : Catégorie = "Surpoids/Obésité" $\rightarrow$ Recommandation = "Essayez de perdre du poids sainement jusqu'à votre plage bien-être."

---

## 8. Règle RM-IMG-01 : Calcul de la Masse Grasse (Deurenberg)
*   **Formule** : $IMG = (1.20 \times IMC) + (0.23 \times \hat{A}ge) - (10.8 \times Sexe) - 5.4$
    *   *Sexe = 1 pour Masculin, 0 pour Féminin.*

---

## 9. Règle RM-MAC-01 : Ventilation Automatique des Macros (Ratio Parfait)
En fonction de la cible calorique calculée pour l'objectif S.M.A.R.T, les coefficients énergétiques appliqués sont :
1.  **Perte de Graisse** : Protéines (30%), Glucides (40%), Lipides (30%).
2.  **Maintien** : Protéines (25%), Glucides (45%), Lipides (30%).
3.  **Prise de Masse Musculaire** : Protéines (25%), Glucides (50%), Lipides (25%).

---

## 10. Règle RM-ACT-01 : Calcul des Calories Brûlées d'Activité
*   **Formule** : $Calories_{Activité} = TDEE_{Dynamique} - BMR$
*   Elle isole l'énergie dépensée uniquement par le mouvement (pas) par rapport aux fonctions vitales au repos.

---

## 11. RM-KPI-01 : Calcul du Progrès Linéaire (Objectif Perte/Sèche)
Le pourcentage de progression exclut les valeurs négatives :
*   Si `Poids_Actuel < Poids_Depart` : 
    $$\text{Progrès (\%)} = \left( \frac{\text{Poids\_Départ} - \text{Poids\_Actuel}}{\text{Poids\_Départ} - \text{Poids\_Cible}} \right) \times 100$$
*   Si `Poids_Actuel >= Poids_Depart` :
    $$\text{Progrès (\%)} = 0.0\%$$
    
---

## 12. RM-SRT-01 : Recommandation d'Activité de Soutien
Si l'objectif est défini sur "perte" ou "seche", le système génère la directive d'accompagnement suivante dans le plan d'action de l'utilisateur : 
*   *Apport cible = Budget_Calories. Option cardio obligatoire : Programmer un volume minimum de 8 000 pas quotidiens ou l'équivalent en cardio LISS pour forcer la dépense d'activité.*

---
## 13. RM-COR-01 : Algorithme de Recherche d'Équilibre (Moteur de Rattrapage)
Cet algorithme prescriptif sous contraintes s'active en cas de déviation pour ramener l'utilisateur vers sa trajectoire sur un horizon glissant de 7 jours.

### Étape A : Calcul de l'Écart Énergétique Global
*   `Surplus_A_Rattraper = (DEE_Actuelle - DEE_Initiale) * Deficit_Quotidien_Initial`
*   `Effort_Quotidien_Requis = Surplus_A_Rattraper / 7`

### Étape B : Modélisation des Leviers sous Contraintes (Règle 40/60)
1.  **Calcul du Levier Alimentaire (40%)** :
    *   `Ajustement_Calorique = Effort_Quotidien_Requis * 0.40`
    *   `Cible_Temporaire_Calories = Budget_Calories_Initial - Ajustement_Calorique`

2.  **Application de la Contrainte Métabolique (Hard Guardrail)** :
    *   **SI** `Cible_Temporaire_Calories < BMR` :  
        *   `Cible_Temporaire_Calories = BMR` (Blocage au niveau de survie)
        *   `Effort_Residuel = Effort_Quotidien_Requis - (Budget_Calories_Initial - BMR)` (Le surplus restant est basculé sur l'activité)
    *   **SINON** :  
        *   `Effort_Residuel = Effort_Quotidien_Requis * 0.60`

### Étape C : Conversion de l'Effort Résiduel en Activité (Pas / LISS)
Le système utilise un modèle prédictif linéaire lié au poids de l'utilisateur (1 000 pas brûlent environ `Poids_Actuel * 0.5` kcal) :

*   `Calories_Par_1000_Pas = Poids_Actuel * 0.5`
*   `Pas_Supplementaires_Requis = (Effort_Residuel / Calories_Par_1000_Pas) * 1000`
*   `Nouvelle_Cible_Pas = Objectif_Pas_Initial + Pas_Supplementaires_Requis`

*   **Contrainte d'Épuisement (Hard Guardrail)** : Si `Nouvelle_Cible_Pas > 18000`, le système force la valeur à 18 000 pas et déclenche l'alerte de révision de l'échéance temporelle (`SF-ANP-03`).
