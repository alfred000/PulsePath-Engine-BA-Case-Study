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
Cette règle permet d'évaluer le pourcentage de réalisation de l'objectif de l'utilisateur tout en protégeant le système contre les valeurs négatives ou aberrantes en cas de surplus.

*   **Calcul de la Perte Totale (kg)** :  
    *   **SI** `Poids_Actuel < Poids_Depart` :  
        `Perte_Totale_Kg = Poids_Depart - Poids_Actuel`
    *   **SINON** :  
        `Perte_Totale_Kg = 0.0`

*   **Calcul du Pourcentage de Progrès (%)** :  
    *   **SI** `Poids_Actuel >= Poids_Depart` :  
        `Progres_Global = 0.0` (L'utilisateur est en surplus temporaire, le progrès ne peut pas être négatif)
    *   **SI** `Poids_Actuel <= Poids_Cible` :  
        `Progres_Global = 100.0` (L'objectif est atteint ou dépassé)
    *   **SINON** :  
        `Total_A_Perdre = Poids_Depart - Poids_Cible`  
        `Progres_Global = (Perte_Totale_Kg / Total_A_Perdre) * 100`

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

---
# 14. RM-STD-01 : Simulateur de Trajectoire de Composition Corporelle

Cet algorithme centralise les règles de gestion (RG), les formules mathématiques et les algorithmes de calcul appliqués par le moteur PulsePath Engine pour projeter l'évolution de la composition corporelle à partir des données d'une balance intelligente.

## I. Règles de Gestion (Business Rules)

### RG001 : Priorisation de la Masse Maigre Sèche (LBM)
Le calcul de la dépense énergétique de base (BMR) ne doit jamais s'appuyer sur le poids total de l'utilisateur, mais exclusivement sur sa masse non grasse (`fatFreeBodyWeight`). Cela garantit une précision accrue chez les pratiquants de musculation de niveau intermédiaire à avancé.

### RG002 : Limite Absolue de Mobilisation Adipeuse (Seuil d'Alpert)
Le corps humain ne peut pas extraire plus de **69,2 kcal par kilogramme de graisse pure et par jour**. Tout déficit calorique quotidien qui dépasse ce seuil physiologique force le système à détruire du tissu musculaire pour combler le manque énergétique, indépendamment de la quantité de protéines consommée.

### RG003 : Déclenchement de l'Alerte de Catabolisme Musculaire
Si le déficit énergétique programmé par l'utilisateur ($| \Delta E |$) est supérieur au Transfert Maximal de la Masse Grasse ($MFT$), le système doit :
1. Lever une alerte de sécurité visuelle sur l'interface (Code couleur : Rouge/Orange).
2. Calculer le taux de dégradation musculaire quotidien et l'imputer sur la métrique `muscleMass`.

### RG004 : Ajustement Dynamique Restreint de la Masse Osseuse
La masse osseuse (`boneMass`) est considérée comme une constante structurelle à l'échelle de la simulation (durée de 12 à 20 semaines). Elle ne subit aucune fluctuation liée au déficit ou au surplus calorique.

## II. Formules Mathématiques et Algorithmes de Calcul

### 1. Variables Initiales (Inputs de la Balance Intelligente)
Les calculs s'exécutent à chaque jour $d$ de la simulation à partir des états du jour précédant $d-1$.
- $W_{0}$ = Poids total initial (`bodyWeightKg`)
- $BF_{0}$ = Pourcentage de masse grasse initial (`bodyFatPercentage`)
- $H$ = Taille en mètres (ex: $1.63$)
- $Age$ = Âge chronologique en années

### 2. Le Bloc Énergétique (Calculs Quotidiens)

#### Étape A : Calcul de la Masse Grasse ($FM$) et de la Masse Maigre ($FFM$)
$$FM_d = W_d \times \left( \frac{BF_d}{100} \right)$$
$$FFM_d = W_d - FM_d$$

#### Étape B : Calcul du Métabolisme de Base (Katch-McArdle)
$$BMR_d = 370 + (21.6 \times FFM_d)$$

#### Étape C : Calcul de la Dépense Énergétique Totale (TDEE)
$$TDEE_d = BMR_d \times \text{Multiplicateur Activité}$$

#### Étape D : Calcul de la Balance Énergétique ($\Delta E_d$)
$$\Delta E_d = \text{Intake Calories (Saisie)} - TDEE_d$$

### 3. L'Algorithme de Partitionnement des Tissus (Boucle Temporelle)

Pour chaque jour $d$ allant de $1$ à `SimulationLengthDays` :

1. **Calcul du Seuil Limite d'Alpert ($MFT$)** :
   $$MFT_d = FM_{d-1} \times 69.2$$

2. **Évaluation du Déficit (Si $\Delta E_d < 0$)** :
   - **Cas 1 : Déficit Sécurisé ($| \Delta E_d | \le MFT_d$)**
     - L'intégralité du déficit est puisée dans les graisses.
     - $\text{Actual Fat Transfer (ATF)}_d = | \Delta E_d |$
     - $\text{Muscle Catabolism}_d = 0$
   - **Cas 2 : Déficit Excessif ($| \Delta E_d | > MFT_d$)**
     - Le gras fournit son maximum, le muscle fournit le reste.
     - $\text{Actual Fat Transfer (ATF)}_d = MFT_d$
     - $\text{Residual Deficit}_d = | \Delta E_d | - MFT_d$
     - $\text{Muscle Catabolism (kcal)}_d = \text{Residual Deficit}_d$

3. **Mise à jour des Masses Tissulaires** :
   $$\Delta FM_d = - \frac{\text{ATF}_d}{9440} \quad \text{(1 kg de tissu adipeux = 9440 kcal structurées)}$$
   $$\Delta MM_d = - \frac{\text{Muscle Catabolism (kcal)}_d}{4300} \quad \text{(1 kg de tissu musculaire = 4300 kcal structurées)}$$

## III. Matrice Évolutive des 13 Métriques de la Balance

À la fin de chaque journée de simulation $d$, les propriétés du vecteur de composition corporelle sont recalculées de manière récursive :

| Code Métrique | Libellé Métrique | Formule Mathématique de Résolution Spécifique |
| :--- | :--- | :--- |
| `bodyWeightKg` | Poids Global | $W_d = FM_{d-1} + \Delta FM_d + FFM_{d-1} + \Delta MM_d$ |
| `bodyMassIndexBMI` | Indice de Masse Corporelle | $BMI_d = \frac{W_d}{H^2}$ |
| `bodyFatPercentage` | Taux de Masse Grasse | $BF_d = \left( \frac{FM_d}{W_d} \right) \times 100$ |
| `fatFreeBodyWeight` | Masse Hors Graisse | $FFM_d = W_d - FM_d$ |
| `muscleMass` | Masse Musculaire Totale | $MM_d = FFM_d \times 0.78$ |
| `skeletalMusclePercentage` | % Muscle Squelettique | $SMP_d = \left( \frac{MM_d}{W_d} \right) \times 100$ |
| `bodyWaterPercentage` | Pourcentage d'Eau Totale | $BWP_d = \frac{(FFM_d \times 0.73) + (FM_d \times 0.10)}{W_d} \times 100$ |
| `boneMass` | Masse Osseuse | $BM_d = BM_0 \quad \text{(Valeur Constante)}$ |
| `proteinPercentage` | Pourcentage de Protéines | $PP_d = \left( \frac{FFM_d \times 0.19}{W_d} \right) \times 100$ |
| `basalMetabolicRateBMR`| Métabolisme de Base | $BMR_d = 370 + (21.6 \times FFM_d)$ |
| `subcutaneousFat` | Graisse Sous-Cutanée | $SF_d = BF_d \times 0.88$ |
| `visceralFat` | Indice de Graisse Viscérale | $VF_d = \text{Arrondir}\left( FM_d \times 0.45 \right)$ |
| `metabolicAge` | Âge Métabolique | $MA_d = Age + \text{Limiter}\left((BMI_d - 22.0) \times 0.5, -5, 15\right)$ |

## IV. Règles de Distribution des Macro-nutriments (Profils)

L'ajustement des macro-nutriments s'effectue sur la base des calories cibles configurées (`Intake`), en respectant le poids du jour $W_d$.

1. **Profil Hyperprotéiné (Recommandé en déficit)**
   - Protéines : $W_d \times 2,2 \text{ g}$
   - Lipides : $(\text{Intake} \times 0,25) / 9 \text{ g}$
   - Glucides : $(\text{Intake} - (\text{Protéines} \times 4) - (\text{Lipides} \times 9)) / 4 \text{ g}$

2. **Profil Équilibré**
   - Protéines : $W_d \times 1,8 \text{ g}$
   - Lipides : $(\text{Intake} \times 0,30) / 9 \text{ g}$
   - Glucides : $(\text{Intake} - (\text{Protéines} \times 4) - (\text{Lipides} \times 9)) / 4 \text{ g}$

3. **Profil Low Fat**
   - Protéines : $W_d \times 1,8 \text{ g}$
   - Lipides : Plancher fixe de $40 \text{ g}$
   - Glucides : Enveloppe budgétaire restante

4. **Profil Low Carb**
   - Protéines : $W_d \times 2,0 \text{ g}$
   - Glucides : Plancher fixe de $75 \text{ g}$
   - Lipides : Enveloppe budgétaire restante

*   `Calories_Par_1000_Pas = Poids_Actuel * 0.5`
*   `Pas_Supplementaires_Requis = (Effort_Residuel / Calories_Par_1000_Pas) * 1000`
*   `Nouvelle_Cible_Pas = Objectif_Pas_Initial + Pas_Supplementaires_Requis`

*   **Contrainte d'Épuisement (Hard Guardrail)** : Si `Nouvelle_Cible_Pas > 18000`, le système force la valeur à 18 000 pas et déclenche l'alerte de révision de l'échéance temporelle (`SF-ANP-03`).
