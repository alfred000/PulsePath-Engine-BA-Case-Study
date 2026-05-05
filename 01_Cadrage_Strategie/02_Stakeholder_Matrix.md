# Registre des Contributeurs : PulsePath Engine

# 1. Objectif du Document
   
Ce document recense l'ensemble des parties prenantes du projet PulsePath Engine, définit leurs rôles, leurs attentes et leur niveau d'influence sur les décisions finales.

# 2. Matrice Pouvoir / Intérêt (Modèle de Mendelow)
   
| Nom / Rôle | Niveau d'Intérêt | Pouvoir / Influence | Stratégie de Gestion |
| :--- | :--- | :--- | :--- |
| **Product Owner (PO)** | Élevé | Élevé | **Gérer étroitement** : Validation des priorités du backlog. |
| **Coach Sportif (SME)** | Élevé | Moyen | **Maintenir satisfait** : Garant de la validité scientifique. |
| **Équipe de Développement** | Moyen | Élevé | **Collaborer** : Faisabilité technique des spécifications. |
| **Utilisateurs Finaux** | Élevé | Faible | **Tenir informé** : Collecte des feedbacks et tests. |
| **Responsable RGPD** | Faible | Élevé | **Satisfaire** : Conformité des données de santé. |

# 3. Détail des Besoins par Contributeur
   
# A. Expert Métier (Nutritionniste)

* Attentes : Précision des formules de calcul (Mifflin-St Jeor) et respect des seuils de sécurité (ex: ne pas descendre sous 1200 kcal/jour).
* Contribution : Fournit les règles métiers (RM) et valide les seuils d'alerte pour le coaching.

# B. Product Owner

* Attentes : Livraison d'une solution qui augmente la rétention utilisateur via la gamification.
* Contribution : Arbitre les choix fonctionnels entre le moteur de calcul et l'interface utilisateur.

# C. Équipe de Développement
* Attentes : Spécifications fonctionnelles claires, sans ambiguïté, et accompagnées de User Stories prêtes à être estimées.
* Contribution : Implémente la logique de calcul et les interfaces de saisie.

# 4. Plan de Communication
* Comité de Pilotage (Copil) : Mensuel (PO, BA, Sponsors) pour valider la roadmap.
* Réunions de Conception : Hebdomadaire (BA, Experts Métiers) pour affiner les règles de calcul.
* Daily Stand-up : Quotidien (BA, Devs) pour répondre aux questions sur les spécifications durant la phase de réalisation.
