# 📄 Cas d’Utilisation (Use Cases) : PulsePath Engine

## 1. Présentation
Les cas d’utilisation ci-dessous décrivent les interactions majeures entre l’**Utilisateur** et le **Moteur PulsePath**. Ils se concentrent sur les besoins fonctionnels nécessaires pour maintenir la "Source Unique de Vérité" (le journal quotidien) et générer les indicateurs de "Vélocité".

---

## 2. UC-01 : Saisir les métriques quotidiennes (Cœur du système)
*   **Acteur principal :** Utilisateur
*   **Objectif :** Enregistrer les données physiologiques et nutritionnelles pour mettre à jour la trajectoire.
*   **Pré-conditions :** L'utilisateur est authentifié ; les données de profil (poids cible, âge, taille) sont renseignées.
*   **Scénario nominal (Success Scenario) :**
    1. L'utilisateur sélectionne "Ajouter une entrée au journal".
    2. L'utilisateur saisit : **Poids**, **Calories**, **Macros (Protéines)**, **Heures de sommeil** et **Durée du jeûne**.
    3. Le système valide que les entrées sont dans des plages réalistes (ex: Calories > 500).
    4. Le système calcule le **Score d'Intégrité**.
    5. Le système enregistre l'entrée et déclenche automatiquement le recalcul de la vélocité.
*   **Post-conditions :** Les données sont sauvegardées et l'historique est mis à jour.
*   **Exceptions :**
    *   *Données invalides :* Le système affiche un message d'erreur et bloque l'enregistrement si les valeurs sont aberrantes (ex: Poids < 30kg).

---

## 3. UC-02 : Consulter la trajectoire dynamique (Analyse prédictive)
*   **Acteur principal :** Utilisateur
*   **Objectif :** Visualiser l'impact du comportement récent sur la date d'échéance finale.
*   **Pré-conditions :** Au moins 3 jours de saisies consécutives sont présents en base de données.
*   **Scénario nominal :**
    1. L'utilisateur navigue vers le tableau de bord "Trajectoire".
    2. Le système calcule la moyenne glissante du bilan énergétique net sur les 7 derniers jours.
    3. Le système applique la règle **RM-VEL-01** pour générer la nouvelle **Date d'Échéance Estimée (DEE)**.
    4. Le système affiche graphiquement le "Delta" (jours gagnés ou perdus) par rapport à la DEE initiale.
*   **Post-conditions :** L'utilisateur visualise clairement sa progression réelle au-delà du simple chiffre sur la balance.

---

## 4. UC-03 : Recevoir un "Coach Insight" automatisé
*   **Acteur principal :** Système (Automatisé)
*   **Objectif :** Fournir un conseil stratégique basé sur les corrélations de données.
*   **Scénario nominal :**
    1. Le système détecte une corrélation (ex: Sommeil < 7h entraînant une hausse calorique le lendemain).
    2. Le système génère une notification contextuelle personnalisée.
    3. L'utilisateur consulte l'insight et reçoit une recommandation actionnable (ex: "Améliorez votre sommeil pour stabiliser votre faim").
*   **Post-conditions :** L'utilisateur est éduqué sur l'interdépendance de ses métriques de santé.

---

# 5. UC-04 : Simulation de la Trajectoire de Composition Corporelle

## 1. Description
Permet à un utilisateur intermédiaire ou avancé de configurer une simulation dynamique de sa perte de gras sur une période donnée. Le système s'appuie sur les données réelles d'une balance connectée et applique le principe thermodynamique d'Alpert (69,2 kcal/kg de gras/jour) pour projeter l'évolution de 13 métriques corporelles et valider la sécurité du déficit calorique choisi.

## 2. Acteurs
- **Acteur Principal :** Utilisateur Authentifié (Athlète / Pratiquant)
- **Acteur Secondaire :** API de calcul PulsePath Engine (Backend .NET)

## 3. Préconditions
- L'utilisateur est connecté à son compte PulsePath.
- L'utilisateur dispose de données récentes synchronisées depuis sa balance intelligente (poids, taux de masse grasse initial, etc.).

## 4. Flux Principal (Scénario Nominal)
1. L'utilisateur accède à l'onglet "Simulateur Avancé de Composition Corporelle".
2. Le système charge automatiquement les 13 métriques initiales issues de la dernière pesée de la balance intelligente.
3. L'utilisateur saisit les paramètres de sa planification :
   - Durée de la simulation (en jours).
   - Objectif d'apport calorique quotidien (Intake).
   - Niveau d'activité physique (Multiplicateur TDEE).
   - Type de régime souhaité (Équilibré, Hyperprotéiné, Low Carb, Low Fat).
4. L'utilisateur clique sur "Lancer la Simulation".
5. Le système valide la cohérence des données et transmet le vecteur au moteur de calcul.
6. Le moteur de calcul applique la boucle itérative journalière (Principe d'Alpert) et génère :
   - Le résumé du transfert énergétique (Déficit optimal vs Déficit choisi).
   - Le tableau de projection des 13 métriques temporelles.
   - Les courbes d'évolution (Masse Grasse vs Masse Sèche).
7. Le système affiche les résultats sous forme de graphiques interactifs et de tableaux triables.

## 5. Flux Alternatifs (Exceptions et Variantes)
- **A1 : Alerte de Catabolisme Musculaire (Déficit Excessif)**
  - Si au cours de la simulation, le déficit calorique choisi dépasse le seuil critique de transfert de la masse grasse ($FM \times 69,2\text{ kcal}$), le système calcule la perte de muscle résultante.
  - Le système affiche une alerte visuelle orange/rouge indiquant : *"Attention, votre déficit dépasse votre capacité de mobilisation adipeuse. Risque de perte musculaire estimé à X.X kg sur la période."*
- **A2 : Absence de Données de Balance Connectée**
  - Si aucune donnée de balance n'est disponible, le système invite l'utilisateur à saisir manuellement les 13 métriques obligatoires ou utilise un profil morphologique par défaut basé sur le ratio poids/taille/âge.

## 6. Postconditions
- La simulation est générée avec succès.
- L'utilisateur peut exporter la projection au format CSV ou sauvegarder ce scénario de planification dans son profil.

---
# 6. UC-05 : Agrégation Automatique et Traitement de la Télémétrie Biométrique

## 1. Description
Permet à l'utilisateur de centraliser et d'automatiser la collecte de ses données métaboliques et biométriques à partir de son écosystème matériel (Balance connectée, Apple Health, MyFitnessPal, Garmin/Apple Watch). Le système analyse ces flux en continu pour mettre à jour la boucle thermodynamique de simulation et évaluer la récupération du système nerveux central (SNC). Un mécanisme de repli (fallback) manuel est disponible à tout moment en cas de défaillance des API tierces.

## 2. Acteurs
- **Acteur Principal :** Utilisateur Authentifié (Athlète)
- **Acteurs Secondaires (Systèmes) :** 
  - API Cloud/SDK Balance (Renpho / Withings)
  - API Nutrition (MyFitnessPal Core API)
  - Passerelle d'Activité mobile (Apple HealthKit / Garmin Connect API)

## 3. Préconditions
- L'utilisateur possède des jetons d'autorisation OAuth2 valides pour chaque service externe connecté.
- L'application mobile PulsePath dispose des autorisations système de lecture sur Apple HealthKit (iOS).

## 4. Flux Principal (Scénario Nominal : Synchronisation Automatique)
1. L'application déclenche une tâche d'arrière-plan synchrone ou asynchrone (Web Job / Background Service) à intervalles réguliers.
2. **Collecte Biométrique :** Le système interroge l'API Cloud de la balance connectée et extrait le vecteur brut des 13 métriques corporelles.
3. **Collecte Nutritionnelle :** Le système interroge l'API MyFitnessPal et extrait les calories totales consommées ainsi que la structure exacte des macro-nutriments (Protéines, Glucides, Lipides) du journal alimentaire.
4. **Collecte de l'Activité & Récupération :** Le système extrait le nombre de pas quotidiens via Apple Health et extrait la Fréquence Cardiaque au Repos (RHR) ainsi que la Variabilité de la Fréquence Cardiaque (HRV) depuis Garmin/Apple Watch.
5. Le système valide la qualité des données (absence de doublons, valeurs aberrantes).
6. Le système recalcule la tendance lissée (Rolling Average) de l'utilisateur et actualise automatiquement les prévisions de sa trajectoire de masse grasse.

## 5. Flux Alternatifs (Exceptions et Repli Manuel)
- **A1 : Échec d'Authentification / Jeton OAuth2 Expiré**
  - Le système suspend la synchronisation automatique du service concerné.
  - Le système lève une alerte discrète dans l'interface Angular : *"Action requise : Reconnectez votre compte [Nom du Service] pour éviter l'interruption des données."*
- **A2 : Mode de Repli Manuel Intégral (Fallback)**
  - Si l'utilisateur refuse de lier des API tierces ou si un service tiers est indisponible (Panne d'API MyFitnessPal ou Renpho).
  - L'utilisateur accède au formulaire de saisie manuelle.
  - L'utilisateur saisit manuellement son poids, ses calories du jour, ses macros et ses pas.
  - Le système accepte les entrées manuelles, les marque avec le tag `Source: Manual` dans la base de données, et force l'exécution de la boucle de calcul.

## 6. Postconditions
- La base de données temporelle (Time-Series) est alimentée avec les métriques biométriques du jour.
- Les indicateurs de fatigue du SNC (HRV/RHR) sont mis à disposition du tableau de bord d'analyse.

---
# UC-06 : Planification et Génération de Menus Nutritionnels Intelligents

## 1. Description
Permet à l'utilisateur de configurer, générer et personnaliser un plan de repas sur 7 jours adapté à ses objectifs physiques (perte de gras via le moteur d'Alpert, maintien ou gain musculaire) et à ses préférences diététiques. Le système compile automatiquement les besoins macro-nutritionnels calculés, génère une liste de courses catégorisée par rayon, fournit des instructions de préparation de repas (Meal Prep) et permet des substitutions à la volée.

## 2. Acteurs
- **Acteur Principal :** Utilisateur Authentifié (Athlète)
- **Acteur Secondaire :** Moteur de Calcul Nutritionnel PulsePath Engine (Backend .NET)

## 3. Préconditions
- L'utilisateur a configuré ses données anthropométriques et ses cibles journalières dans son profil ou via la simulation de composition corporelle (`UC001`).

## 4. Flux Principal (Scénario Nominal)
1. L'utilisateur accède au module "Générateur de Plan de Repas".
2. L'utilisateur configure les paramètres de sa planification :
   - Nombre de jours (par défaut 7 jours).
   - Nombre de repas par jour (Fréquence).
   - Nombre de portions par repas et nombre de personnes (Modèle Familial).
   - Type de régime (Hyperprotéiné, Low Fat, Low Carb, Équilibré, Végétarien, Keto, Family-Friendly).
3. L'utilisateur affine ses préférences :
   - Aliments favoris par groupe (Sources de Protéines, Glucides, Lipides).
   - Aliments à exclure (Allergies ou aversions).
   - Temps limite de préparation par repas (ex: moins de 20 minutes).
4. L'utilisateur clique sur "Générer le Plan de Repas".
5. Le système interroge la base de données des recettes, applique l'algorithme d'optimisation de sac à dos (Knapsack Problem) pour faire correspondre le total calorique et les macros sur les 7 jours de la grille calendaire.
6. Le système affiche le calendrier hebdomadaire avec une vue quotidienne détaillée de la répartition des calories et macro-nutriments.
7. L'utilisateur clique sur "Générer la Liste de Courses".
8. Le système consolide tous les ingrédients des recettes planifiées, les multiplie par le nombre de portions/personnes et produit une liste d'achat catégorisée par rayons.

## 5. Flux Alternatifs (Modifications et Gestion)
- **A1 : Remplacement d'un repas (Swap Feature)**
  - L'utilisateur n'aime pas une recette proposée sur un jour donné.
  - Il clique sur le bouton "Remplacer le repas".
  - Le système effectue une recherche filtrée et présente des alternatives de recettes isoménus (calories et macronutriments similaires à $\pm 5\%$).
  - L'utilisateur valide le remplacement; la liste de courses globale est automatiquement recalculée.
- **A2 : Enregistrement de Repas Favoris & Ajout Rapide**
  - L'utilisateur peut marquer un repas ou une combinaison d'aliments comme "Favori".
  - À tout moment dans le calendrier, il peut faire un "Ajout Rapide" de ce favori, ce qui écrase la suggestion automatique et recalcule le solde énergétique de la journée.

## 6. Postconditions
- Le plan de repas 7 jours est enregistré dans l'agenda de l'utilisateur.
- La liste de courses est disponible pour exportation (PDF ou synchronisation sur l'application mobile).

---
## 7. Matrice de Priorité des Cas d'Utilisation

| ID | Nom du Cas d'Utilisation | Importance Métier | Complexité |
| :--- | :--- | :--- | :--- |
| **UC-01** | Saisie des métriques | 🔴 Critique | Basse |
| **UC-02** | Consultation Trajectoire | 🔴 Critique | Haute |
| **UC-03** | Coach Insights | 🟠 Haute | Moyenne |
| **UC-04** |  Simulation Dynamique | 🟠 Haute | Moyenne |
| **UC-05** | Télémétrie Biométrique | 🔴 Critique | Basse |
| **UC-06** | Menus Nutritionnels | 🔴 Critique | Basse |
