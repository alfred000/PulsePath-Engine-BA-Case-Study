# 📄 User Stories Détaillées : PulsePath Engine

## 1. Présentation
Ce document détaille les spécifications fonctionnelles sous forme de **User Stories**. Chaque story définit un besoin utilisateur, sa valeur métier et les critères précis permettant de valider sa bonne implémentation (Definition of Done).

---

## 2. US-01 : Recalcul dynamique de l'échéance cible
**Priorité :** Must-have | **Estimation :** 5 SP

*   **Énoncé :** En tant qu'utilisateur, je veux que le système mette à jour ma date d'échéance estimée dès que je saisis mes données du jour, afin de comprendre l'impact immédiat de ma discipline sur mon objectif.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : Le calcul doit impérativement utiliser la règle **RM-VEL-01** (moyenne glissante sur 7 jours).
    *   **CA 2** : Si le déficit cumulé augmente, la date doit avancer ; s'il y a un surplus, la date doit reculer.
    *   **CA 3** : L'interface doit afficher le "Delta" de jours (ex: +2 jours) de manière visuelle (Vert pour une avance, Rouge pour un recul).
    *   **CA 4** : En l'absence de données suffisantes (< 3 jours), le système doit afficher "Calcul en cours..." pour éviter toute donnée erronée.

---

## 3. US-03 : Algorithme de TDEE dynamique (Pas)
**Priorité :** Must-have | **Estimation :** 5 SP

*   **Énoncé :** En tant qu'utilisateur, je veux que ma dépense énergétique soit ajustée en fonction de mon activité réelle (pas), afin d'avoir un bilan calorique net précis chaque jour.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : Le système doit appliquer les paliers d'activité définis dans la règle **RM-MET-01**.
    *   **CA 2** : Si le log de pas est vide, le système doit appliquer par défaut le multiplicateur "Sédentaire" (1.2).
    *   **CA 3** : Le TDEE recalculé doit être visible sur l'interface de saisie pour donner un feedback immédiat à l'utilisateur.

---

## 4. US-04 : Suivi de la cible de Protéines
**Priorité :** Should-have | **Estimation :** 3 SP

*   **Énoncé :** En tant qu'utilisateur, je veux visualiser mon apport protéique par rapport à ma cible personnalisée, afin de m'assurer que je préserve ma masse musculaire pendant ma perte de poids.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : La cible doit être calculée selon le poids cible de l'utilisateur (2g / kg de poids cible).
    *   **CA 2** : Une jauge de progression doit s'afficher en temps réel lors de la saisie.
    *   **CA 3** : Si l'apport est inférieur à 1.5g/kg de poids actuel, une icône d'alerte orange doit apparaître.

---

## 5. US-05 : Tracking de la fenêtre de Jeûne Intermittent
**Priorité :** Should-have | **Estimation :** 5 SP

*   **Énoncé :** En tant qu'utilisateur, je veux enregistrer mes heures de début et de fin de repas, afin de vérifier le respect de mon protocole de jeûne.
*   **Critères d'Acceptation (CA) :**
    *   **CA 1** : Le système doit calculer la durée entre l'heure du premier repas et celle du dernier.
    *   **CA 2** : Si cette durée est ≤ 8 heures, la journée est validée "Objectif Jeûne Atteint".
    *   **CA 3** : Le système doit empêcher la saisie d'une heure de fin antérieure à l'heure de début sur la même journée civile.


