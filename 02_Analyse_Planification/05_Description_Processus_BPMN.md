# 📄 Description du Processus (To-Be) : PulsePath Engine (V2)

## 1. Présentation du Nouveau Flux Métier
Le processus métier est restructuré pour garantir un onboarding analytique complet. Les indicateurs d'évolution (perte en kg et progrès en %) sont isolés et protégés par un système de gel conditionnel afin de ne pas fausser les rapports en cas de surplus temporaire.

## 2. Diagramme de Flux (Syntaxe Mermaid)

```mermaid
graph TD
    %% ÉTAPE 1: ONBOARDING & DIAGNOSTIC
    Start([Début : Inscription Utilisateur]) --> A[Saisie Profil : Genre, Âge, Taille, Poids de Départ, Niveau d'Activité de base]
    A --> B[Calculs Biométriques : IMC, IMG Formule Deurenberg, Plage Bien-être]
    B --> C[Calcul du TDEE de Maintenance Initial]
    
    %% ÉTAPE 2: PLANIFICATION S.M.A.R.T
    C --> D[Sélection Objectif : Perte, Seiche, Maintien, Gain]
    D --> E[Saisie Fréquence Sportive : Nombre de séances ciblées par semaine]
    E --> F[Calcul Automatique du Budget Calorique & Ventilation des Macros Ratios]
    
    %% ÉTAPE 3: JOURNALISATION QUOTIDIENNE & KPIS
    F --> G[Déclenchement Quotidien : Saisie du Journal Log]
    G --> H[Calcul du TDEE Dynamique via le volume de pas]
    H --> I{Poids du jour > Poids de départ ?}
    I -- OUI --> J[Gel des KPIs : Perte = 0kg, Progrès = 0%, Date Échéance Gelée]
    I -- NON --> K[Calcul des KPIs : Perte Réelle, % de Progrès Global, Date d'Échéance Révisée]
    J --> L[Incrémentation automatique du compteur de Jours Écoulés]
    K --> L
    L --> M[Agrégation des Séances de Sport Effectuées dans la Semaine]
    M --> N[Génération des Coach Insights & Restitution Dashboard]
    N --> G
```

---

## 3. Détail des Étapes (Swimlanes)

### 👤 Ligne Utilisateur
*   **Action** : Saisit le poids, les calories, les macros, le sommeil et la fenêtre de jeûne.
*   **Contrainte** : Doit effectuer la saisie avant minuit pour valider le bonus "Intégrité".

### ⚙️ Ligne Système (Moteur PulsePath)
*   **Validation** : Vérifie que les données respectent les seuils de sécurité (ex: Calories > BMR).
*   **Calculs** :
    *   Applique la règle **RM-MET-01** pour ajuster le métabolisme selon les pas.
    *   Applique la règle **RM-VEL-01** pour recalculer la date de fin via la moyenne glissante (SMA 7j).
*   **Intelligence** : Croise les données (ex: impact d'un sommeil court sur la faim du lendemain) pour générer des recommandations personnalisées.

### 💾 Ligne Base de Données
*   **Action** : Archive les logs quotidiens et met à jour l'historique de vélocité pour les futurs calculs de tendance.

---

## 4. Analyse de la Valeur Ajoutée (Business Value)
Ce processus automatisé supprime la charge mentale de l'utilisateur. Au lieu de simplement stocker des chiffres, le système **interprète** la donnée. 

**Exemple d'optimisation :** Si le système détecte une baisse de vélocité, il n'attend pas que l'utilisateur s'en rende compte ; il propose immédiatement un ajustement via le "Coach Insight".
