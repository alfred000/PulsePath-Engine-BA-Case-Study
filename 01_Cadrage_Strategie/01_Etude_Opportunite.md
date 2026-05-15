# 📄 Étude d'Opportunité : PulsePath Engine (Version Évoluée)

## 1. Résumé Exécutif
L'industrie des applications de fitness échoue à maintenir l'engagement utilisateur car elle propose un suivi passif et déconnecté de la réalité biologique. L'utilisateur enregistre des données sans comprendre ses métriques de maintenance ni l'interdépendance de ses facteurs de santé. **PulsePath Engine** révolutionne cette approche en instaurant un parcours en 3 étapes : Diagnostic de départ $\rightarrow$ Planification d'Objectifs S.M.A.R.T $\rightarrow$ Suivi prédictif via Journal Quotidien (Source Unique de Vérité).

## 2. Analyse du Problème (Pain Points)
*   **Objectifs irréalistes (Non-SMART)** : Les utilisateurs fixent des poids cibles arbitraires sans connaître leur plage de poids bien-être ni leur métabolisme de maintenance.
*   **Friction de saisie manuelle** : Les applications actuelles exigent trop d'efforts manuels. Bien que le MVP utilise un modèle déclaratif, l'architecture doit nativement anticiper l'intégration d'API (Apple Health, Garmin, balances connectées).
*   **Manque de granularité métabolique** : L'Indice de Masse Corporelle (IMC) seul est insuffisant ; il doit être croisé avec l'Indice de Masse Grasse (IMG) pour éviter les erreurs d'interprétation (ex: profils athlétiques).

## 3. Objectifs Stratégiques (S.M.A.R.T)
*   **Spécifique & Mesurable** : Définir une plage de poids bien-être stricte (IMC entre 18.5 et 25) et un ratio de macro-nutriments adapté à l'objectif choisi.
*   **Atteignable & Réaliste** : Brider les déficits caloriques à un maximum de 1% du poids de corps par semaine.
*   **Temporellement défini** : Recalculer dynamiquement l'échéance par un moteur de vélocité basé sur une moyenne glissante de 7 jours.

# 4. Analyse des Parties Prenantes (Stakeholders)
* Utilisateurs Finaux : Personnes souhaitant perdre du gras tout en préservant leur masse musculaire.
* Coachs/Nutritionnistes : Besoin d'un outil de suivi précis pour leurs clients sans analyse manuelle fastidieuse.
* Développeurs : Besoin de spécifications claires et de règles métier mathématiques pour l'implémentation.

# 5. Solution Proposée
Le PulsePath Engine, un moteur d'analyse métier qui centralise :
* Un algorithme de dépense énergétique dynamique (TDEE).
* Un module de suivi de vélocité (recalcul de la trajectoire).
* Un système de coaching par exception (alertes si les macros ou le sommeil dévient).

# 6. Critères de Succès (KPIs)
* Précision : Écart moyen entre la date d'échéance prédite et réelle < 10%.
* Engagement : Augmentation du taux de complétude du journal (Score d'Intégrité > 80%).
* Rétention : Réduction du taux d'abandon après les 30 premiers jours (période critique).

# 7. Analyse de Valeur
En tant que Business Analyst, mon rôle est de maximiser la valeur pour l'utilisateur en priorisant les fonctionnalités ayant le plus d'impact sur sa réussite, tout en minimisant la complexité technique pour les développeurs.
