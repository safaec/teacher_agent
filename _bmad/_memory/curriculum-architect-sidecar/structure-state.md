# Structure State

## Current Course

**Boot Camp Data & Intelligence Artificielle**

## Structured Modules

| Module | Status | Chapters | Lessons |
|--------|--------|----------|---------|
| 1 - Prompt Engineering | ✅ Structured (by teacher) | — | — |
| **2 - Pipeline Data** | ✅ Structured | 11 | 65 |
| **3 - Intelligence Artificielle (ML & LLM)** | ✅ Structured | 11 | 60 |
| 4 - Power BI | Not structured | — | — |
| Final Capstone | Not structured | — | — |

---

# 📘 Module 2 — Pipeline Data : de la collecte à l'analyse en cloud

**Durée totale :** 85-105h | **Public :** Reconversion avec bases Python, Excel, SQL

**Méthodologie :** Progressive PBL with scaffolded milestones

**Projet Module :** "Mon Pipeline Data de A à Z"

---

## Chapitre 1 : Pipeline data — vue d'ensemble
**Durée : 4-5h** | **Activité principale :** Concept Map

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 1.1 | Position du module dans le parcours Data & IA | 45min | Théorie + Discussion |
| 1.2 | ETL vs ELT : deux approches | 1h | Théorie + Schéma |
| 1.3 | Les étapes de la pipeline | 1h | Théorie + Concept Map |
| 1.4 | Méthodologie CRISP-DM | 45min | Théorie |
| 1.5 | Sources de données en entreprise | 30min | Discussion |
| — | **🎯 Activité : Concept Map ETL/ELT** | 1h | Groupe |

---

## Chapitre 2 : Extraction des données
**Durée : 10-12h** | **Activité principale :** Guided Lab

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 2.1 | Extraction depuis fichiers avec pandas (CSV, Excel, JSON) | 2h | Théorie + Code |
| 2.2 | Extraction depuis bases SQL (sqlalchemy, read_sql) | 2h | Théorie + Lab |
| 2.3 | Extraction via API REST (requests, pagination) | 2.5h | Théorie + Lab |
| 2.4 | Introduction au web scraping (BeautifulSoup) | 1.5h | Survol + Démo |
| 2.5 | Inspection initiale des données | 1h | Théorie + Pratique |
| — | **🎯 Guided Lab : Extraire CSV, SQL, API** | 2h | Pair work |
| — | **🎯 Mini-projet : Extraire & profiler un dataset réel** | 1h | Individuel |

🤖 *IA : Utiliser un LLM pour comprendre une documentation d'API*

---

## Chapitre 3 : Le Cloud pour la data
**Durée : 8-10h** | **Activité principale :** Hands-on Setup

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 3.1 | Introduction au cloud computing | 1h | Théorie |
| 3.2 | Types de stockage cloud (objet vs fichier vs DB) | 1h | Théorie + Comparaison |
| 3.3 | AWS S3 : concepts (buckets, objets, IAM) | 1.5h | Théorie |
| 3.4 | Hands-on : configuration AWS (compte, credentials) | 1.5h | Lab guidé |
| 3.5 | Hands-on : lire depuis S3 avec Python (boto3, pandas) | 2h | Lab guidé |
| 3.6 | Panorama GCP et Azure (conceptuel) | 1h | Théorie |
| — | **🎯 Lab : Setup AWS + lire depuis S3** | 1h | Checklist validation |

---

## Chapitre 4 : EDA diagnostique et qualité
**Durée : 8-10h** | **Activité principale :** Dataset Autopsy

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 4.1 | Objectif de l'EDA diagnostique | 30min | Théorie |
| 4.2 | Dimensions de la qualité (complétude, unicité, cohérence, exactitude) | 1h | Théorie |
| 4.3 | Statistiques descriptives de diagnostic | 1h | Théorie + Code |
| 4.4 | Détection des valeurs manquantes (missingno) | 1.5h | Théorie + Lab |
| 4.5 | Détection des doublons | 1h | Théorie + Lab |
| 4.6 | Détection des outliers (IQR, Z-score) | 1.5h | Théorie + Lab |
| 4.7 | Visualisations de diagnostic | 1h | Pratique |
| 4.8 | Documentation des problèmes | 30min | Méthodologie |
| — | **🎯 Dataset Autopsy : Analyser un dirty dataset** | 1h | Groupe |

🤖 *IA : Générer du code d'exploration automatiquement*

---

## Chapitre 5 : Nettoyage des données
**Durée : 10-12h** | **Activité principale :** Cleaning Challenge

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 5.1 | Gestion des valeurs manquantes (suppression, imputation) | 2h | Théorie + Lab |
| 5.2 | Traitement des doublons | 1h | Théorie + Lab |
| 5.3 | Traitement des valeurs aberrantes | 1.5h | Théorie + Lab |
| 5.4 | Nettoyage des types de données (dates, numériques) | 2h | Théorie + Lab |
| 5.5 | Nettoyage de texte (regex, normalisation) | 1.5h | Théorie + Lab |
| 5.6 | Nettoyage avec SQL | 1h | Théorie |
| 5.7 | Traçabilité et documentation | 30min | Méthodologie |
| — | **🎯 Cleaning Challenge** | 1.5h | Individuel |
| — | **🎯 Peer Code Review** | 1h | Pair work |
| — | **🎯 Mini-projet : Quality report + cleaning pipeline** | — | Livrable |

🤖 *IA : Suggérer une stratégie de nettoyage adaptée*

---

## Chapitre 6 : Structuration et transformation
**Durée : 8-10h** | **Activité principale :** Puzzle Lab

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 6.1 | Restructuration (pivot, melt, wide vs long) | 2h | Théorie + Lab |
| 6.2 | Combinaison de données (merge, concat) | 2h | Théorie + Lab |
| 6.3 | Agrégations (groupby, agg) | 1.5h | Théorie + Lab |
| 6.4 | Feature engineering basique | 1.5h | Théorie + Lab |
| 6.5 | Création du dataset final | 1h | Pratique |
| — | **🎯 Puzzle Lab : Répondre à des questions métier** | 1.5h | Groupe + Live coding |

🤖 *IA : Générer du code de merge complexe*

---

## Chapitre 7 : EDA analytique
**Durée : 10-12h** | **Activité principale :** Hypothesis Hunt

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 7.1 | Différence EDA diagnostique vs analytique | 30min | Théorie |
| 7.2 | Analyse univariée | 2h | Théorie + Lab |
| 7.3 | Analyse bivariée (corrélation, scatter, contingence) | 2.5h | Théorie + Lab |
| 7.4 | Segmentation et analyse par groupes | 2h | Théorie + Lab |
| 7.5 | Formulation d'hypothèses | 1h | Méthodologie |
| 7.6 | Pensée critique (corrélation ≠ causalité) | 1h | Discussion |
| — | **🎯 Hypothesis Hunt** | 1.5h | Groupe |
| — | **🎯 Discussion : Biais et limites** | 1h | Groupe |
| — | **🎯 Mini-projet : Dataset analysis-ready** | — | Livrable |

🤖 *IA : Interpréter des corrélations, suggérer des analyses*

---

## Chapitre 8 : Visualisation exploratoire
**Durée : 8-10h** | **Activité principale :** Viz Critique

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 8.1 | Principes de visualisation | 1h | Théorie |
| 8.2 | Matplotlib : fondamentaux | 2h | Théorie + Lab |
| 8.3 | Seaborn : visualisations statistiques | 2h | Théorie + Lab |
| 8.4 | Plotly : interactivité (survol) | 1h | Démo |
| 8.5 | Bonnes pratiques (lisibilité, accessibilité) | 1h | Théorie |
| 8.6 | Préparation pour Power BI | 30min | Théorie |
| — | **🎯 Viz Critique : Analyser des graphiques** | 1h | Groupe |
| — | **🎯 Mini-challenge : Best Viz Contest** | 1h | Compétition |

🤖 *IA : Générer du code matplotlib*

---

## Chapitre 9 : Chargement vers le cloud
**Durée : 4-5h** | **Activité principale :** Pipeline Completion

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 9.1 | Écrire vers S3 avec Python | 1h | Théorie + Lab |
| 9.2 | Organisation d'un data lake basique (raw/processed/final) | 1h | Théorie |
| 9.3 | Documentation du dataset (data dictionary) | 45min | Pratique |
| 9.4 | Vérification post-upload | 30min | Pratique |
| 9.5 | Bonnes pratiques professionnelles | 30min | Théorie |
| — | **🎯 Lab : Upload to S3 + Checklist validation** | 1h | Individuel |

🤖 *IA : Générer automatiquement une documentation*

---

## Chapitre 10 : Cas pratique fil rouge
**Durée : 12-15h** | **Activité principale :** MODULE PROJECT

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 10.1 | Contexte et objectifs métier | 1h | Briefing |
| 10.2 | Extraction multi-sources | 2h | Projet |
| 10.3 | EDA diagnostique | 1.5h | Projet |
| 10.4 | Nettoyage | 2h | Projet |
| 10.5 | Structuration et transformation | 2h | Projet |
| 10.6 | EDA analytique | 1.5h | Projet |
| 10.7 | Visualisations exploratoires | 1.5h | Projet |
| 10.8 | Chargement vers S3 | 1h | Projet |
| 10.9 | Préparation présentation | 1.5h | Projet |
| — | **🎯 Checkpoints réguliers** | — | Office hours |
| — | **🎯 Livrable : Notebook + Dataset S3 + Documentation** | — | Évaluation |

🤖 *IA : Utilisation libre comme assistant tout au long*

---

## Chapitre 11 : Conclusion et aide-mémoire
**Durée : 2-3h** | **Activité principale :** Reflection + Demo Day

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 11.1 | Synthèse des compétences acquises | 30min | Réflexion |
| 11.2 | Erreurs fréquentes à éviter (Top 10) | 30min | Discussion |
| 11.3 | Lien avec les modules suivants (M3 ML, M4 Power BI) | 30min | Théorie |
| — | **🎯 Quiz de validation** | 30min | Individuel |
| — | **🎯 CLASS DEMO DAY** | 1h | Présentations (3min/étudiant) |

---

## Résumé Module 2

| Ch | Titre | Leçons | Activités | Durée |
|----|-------|--------|-----------|-------|
| 1 | Pipeline vue d'ensemble | 5 | 1 | 4-5h |
| 2 | Extraction | 5 | 2 + mini-projet | 10-12h |
| 3 | Cloud (AWS S3) | 6 | 1 | 8-10h |
| 4 | EDA diagnostique | 8 | 1 | 8-10h |
| 5 | Nettoyage | 7 | 2 + mini-projet | 10-12h |
| 6 | Transformation | 5 | 1 | 8-10h |
| 7 | EDA analytique | 6 | 2 + mini-projet | 10-12h |
| 8 | Visualisation | 6 | 2 | 8-10h |
| 9 | Cloud Upload | 5 | 1 | 4-5h |
| 10 | Fil Rouge | 9 | PROJECT | 12-15h |
| 11 | Conclusion | 3 | 2 | 2-3h |
| **Total** | | **65 leçons** | **15+ activités** | **85-105h** |

---

# 📘 Module 3 — Intelligence Artificielle : du ML classique aux LLMs

**Durée totale :** 120h | **Public :** Reconversion avec bases Python, notebooks, pipelines (Module 2), prompt engineering (Module 1)

**Méthodologie :** Two-Track PBL with Conceptual Bridge

**Projets Module :** "Mon Modèle ML" (Track 1) + "Mon App LLM" (Track 2) + Final Project

---

## 🎯 TRACK 1 : ML PRACTITIONER (40h)

---

### Chapitre 1 : Introduction à l'IA et au Machine Learning
**Durée : 8h** | **Activité principale :** Use Case Gallery

---

#### Leçon 1.1 : Qu'est-ce que l'IA ? (1.5h) — Théorie + Discussion

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 1.1.1 | Histoire de l'IA | 30min | Test de Turing (1950), Hiver de l'IA, Deep Blue (1997), AlphaGo (2016), GPT (2020+) |
| 1.1.2 | Définitions : IA vs ML vs DL | 30min | IA = intelligence simulée, ML = apprentissage par données, DL = réseaux profonds. Schéma des cercles concentriques |
| 1.1.3 | L'état actuel de l'IA | 30min | Ce que l'IA sait faire aujourd'hui, ce qu'elle ne sait pas encore. Démystification : "l'IA n'est pas magique, c'est de la reconnaissance de patterns à grande échelle" |

**Ressources pour Content Creator :**
- Timeline visuelle de l'histoire de l'IA
- Schéma IA ⊃ ML ⊃ DL avec exemples

---

#### Leçon 1.2 : Types d'apprentissage (2h) — Théorie + Exemples

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 1.2.1 | Apprentissage supervisé | 45min | Définition : données étiquetées. Régression (prédire un nombre), Classification (prédire une catégorie). Exemples : prix immobilier, spam/non-spam |
| 1.2.2 | Apprentissage non-supervisé | 45min | Définition : pas d'étiquettes. Clustering (grouper), Réduction de dimension. Exemples : segmentation clients, compression |
| 1.2.3 | Apprentissage par renforcement | 30min | Définition : agent + environnement + récompenses. Exemples : jeux vidéo, robotique. **Survol conceptuel uniquement** (hors scope pratique) |

**Ressources pour Content Creator :**
- Tableau comparatif des 3 types avec exemples concrets
- Schéma visuel : Input → Modèle → Output pour chaque type

---

#### Leçon 1.3 : Le paysage de l'IA (1.5h) — Théorie + Schéma

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 1.3.1 | La pyramide de l'IA | 30min | Big Tech (entraînent from scratch) → Enterprise (fine-tune + RAG) → Startups/PME (APIs). Qui fait quoi ? |
| 1.3.2 | Les acteurs majeurs | 30min | OpenAI, Anthropic, Google DeepMind, Meta AI, Mistral. Modèles propriétaires vs open-source |
| 1.3.3 | L'écosystème technique | 30min | Frameworks (TensorFlow, PyTorch), Librairies (scikit-learn, Hugging Face), Cloud (AWS, GCP, Azure). Où se situe un data practitioner ? |

**Ressources pour Content Creator :**
- Pyramide visuelle avec logos des acteurs
- Carte mentale de l'écosystème IA

---

#### Leçon 1.4 : Cas d'usage réels (2h) — Théorie + Démos

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 1.4.1 | Recommandation | 25min | Netflix, Spotify, Amazon. Comment ça marche (collaborative filtering, content-based). Démo interactive |
| 1.4.2 | Détection de fraude | 25min | Transactions bancaires, assurance. Classification binaire sur données déséquilibrées |
| 1.4.3 | NLP (Traitement du langage) | 25min | Chatbots, traduction, résumé automatique. De BERT à ChatGPT |
| 1.4.4 | Vision par ordinateur | 25min | Reconnaissance faciale, véhicules autonomes, médical. CNN en action |
| 1.4.5 | Analyse prédictive | 20min | Churn prediction, maintenance prédictive, demand forecasting |

**🎯 Activité intégrée : Use Case Gallery**
- Analyser 5 produits IA réels (interface, fonctionnalité, type de ML probable)

**Ressources pour Content Creator :**
- 5 fiches produits avec screenshots et analyse technique
- Liens vers démos interactives en ligne

---

#### Leçon 1.5 : Éthique et limites (1h) — Discussion

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 1.5.1 | Biais algorithmiques | 20min | Sources de biais (données, conception), exemples célèbres (recrutement Amazon, reconnaissance faciale), comment mitiger |
| 1.5.2 | Hallucinations et limites | 20min | Pourquoi les LLMs inventent, limites de la généralisation, l'IA n'est pas omnisciente |
| 1.5.3 | Responsabilité et IA responsable | 20min | Coût environnemental (entraînement), impact emploi, réglementations (EU AI Act), principes d'IA responsable |

**🎯 Activité finale chapitre :** Discussion guidée "Quel type de ML pour ces 5 problèmes business ?"

**Ressources pour Content Creator :**
- Études de cas de biais documentés
- Questions de discussion préparées

---

### Chapitre 2 : Le Workflow ML
**Durée : 10h** | **Activité principale :** Pipeline Lab

---

#### Leçon 2.1 : Le pipeline ML (1h) — Théorie

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 2.1.1 | Vue d'ensemble du pipeline | 20min | Data → Preprocess → Train → Evaluate → Deploy. Schéma complet avec feedback loops |
| 2.1.2 | Lien avec Module 2 | 20min | Data pipeline (Module 2) → ML pipeline (Module 3). Les données nettoyées comme input |
| 2.1.3 | Itération et expérimentation | 20min | ML = processus itératif. Importance de tracker les expériences. Intro MLflow (conceptuel) |

**Ressources pour Content Creator :**
- Schéma pipeline ML annoté
- Comparaison data pipeline vs ML pipeline

---

#### Leçon 2.2 : Train / Test / Validation (2h) — Théorie + Lab

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 2.2.1 | Pourquoi séparer les données ? | 30min | Généralisation vs mémorisation. Métaphore : examen avec questions vues vs nouvelles |
| 2.2.2 | Train / Test split | 30min | Ratio typique 80/20, `train_test_split()`, `random_state` pour reproductibilité |
| 2.2.3 | Validation set | 30min | Train / Val / Test (60/20/20). Quand l'utiliser (tuning hyperparamètres) |
| 2.2.4 | Data leakage : l'erreur #1 | 30min | Définition, exemples concrets, comment l'éviter. **Critique : preprocessing APRÈS split** |

**Code patterns :**
```python
from sklearn.model_selection import train_test_split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)
```

**Ressources pour Content Creator :**
- Schéma des 3 ensembles avec rôles
- Exemples de data leakage avec code incorrect vs correct

---

#### Leçon 2.3 : Le pattern fit/predict (2h) — Théorie + Code

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 2.3.1 | L'API universelle scikit-learn | 30min | Tous les modèles : `.fit()`, `.predict()`, `.score()`. Cohérence = productivité |
| 2.3.2 | fit() : entraîner le modèle | 30min | Ce qui se passe : le modèle "apprend" les patterns. Paramètres vs hyperparamètres |
| 2.3.3 | predict() : faire des prédictions | 30min | Input → Output. Prédire sur données jamais vues |
| 2.3.4 | score() et transform() | 30min | `.score()` pour évaluer, `.transform()` pour preprocessing, `.fit_transform()` combiné |

**Code patterns :**
```python
from sklearn.linear_model import LinearRegression
model = LinearRegression()
model.fit(X_train, y_train)      # Apprendre
predictions = model.predict(X_test)  # Prédire
score = model.score(X_test, y_test)  # Évaluer
```

**Ressources pour Content Creator :**
- Diagramme du pattern fit/predict
- Tableau des méthodes communes sklearn

---

#### Leçon 2.4 : Pipelines scikit-learn (3h) — Théorie + Lab

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 2.4.1 | Pourquoi les pipelines ? | 30min | Problème : code spaghetti, data leakage. Solution : Pipeline = séquence encapsulée |
| 2.4.2 | Pipeline simple | 45min | `Pipeline([('scaler', StandardScaler()), ('model', LogisticRegression())])`. Fit sur train, transform automatique |
| 2.4.3 | ColumnTransformer | 45min | Traitement différent par colonnes. Numériques → scaler, Catégorielles → encoder |
| 2.4.4 | Pipeline complet | 60min | Combiner ColumnTransformer + Pipeline. Pattern production-ready |

**Code patterns :**
```python
from sklearn.pipeline import Pipeline
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import StandardScaler, OneHotEncoder

preprocessor = ColumnTransformer([
    ('num', StandardScaler(), numeric_cols),
    ('cat', OneHotEncoder(), categorical_cols)
])

pipeline = Pipeline([
    ('preprocessor', preprocessor),
    ('classifier', RandomForestClassifier())
])

pipeline.fit(X_train, y_train)
```

**Ressources pour Content Creator :**
- Schéma visuel d'un Pipeline avec ColumnTransformer
- Template de pipeline réutilisable

---

#### Leçon 2.5 : Premier modèle complet (2h) — Lab guidé

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 2.5.1 | Chargement et exploration | 20min | Charger un dataset (housing, iris), exploration rapide (`.info()`, `.describe()`) |
| 2.5.2 | Préparation des données | 30min | Identifier X et y, séparer train/test, gérer les types |
| 2.5.3 | Construire le pipeline | 40min | ColumnTransformer + modèle simple |
| 2.5.4 | Entraîner et évaluer | 30min | `.fit()`, `.predict()`, `.score()`, premières métriques |

**🎯 Activité : Pipeline Lab** — Construire un pipeline sklearn complet de A à Z
**🎯 Activité : Pair Debug** — Trouver et corriger du data leakage dans du code donné

**Ressources pour Content Creator :**
- Notebook template avec TODOs
- Code avec bugs intentionnels pour l'activité debug

---

### Chapitre 3 : Algorithmes ML Essentiels
**Durée : 10h** | **Activité principale :** Algorithm Tournament

---

#### Leçon 3.1 : Régression linéaire (2h) — Théorie + Lab

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 3.1.1 | Intuition | 30min | Tracer la "meilleure ligne". y = wx + b. Visualisation 2D |
| 3.1.2 | Comment ça apprend | 30min | Moindres carrés ordinaires (intuition, pas de formules complexes). Minimiser l'erreur |
| 3.1.3 | Interprétation des coefficients | 30min | Chaque coefficient = impact de la feature. Positive/négative, magnitude |
| 3.1.4 | Hypothèses et limites | 30min | Linéarité, pas de multicollinéarité, résidus normaux. Quand NE PAS utiliser |

**Code patterns :**
```python
from sklearn.linear_model import LinearRegression
model = LinearRegression()
model.fit(X_train, y_train)
print(f"Coefficients: {model.coef_}")
print(f"Intercept: {model.intercept_}")
```

**Ressources pour Content Creator :**
- Visualisations de régression linéaire
- Interprétation guidée des coefficients

---

#### Leçon 3.2 : Régression logistique (2h) — Théorie + Lab

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 3.2.1 | De la régression à la classification | 30min | Problème : prédire une catégorie. Régression linéaire + fonction sigmoid |
| 3.2.2 | La fonction sigmoid | 30min | Transformer un score en probabilité [0,1]. Visualisation de la courbe S |
| 3.2.3 | Seuil de décision | 30min | Threshold = 0.5 par défaut. Ajuster selon le contexte (coût des erreurs) |
| 3.2.4 | Classification multiclasse | 30min | One-vs-Rest, Softmax. Extension au-delà du binaire |

**Code patterns :**
```python
from sklearn.linear_model import LogisticRegression
model = LogisticRegression()
model.fit(X_train, y_train)
probas = model.predict_proba(X_test)  # Probabilités
preds = model.predict(X_test)          # Classes
```

**Ressources pour Content Creator :**
- Visualisation sigmoid
- Exemple de seuil ajusté selon le business

---

#### Leçon 3.3 : Arbres de décision & Random Forest (2.5h) — Théorie + Lab

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 3.3.1 | Arbre de décision : intuition | 30min | Série de questions if/else. Visualisation d'un arbre. Interprétable par un humain |
| 3.3.2 | Comment l'arbre se construit | 30min | Critères de split (Gini, Entropy). Profondeur, feuilles, pruning |
| 3.3.3 | Le problème de l'overfitting | 30min | Arbres profonds = mémorisation. Visualiser un arbre overfit |
| 3.3.4 | Random Forest : l'ensemble | 30min | Beaucoup d'arbres → vote majoritaire. Bootstrap, random features |
| 3.3.5 | Feature importance | 30min | Quelles features comptent le plus ? `.feature_importances_` |

**Code patterns :**
```python
from sklearn.tree import DecisionTreeClassifier, plot_tree
from sklearn.ensemble import RandomForestClassifier

tree = DecisionTreeClassifier(max_depth=3)
tree.fit(X_train, y_train)
plot_tree(tree, feature_names=X.columns, filled=True)

rf = RandomForestClassifier(n_estimators=100)
rf.fit(X_train, y_train)
print(rf.feature_importances_)
```

**🎯 Activité intégrée :** Dessiner un arbre de décision à la main

**Ressources pour Content Creator :**
- Visualisations d'arbres avec plot_tree
- Comparaison arbre simple vs Random Forest

---

#### Leçon 3.4 : K-Means clustering (2h) — Théorie + Lab

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 3.4.1 | Qu'est-ce que le clustering ? | 30min | Non-supervisé : trouver des groupes naturels. Pas d'étiquettes |
| 3.4.2 | Algorithme K-Means | 30min | 1. Choisir K centres, 2. Assigner points, 3. Recalculer centres, 4. Répéter |
| 3.4.3 | Choisir K : méthode du coude | 30min | Elbow method : inertie vs K. Silhouette score |
| 3.4.4 | Cas d'usage | 30min | Segmentation clients, anomaly detection, compression d'images |

**Code patterns :**
```python
from sklearn.cluster import KMeans

kmeans = KMeans(n_clusters=3, random_state=42)
clusters = kmeans.fit_predict(X)

# Méthode du coude
inertias = [KMeans(n_clusters=k).fit(X).inertia_ for k in range(1, 10)]
```

**Ressources pour Content Creator :**
- Animation de l'algorithme K-Means
- Graphique de la méthode du coude

---

#### Leçon 3.5 : Quand utiliser quoi ? (1.5h) — Théorie + Discussion

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 3.5.1 | Arbre de décision pour choisir | 30min | Flowchart : Type de problème → Type de données → Algorithme recommandé |
| 3.5.2 | Compromis à considérer | 30min | Interprétabilité vs performance, vitesse vs précision, simplicité vs complexité |
| 3.5.3 | Recommandations pratiques | 30min | "Commencer simple" (baseline), puis complexifier. Ne pas sur-ingénierer |

**🎯 Activité : Algorithm Tournament** — Même dataset, 4 algorithmes différents, comparer les résultats

**Ressources pour Content Creator :**
- Flowchart de sélection d'algorithme (PDF imprimable)
- Tableau comparatif des algorithmes

---

### Chapitre 4 : Évaluation et Optimisation
**Durée : 12h** | **Activité principale :** Diagnostic Challenge

---

#### Leçon 4.1 : Métriques de régression (2h) — Théorie + Code

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 4.1.1 | MSE et RMSE | 30min | Mean Squared Error : pénalise les grandes erreurs. RMSE : même unité que y |
| 4.1.2 | MAE | 30min | Mean Absolute Error : interprétable, moins sensible aux outliers |
| 4.1.3 | R² (coefficient de détermination) | 30min | % de variance expliquée. 0 = nul, 1 = parfait. Peut être négatif ! |
| 4.1.4 | Quelle métrique choisir ? | 30min | Contexte business : grosses erreurs coûteuses → MSE. Robustesse → MAE |

**Code patterns :**
```python
from sklearn.metrics import mean_squared_error, mean_absolute_error, r2_score
import numpy as np

mse = mean_squared_error(y_test, predictions)
rmse = np.sqrt(mse)
mae = mean_absolute_error(y_test, predictions)
r2 = r2_score(y_test, predictions)
```

---

#### Leçon 4.2 : Métriques de classification (2h) — Théorie + Code

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 4.2.1 | Accuracy et ses limites | 30min | (TP+TN)/Total. **Problème : classes déséquilibrées** (99% accuracy ≠ bon) |
| 4.2.2 | Precision | 30min | "Des positifs prédits, combien sont vrais ?" TP/(TP+FP). Important si FP coûteux |
| 4.2.3 | Recall (Sensibilité) | 30min | "Des vrais positifs, combien trouvés ?" TP/(TP+FN). Important si FN coûteux |
| 4.2.4 | F1-Score | 30min | Moyenne harmonique precision/recall. Équilibre les deux |

**Code patterns :**
```python
from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score

accuracy = accuracy_score(y_test, predictions)
precision = precision_score(y_test, predictions)
recall = recall_score(y_test, predictions)
f1 = f1_score(y_test, predictions)
```

---

#### Leçon 4.3 : Matrice de confusion et ROC-AUC (1h) — Théorie + Visualisation

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 4.3.1 | Matrice de confusion | 30min | TP, TN, FP, FN. Visualiser les erreurs. `confusion_matrix()`, heatmap |
| 4.3.2 | Courbe ROC et AUC | 30min | ROC = TPR vs FPR à différents seuils. AUC = aire sous la courbe. Qualité du ranking |

**Code patterns :**
```python
from sklearn.metrics import confusion_matrix, roc_auc_score, roc_curve
import seaborn as sns

cm = confusion_matrix(y_test, predictions)
sns.heatmap(cm, annot=True, fmt='d')

auc = roc_auc_score(y_test, model.predict_proba(X_test)[:, 1])
```

---

#### Leçon 4.4 : Overfitting / Underfitting (2h) — Théorie + Lab

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 4.4.1 | Définitions | 30min | Underfitting = modèle trop simple. Overfitting = modèle mémorise le train |
| 4.4.2 | Diagnostic visuel | 30min | Train score vs Test score. Gap = overfitting. Both low = underfitting |
| 4.4.3 | Le compromis biais-variance | 30min | Biais = erreur systématique, Variance = sensibilité aux données. Tradeoff |
| 4.4.4 | Solutions | 30min | Overfitting → plus de données, régularisation, modèle plus simple. Underfitting → modèle plus complexe, plus de features |

**Ressources pour Content Creator :**
- Graphiques biais-variance
- Exemples visuels under/overfitting

---

#### Leçon 4.5 : Validation croisée (2h) — Théorie + Lab

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 4.5.1 | Problème du train/test unique | 30min | Un seul split = résultat chanceux ou malchanceux. Besoin de robustesse |
| 4.5.2 | K-Fold Cross-Validation | 45min | Diviser en K folds, entraîner K fois, moyenner. Visualisation |
| 4.5.3 | Stratified K-Fold | 30min | Préserver les proportions de classes. Essentiel pour données déséquilibrées |
| 4.5.4 | Interprétation des résultats | 15min | Moyenne ± écart-type. Stabilité du modèle |

**Code patterns :**
```python
from sklearn.model_selection import cross_val_score, StratifiedKFold

scores = cross_val_score(model, X, y, cv=5)
print(f"Accuracy: {scores.mean():.3f} (+/- {scores.std()*2:.3f})")

# Stratified pour classification
skf = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
scores = cross_val_score(model, X, y, cv=skf)
```

---

#### Leçon 4.6 : Optimisation des hyperparamètres (3h) — Théorie + Lab

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 4.6.1 | Paramètres vs Hyperparamètres | 30min | Paramètres = appris par le modèle. Hyperparamètres = définis avant entraînement |
| 4.6.2 | GridSearchCV | 60min | Recherche exhaustive. Définir une grille, tester toutes combinaisons |
| 4.6.3 | RandomizedSearchCV | 45min | Recherche aléatoire. Plus rapide, souvent suffisant |
| 4.6.4 | Hyperparamètres clés | 45min | RandomForest : n_estimators, max_depth. LogReg : C. KMeans : n_clusters |

**Code patterns :**
```python
from sklearn.model_selection import GridSearchCV, RandomizedSearchCV

param_grid = {
    'n_estimators': [50, 100, 200],
    'max_depth': [None, 10, 20],
    'min_samples_split': [2, 5, 10]
}

grid_search = GridSearchCV(RandomForestClassifier(), param_grid, cv=5, scoring='f1')
grid_search.fit(X_train, y_train)
print(f"Best params: {grid_search.best_params_}")
print(f"Best score: {grid_search.best_score_}")
```

**🎯 Activité : Diagnostic Challenge** — Recevoir un modèle overfit, diagnostiquer et corriger
**🎯 Activité : GridSearch Competition** — Meilleur F1-score sur un dataset commun

**🎯 Mini-projet livrable : "Mon Modèle ML"**

**Ressources pour Content Creator :**
- Tableau des hyperparamètres par algorithme
- Template de rapport d'évaluation

---

## 🌉 BRIDGE : CONCEPTUAL (24h)

---

### Chapitre 5 : Réseaux de Neurones (Conceptuel)
**Durée : 12h** | **Activité principale :** Playground Exploration

**Objectif :** Comprendre le passage du ML classique au Deep Learning, maîtriser l'architecture universelle (MLP) et comprendre la mécanique d'apprentissage (Backpropagation).

**Narration logique :** Le Problème → La Brique → La Structure → La Vie → Les Défis → La Pratique

> 📄 **Détails complets :** Voir `chap5.md` pour le contenu détaillé des 6 leçons.

| Leçon | Titre | Durée | Focus |
|-------|-------|-------|-------|
| 5.1 | De l'Ingénierie à l'Apprentissage | 1h30 | Le "Pourquoi" — Limites du ML, apprentissage de représentation |
| 5.2 | Le Neurone Artificiel | 2h | La Brique — Lien avec Régression Logistique, activations |
| 5.3 | L'Architecture Standard — Le MLP | 2h | La Structure — Shallow vs Deep, Fully Connected |
| 5.4 | La Mécanique d'Apprentissage | 2h30 | Le Cerveau — Forward, Loss, Gradient Descent, Backprop |
| 5.5 | Les Défis de la Profondeur | 1h30 | Les Solutions — Vanishing gradients, ReLU, BatchNorm, Dropout |
| 5.6 | Atelier Pratique & Synthèse | 2h30 | La Pratique — TensorFlow Playground, Keras MNIST, Quiz |

**🎯 Activité principale :** Playground Exploration — TensorFlow Playground
**🎯 Activité secondaire :** Démo Keras — MNIST en 10 lignes

**Transition vers Ch6 :** "Nous avons maîtrisé le MLP, l'architecture universelle. Mais pour les images et le texte, il existe des architectures spécialisées. Direction : CNN, RNN, et les révolutionnaires Transformers."

---

### Chapitre 6 : Architectures Spécialisées, Transformers & Fine-tuning
**Durée : 12h** | **Activité principale :** Attention Visualization

> ⚠️ **Note (2026-02-01):** Ce chapitre doit maintenant inclure CNN et RNN (déplacés depuis Ch5) avant les Transformers. Nouvelle progression : CNN/RNN → Limites RNN → Attention → Transformers → Fine-tuning.

---

#### Leçon 6.1 : Les limites des RNN (1h) — Théorie

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 6.1.1 | Traitement séquentiel | 20min | RNN traite mot par mot → lent, pas parallélisable |
| 6.1.2 | Problème des longues séquences | 20min | Vanishing gradients : oublie le début. LSTM/GRU = solutions partielles |
| 6.1.3 | Besoin d'une nouvelle approche | 20min | 2017 : "Attention is All You Need" → révolution Transformer |

---

#### Leçon 6.2 : L'attention (3h) — Théorie + Visualisation

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 6.2.1 | Intuition de l'attention | 45min | "Quels mots sont importants pour comprendre ce mot ?" Focus sélectif |
| 6.2.2 | Query, Key, Value | 60min | Métaphore bibliothèque : Query = question, Key = étiquettes livres, Value = contenu |
| 6.2.3 | Self-attention | 45min | Chaque mot "regarde" tous les autres mots. Matrice d'attention |
| 6.2.4 | Multi-head attention | 30min | Plusieurs "têtes" = plusieurs perspectives en parallèle |

**Ressources pour Content Creator :**
- Visualisation de matrice d'attention
- Exemples : "The cat sat on the mat because it was tired" → "it" attend à "cat"

---

#### Leçon 6.3 : Architecture Transformer (2.5h) — Théorie + Schémas

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 6.3.1 | Vue d'ensemble | 30min | Encoder-Decoder. Schéma de l'architecture complète |
| 6.3.2 | Positional encoding | 30min | Transformers voient tout en parallèle → besoin de position. Sinusoïdes |
| 6.3.3 | L'encoder | 45min | Self-attention + Feed-forward. Comprendre l'input |
| 6.3.4 | Le decoder | 45min | Masked self-attention + cross-attention. Générer l'output |

**Ressources pour Content Creator :**
- Schéma annoté du Transformer original
- Comparaison encoder-only vs decoder-only

---

#### Leçon 6.4 : Du Transformer aux LLMs (2h) — Théorie

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 6.4.1 | BERT (2018) | 40min | Encoder-only. Bidirectionnel. Tâches : classification, NER, Q&A |
| 6.4.2 | GPT (2018→) | 40min | Decoder-only. Autorégressif. Génération de texte token par token |
| 6.4.3 | L'effet de l'échelle | 40min | GPT-3 = 175B paramètres. Capacités émergentes. Scaling laws |

---

#### Leçon 6.5 : Pre-training vs Fine-tuning (2.5h) — Théorie

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 6.5.1 | Pre-training | 30min | Entraînement massif sur internet. Fait par Big Tech. Transfer learning |
| 6.5.2 | Quand fine-tuner ? | 30min | Style spécifique, vocabulaire domaine, comportement consistant. vs RAG |
| 6.5.3 | Full fine-tuning | 30min | Mettre à jour tous les poids. Coûteux, risque de catastrophic forgetting |
| 6.5.4 | LoRA et QLoRA | 45min | Adapters : petits modules ajoutés. Efficace, moderne. Low-Rank Adaptation |
| 6.5.5 | Comparaison des approches | 15min | Tableau : coût, complexité, use cases pour chaque approche |

---

#### Leçon 6.6 : Fine-tuning pratique (1h) — Lab Colab

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 6.6.1 | Setup Hugging Face | 20min | Transformers library, datasets, tokenizers |
| 6.6.2 | Fine-tuner BERT | 40min | Classification de sentiment. Code minimal avec Trainer |

**🎯 Activité : Attention Visualization** — Voir les patterns d'attention de BERT sur une phrase
**🎯 Activité : Colab Lab** — Fine-tuner un classifier de sentiment

**Code patterns :**
```python
from transformers import AutoTokenizer, AutoModelForSequenceClassification, Trainer

tokenizer = AutoTokenizer.from_pretrained("bert-base-uncased")
model = AutoModelForSequenceClassification.from_pretrained("bert-base-uncased", num_labels=2)

# ... training code
```

**Ressources pour Content Creator :**
- Notebook Colab prêt à l'emploi
- Dataset de sentiment préparé

---

## 🚀 TRACK 2 : LLM BUILDER (44h)

---

### Chapitre 7 : Embeddings et Recherche Vectorielle
**Durée : 10h** | **Activité principale :** Semantic Search Lab

---

#### Leçon 7.1 : Qu'est-ce qu'un embedding ? (2h) — Théorie + Visualisation

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 7.1.1 | Mots comme vecteurs | 30min | Représenter du texte par des nombres. Dimension = 384, 768, ou 1536 |
| 7.1.2 | Sens sémantique | 30min | Mots similaires → vecteurs proches. "roi" - "homme" + "femme" ≈ "reine" |
| 7.1.3 | Word embeddings vs Sentence embeddings | 30min | Word2Vec (mots) vs Sentence Transformers (phrases entières) |
| 7.1.4 | Visualisation | 30min | Projeter en 2D (t-SNE, UMAP). Voir les clusters sémantiques |

**Ressources pour Content Creator :**
- Visualisation d'embeddings projetés
- Démo interactive word2vec

---

#### Leçon 7.2 : Créer des embeddings (2h) — Théorie + Code

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 7.2.1 | OpenAI Embeddings | 30min | ada-002 : 1536 dim, bonne qualité. API payante, simple d'usage |
| 7.2.2 | Sentence Transformers | 45min | Gratuit, local. all-MiniLM-L6-v2 (384 dim, rapide), all-mpnet-base-v2 (768, meilleur) |
| 7.2.3 | Autres options | 20min | Voyage AI, Cohere, BGE. Critères de choix |
| 7.2.4 | Choisir un modèle | 25min | Tradeoffs : qualité vs coût vs vitesse vs taille |

**Code patterns :**
```python
from sentence_transformers import SentenceTransformer

model = SentenceTransformer('all-MiniLM-L6-v2')
embeddings = model.encode(["Hello world", "Bonjour le monde"])
print(embeddings.shape)  # (2, 384)
```

---

#### Leçon 7.3 : Similarité et distance (1.5h) — Théorie + Lab

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 7.3.1 | Cosine similarity | 30min | Angle entre vecteurs. -1 (opposé) à 1 (identique). Le plus utilisé |
| 7.3.2 | Distance euclidienne | 20min | Distance absolue. Sensible à la magnitude |
| 7.3.3 | Dot product | 20min | Produit scalaire. Lié au cosine si normalisé |
| 7.3.4 | Quelle métrique choisir ? | 20min | Cosine pour texte (presque toujours). Euclidean pour images parfois |

**Code patterns :**
```python
from sklearn.metrics.pairwise import cosine_similarity
import numpy as np

sim = cosine_similarity([emb1], [emb2])[0][0]
```

---

#### Leçon 7.4 : Bases vectorielles (2.5h) — Théorie + Lab

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 7.4.1 | Pourquoi une base vectorielle ? | 30min | Millions de vecteurs → recherche rapide impossible avec brute force |
| 7.4.2 | ChromaDB | 45min | Simple, local, persistant. Parfait pour apprendre et prototypes |
| 7.4.3 | Pinecone | 30min | Managed cloud. Scalable, production-ready. API simple |
| 7.4.4 | FAISS | 30min | Facebook AI. Très rapide. Pas de persistence par défaut. Pour gros volumes |
| 7.4.5 | Comparaison et choix | 15min | Tableau : local vs cloud, gratuit vs payant, cas d'usage |

**Code patterns :**
```python
import chromadb

client = chromadb.Client()
collection = client.create_collection("my_collection")

collection.add(
    documents=["doc1", "doc2"],
    ids=["id1", "id2"]
)

results = collection.query(query_texts=["search query"], n_results=3)
```

---

#### Leçon 7.5 : Chunking de documents (2h) — Théorie + Pratique

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 7.5.1 | Pourquoi chunker ? | 25min | LLMs ont des limites de contexte. Documents longs → morceaux |
| 7.5.2 | Taille des chunks | 30min | Typiquement 256-1024 tokens. Trop petit = perte contexte. Trop grand = bruit |
| 7.5.3 | Overlap | 25min | 10-20% de chevauchement. Éviter de couper au milieu d'une idée |
| 7.5.4 | Stratégies de chunking | 40min | Par paragraphe, par phrase, taille fixe, RecursiveCharacterTextSplitter |

**Code patterns :**
```python
from langchain.text_splitter import RecursiveCharacterTextSplitter

splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=200,
    separators=["\n\n", "\n", " ", ""]
)
chunks = splitter.split_text(long_document)
```

**🎯 Activité : Semantic Search Lab** — Recherche sémantique sur des phrases
**🎯 Activité : Challenge** — Trouver l'intrus sémantique (quel mot n'appartient pas au groupe ?)

**Ressources pour Content Creator :**
- Notebook de semantic search complet
- Exercice de l'intrus avec solution

---

### Chapitre 8 : RAG — Connecter LLM et Données
**Durée : 14h** | **Activité principale :** RAG Debugging

---

#### Leçon 8.1 : Pourquoi RAG ? (1.5h) — Théorie

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 8.1.1 | Limites des LLMs | 30min | Knowledge cutoff (ne connaît pas l'actualité). Pas accès à vos données |
| 8.1.2 | Hallucinations | 30min | LLMs inventent des faits. Problème pour applications critiques |
| 8.1.3 | RAG comme solution | 30min | Retrieval-Augmented Generation. Donner le contexte au LLM, pas d'entraînement |

---

#### Leçon 8.2 : Architecture RAG (2h) — Théorie + Schéma

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 8.2.1 | Vue d'ensemble | 30min | Documents → Chunk & Embed → Vector Store → Query → Retrieve → Generate → Answer |
| 8.2.2 | Phase d'ingestion | 30min | Loader → Splitter → Embedder → Store. Fait une fois |
| 8.2.3 | Phase de query | 30min | Query → Embed → Retrieve Top-K → Context → LLM → Answer. À chaque question |
| 8.2.4 | Flux de données | 30min | Schéma annoté complet avec exemples |

**Ressources pour Content Creator :**
- Schéma d'architecture RAG (imprimable)
- Exemple concret avec un document PDF

---

#### Leçon 8.3 : LangChain & LCEL (3h) — Théorie + Lab

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 8.3.1 | Pourquoi LangChain ? | 30min | Abstraction des composants RAG. Écosystème riche. LCEL = nouvelle API |
| 8.3.2 | L'opérateur pipe (|) | 45min | Chaîner des composants : `prompt | llm | parser`. Composable |
| 8.3.3 | Runnables | 45min | RunnablePassthrough, RunnableLambda, RunnableParallel. Blocs de construction |
| 8.3.4 | Construire une chaîne RAG | 60min | De zéro : loader + splitter + embedder + retriever + prompt + llm |

**Code patterns :**
```python
from langchain_core.runnables import RunnablePassthrough
from langchain_core.output_parsers import StrOutputParser
from langchain_openai import ChatOpenAI
from langchain.prompts import ChatPromptTemplate

template = """Answer based on this context:
{context}

Question: {question}"""

prompt = ChatPromptTemplate.from_template(template)

chain = (
    {"context": retriever, "question": RunnablePassthrough()}
    | prompt
    | ChatOpenAI()
    | StrOutputParser()
)

response = chain.invoke("What is...?")
```

---

#### Leçon 8.4 : Stratégies de retrieval (2.5h) — Théorie + Lab

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 8.4.1 | Similarity search | 30min | Nearest neighbors. Simple, rapide. Peut manquer de diversité |
| 8.4.2 | MMR (Maximal Marginal Relevance) | 45min | Équilibre pertinence + diversité. Évite les doublons sémantiques |
| 8.4.3 | Hybrid search | 45min | Combiner keyword (BM25) + semantic. Meilleur des deux mondes |
| 8.4.4 | Reranking | 30min | Second modèle pour réordonner les résultats. Cohere Rerank, cross-encoders |

**Code patterns :**
```python
# Similarity
docs = retriever.get_relevant_documents(query)

# MMR
retriever = vectorstore.as_retriever(
    search_type="mmr",
    search_kwargs={"k": 5, "fetch_k": 20}
)
```

---

#### Leçon 8.5 : Prompts pour RAG (1.5h) — Théorie + Pratique

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 8.5.1 | System prompt | 30min | Définir le rôle. "You are a helpful assistant. Use ONLY the context below." |
| 8.5.2 | Context injection | 30min | Placeholder {context}. Format du contexte (numérotation, séparateurs) |
| 8.5.3 | Citation et sourcing | 30min | Exiger des citations. "Quote the source for each fact." Traçabilité |

---

#### Leçon 8.6 : Évaluation du RAG (1.5h) — Théorie + Pratique

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 8.6.1 | Évaluer le retrieval | 30min | Les bons documents sont-ils récupérés ? Precision@K, Recall@K |
| 8.6.2 | Évaluer la génération | 30min | La réponse est-elle correcte ? Faithfulness, relevance |
| 8.6.3 | Frameworks d'évaluation | 30min | RAGAS, LangSmith. Métriques automatisées |

---

#### Leçon 8.7 : RAG complet (2h) — Lab guidé

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 8.7.1 | Charger des PDFs | 30min | PyPDFLoader, DirectoryLoader. Extraire le texte |
| 8.7.2 | Chunker et embedder | 30min | RecursiveCharacterTextSplitter + OpenAI/SentenceTransformer |
| 8.7.3 | Stocker dans ChromaDB | 30min | Créer la collection, ajouter les documents |
| 8.7.4 | Créer le chatbot | 30min | Chaîne LCEL complète. Interface simple (input/output) |

**🎯 Activité : RAG Debugging** — Réparer un pipeline RAG cassé (bugs intentionnels)
**🎯 Activité : Compare** — Similarity vs MMR sur les mêmes queries, analyser les différences

**Ressources pour Content Creator :**
- Notebook RAG complet avec PDFs
- Code bugué pour l'activité debugging

---

### Chapitre 9 : Agents et Automatisation IA
**Durée : 14h** | **Activité principale :** Tool Builder Workshop

---

#### Leçon 9.1 : Du prompt à l'exécution (1h) — Théorie + Discussion

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 9.1.1 | Rappel Module 1 | 20min | Agents en .md : orchestrateur, instructions, routing conceptuel |
| 9.1.2 | Le pont Module 1 → Module 3 | 20min | Module 1 = LLM "pretend to act". Module 3 = LLM ACTUALLY executes |
| 9.1.3 | Ce que ce chapitre ajoute | 20min | Code-based agents : LangGraph, MCP, vraie exécution d'outils |

**Tableau de comparaison :**
| Module 1 (Prompt) | Module 3 (Code) |
|-------------------|-----------------|
| orchestrator.md | LangGraph StateGraph |
| "Route to search agent" | def route(state): return "search" |
| LLM pretends to search | Agent ACTUALLY calls search API |

---

#### Leçon 9.2 : Function calling (2h) — Théorie + Lab

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 9.2.1 | Qu'est-ce que le function calling ? | 30min | LLM output = JSON structuré décrivant un appel de fonction |
| 9.2.2 | Format OpenAI | 30min | tools parameter, function definitions, tool_calls response |
| 9.2.3 | Format Claude | 30min | tool_use blocks, format Anthropic |
| 9.2.4 | Parser et exécuter | 30min | Votre code parse le JSON, exécute la fonction, renvoie le résultat |

**Code patterns :**
```python
tools = [{
    "type": "function",
    "function": {
        "name": "get_weather",
        "description": "Get weather for a city",
        "parameters": {
            "type": "object",
            "properties": {
                "city": {"type": "string"}
            }
        }
    }
}]

response = client.chat.completions.create(
    model="gpt-4",
    messages=messages,
    tools=tools
)
```

---

#### Leçon 9.3 : LangGraph : état et flux (3.5h) — Théorie + Lab

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 9.3.1 | Pourquoi LangGraph ? | 30min | Limitations des chains linéaires. Besoin de boucles, conditions, état |
| 9.3.2 | Concepts fondamentaux | 45min | Graph = Nodes + Edges. State = contexte partagé. START, END |
| 9.3.3 | Créer des nœuds | 45min | Fonctions qui prennent state, retournent state modifié |
| 9.3.4 | Conditional edges | 45min | Routing dynamique selon le state. should_continue, route functions |
| 9.3.5 | Construire un graph complet | 45min | Exemple : agent qui décide → agit → évalue → recommence ou finit |

**Code patterns :**
```python
from langgraph.graph import StateGraph, END
from typing import TypedDict

class AgentState(TypedDict):
    messages: list
    next_action: str

def agent_node(state: AgentState) -> AgentState:
    # LLM decides next action
    return {"next_action": "search"}

def tool_node(state: AgentState) -> AgentState:
    # Execute tool
    return {"messages": state["messages"] + [result]}

def should_continue(state: AgentState) -> str:
    if state["next_action"] == "finish":
        return END
    return "tool_node"

graph = StateGraph(AgentState)
graph.add_node("agent", agent_node)
graph.add_node("tool_node", tool_node)
graph.add_conditional_edges("agent", should_continue)
graph.add_edge("tool_node", "agent")

app = graph.compile()
```

---

#### Leçon 9.4 : Créer des tools (2.5h) — Théorie + Lab

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 9.4.1 | Anatomie d'un tool | 30min | Nom, description, paramètres, fonction. Le LLM lit la description pour décider |
| 9.4.2 | @tool decorator | 45min | LangChain @tool. Docstring = description. Type hints = paramètres |
| 9.4.3 | Tools utiles | 45min | Calculator, search (Tavily, DuckDuckGo), file operations, API calls |
| 9.4.4 | Bonnes pratiques | 30min | Descriptions claires, gestion d'erreurs, timeout, validation inputs |

**Code patterns :**
```python
from langchain_core.tools import tool

@tool
def calculate(expression: str) -> str:
    """Evaluate a mathematical expression. Use this for any math calculations.

    Args:
        expression: A valid Python math expression like "2 + 2" or "sqrt(16)"
    """
    return str(eval(expression))

@tool
def search_web(query: str) -> str:
    """Search the web for current information.

    Args:
        query: The search query
    """
    # Implementation
    return results
```

---

#### Leçon 9.5 : MCP en pratique (3h) — Théorie + Lab

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 9.5.1 | Qu'est-ce que MCP ? | 30min | Model Context Protocol (Anthropic). Standard pour LLM ↔ tools communication |
| 9.5.2 | Architecture MCP | 45min | MCP Server (expose tools/resources) ↔ MCP Client (LLM/app). Interopérabilité |
| 9.5.3 | Resources vs Tools | 30min | Resources = données accessibles. Tools = actions exécutables |
| 9.5.4 | Créer un MCP server | 45min | Python SDK. Exposer des tools, définir des resources |
| 9.5.5 | Connecter à Claude | 30min | Configuration Claude Desktop. Tester le server |

**Code patterns :**
```python
from mcp.server import Server
from mcp.types import Tool

server = Server("my-server")

@server.tool()
async def my_tool(param: str) -> str:
    """Tool description"""
    return f"Result: {param}"

# Run server
```

**🎯 Activité : Convertir un agent Module 1 (.md) en MCP server**

---

#### Leçon 9.6 : Agent complet (2h) — Lab guidé

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 9.6.1 | Architecture cible | 20min | Orchestrator + Tools + State + Error handling |
| 9.6.2 | Implémenter l'agent | 60min | LangGraph avec 3 tools : search, calculate, file_read |
| 9.6.3 | Gestion d'erreurs | 20min | Try/catch, retry logic, fallbacks |
| 9.6.4 | Tester et itérer | 20min | Debug, logging, amélioration des prompts |

**🎯 Activité : Tool Builder Workshop** — Créer 3 tools custom
**🎯 Activité : LangGraph Lab** — Workflow conditionnel complet
**🎯 Mini-projet livrable : "Mon App LLM"**

**Ressources pour Content Creator :**
- Template d'agent LangGraph
- Exemples de tools réutilisables

---

### Chapitre 10 : Stratégie IA en Entreprise
**Durée : 6h** | **Activité principale :** Case Study Debate

---

#### Leçon 10.1 : Framework de décision (1.5h) — Théorie

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 10.1.1 | Quand utiliser une API ? | 25min | Low volume, pas de données sensibles, besoin de démarrer vite |
| 10.1.2 | Quand utiliser RAG ? | 25min | Besoin de knowledge custom, données changent souvent, pas de training |
| 10.1.3 | Quand fine-tuner ? | 25min | Style spécifique, vocabulaire domaine, comportement consistant, volume élevé |
| 10.1.4 | Quand self-host ? | 25min | Données très sensibles, réglementations strictes, contrôle total |

**Ressources pour Content Creator :**
- Flowchart de décision (PDF)
- Exemples par cas d'usage

---

#### Leçon 10.2 : Analyse des coûts (1.5h) — Théorie + Calculs

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 10.2.1 | Coût des tokens | 30min | Input vs output tokens. Pricing OpenAI, Claude, Mistral |
| 10.2.2 | TCO (Total Cost of Ownership) | 30min | API fees + infrastructure + développement + maintenance |
| 10.2.3 | Calculer pour différents scénarios | 30min | Exercice : 1000 users/jour, documents de 10 pages, réponses de 500 tokens |

**Tableau de coûts :**
| Approche | Upfront | Per-use |
|----------|---------|---------|
| API only | $0 | $$$/token |
| RAG + API | $ (infra) | $$$/token |
| Fine-tuned API | $$ (train) | $$/token |
| Self-hosted | $$$ (GPUs) | $/token |

---

#### Leçon 10.3 : Comparaison des fournisseurs (1h) — Théorie + Tableau

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 10.3.1 | OpenAI | 15min | Meilleur écosystème, GPT-4/o1, cher, closed-source |
| 10.3.2 | Anthropic (Claude) | 15min | Safety-focused, contexte long (200K), enterprise, Claude 3.5 Sonnet |
| 10.3.3 | Mistral | 15min | EU-based, modèles ouverts (Mixtral), compétitif en prix |
| 10.3.4 | Open-source | 15min | LLaMA, Mixtral local. Contrôle total, besoin d'infra |

---

#### Leçon 10.4 : Études de cas (1.5h) — Discussion

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 10.4.1 | Startup (< 50 personnes) | 20min | Budget limité, rapidité. Recommandation : API only |
| 10.4.2 | Scale-up (50-500) | 20min | Croissance, données propriétaires. Recommandation : RAG + APIs |
| 10.4.3 | Enterprise (500+) | 25min | Volume, compliance, multi-use cases. Recommandation : Hybrid (RAG + fine-tune) |
| 10.4.4 | Secteur régulé | 25min | Santé, finance. Contraintes : données on-premise, audit. Solutions adaptées |

---

#### Leçon 10.5 : Rédiger une recommandation (0.5h) — Méthodologie

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 10.5.1 | Structure d'un document stratégie IA | 30min | Contexte → Problème → Options → Recommandation → Coûts → Risques → Timeline |

**🎯 Activité : Case Study Debate** — Défendre une approche IA pour un scénario donné
**🎯 Activité : Cost Calculator** — Calculer le TCO pour 3 scénarios différents

**Ressources pour Content Creator :**
- Template de recommandation IA (1 page)
- Spreadsheet de calcul de coûts

---

## 🏆 FINAL PROJECT (14h)

---

### Chapitre 11 : Projet Intégrateur
**Durée : 14h** | **Activité principale :** MODULE PROJECT

---

#### Leçon 11.1 : Cahier des charges (2h) — Briefing + Planning

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 11.1.1 | Choisir une option | 30min | Option A (Data Analyst), B (Research Agent), ou C (Knowledge Assistant) |
| 11.1.2 | Définir le scope | 45min | Features obligatoires vs optionnelles. MVP mindset |
| 11.1.3 | Critères de succès | 30min | Comment savoir si le projet est réussi ? Métriques |
| 11.1.4 | Planning du projet | 15min | Timeline, milestones, livrables intermédiaires |

---

#### Leçon 11.2 : Pipeline de données (3h) — Projet

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 11.2.1 | Connexion avec Module 2 | 45min | Réutiliser les compétences data pipeline |
| 11.2.2 | Extraction des données | 1h | Sources multiples si applicable |
| 11.2.3 | Nettoyage et préparation | 1h15 | Qualité des données pour ML/LLM |

---

#### Leçon 11.3 : Modèle ML (3h) — Projet

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 11.3.1 | Entraînement | 1h | Appliquer les compétences Track 1 |
| 11.3.2 | Évaluation | 1h | Métriques appropriées |
| 11.3.3 | Tuning | 1h | Optimisation des hyperparamètres |

---

#### Leçon 11.4 : Intégration LLM (4h) — Projet

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 11.4.1 | RAG ou Agent layer | 2h | Appliquer les compétences Track 2 |
| 11.4.2 | Connexion ML ↔ LLM | 1h | L'output ML devient input LLM |
| 11.4.3 | Interface utilisateur | 1h | Simple input/output ou Streamlit basique |

---

#### Leçon 11.5 : Présentation (2h) — Projet + Présentation

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 11.5.1 | Documentation | 30min | README, architecture diagram, data dictionary |
| 11.5.2 | Préparation démo | 30min | Script de démonstration, cas d'usage |
| 11.5.3 | Slides | 30min | 5 slides max : Problème, Solution, Démo, Résultats, Learnings |
| 11.5.4 | Répétition | 30min | Timing, anticipation questions |

**🎯 Checkpoints réguliers** — Office hours avec l'instructeur
**🎯 CLASS DEMO DAY** — Présentations (15min/étudiant : 5min présentation + 3min démo + 5min Q&A + 2min feedback)

**Ressources pour Content Creator :**
- Template de cahier des charges
- Checklist de livrables
- Rubrique d'évaluation détaillée

---

## 📊 Résumé Final Module 3

| Ch | Titre | Leçons | Sous-parties | Activités | Durée | Track |
|----|-------|--------|--------------|-----------|-------|-------|
| 1 | Introduction IA & ML | 5 | 17 | 2 | 8h | Track 1 |
| 2 | Workflow ML | 5 | 18 | 2 | 10h | Track 1 |
| 3 | Algorithmes ML | 5 | 19 | 2 | 10h | Track 1 |
| 4 | Évaluation & Optimisation | 6 | 22 | 3 + mini-projet | 12h | Track 1 |
| 5 | Réseaux de Neurones (MLP) | 6 | 19 | 2 | 12h | Bridge |
| 6 | CNN, RNN, Transformers & Fine-tuning | 6+ | 18+ | 2 | 12h | Bridge |
| 7 | Embeddings & Vecteurs | 5 | 17 | 2 | 10h | Track 2 |
| 8 | RAG | 7 | 22 | 2 | 14h | Track 2 |
| 9 | Agents & Automatisation | 6 | 23 | 3 + mini-projet | 14h | Track 2 |
| 10 | Stratégie IA | 5 | 12 | 2 | 6h | Track 2 |
| 11 | Projet Intégrateur | 5 | 14 | PROJECT | 14h | Final |
| **Total** | | **61+ leçons** | **201+ sous-parties** | **24+ activités** | **122h** | |

---

## Learning Blocks (per Instructional Designer)

```
Track 1 (Ch 1-4)  → Mini-projet: "Mon Modèle ML" (uses Module 2 data)
Bridge (Ch 5-6)   → Conceptual understanding (no project)
Track 2 (Ch 7-9)  → Mini-projet: "Mon App LLM" (fresh corpus)
Strategy (Ch 10)  → Case studies + cost analysis
Ch 11             → FINAL PROJECT: Integration ML + LLM
```

---

## Next Action

Module 3 structured. Ready for Content Creator to write lesson content.

Remaining modules to structure:
- Module 4: Power BI
- Final Capstone

---

## Session History

- **2026-01-14:** Module 2 structured — 11 chapters, 65 lessons, 15+ activities
- **2026-01-26:** Module 1 noted as completed by teacher
- **2026-01-26:** Module 3 structured — 11 chapters, 60 lessons, 199 sub-parts, 24+ activities, 120h total. Two-Track PBL structure with detailed sub-parts for Content Creator.
- **2026-02-01:** Chapter 5 rewritten by teacher and refined:
  - New narrative structure: Problème → Brique → Structure → Vie → Défis → Pratique
  - Added Lesson 5.1 "De l'Ingénierie à l'Apprentissage" (the WHY)
  - Extended from 10h to 12h (now 6 lessons)
  - CNN/RNN moved to Chapter 6 (to be integrated with Transformers)
  - Bridge section now 24h total (was 22h)
  - Module 3 total now 122h (was 120h)
  - Detailed content saved in `chap5.md`
