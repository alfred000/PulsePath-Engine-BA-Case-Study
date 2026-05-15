# 📄 Description du Processus (To-Be) : PulsePath Engine

## 1. Présentation du Nouveau Flux Métier
Le processus impose un onboarding analytique strict pour éliminer l'incertitude utilisateur et garantir la qualité de la planification initiale.

## 2. Diagramme de Flux (Syntaxe Mermaid)

```mermaid
graph TD
    %% ÉTAPE 1: ONBOARDING & DIAGNOSTIC
    Start([Début : Inscription Utilisateur]) --> A[Saisie Profil : Genre, Âge, Taille, Poids Actuel]
    A --> B[Calculs Profil : IMC, IMG, Plage de Poids Bien-être]
    B --> C[Calcul du TDEE de Maintenance initial]
    
    %% ÉTAPE 2: PLANIFICATION S.M.A.R.T
    C --> D[Sélection Objectif : Perte, Maintien, Prise de Masse]
    D --> E[Calcul Automatique du Budget Calorique Ciblé & Ratio Macros]
    
    %% ÉTAPE 3: JOURNALISATION QUOTIDIENNE (Isolée pour futures API)
    E --> F[Déclenchement Quotidien : Saisie du Journal Log]
    F --> G[Traitement Log : Poids, Calories, Pas, Sommeil, Macros, Jeûne]
    G --> H[Vérification du Score d'Intégrité de la donnée]
    H --> I[Calcul du TDEE Dynamique via le volume de pas]
    I --> J[Moteur de Vélocité : Ajustement de la Date d'Échéance Estimée]
    J --> K[Génération des Coach Insights]
    K --> F
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
