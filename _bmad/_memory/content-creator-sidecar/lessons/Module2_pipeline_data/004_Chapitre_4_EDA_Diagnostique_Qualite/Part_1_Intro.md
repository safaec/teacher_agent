# Chapitre 4 — EDA diagnostique et qualité des données

**Durée estimée : 8-10 heures**

---

## Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

1. **Distinguer** l'EDA diagnostique (trouver les problèmes) de l'EDA analytique (comprendre les patterns)
2. **Évaluer** la qualité des données selon les 6 dimensions standardisées
3. **Détecter** les valeurs manquantes, doublons et outliers avec les outils pandas appropriés
4. **Documenter** les problèmes identifiés dans une checklist de qualité priorisée

---

## 🎯 Le Hook : Le satellite qui s'est crashé à cause d'une virgule

En 1999, la NASA a perdu la sonde Mars Climate Orbiter, un projet souvent cité à environ 125 millions de dollars pour l’orbiteur lui-même — et plus de 300 millions si l’on inclut l’ensemble de la mission.

La cause n’était ni une panne matérielle, ni une tempête solaire, ni un bug spectaculaire.

C’était une erreur de données.

Un logiciel chargé de calculer la poussée utilisait des valeurs exprimées en livres-force secondes (unités anglo-saxonnes), tandis que le système de navigation s’attendait à recevoir ces mêmes données en newton-secondes (unités métriques).
Aucune conversion correcte n’a été appliquée.
Aucun contrôle de cohérence n’a détecté l’incompatibilité.

Résultat : la trajectoire calculée pour l’insertion orbitale était fausse.

Au lieu de survoler Mars à une altitude sûre, la sonde est passée beaucoup trop bas, a pénétré l’atmosphère martienne, et s’est désintégrée — ou a été rendue définitivement incontrôlable.

Une mission entière perdue, non pas par manque de technologie, mais par absence de vérification des unités.

Avant de nettoyer, transformer ou analyser vos données, vous devez d'abord **diagnostiquer** ce qui ne va pas. C'est l'objet de ce chapitre.

> 💭 **Question Socratique #1** : À votre avis, pourquoi les équipes de la NASA — composées d'ingénieurs brillants — n'ont-elles pas détecté cette incohérence d'unités ? Quel processus aurait pu prévenir ce désastre ?

> 💭 **Réponse possible**

**Pourquoi l’erreur n’a pas été détectée**

1. Responsabilités fragmentées

• Un sous-traitant produisait les données de poussée.
• Une autre équipe consommait ces données pour la navigation.
• Chacun supposait que l’autre respectait le contrat implicite.

→ Personne n’était explicitement responsable de la cohérence globale.

1. Contrat de données mal défini

• Les unités n’étaient pas formellement imposées, validées ou testées.

• Les interfaces reposaient sur des hypothèses, pas sur des garanties.

→ Une donnée “numériquement valide” pouvait être sémantiquement fausse.

1. Confiance excessive dans des systèmes réputés fiables

• Les valeurs semblaient plausibles.

• Les écarts étaient progressifs, pas brutaux.

→ Aucun signal d’alerte évident pour l’humain ou la machine.

1. Pression budgétaire et calendaire

• Programme “faster, better, cheaper”.

• Moins de tests end-to-end, moins de revues croisées.

→ Les contrôles de cohérence ont été sacrifiés au profit de la vitesse.

**Quel processus aurait pu prévenir le désastre**

1. Un contrat d’interface explicite

• Unités obligatoires, documentées et vérifiées automatiquement.

• Impossible de consommer une donnée sans connaître son sens.

1. Des tests de cohérence physique

• Vérifier les ordres de grandeur attendus.

• Rejeter toute trajectoire violant des contraintes physiques réalistes.

1. Une responsabilité claire de bout en bout

• Une équipe ou un rôle garant de la cohérence système globale.

• Pas seulement du “code qui marche”, mais du sens qui est correct.

1. Des garde-fous automatisés

• Validation d’unités au runtime.

• Typage fort ou métadonnées obligatoires sur les données critiques.

---

## 4.1 Objectif de l'EDA diagnostique

### Diagnostique vs Analytique : deux missions différentes

| Aspect | EDA Diagnostique (Chapitre 4) | EDA Analytique (Chapitre 7) |
|--------|------------------------------|----------------------------|
| **Question** | *"Qu'est-ce qui ne va pas ?"* | *"Que disent les données ?"* |
| **Objectif** | Trouver les problèmes de qualité | Comprendre les patterns |
| **Timing** | Avant le nettoyage | Après le nettoyage |
| **Focus** | Valeurs manquantes, erreurs, incohérences | Corrélations, tendances, segments |
| **Mindset** | Détective / Auditeur | Explorateur / Scientifique |

```
Données brutes → [EDA DIAGNOSTIQUE] → [NETTOYAGE] → [EDA ANALYTIQUE] → Insights
                  (Vous êtes ici)
```

### Pourquoi ne pas sauter cette étape ?

**Erreur courante :** Se lancer directement dans l'analyse sans vérifier la qualité.

**Conséquences :**

- Conclusions fausses basées sur des données erronées
- Temps perdu à recommencer
- Décisions business incorrectes
- Modèles ML qui ne fonctionnent pas en production

**Règle d'or :** Passez 20% de votre temps à diagnostiquer pour économiser 80% de corrections futures.

### ✍️ Exercice 4.1 : Diagnostic ou Analytique ? (5 min)

Classez chaque action dans la bonne catégorie :

| Action | Diagnostique | Analytique | Explication |
|--------|--------------|------------|-------------|
| Calculer la corrélation entre prix et ventes | ❌ | ✅ | Analyse d’une relation entre variables pour comprendre un phénomène métier |
| Compter les valeurs manquantes par colonne | ✅ | ❌ | Vérification de la complétude et de la qualité des données |
| Identifier les segments de clients | ❌ | ✅ | Recherche de structures et de comportements dans les données |
| Vérifier si les dates sont au bon format | ✅ | ❌ | Contrôle technique de cohérence et de format des données |
| Analyser la distribution des âges | ❌ | ✅ | Compréhension statistique de la population étudiée |
| Chercher les doublons dans les emails | ✅ | ❌ | Détection d’anomalies et de problèmes d’unicité des données |
