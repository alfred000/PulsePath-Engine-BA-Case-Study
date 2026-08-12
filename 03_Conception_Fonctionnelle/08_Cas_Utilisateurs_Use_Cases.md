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
## 6. Matrice de Priorité des Cas d'Utilisation

| ID | Nom du Cas d'Utilisation | Importance Métier | Complexité |
| :--- | :--- | :--- | :--- |
| **UC-01** | Saisie des métriques | 🔴 Critique | Basse |
| **UC-02** | Consultation Trajectoire | 🔴 Critique | Haute |
| **UC-03** | Coach Insights | 🟠 Haute | Moyenne |
| **UC-04** |  Simulation Dynamique | 🟠 Haute | Moyenne |
