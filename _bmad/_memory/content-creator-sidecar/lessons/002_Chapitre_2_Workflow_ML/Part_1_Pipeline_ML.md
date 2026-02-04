# Chapitre 2 — Leçon 1 : Le Pipeline ML

## Objectifs d'apprentissage

À la fin de cette leçon, vous serez capable de :
- **Décrire** les étapes clés d'un pipeline de Machine Learning
- **Expliquer** la connexion entre le data pipeline (Module 2) et le ML pipeline
- **Reconnaître** l'importance de l'itération et de l'expérimentation dans un projet ML

---

## 🎯 Accroche : Pourquoi 87% des projets ML échouent ?

Selon une étude de VentureBeat (2019), **87% des projets de data science n'atteignent jamais la production**. Pourquoi un taux d'échec aussi élevé ?

Ce n'est pas parce que les algorithmes sont mauvais. Ce n'est pas non plus un manque de données. La raison principale ? **L'absence d'un workflow structuré et reproductible.**

> *Source : [IBM - Machine Learning Pipeline](https://www.ibm.com/think/topics/machine-learning-pipeline)*

**Question :** Si vous deviez construire un modèle ML pour prédire le churn (départ des clients), par où commenceriez-vous ?

*(Réponse attendue : Par les données — il faut d'abord avoir des données propres et pertinentes avant de penser à l'algorithme)*

---

## 1.1 Vue d'ensemble du pipeline ML

### Qu'est-ce qu'un pipeline ?

Imaginons que vous construisez une voiture. Vous ne commencez pas par peindre la carrosserie avant d'avoir assemblé le moteur, n'est-ce pas ? Il y a un **ordre logique** des opérations.

Un pipeline ML, c'est exactement cela : une **séquence structurée d'étapes** qui transforme des données brutes en un modèle capable de faire des prédictions.

Avant de voir les étapes de la pipeline, qu'est-ce qu'un Modèle en Machine Learning ?

**L'Analogie Humaine**

Imaginez que vous regardez par la fenêtre et voyez des nuages sombres. Votre modèle mental du monde (acquise par l'expérience) vous fait prédire : "Il va pleuvoir".

Un modèle est une représentation simplifiée de la réalité qui permet de faire des prédictions. Nous construisons nos modèles de deux façons :

- En apprenant des autres (parents, mentors)
- En apprenant par l'expérience

L'Analogie Informatique : Comment les Ordinateurs Apprennent

L'apprentissage machine (Machine Learning) permet aux ordinateurs d'apprendre sans instructions explicites, contrairement à la programmation traditionnelle où on donne des étapes précises.

**Question :** Selon vous, quelle est la PREMIÈRE étape d'un projet ML — choisir l'algorithme ou préparer les données ?

*(Réponse attendue : Préparer les données — un bon algorithme sur de mauvaises données donnera de mauvais résultats)*

---

### Les 5 étapes du pipeline ML

Voici le workflow standard que suivent les data scientists du monde entier :

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                         LE PIPELINE ML EN 5 ÉTAPES                          │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   ┌──────────┐    ┌──────────────┐    ┌─────────┐    ┌──────────┐    ┌────────────┐
│   │   DATA   │───►│  PREPROCESS  │───►│  TRAIN  │───►│ EVALUATE │───►│   DEPLOY   │
│   │          │    │              │    │         │    │          │    │            │
│   │ Collecte │    │ Nettoyage    │    │ Entraîner│   │ Mesurer  │    │ Mettre en  │
│   │ des      │    │ Features     │    │ le modèle│   │ la       │    │ production │
│   │ données  │    │ Séparation   │    │          │    │ qualité  │    │            │
│   └──────────┘    └──────────────┘    └─────────┘    └──────────┘    └────────────┘
│                                                                             │
│        ◄─────────────────── ITÉRATION ───────────────────►                  │
│                    (retour en arrière si nécessaire)                        │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

Détaillons chaque étape :

| Étape | Ce qui se passe | Questions clés |
|-------|-----------------|----------------|
| **1. DATA** | Collecte et exploration des données | D'où viennent les données ? Sont-elles suffisantes ? |
| **2. PREPROCESS** | Nettoyage, feature engineering, séparation train/test | Les données sont-elles propres ? Quelles features utiliser ? |
| **3. TRAIN** | Le modèle "apprend" les patterns dans les données | Quel algorithme choisir ? Quels hyperparamètres ? |
| **4. EVALUATE** | Mesurer la performance du modèle | Le modèle est-il bon ? Généralise-t-il bien ? |
| **5. DEPLOY** | Mettre le modèle en production | Comment l'intégrer dans un système réel ? |

<details>
<summary>🤔 Question Socratique : Pourquoi le pipeline est-il représenté avec des flèches de retour (itération) ?</summary>

### 🔑 Réponse

Le Machine Learning est un processus **itératif**, pas linéaire. À chaque étape, vous pouvez découvrir un problème qui vous oblige à revenir en arrière :

- **Évaluation révèle un problème** → Retour au preprocessing (peut-être qu'une feature est mal encodée)
- **Modèle sous-performe** → Retour à la collecte (peut-être qu'il manque des données)
- **Déploiement échoue** → Retour à l'entraînement (peut-être que le modèle ne gère pas certains cas)

Un data scientist passe rarement d'un bout à l'autre du pipeline du premier coup. C'est normal, et c'est même souhaitable — chaque itération améliore le modèle.

</details>

---

### La réalité du temps passé

Voici une surprise pour beaucoup de débutants :

```
┌─────────────────────────────────────────────────────────────────┐
│         OÙ LES DATA SCIENTISTS PASSENT LEUR TEMPS               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  DATA + PREPROCESS  ████████████████████████████████████  80%   │
│                                                                 │
│  TRAIN              ████████                              10%   │
│                                                                 │
│  EVALUATE + DEPLOY  ████████                              10%   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Question :** Pourquoi pensez-vous que 80% du temps est consacré aux données, et seulement 10% à l'entraînement du modèle ?

*(Réponse attendue : Parce que la qualité des données détermine la qualité du modèle — "garbage in, garbage out")*

C'est un dicton fondamental en data science : **« Garbage In, Garbage Out »** (déchets en entrée, déchets en sortie). Le meilleur algorithme du monde ne peut rien faire avec des données de mauvaise qualité.

┌─────────────────────────────────────────────────────────────────┐
│ 📖 DÉFINITION : Pipeline de Machine Learning                    │
│                                                                 │
│ Un pipeline de Machine Learning est une séquence structurée     │
│ et automatisable d'étapes qui transforme des données brutes     │
│ en un modèle prédictif déployé. Il comprend : la collecte des   │
│ données, le prétraitement, l'entraînement, l'évaluation et le   │
│ déploiement. Sa nature itérative permet d'améliorer             │
│ continuellement le modèle en revenant sur les étapes            │
│ précédentes.                                                    │
└─────────────────────────────────────────────────────────────────┘

---

## 1.2 Lien avec le Module 2 : Du Data Pipeline au ML Pipeline

### Vous avez déjà fait la moitié du travail !

Si vous avez complété le Module 2 (Pipeline Data), excellente nouvelle : vous maîtrisez déjà les **étapes les plus chronophages** du ML pipeline !

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    CONNEXION MODULE 2 → MODULE 3                            │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   MODULE 2 : DATA PIPELINE                MODULE 3 : ML PIPELINE            │
│   ═══════════════════════                 ══════════════════════            │
│                                                                             │
│   ┌────────────────┐                                                        │
│   │  Extraction    │──┐                                                     │
│   │  (CSV, API,    │  │                                                     │
│   │   SQL, Web)    │  │                                                     │
│   └────────────────┘  │         ┌─────────────────────────────────────┐    │
│                       │         │                                     │    │
│   ┌────────────────┐  │         │   ┌─────────┐   ┌──────────┐       │    │
│   │  EDA           │  ├────────►│   │  TRAIN  │──►│ EVALUATE │       │    │
│   │  Diagnostique  │  │         │   └─────────┘   └──────────┘       │    │
│   └────────────────┘  │         │         │             │            │    │
│                       │         │         ▼             ▼            │    │
│   ┌────────────────┐  │         │   ┌─────────────────────┐          │    │
│   │  Nettoyage     │  │         │   │      DEPLOY         │          │    │
│   └────────────────┘  │         │   └─────────────────────┘          │    │
│                       │         │                                     │    │
│   ┌────────────────┐  │         │   Ce que vous allez apprendre      │    │
│   │  Transformation│──┘         │   dans ce module !                 │    │
│   └────────────────┘            └─────────────────────────────────────┘    │
│                                                                             │
│   Ce que vous savez déjà !                                                  │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Question :** Quelles compétences du Module 2 allez-vous réutiliser directement dans un projet ML ?

*(Réponse attendue : L'extraction de données, le nettoyage, la gestion des valeurs manquantes, le feature engineering, la structuration avec pandas)*

---

### Le Dataset "Analysis-Ready" : votre point de départ

À la fin du Module 2, vous avez appris à produire un dataset **"analysis-ready"** — propre, structuré, documenté. Ce dataset devient l'**input** du ML pipeline :

| Étape Module 2 | → | Étape ML Pipeline |
|----------------|---|-------------------|
| Extraction (CSV, API, SQL) | → | DATA (déjà fait !) |
| EDA diagnostique + Nettoyage | → | PREPROCESS (partiellement fait) |
| Transformation + Feature engineering | → | PREPROCESS (partiellement fait) |
| Upload vers S3 | → | Source de données pour TRAIN |

<details>
<summary>🤔 Question Socratique : Qu'est-ce qui reste à faire dans PREPROCESS pour le ML, même avec un dataset propre ?</summary>

### 🔑 Réponse

Même avec un dataset parfaitement nettoyé, le préprocessing ML ajoute des étapes spécifiques :

1. **Séparation Train/Test** — Diviser les données en ensembles d'entraînement et de test
2. **Scaling** — Normaliser les variables numériques (StandardScaler, MinMaxScaler)
3. **Encoding** — Transformer les variables catégorielles en nombres (OneHotEncoder, LabelEncoder)
4. **Feature Selection** — Sélectionner les variables les plus pertinentes

Ces étapes sont spécifiques au ML et seront couvertes dans ce chapitre !

</details>

---

## 1.3 Itération et expérimentation

### Le ML n'est pas linéaire

Contrairement à ce que suggère le diagramme, un projet ML réel ressemble davantage à ceci :

```
                    ┌──────────────────────────────────────────┐
                    │        LA RÉALITÉ D'UN PROJET ML         │
                    └──────────────────────────────────────────┘

      Essai 1:  DATA ──► PREPROCESS ──► TRAIN ──► EVALUATE ──► 😞 (mauvais)
                                          │
                                          ▼
      Essai 2:  ◄───────────────────── Ajuster features
                DATA ──► PREPROCESS ──► TRAIN ──► EVALUATE ──► 😐 (moyen)
                                          │
                                          ▼
      Essai 3:  ◄───────────────────── Changer algorithme
                DATA ──► PREPROCESS ──► TRAIN ──► EVALUATE ──► 😊 (bon)
                                          │
                                          ▼
      Essai 4:  ◄───────────────────── Optimiser hyperparamètres
                DATA ──► PREPROCESS ──► TRAIN ──► EVALUATE ──► 🎉 (excellent!)
                                                      │
                                                      ▼
                                                   DEPLOY
```

**Question :** Quel problème voyez-vous avec cette approche "essai après essai" si vous ne gardez pas trace de vos expériences ?

*(Réponse attendue : On risque d'oublier ce qu'on a testé, de répéter les mêmes erreurs, ou de ne pas pouvoir reproduire un bon résultat)*

---

### Le problème de la reproductibilité

Imaginez : après 15 essais, vous obtenez enfin un excellent modèle. Votre manager vous demande : "Quels paramètres as-tu utilisés ?" Et là... vous ne vous souvenez plus.

C'est un problème **critique** en data science professionnelle. Une étude de 2023 a révélé que le data leakage et les problèmes de reproductibilité ont affecté **au moins 294 publications académiques** dans 17 disciplines différentes.

> *Source : [Princeton - Leakage and the Reproducibility Crisis in ML-based Science](https://reproducible.cs.princeton.edu/)*

---

### La solution : tracker ses expériences

Les data scientists professionnels utilisent des outils de **tracking d'expériences**. Le plus populaire est **MLflow** (open-source, gratuit).

```
┌─────────────────────────────────────────────────────────────────┐
│                    MLFLOW : TRACKING D'EXPÉRIENCES              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Expérience: "Prédiction Churn"                                │
│                                                                 │
│   ┌─────────┬───────────────┬────────────┬──────────┬────────┐ │
│   │  Run    │  Algorithme   │  Features  │ Accuracy │ F1     │ │
│   ├─────────┼───────────────┼────────────┼──────────┼────────┤ │
│   │  #1     │  LogReg       │  10        │  0.72    │  0.68  │ │
│   │  #2     │  RandomForest │  10        │  0.81    │  0.76  │ │
│   │  #3     │  RandomForest │  15        │  0.83    │  0.79  │ │
│   │  #4 ⭐  │  RandomForest │  15+scaled │  0.87    │  0.84  │ │
│   └─────────┴───────────────┴────────────┴──────────┴────────┘ │
│                                                                 │
│   Chaque "run" enregistre automatiquement :                     │
│   • Les hyperparamètres utilisés                                │
│   • Les métriques de performance                                │
│   • Le code et les données utilisés                             │
│   • Le modèle entraîné (artifact)                               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Note :** Dans ce bootcamp, nous n'utiliserons pas MLflow en détail (c'est du MLOps avancé), mais sachez qu'il existe. Pour vos projets, vous pouvez simplement tenir un **journal d'expériences** dans un fichier Markdown ou un notebook.

<details>
<summary>🤔 Question Socratique : Pourquoi est-il important de fixer le `random_state` dans vos expériences ML ?</summary>

### 🔑 Réponse

Beaucoup d'opérations ML impliquent du **hasard** :
- La séparation train/test (`train_test_split`)
- L'initialisation des poids d'un modèle
- L'ordre de traitement des données

Si vous ne fixez pas le `random_state`, vous obtiendrez des résultats **différents** à chaque exécution. Cela rend impossible :
- La **comparaison** entre deux expériences
- La **reproduction** d'un bon résultat
- Le **debugging** d'un problème

En fixant `random_state=42` (ou n'importe quel nombre), vous garantissez que le hasard sera **le même** à chaque fois :

```python
# ✅ Reproductible
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# ❌ Non reproductible
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)
```

</details>

---

┌─────────────────────────────────────────────────────────────────┐
│ 📖 DÉFINITION : Reproductibilité en Machine Learning            │
│                                                                 │
│ La reproductibilité est la capacité à obtenir les mêmes         │
│ résultats en réexécutant une expérience avec les mêmes          │
│ données, le même code et les mêmes paramètres. Elle est         │
│ essentielle pour : valider scientifiquement un modèle,          │
│ collaborer en équipe, debugger des problèmes, et mettre en      │
│ production un modèle vérifié. Elle nécessite de fixer les       │
│ graines aléatoires (random_state), versionner le code et les    │
│ données, et documenter les hyperparamètres.                     │
└─────────────────────────────────────────────────────────────────┘

---

## 🧠 Réflexion métacognitive

Avant de passer à la leçon suivante, prenez un moment pour réfléchir :

1. **Quelle étape du pipeline ML vous semble la plus difficile ?** Pourquoi ?

2. **Avez-vous déjà commis l'erreur de sauter directement à l'algorithme** sans bien préparer les données ? Qu'avez-vous appris ?

3. **Comment allez-vous organiser vos expériences** dans le projet fil rouge de ce module ?

---

## 📝 Résumé

| Concept | Point clé |
|---------|-----------|
| Pipeline ML | 5 étapes : Data → Preprocess → Train → Evaluate → Deploy |
| Nature itérative | On revient en arrière — c'est normal et souhaitable |
| 80/20 | 80% du temps sur les données, 20% sur le modèle |
| Module 2 → Module 3 | Vos compétences data pipeline = fondation du ML pipeline |
| Reproductibilité | Fixer `random_state`, documenter les expériences |

---

## ➡️ Prochaine leçon

Dans la **Leçon 2.2 : Train / Test / Validation**, nous allons découvrir pourquoi et comment séparer vos données — une étape **critique** pour éviter le piège du data leakage.

**Question de transition :** Si vous entraînez un modèle sur 100% de vos données, comment saurez-vous s'il fonctionne bien sur de nouvelles données qu'il n'a jamais vues ?

*(Indice : c'est le cœur du problème que nous allons résoudre)*
