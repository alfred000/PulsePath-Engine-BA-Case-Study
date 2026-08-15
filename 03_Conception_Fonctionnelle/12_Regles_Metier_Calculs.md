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
---
# 15. RM-PA-01 : Module de Planification Alimentaire

Ce document spécifie les règles logiques d'agrégation, les tolérances de calcul algorithmique et les formules de mise à l'échelle des portions utilisées par le planificateur PulsePath Engine.

## I. Règles de Gestion (Business Rules)

### RG005 : Règle de Tolérance d'Ajustement Macro-nutritionnel
Pour être validé par le système, le plan d'une journée complète doit respecter l'encadrement mathématique suivant par rapport aux cibles théoriques de l'utilisateur :
- $\text{Calories Totales} = \text{Cible} \pm 50 \text{ kcal}$
- $\text{Protéines Totales} = \text{Cible} \pm 5 \text{ g}$

### RG006 : Priorisation des Aliments à Exclure (Blacklist)
La liste des aliments à éviter configurée par l'utilisateur agit comme un filtre de sécurité prioritaire de niveau 0 (Masquage SQL hard filter). Si une recette contient ne serait-ce qu'un seul ingrédient présent dans la liste d'exclusion, elle est définitivement bannie de l'arbre de sélection de l'algorithme de génération.

### RG007 : Règle d'Isoménu pour la Substitution (Swap)
Lorsqu'un utilisateur demande le remplacement d'un repas dans le calendrier, le moteur de recherche filtre les recettes alternatives selon l'indice de proximité macronutritionnelle ($IPM$) :
$$\text{Proximité Calories} \le 10\% \quad \text{et} \quad \text{Proximité Protéines} \le 10\%$$

## II. Formules Mathématiques de Mise à l'Échelle et d'Agrégation

### 1. Ajustement Dynamique des Portions et des Personnes
Les recettes en base de données possèdent un poids d'ingrédient de référence calibré pour **1 portion standard (1 personne)**. 
Lors de l'intégration dans le calendrier, le système calcule le multiplicateur de masse de l'ingrédient ($M_i$) :

$$M_i = \text{Portions Demandées} \times \text{Nombre de Personnes}$$

*Exemple d'application :* Si une recette de Chili de Bœuf requiert 120g de viande hachée de référence pour 1 portion, et que l'utilisateur planifie ce repas pour 2 portions par personne pour une tablée de 4 personnes :
$$M_i = 2 \times 4 = 8$$
$$\text{Quantité requise en cuisine} = 120\text{g} \times 8 = 960\text{g}$$

### 2. Algorithme de Consolidation de la Liste de Courses (Mise à plat)
Pour générer la liste de courses, le système parcourt la matrice des repas planifiés sur la période $P$, extrait les ingrédients, convertit les unités dans un référentiel standardisé (Grammes ou Millilitres) et applique la formule de sommation par groupe d'ingrédient unique ($I$) et par rayon ($R$) :

$$\text{Quantité Totale L'ingrédient } I = \sum_{d=1}^{7} \sum_{m=1}^{M} \left( Q_{\text{réf}}(I) \times M_i \right)$$

Où :
- $d$ représente le jour de la semaine (1 à 7).
- $m$ représente le repas de la journée (Repas 1 à $M$).
- $Q_{\text{réf}}(I)$ est la quantité unitaire de base de l'ingrédient dans la recette sélectionnée.

### 3. Profils de Distribution Calorique par Repas (Fréquence)
Selon le nombre de repas par jour choisi par l'utilisateur, l'enveloppe calorique globale journalière ($\text{Intake}$) est segmentée par le backend .NET selon les coefficients d'impact suivants pour équilibrer la satiété :

| Fréquence de Repas | Repas 1 (Petit-Déj) | Repas 2 (Déjeuner) | Repas 3 (Collation) | Repas 4 (Dîner) |
| :--- | :--- | :--- | :--- | :--- |
| **3 Repas / jour** | $30\%$ du total kcal | $40\%$ du total kcal | — | $30\%$ du total kcal |
| **4 Repas / jour** | $25\%$ du total kcal | $35\%$ du total kcal | $15\%$ du total kcal | $25\%$ du total kcal |
| **5 Repas / jour** | $20\%$ du total kcal | $30\%$ du total kcal | $15\%$ du total kcal | $25\%$ du total kcal + (Collation 2 : $10\%$) |
---
# 16. RM-APA-01 : Moteur d'Analyse de Performance Athlétique

Ce document formalise les algorithmes d'estimation de puissance, les calculs de répartition de fonte pour les barres, et les règles d'évaluation de la fatigue de la carte thermique musculaire.

## I. Règles de Gestion (Business Rules)

### RG008 : Protection de la Surcharge Progressive (Règle d'Incrémentation)
Le poids de référence pré-rempli pour la séance $N$ correspond à la charge maximale avec laquelle l'utilisateur a réussi à atteindre la borne haute de sa fourchette de répétitions cible lors de la séance $N-1$. 

### RG009 : Exclusion des Séries d'Échauffement du Volume Effectif
Lors du calcul du volume d'entraînement total d'un muscle ou d'une séance, le système doit ignorer toutes les séries marquées du tag `Warmup`. Seules les séries `WorkingSet`, `DropSet` et `Failure` entrent dans l'équation de calcul du volume de croissance.

### RG010 : Règle de Priorité du Calculateur de Plaques (Plate Calculator)
Le calculateur de plaques doit toujours chercher à minimiser le nombre total de disques disposés sur la barre en sélectionnant d'abord les unités de masse les plus élevées disponibles en salle (Ordre de priorité : 25 kg > 20 kg > 15 kg > 10 kg > 5 kg > 2.5 kg > 1.25 kg).

## II. Formules Mathématiques et Algorithmes Quantitatifs

### 1. Algorithme d'Estimation de la Force Maximale (Formule d'Epley pour le 1RM)
Pour estimer le poids maximal théorique qu'un athlète peut soulever sur 1 seule répétition (1RM) à partir d'une série menée proche de l'échec ($Reps \le 12$), le système applique la formule d'Epley :

$$\text{1RM Estimé} = Poids \times \left( 1 + \frac{Reps}{30} \right)$$

*Exemple d'application :* Si lors du Day 1, vous validez une série lourde au développé couché à 90 kg pour 8 répétitions :
$$\text{1RM} = 90 \times \left( 1 + \frac{8}{30} \right) = 90 \times 1.2667 = 114\text{ kg}$$

### 2. Algorithme Mathématique du Calculateur de Plaques (Barbell Load Decomposition)
Soit $M_{\text{cible}}$ la masse totale souhaitée par l'athlète et $M_{\text{barre}}$ la masse à vide de la barre olympique choisie (ex: 20 kg). La masse résiduelle à répartir équitablement de chaque côté de la barre ($M_{\text{côté}}$) est définie par :

$$M_{\text{côté}} = \frac{M_{\text{cible}} - M_{\text{barre}}}{2}$$

L'algorithme de division entière itère ensuite sur le tableau trié des disques disponibles $[25, 20, 15, 10, 5, 2.5, 1.25]$ pour trouver le nombre de disques $N_d$ par côté :

$$N_d = \lfloor \frac{M_{\text{côté}}}{Poids_{\text{disque}}} \rfloor$$
$$M_{\text{côté}} = M_{\text{côté}} \pmod{Poids_{\text{disque}}}$$

*Exemple de trace d'exécution pour charger une barre à 102.5 kg avec une barre de 20 kg :*
- $M_{\text{côté}} = (102.5 - 20) / 2 = 41.25\text{ kg}$ par côté.
- Étape 1 (Disque 25 kg) : $N_{25} = \lfloor 41.25 / 25 \rfloor = 1$ disque. Reste = $16.25\text{ kg}$.
- Étape 2 (Disque 20 kg) : Reste inférieur à 20. $N_{20} = 0$.
- Étape 3 (Disque 15 kg) : $N_{15} = \lfloor 16.25 / 15 \rfloor = 1$ disque. Reste = $1.25\text{ kg}$.
- Étape 4 (Disques 10, 5, 2.5 kg) : Reste trop bas, compteurs à 0.
- Étape 5 (Disque 1.25 kg) : $N_{1.25} = \lfloor 1.25 / 1.25 \rfloor = 1$ disque. Reste = $0\text{ kg}$.
- **Résultat UI :** Placez 1 disque de 25 kg, 1 disque de 15 kg et 1 disque de 1.25 kg de chaque côté.

### 3. Calcul de l'Indice d'Épuisement Tissulaire (Muscle Heatmap Index)
La coloration de chaque groupe musculaire (ex: Pectoraux, Quadriceps) sur la carte de chaleur 3D de l'interface Angular dépend de l'Indice de Fatigue Accumulée ($IFA$) calculé sur une fenêtre glissante de 48 heures :

$$IFA = \sum_{\text{séries}} \left( \text{Poids} \times \text{Reps} \times \text{CoefficientType} \right) \times \text{FacteurDégradationHRV}$$

Où les coefficients de stress valent :
- $\text{WorkingSet} = 1.0$
- `Failure` $= 1.4$ (L'échec engendre un stress nerveux supérieur)
- `DropSet` $= 1.2$
- $\text{FacteurDégradationHRV} = \text{Si la HRV du jour (UC002) est basse, multiplier l'IFA par 1.25 pour ralentir la récupération visuelle.}$$
---

# 17. RM-MML-01 : Moteur Métabolique et Modélisation Linéaire

Ce document formalise les équations de base du métabolisme, la constante de conversion de masse, et l'algorithme de projection itérative hebdomadaire calqué sur l'outil de référence.

## I. Règles de Gestion (Business Rules)

### RG011 : Constante Énergétique du Changement de Poids
Le système utilise la constante standardisée de **7 700 kcal pour 1 kilogramme de variation de poids corporel**. Toute accumulation ou restriction de 7 700 kcal par rapport à la maintenance est traduite par un gain ou une perte de 1 kg de masse.

### RG012 : Algorithme de Rescaling Forcé des Macros
Si la somme des pourcentages des macros ($P_{\%} + C_{\%} + F_{\%}$) est différente de $100$, le système applique un facteur de correction $K = \frac{100}{P_{\%} + C_{\%} + F_{\%}}$. Les nouveaux pourcentages appliqués pour le calcul des grammes sont :
$$P_{\text{final}} = P_{\%} \times K, \quad C_{\text{final}} = C_{\%} \times K, \quad F_{\text{final}} = F_{\%} \times K$$

## II. Formules Mathématiques et Analyse Métabolique (Données Démo)

### 1. Calcul du BMR (Mifflin-St Jeor par défaut)
Pour un individu de sexe masculin :
$$\text{BMR} = (10 \times \text{Poids (kg)}) + (6.25 \times \text{Taille (cm)}) - (5 \times \text{Âge (ans)}) + 5$$

*Application avec les données de démonstration (72 kg, 170 cm, 30 ans) :*
$$\text{BMR} = (10 \times 72) + (6.25 \times 170) - (5 \times 30) + 5$$
$$\text{BMR} = 720 + 1062.5 - 150 + 5 = 1637.5 \text{ kcal}$$

### 2. Multiplicateurs d'Activité Standardisés (TDEE)
Le TDEE initial est obtenu en multipliant le BMR par le facteur de l'activité choisie :
- Sédentaire : $\times 1.2$
- Légèrement Actif : $\times 1.375$
- Modérément Actif (Choix Démo) : $\times 1.55$
- Actif : $\times 1.725$
- Très Actif : $\times 1.9$

$$\text{TDEE}_{\text{Initial}} = 1637.5 \times 1.55 \approx 2538 \text{ kcal}$$

### 3. Résolution Mathématique par Mode : Mode Target by Date
Pour un objectif de poids de $65.8 \text{ kg}$ sur une durée de $18 \text{ semaines}$ ($126 \text{ jours}$) :
- Poids total à perdre : $72 - 65.8 = 6.2 \text{ kg}$
- Énergie totale du déficit requise : $6.2 \text{ kg} \times 7700 \text{ kcal/kg} = 47740 \text{ kcal}$
- Déficit quotidien requis : $47740 \text{ kcal} / 126 \text{ jours} \approx 379 \text{ kcal/jour}$
- **Apport Calorique Quotidien Cible :** $\text{TDEE}_{\text{Initial}} - 379 = 2538 - 379 = 2159 \text{ kcal/jour}$

## III. Algorithme de Projection Hebdomadaire Dynamique

Pour générer le tableau de projection pas à pas :
- Perte de poids hebdomadaire : $\frac{379 \text{ kcal/jour} \times 7 \text{ jours}}{7700 \text{ kcal/kg}} \approx 0.344 \text{ kg/semaine}$

À la fin de chaque semaine $n$, le système met à jour le poids et recalcule le TDEE pour la ligne suivante afin de simuler l'adaptation métabolique :

$$\text{Poids}_{n} = \text{Poids}_{n-1} - 0.344$$
$$\text{BMR}_{n} = (10 \times \text{Poids}_{n}) + (6.25 \times 170) - (5 \times 30) + 5$$
$$\text{TDEE}_{n} = \text{BMR}_{n} \times 1.55$$

### Extrait de la Matrice Finale de Projection Générée :

| Semaine | Poids Projeté (kg) | TDEE Projeté (kcal) | Apport Recommandé (kcal) |
| :--- | :--- | :--- | :--- |
| Semaine 0 (Initial) | 72.00 kg | 2538 kcal | 2159 kcal |
| Semaine 1 | 71.66 kg | 2533 kcal | 2159 kcal |
| Semaine 2 | 71.31 kg | 2527 kcal | 2159 kcal |
| ... | ... | ... | ... |
| Semaine 18 (Final) | 65.80 kg | 2442 kcal | 2159 kcal |

## IV. Algorithme de Résolution Hybride : Mode Target Body Fat (Alpert Integration)

Lorsque l'utilisateur choisit le ciblage par taux de masse grasse, les calculs de l'apport énergétique quotidien (`Intake`) abandonnent la linéarité des 7 700 kcal et se synchronisent sur l'état dynamique du tissu adipeux de la semaine en cours :

1. **Calcul du Poids Final Théorique ($W_{\text{cible}}$)** :
   $$FFM_{\text{initial}} = W_{\text{actuel}} \times \left(1 - \frac{BF_{\text{actuel}}}{100}\right)$$
   $$W_{\text{cible}} = \frac{FFM_{\text{initial}}}{1 - \left(\frac{\text{targetBodyFatPercentage}}{100}\right)}$$

2. **Génération Récursive Non-Linéaire de la Trajectoire (Boucle de calcul du Planner)** :
   Pour chaque semaine de projection $n$, le déficit maximal autorisé s'adapte à la raréfaction des stocks de gras :
   $$FM_n = W_n \times \left(\frac{BF_n}{100}\right)$$
   $$\text{Déficit Autorisé Quotidien}_n = FM_n \times 69.2 \text{ kcal/jour}$$
   $$\text{Apport Recommandé Cible}_n = TDEE_n - \text{Déficit Autorisé Quotidien}_n$$
   $$\text{Perte de poids pour la semaine } n = \frac{\text{Déficit Autorisé Quotidien}_n \times 7 \text{ jours}}{9440 \text{ kcal/kg}}$$

La simulation s'arrête automatiquement et valide l'échéance temporelle finale (Deadline) dès que $W_n \le W_{\text{cible}}$.
---
# 18. RM-LC-01 : Logique Comportementale et Facteur d'Activité

Ce document détaille les règles algorithmiques de calcul du NEAT, la notation de la régularité du sommeil et le fonctionnement du minuteur de jeûne en arrière-plan.

## I. Règles de Gestion (Business Rules)

### RG013 : Calcul du Taux de Complétion Journalier des Habitudes
Pour qu'une journée soit marquée comme "Validée" dans le calendrier des habitudes de perte de gras, les conditions minimales suivantes doivent être réunies simultanément :
- $\text{Pas Quotidiens} \ge \text{Cible Choisie (ex: 8000)}$
- $\text{Durée Sommeil} \ge 7 \text{ heures}$
- $\text{Durée du Jeûne Actualisé} \ge \text{Durée Protocole (ex: 16 heures)} - 15 \text{ min (Marge de tolérance)}$

### RG014 : Règle d'Inactivité Horaire (Sedentary Alert)
L'algorithme de détection de la sédentarité analyse les pas heure par heure entre 08h00 et 20h00. Si le différentiel de pas d'une heure donnée est inférieur à 250 pas à la 50ème minute de l'heure en cours, le système génère un signal d'alerte de mouvement.

## II. Formules Mathématiques et Analyse Quantitative

### 1. Modélisation de l'Indice d'Efficacité du Sommeil ($IES$)
Le score de sommeil affiché sur l'interface Angular (sur une échelle de 100) intègre la durée totale ($D$), la proportion de sommeil profond ($SP$) et la régularité horaire ($R$) :

$$IES = \left( \text{FacteurDurée} \times 0,5 \right) + \left( \frac{SP}{\text{Sommeil Total}} \times 100 \times 0,3 \right) + \left( R \times 0,2 \right)$$

Où :
- $\text{FacteurDurée} = 100$ si $7 \le D \le 9 \text{ heures}$. Si $D < 7$, $\text{FacteurDurée} = (D / 7) \times 100$.
- $R$ est un score de 0 à 100 inversement proportionnel à la variance en minutes de l'heure de coucher par rapport à la moyenne des 7 derniers jours.

### 2. Algorithme du Minuteur de Jeûne en Arrière-plan
Pour éviter de vider la batterie du téléphone de l'utilisateur, le compte à rebours de l'interface Angular ne doit pas dépendre d'un processus d'exécution continue en tâche de fond. Le système s'appuie sur des calculs d'horodatages absolus de type Unix (Unix Timestamps) :

Lors du clic sur "Démarrer le Jeûne" à l'instant $T_{\text{début}}$ :
$$T_{\text{fin théorique}} = T_{\text{début}} + \left( \text{Heures du Protocole} \times 3600 \right)$$

À chaque cycle de rafraîchissement de l'interface graphique du Frontend Angular ($T_{\text{actuel}}$), le temps restant affiché ($Temps_{\text{restant}}$ en secondes) vaut :
$$Temps_{\text{restant}} = T_{\text{fin théorique}} - T_{\text{actuel}}$$

Si $Temps_{\text{restant}} \le 0$, l'état de l'interface bascule automatiquement sur `State : FeedingWindowOpen` et appelle le service de notification du backend .NET.

