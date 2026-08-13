# 📄 User Stories Détaillées : PulsePath Engine

## Présentation
Ce document détaille les spécifications fonctionnelles sous forme de **User Stories**. Chaque story définit un besoin utilisateur, sa valeur métier et les critères précis permettant de valider sa bonne implémentation (Definition of Done).

---

## US-01 : Recalcul dynamique de l'échéance cible
**Priorité :** Must-have | **Estimation :** 5 SP

*   **Énoncé :** En tant qu'utilisateur, je veux que le système mette à jour ma date d'échéance estimée dès que je saisis mes données du jour, afin de comprendre l'impact immédiat de ma discipline sur mon objectif.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : Le calcul doit impérativement utiliser la règle **RM-VEL-01** (moyenne glissante sur 7 jours).
    *   **CA 2** : Si le déficit cumulé augmente, la date doit avancer ; s'il y a un surplus, la date doit reculer.
    *   **CA 3** : L'interface doit afficher le "Delta" de jours (ex: +2 jours) de manière visuelle (Vert pour une avance, Rouge pour un recul).
    *   **CA 4** : En l'absence de données suffisantes (< 3 jours), le système doit afficher "Calcul en cours..." pour éviter toute donnée erronée.

---

## US-03 : Algorithme de TDEE dynamique (Pas)
**Priorité :** Must-have | **Estimation :** 5 SP

*   **Énoncé :** En tant qu'utilisateur, je veux que ma dépense énergétique soit ajustée en fonction de mon activité réelle (pas), afin d'avoir un bilan calorique net précis chaque jour.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : Le système doit appliquer les paliers d'activité définis dans la règle **RM-MET-01**.
    *   **CA 2** : Si le log de pas est vide, le système doit appliquer par défaut le multiplicateur "Sédentaire" (1.2).
    *   **CA 3** : Le TDEE recalculé doit être visible sur l'interface de saisie pour donner un feedback immédiat à l'utilisateur.

---

## US-04 : Suivi de la cible de Protéines
**Priorité :** Should-have | **Estimation :** 3 SP

*   **Énoncé :** En tant qu'utilisateur, je veux visualiser mon apport protéique par rapport à ma cible personnalisée, afin de m'assurer que je préserve ma masse musculaire pendant ma perte de poids.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : La cible doit être calculée selon le poids cible de l'utilisateur (2g / kg de poids cible).
    *   **CA 2** : Une jauge de progression doit s'afficher en temps réel lors de la saisie.
    *   **CA 3** : Si l'apport est inférieur à 1.5g/kg de poids actuel, une icône d'alerte orange doit apparaître.

---

## US-05 : Tracking de la fenêtre de Jeûne Intermittent
**Priorité :** Should-have | **Estimation :** 5 SP

*   **Énoncé :** En tant qu'utilisateur, je veux enregistrer mes heures de début et de fin de repas, afin de vérifier le respect de mon protocole de jeûne.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : Le système doit calculer la durée entre l'heure du premier repas et celle du dernier.
    *   **CA 2** : Si cette durée est ≤ 8 heures, la journée est validée "Objectif Jeûne Atteint".
    *   **CA 3** : Le système doit empêcher la saisie d'une heure de fin antérieure à l'heure de début sur la même journée civile.

---
## US-06 : Inscription & Connexion Sécurisée 
**Priorité :** Should-have | **Estimation :** 5 SP

*   **Énoncé :**  En tant que nouvel utilisateur, Je veux créer un compte avec un email et un mot de passe sécurisé, puis m'authentifier, Afin de protéger mes données physiologiques personnelles et accéder à mon espace sécurisé.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : Sécurité du stockage (Back-End)
        *   **Given** Un utilisateur qui soumet le formulaire d'inscription.
        *   **When** L'API traite la demande d'inscription.
        *   **Then** Le mot de passe doit être obligatoirement salé et haché avec l'algorithme BCrypt avant l'insertion dans la table `Users` de SQLite.
    *   **CA 2** : Délivrance du Jeton de Session (JWT)
        *    **Given** Un utilisateur enregistré qui fournit des identifiants valides sur la route `/api/auth/login`.
        *    **When** L'authentification réussit.
        *    **Then** Le serveur doit retourner un code `200 OK` contenant un token JWT valide signé contenant le `UserId`.
    *   **CA 3** : Gestion des erreurs de connexion
         *    **Given** Un utilisateur saisissant un email inexistant ou un mot de passe erroné.
         *    **When** La requête de connexion est envoyée.
         *    **Then** L'API doit retourner une erreur standardisée `401 Unauthorized` avec un message générique pour éviter l'énumération de comptes.

---

## US-07 : Initialisation du Profil Métabolique (Should)
**Priorité :** Should-have | **Estimation :** 5 SP

*   **Énoncé :** En tant qu'utilisateur authentifié, Je veux renseigner mon profil biologique (âge, sexe, taille, poids, facteur d'activité), Afin de fournir au système expert les variables nécessaires au calcul de mon métabolisme de base (BMR).

*   **Critères d'Acceptation (CA) :**
   *    **CA 1** : Validation stricte des données d'entrée (Front-End & Back-End)
        *    **Given** L'écran de configuration du profil utilisateur.
        *    **When** L'utilisateur valide le formulaire.
        *    **Then** Les champs doivent respecter les bornes de sécurité suivantes : *Âge [15 - 90 ans]*, *Taille [100 - 250 cm]*, *Poids [40 - 250 kg]*.
        *    **And** Si une valeur est hors bornes, le bouton de validation reste désactivé et l'API rejette un code `400 Bad Request`.
   *   **CA 2** : Persistance et isolation des données
       *    **Given** Une requête valide `POST /api/profile`.
       *    **When** L'API persiste les données dans le modèle `UserProfile` de la base SQLite.
       *    **Then** Les données physiologiques doivent être strictement associées au `UserId` extrait du token JWT de la session courante.

---

## US-08 : Définition d'Objectifs S.M.A.R.T (Should)
**Priorité :** Should-have | **Estimation :** 5 SP

*   **Énoncé :** En tant qu'utilisateur ayant configuré son profil, Je veux définir un objectif de poids et un rythme hebdomadaire de progression, Afin de générer automatiquement ma prescription calorique et mes macros sans mettre ma santé en danger.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : Garde-fou de sécurité métabolique (RM-GOAL-01)
        * **Given** Un utilisateur ciblant une perte de poids agressive.
        * **When** L'utilisateur tente de valider un rythme supérieur à **1% de son poids corporel par semaine** OU un déficit qui abaisse ses calories sous son **BMR de survie**.
        * **Then** Le système bloque la validation.
        * **And** Le système affiche une alerte contextuelle de sécurité et refuse de générer la feuille de route.
    *   **CA 2** : Calcul instantané de la feuille de route
        * **Given** Un objectif physiologiquement validé par le système expert.
        * **When** L'enregistrement de l'objectif est confirmé en base de données.
        * **Then** Le système calcule instantanément et affiche sur le Dashboard les valeurs cibles de la phase.
        * **And** Ces valeurs incluent le **TDEE dynamique**, les **calories cibles journalières**, et la répartition des **macro-nutriments** (RM-MAC-01).

---

## US-09 : Suivi des Progrès & Journal Quotidien (Should)
**Priorité :** Should-have | **Estimation :** 8 SP (Estimation augmentée à cause de la complexité des calculs de durées et des nouvelles sections)

*   **Énoncé :** utilisateur engagé dans mon optimisation métabolique,Je veux disposer d'un journal quotidien unique, trié par date et navigable, pour consigner mes calories, macros, poids, pas, entraînements, fenêtres de jeûne et sommeil,Afin de centraliser toutes mes données vitales,       alimenter l'algorithme de trajectoire et suivre précisément l'évolution de mes progrès.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : Navigation temporelle et Unicité (Idempotence)
        * **Given**  Un utilisateur connecté sur son Dashboard.
        * **When** Il accède à la page /diary.
        * **Then** Le système charge par défaut le journal de la date du jour.
        * **And** Des boutons permettent de reculer ou d'avancer d'un jour, ou de choisir une date via un calendrier.
        * **And** Si un journal existe déjà pour la date sélectionnée, le formulaire charge les données existantes en mode édition (PUT). Sinon, il affiche un formulaire vierge prêt à être créé (POST).
    *   **CA 2** : Calculs automatisés des Fenêtres Circadiennes (Sommeil & Jeûne)
        * **Given** Un utilisateur qui remplit ses sections Sommeil et Jeûne.
        * **When** Il saisit l'heure de coucher (23h00) et de réveil (07h00), ainsi que l'heure du premier repas (12h00) et du dernier repas (20h00).
        * **Then** Le système calcule et affiche dynamiquement :
        * La durée totale du sommeil (Ex: 8 heures).
        * La fenêtre d'alimentation (Ex: 8 heures) et en déduit la fenêtre de jeûne (Ex: 16 heures de jeûne).
    *   **CA 3.1** : Ajout d'Exercice (Musculation)
        * **Given**  Un utilisateur affichant la section "Musculation".
        * **When** Il clique sur le lien ➕ Ajouter l'exercice.
        * **Then** Le lien disparaît pour laisser place aux champs de sélection de la séance (liée au programme) et aux saisies d'heures (entrée/sortie).
   *   **CA 3.2** : Ajout de Cardio (LISS)
       * **Given**  Un utilisateur affichant la section "Cardio".
       * **When** Il clique sur le lien ➕ Ajouter du cardio (LISS).
       * **Then** Le système affiche un champ texte pour le type de cardio (ex: Tapis, Vélo) et un champ numérique pour la durée (minutes).
   *   **CA 3.3** : Ajout de Repas (Jeûne Intermittent)
       * **Given**  Un utilisateur affichant la section "Jeûne Intermittent".
       * **When** Il clique sur le lien ➕ Ajouter un repas.
       * **Then** Le système incrémente le compteur de repas et, s'il s'agit du premier ou du dernier repas de la journée, met à jour dynamiquement les sélecteurs d'horaires correspondants.
   *   **CA 4** : Déclenchement de l'Insight de Trajectoire (Lien US-04)
       * **Given**  Un historique de journaux validés sur les 3 derniers jours présentant un écart calorique/activité moyen de plus de 15% par rapport au plan initial.
       * **When** L'utilisateur valide son journal ou charge son Dashboard.
       * **Then** Un encadré d'alerte orange s'affiche dans la zone des Insights pour annoncer : "⚠️ Écart constaté. Le moteur de correction de trajectoire ajustera vos cibles lors de la prochaine synchronisation hebdomadaire."
---
## US-10 : Prévision et Optimisation de la Perte de Masse Grasse (Should)
**Priorité :** Should-have | **Estimation :** 5 SP

*   **Énoncé :** En tant que Pratiquant de musculation de niveau intermédiaire, Saisir mes calories cibles ainsi que mes données de balance connectée afin de générer une simulation précise de ma composition corporelle sur le long terme, En sorte que Je puisse identifier visuellement mon apport calorique optimal pour maximiser la perte de tissu adipeux tout en protégeant l'intégralité de ma masse musculaire.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : Interface de configuration de l'intervention
        * **Given** que je suis sur l'écran du planificateur.
        * **When** je saisis un apport calorique ou que je modifie mon niveau d'activité à l'aide des curseurs.
        * **Then** l'interface doit instantanément mettre à jour le calcul de mon TDEE théorique basé sur ma masse maigre réelle (Formule de Katch-McArdle).
    *   **CA 2** : Calcul du Seuil Limite d'Alpert (Maximum Fat Loss)
        * **Given** mes données initiales de balance connectée (Poids : 76 kg, Masse Grasse : 25%).
        * **When** le moteur traite le Jour 1 de la simulation.
        * **Then** il doit calculer informatiquement :
  - La Masse Grasse Totale (19.0 kg).
  - Le Transfert Maximal depuis le gras par jour ($19.0 \times 69,2 = 1314,8\text{ kcal/jour}$).
  - La perte de gras maximale sécurisée par semaine ($\approx 0.97\text{ kg/semaine}$).

    *   **CA 3** : Matrice de Résolution des 13 Métriques Connectées
        * **Given** une simulation validée sur 140 jours.
        * **When** il consulte le tableau de projection ou le graphique.
        * **Then** il doit voir l'évolution synchronisée et cohérente des 13 métriques requises :
  1. Poids total (`bodyWeightKg`)
  2. IMC (`bodyMassIndexBMI`)
  3. Pourcentage de graisse (`bodyFatPercentage`)
  4. Pourcentage d'eau (`bodyWaterPercentage`) - *qui doit augmenter à mesure que le gras diminue*
  5. Pourcentage de muscle squelettique (`skeletalMusclePercentage`)
  6. Poids corps sans graisse (`fatFreeBodyWeight`)
  7. Masse musculaire (`muscleMass`)
  8. Masse osseuse (`boneMass`) - *valeur stable*
  9. Pourcentage de protéines (`proteinPercentage`)
  10. Métabolisme de base (`basalMetabolicRateBMR`) - *qui s'adapte à la baisse du poids total*
  11. Graisse sous-cutanée (`subcutaneousFat`)
  12. Graisse viscérale (`visceralFat`)
  13. Âge métabolique (`metabolicAge`)

   *   **CA 4** : Alertes de Sécurité Métabolique
        * **Given** un apport cible trop bas (ex: 1200 kcal/jour provoquant un déficit de 1500 kcal).
        * **When** le déficit dépasse le Transfert Maximal (1314.8 kcal).
        * **Then** l'application doit afficher un indicateur de couleur rouge sur la ligne de temps indiquant la bascule vers le catabolisme musculaire.
---

## US-11 : Hub de Télémétrie et Agrégation Biométrique Avancée 
**Priorité :** Should-have | **Estimation :** 5 SP

*   **Énoncé :**  En tant que Pratiquant de musculation analytique et rigoureux, Je veux Connecter mes applications de suivi (Apple Health, MyFitnessPal) et mes équipements (Balance Renpho, Montre connectée) à un hub centralisé  , De sorte que Mes 13 métriques corporelles, mes calories réelles, mes pas et mes marqueurs de récupération nerveuse (HRV/RHR) soient synchronisés automatiquement sans saisie manuelle fastidieuse.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : Intégration Cloud/SDK Balance Connectée
        *   **Given** que j'ai lié mon compte de balance connectée (ex: Renpho) via OAuth2.
        *   **When** je monte sur ma balance le matin et que les données atteignent leur cloud.
        *   **Then** le backend doit intercepter ou interroger ce flux pour extraire sans perte les 13 métriques de composition corporelle de mon profil.
    *   **CA 2** : Extraction du Journal Alimentaire (MyFitnessPal)
        *   **Given** que je consigne mes repas dans MyFitnessPal.
        *   **When** le processus de synchronisation s'exécute.
        *   **Then** le système doit récupérer l'apport calorique total accumulé ainsi que le grammage précis des macronutriments (Protéines, Glucides, Lipides) pour valider mon adhérence aux cibles de mon profil (ex: Hyperprotéiné).      
    *   **CA 3** : Suivi de l'Activité Terrestre (Pedomètre / Apple Health)
        *   **Given** que je marche tout au long de la journée avec mon iPhone.
        *   **When** j'ouvre l'application Angular PulsePath.
        *   **Then** le plugin HealthKit frontend (ou le relais API) doit pousser le total de pas cumulés au backend pour recalculer dynamiquement ma dépense énergétique non liée à l'exercice (NEAT).
    *   **CA 4** : Monitoring de la Récupération du Système Nerveux Central (SNC)
        *    **Given** que je porte une montre Garmin ou une Apple Watch durant mon sommeil.
        *    **When** les indicateurs de Fréquence Cardiaque au Repos (RHR) et de Variabilité de la Fréquence Cardiaque (HRV) sont synchronisés.
        *    **Then** l'application doit générer un score de récupération nerveuse et corréler une baisse anormale de la HRV avec une alerte de fatigue, suggérant d'ajuster l'intensité du volume d'entraînement (Day 1 à Day 5).
    *   **CA 5** : Algorithme de Repli Temporel et Saisie Manuelle (Fallback)
         *    **Given** que l'API d'un fournisseur tiers est indisponible ou non connectée.
         *    **When** je clique sur le bouton "Saisie Manuelle" de mon tableau de bord.
         *    **Then** l'interface doit me présenter un formulaire complet me permettant d'entrer manuellement mon poids, mes pas et mes macros du jour. Les calculs thermodynamiques du moteur d'Alpert doivent s'exécuter normalement sur la base de ces données saisies.
---

## US-12 : Planificateur de Repas Automatisé et Liste de Courses Dynamique
**Priorité :** Should-have | **Estimation :** 5 SP

*   **Énoncé :**  En tant que Pratiquant de musculation devant concilier sa nutrition avec sa vie de famille, Je veux Paramétrer un planificateur de repas hebdomadaire en spécifiant mes restrictions, mes portions, mon nombre de personnes à table et mes aliments favoris, De sorte que Je puisse générer un menu sur 7 jours parfaitement aligné avec mes macros cibles, interchanger les repas qui ne me conviennent pas et obtenir ma liste de courses triée par rayon.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : Configuration et Grille de Préférences Réelles
        *   **Given** que je configure un nouveau plan de repas.
        *   **When** je saisis les détails logistiques (ex: 4 repas par jour, portions pour 3 personnes, temps de cuisine maximum de 30 minutes).
        *   **Then** le système doit exclure de sa recherche toutes les recettes nécessitant un temps de préparation supérieur à 30 minutes.
    *   **CA 2** : Algorithme d'Alignement Macro-nutritionnel Automatique
        *   **Given** mes cibles caloriques quotidiennes (ex: 1880 kcal, 152g Protéines).
        *   **When** la génération automatique se déclenche.
        *   **Then** la somme des calories et des macros de l'ensemble des repas proposés sur une journée de calendrier doit correspondre à mes objectifs avec une marge d'erreur stricte de $\pm 3\%$.      
    *   **CA 3** : Moteur de Remplacement (Swap) Isoménu
        *   **Given** un repas généré automatiquement dans mon calendrier (ex: Poulet / Riz / Brocoli).
        *   **When** je clique sur l'option "Échanger le repas".
        *   **Then** le système doit me proposer uniquement des alternatives culinaires respectant les filtres de mon type de régime (ex: Végétarien) tout en maintenant les équilibres en protéines, glucides et lipides de la case horaire initiale.
    *   **CA 4** : Consolidation Intelligente de la Liste de Courses
        *    **Given** un menu de 7 jours validé pour 3 personnes.
        *    **When** j'affiche ma liste de courses.
        *    **Then** le système doit regrouper les ingrédients identiques (ex: cumuler 450g de riz d'une recette A et 300g de riz d'une recette B pour afficher "Riz : 750g") et ordonner l'affichage selon des rayons logiques (Boucherie, Épicerie, Fruits & Légumes, Produits Laitiers).
    *   **CA 5** : CRUD de Gestion des Recettes, Aliments et Repas
         *    **Given** que je souhaite enregistrer mes propres créations culinaires.
         *    **When** j'accède à l'espace de gestion.
         *    **Then** je dois pouvoir créer, lire, mettre à jour ou supprimer (CRUD) des ingrédients isolés, des Recettes complexes (combinaisons d'ingrédients avec instructions de préparation) et des Repas types.

---
## US-13 : Journal d'Entraînement Connecté et Analyse Mathématique de la Force
**Priorité :** Should-have | **Estimation :** 5 SP

*   **Énoncé :**  En tant que Pratiquant de musculation rigoureux adepte de la surcharge progressive, Générer mes splits d'entraînement, enregistrer mes séries avec leur typographie exacte (Drop sets, Échec) et analyser mes graphiques de progression de force, De sorte que Je puisse automatiser le chargement de mes barres, respecter mes temps de repos et ne jamais régresser d'une séance à l'autre dans mon déficit calorique.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : Pré-remplissage Intelligent et Surcharge Progressive
        *   **Given** que je démarre mon entraînement "Day 1: Upper Body".
        *   **When** le premier exercice (Développé Couché Halteres / Incline Press 👑) s'affiche à l'écran.
        *   **Then** les champs d'entrée de texte des séries doivent afficher en filigrane (placeholder gris) les valeurs exactes validées lors de ma dernière occurrence de cet exercice (ex: "40 kg x 8").
    *   **CA 2** : Typage des Séries et Ergonomie Intra-Workout
        *   **Given** que je consigne une série lourde menée à l'épuisement total.
        *   **When** je valide la série.
        *   **Then** je dois pouvoir basculer l'état de la série sur le tag `Failure` (Échec). Le système doit enregistrer cette variable pour moduler le score de fatigue de ma carte thermique musculaire.      
    *   **CA 3** : Automatisation des Alertes de Repos (Rest Timer)
        *   **Given** que je coche une série effective sur le Squat (Temps de repos cible configuré : 180 secondes).
        *   **When** la série est marquée comme complétée.
        *   **Then** un widget flottant doit immédiatement lancer un décompte de 3:00 minutes avec une option d'alerte par vibration ou notification push à l'expiration du temps.
    *   **CA 4** : Tableau de Bord Analytique et Estimation du 1RM
        *    **Given** que j'ai enregistré plusieurs séances de Soulevé de Terre (Deadlift).
        *    **When** je consulte les graphiques de performance de cet exercice.
        *    **Then** le système doit tracer une courbe d'évolution temporelle de mon Volume Total de travail (Poids x Reps x Séries) ainsi que mon 1RM estimé calculé via la formule d'Epley.

---
## US-14 : Planificateur Éducatif de Calories et de Ligne de Tendance de Poids
**Priorité :** Should-have | **Estimation :** 5 SP

*   **Énoncé :**  En tant que Utilisateur soucieux de sa planification diététique, Simuler mes apports et mes échéances selon différents modes d'objectifs (par date ou par apport fixe) en choisissant ma méthode de BMR et mes ratios de macros, De sorte que Je puisse visualiser instantanément ma feuille de route hebdomadaire, anticiper mon adaptation métabolique et exporter un lien de partage de ma configuration.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : Prise en charge multi-mode stricte
        *   **Given** que je configure mes données anthropométriques de démonstration (Homme, 30 ans, 170 cm, 72 kg, activité modérée).
        *   **When** je sélectionne le mode `targetByDate` avec un objectif ( ex: de 65,8 kg et une durée de 18 semaines).
        *   **Then** le système doit calculer précisément le déficit total requis et le traduire en un objectif de calories quotidiennes fixes à consommer pour honorer cette échéance.
    *   **CA 2** : Algorithme de Redimensionnement (Rescaling) des Macros
        *   **Given** une saisie de répartition de macro-nutriments imparfaite (Protéines : 29%, Glucides : 45%, Lipides : 25% $\rightarrow$ Total : 99%).
        *   **When** j'exécute le calcul.
        *   **Then** le système ne doit pas bloquer l'application; il doit recalculer les ratios de manière à ce qu'ils atteignent mathématiquement 100% ($P_{\text{ajusté}} = 29/99$, etc.) pour afficher le grammage final exact arrondi.      
    *   **CA 3** : Génération de la Grille de Projection Itérative
        *   **Given** un plan d'action de perte de poids calculé sur plusieurs semaines.
        *   **When** je consulte le tableau des résultats "Projected weekly weight".
        *   **Then** je dois voir une ligne par semaine affichant la décroissance progressive du poids, ainsi que la diminution progressive associée du TDEE due à la perte de masse globale.
    *   **CA 4** : Sauvegarde Locale et Persistance d'URL (State Management)
        *    **Given** une simulation réussie.
        *    **When** je clique sur "Copy shareable URL".
        *    **Then** le système doit sérialiser tous les inputs sous forme de paramètres de requête (`?mode=targetByDate&system=metric&sex=male...`) pour permettre une réhydratation instantanée de l'état de l'écran lors d'une visite future ou d'un partage.
    *   **CA 5** : Pilotage intelligent par objectif de Taux de Masse Grasse
        *    **Given** que je choisis l'option "Objectif par Taux de Masse Grasse".
        *    **When** je saisis une valeur cible (ex: `targetBodyFatPercentage = 15%`) pour mes données initiales (76 kg, 25% de gras, 57 kg de masse maigre).
        *    **Then** le planificateur doit masquer le sélecteur manuel de date limite et calculer informatiquement :
  - Mon poids cible de sécurité : $57\text{ kg} / 0,85 = 67,05\text{ kg}$.
  - Mon déficit optimal initial basé sur le plafond d'Alpert ($19\text{ kg de gras} \times 69,2 = 1314,8\text{ kcal}$).
  - La trajectoire non-linéaire, en ajustant à la baisse le déficit calorique chaque semaine à mesure que ma masse grasse diminue, afin de m'indiquer la durée totale de simulation minimale requise pour atteindre 15% sans perdre un seul gramme de muscle.
