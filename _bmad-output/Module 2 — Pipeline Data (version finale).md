# **Module 2 — Pipeline Data : de la collecte à l'analyse en environnement cloud**

**Durée : 3 semaines (environ 85-100 heures)**

**Public : profils en reconversion avec bases Python, Excel et SQL (niveau intermédiaire incluant JOIN/GROUP BY)**

**Environnement : Python dans VS Code avec notebooks `.ipynb`**

---

## **Objectifs globaux**

* Comprendre le cycle de vie complet de la donnée (ETL/ELT)
* Extraire des données depuis diverses sources (fichiers, SQL, APIs, cloud)
* Maîtriser l'accès aux données dans un environnement cloud (AWS S3)
* Diagnostiquer et améliorer la qualité des données
* Nettoyer, structurer et transformer les données pour l'analyse et le ML
* Réaliser des analyses exploratoires pertinentes
* Charger les données transformées vers le cloud
* Utiliser l'IA comme assistant tout au long de la pipeline

---

## **Chapitre 1 : Pipeline data — vue d'ensemble**

**Objectif :** Comprendre le cycle de vie de la donnée et situer ce module dans la chaîne Data & IA.

**Durée estimée : 4-5h**

**Sous-parties :**

1. **Position du module dans le parcours Data & IA**
   - Rappel : Module 1 (Prompt Engineering) → Module 2 (Data) → Module 3 (ML) → Module 4 (Visualisation)
   - Pourquoi la qualité des données conditionne tout le reste

2. **ETL vs ELT : deux approches**
   - ETL : Extract → Transform → Load (approche classique, data warehouse)
   - ELT : Extract → Load → Transform (approche moderne, data lake, cloud)
   - Quand choisir l'une ou l'autre

3. **Les étapes de la pipeline**
   - Extraction → EDA diagnostique → Nettoyage → Structuration → EDA analytique → Visualisation → Chargement
   - Notion d'itération et retours en arrière

4. **Méthodologie CRISP-DM**
   - Lien avec les standards professionnels
   - Importance de la reproductibilité et traçabilité

5. **Sources de données en entreprise**
   - Vue d'ensemble : fichiers locaux, bases de données, APIs, cloud, web

---

## **Chapitre 2 : Extraction des données (sources locales)**

**Objectif :** Savoir extraire des données depuis diverses sources locales et distantes (hors cloud).

**Durée estimée : 10-12h**

**Sous-parties :**

1. **Extraction depuis fichiers avec pandas**
   - CSV : options de lecture, encodages, séparateurs
   - Excel : lecture multi-feuilles, gestion des headers
   - JSON : structures imbriquées, normalisation
   - Lecture de fichiers volumineux (chunking)

2. **Extraction depuis bases de données SQL**
   - Connexion Python à PostgreSQL/MySQL (sqlalchemy)
   - Requêtes SELECT avec filtres, JOIN, GROUP BY
   - Bonnes pratiques : ne pas surcharger la base de production
   - Pandas `read_sql()` pour récupérer directement un DataFrame

3. **Extraction via API REST**
   - Principes des APIs REST (endpoints, méthodes HTTP, JSON)
   - Librairie `requests` : GET, headers, paramètres
   - Authentification basique (API keys)
   - Pagination et rate limiting
   - Cas pratique : extraction depuis une API publique
   - 🤖 *IA : utiliser un LLM pour comprendre une documentation d'API*

4. **Introduction au web scraping (survol ~2h)**
   - Quand scraper ? Limites légales et éthiques (robots.txt, CGU)
   - BeautifulSoup : principes de base (HTML parsing)
   - Démonstration sur un cas simple
   - Alternatives recommandées : APIs officielles, datasets publics

5. **Inspection initiale des données**
   - Premiers réflexes : shape, dtypes, head, info
   - Compréhension du contexte métier
   - Documentation de la provenance (data lineage basique)

**Hors scope :** Scraping avancé (Selenium, Scrapy), streaming temps réel, OAuth2 complexe, GraphQL

---

## **Chapitre 3 : Le Cloud pour la data**

**Objectif :** Comprendre les fondamentaux du cloud et savoir lire/écrire des données depuis AWS S3.

**Durée estimée : 8-10h**

**Sous-parties :**

1. **Introduction au cloud computing**
   - Pourquoi le cloud ? Avantages vs. environnement local
   - Les trois grands : AWS, GCP, Azure (panorama)
   - Concepts clés : régions, scalabilité, pay-as-you-go

2. **Types de stockage cloud**
   - Stockage objet (S3) vs. fichier vs. bases de données managées
   - Quand utiliser quoi ? Critères de choix
   - Formats de données courants (CSV, Parquet, JSON)

3. **AWS S3 : concepts**
   - Buckets et objets
   - Structure des chemins S3 (`s3://bucket/prefix/file.csv`)
   - Permissions et accès (IAM basics)

4. **Hands-on : configuration AWS**
   - Création d'un compte AWS (free tier)
   - Configuration des credentials (access key, secret key)
   - Sécurité : ne jamais committer ses credentials

5. **Hands-on : lire depuis S3 avec Python**
   - Introduction à boto3
   - Lire un fichier CSV depuis S3
   - Intégration pandas + S3 (`pd.read_csv("s3://...")`)

6. **Panorama GCP et Azure (conceptuel)**
   - Équivalences : S3 ↔ Cloud Storage ↔ Blob Storage
   - Équivalences : RDS ↔ Cloud SQL ↔ Azure SQL
   - Critères de choix d'un cloud en entreprise

**Hors scope :** Infrastructure as code, multi-cloud, Lambda/serverless, optimisation des coûts

---

## **Chapitre 4 : EDA diagnostique et qualité des données**

**Objectif :** Évaluer la qualité des données et identifier les problèmes à corriger avant analyse.

**Durée estimée : 8-10h**

**Sous-parties :**

1. **Objectif de l'EDA diagnostique**
   - Trouver les problèmes, pas encore comprendre les patterns
   - Différence avec l'EDA analytique (Chapitre 7)

2. **Dimensions de la qualité des données**
   - Complétude : valeurs manquantes
   - Unicité : doublons
   - Cohérence : formats, types
   - Exactitude : valeurs aberrantes, erreurs
   - Fraîcheur : données obsolètes

3. **Statistiques descriptives de diagnostic**
   - `shape`, `dtypes`, `describe()`, `info()`
   - `value_counts()`, `nunique()`
   - 🤖 *IA : générer du code d'exploration automatiquement*

4. **Détection des valeurs manquantes**
   - Patterns de missing : aléatoire vs. systématique
   - Visualisation avec `missingno`
   - Quantification : pourcentage par colonne

5. **Détection des doublons**
   - Doublons exacts vs. quasi-doublons
   - `duplicated()`, identification des clés

6. **Détection des outliers**
   - Méthode IQR (interquartile range)
   - Z-score
   - Visualisation : boxplots, histogrammes

7. **Visualisations de diagnostic**
   - Histogrammes pour distributions
   - Boxplots pour outliers
   - Heatmaps pour valeurs manquantes

8. **Documentation des problèmes**
   - Checklist qualité
   - Priorisation : que corriger en premier ?

---

## **Chapitre 5 : Nettoyage des données**

**Objectif :** Corriger les problèmes identifiés pour obtenir des données exploitables.

**Durée estimée : 10-12h**

**Sous-parties :**

1. **Gestion des valeurs manquantes**
   - Suppression : quand c'est acceptable
   - Imputation : moyenne, médiane, mode
   - Imputation contextuelle (par groupe)
   - Flags : créer une colonne indicatrice
   - 🤖 *IA : suggérer une stratégie adaptée au contexte*

2. **Traitement des doublons**
   - Identification avec `duplicated()`
   - Suppression avec `drop_duplicates()`
   - Gestion des quasi-doublons (survol)

3. **Traitement des valeurs aberrantes**
   - Suppression vs. winsorisation vs. conservation
   - Justification métier des choix
   - Quand un outlier est une vraie information

4. **Nettoyage des types de données**
   - Conversion dates : `pd.to_datetime()`, formats, fuseaux horaires
   - Conversion numériques : gestion des formats (virgules, espaces)
   - Standardisation des catégories

5. **Nettoyage de texte**
   - Normalisation : `strip()`, `lower()`, `replace()`
   - Expressions régulières basiques pour extraction/nettoyage
   - Gestion des caractères spéciaux et encodages

6. **Nettoyage avec SQL**
   - Quand nettoyer côté base vs. côté Python
   - Fonctions SQL de nettoyage (TRIM, UPPER, COALESCE)

7. **Traçabilité et documentation**
   - Justifier chaque choix de nettoyage
   - Garder une trace des transformations

**Hors scope :** NLP avancé, record linkage complexe, fuzzy matching avancé

---

## **Chapitre 6 : Structuration et transformation**

**Objectif :** Mettre en forme les données pour l'analyse, le ML et la visualisation.

**Durée estimée : 8-10h**

**Sous-parties :**

1. **Restructuration des DataFrames**
   - Pivot : lignes vers colonnes (`pivot_table()`)
   - Melt : colonnes vers lignes (`melt()`)
   - Wide vs. long : quand utiliser quoi

2. **Combinaison de données**
   - Merge : inner, left, right, outer joins
   - Concat : empiler des datasets verticalement/horizontalement
   - Gestion des clés et conflits de colonnes
   - 🤖 *IA : générer du code de merge complexe*

3. **Agrégations**
   - GroupBy : split-apply-combine
   - Agrégations multiples : `agg()`
   - Fonctions custom d'agrégation

4. **Feature engineering basique**
   - Variables dérivées (calculs, combinaisons)
   - Binning / discrétisation
   - Encoding catégoriel : one-hot, label encoding
   - Variables temporelles : jour, mois, année, jour de semaine

5. **Création du dataset final**
   - Workflow complet : données brutes → dataset propre
   - Validation du schema
   - Export pour analyse et ML

**Hors scope :** Feature stores, pipelines sklearn, automated feature engineering

---

## **Chapitre 7 : EDA analytique**

**Objectif :** Comprendre les données propres, identifier des patterns et formuler des hypothèses.

**Durée estimée : 10-12h**

**Sous-parties :**

1. **Différence EDA diagnostique vs. analytique**
   - Diagnostique = trouver les problèmes (Chapitre 4)
   - Analytique = comprendre les phénomènes

2. **Analyse univariée**
   - Distribution de chaque variable
   - Tendance centrale : moyenne, médiane, mode
   - Dispersion : écart-type, variance, range
   - Forme : skewness, kurtosis, multimodalité

3. **Analyse bivariée**
   - Variables numériques : corrélation, scatter plots
   - Variables catégorielles : tables de contingence
   - Variables mixtes : boxplots par catégorie
   - 🤖 *IA : interpréter des corrélations, suggérer des analyses*

4. **Segmentation et analyse par groupes**
   - GroupBy pour comparer des sous-populations
   - Identification de patterns par segment
   - Questions métier : qui, quoi, quand, où, pourquoi

5. **Formulation d'hypothèses**
   - Passer de l'observation à l'hypothèse
   - Variables candidates pour le ML
   - Questions à approfondir

6. **Pensée critique**
   - Corrélation ≠ causalité
   - Biais de sélection, survivorship bias
   - Ce que les données ne disent pas

**Hors scope :** Tests statistiques formels, analyse multivariée avancée (PCA)

---

## **Chapitre 8 : Visualisation exploratoire**

**Objectif :** Créer des visualisations pour explorer et communiquer les insights.

**Durée estimée : 8-10h**

**Sous-parties :**

1. **Principes de visualisation**
   - Choisir le bon graphique selon les données et la question
   - Visualisation pour explorer vs. pour décider

2. **Matplotlib : fondamentaux**
   - Graphiques de base : line, bar, scatter, hist
   - Personnalisation : titres, labels, couleurs
   - Subplots et figures composées
   - 🤖 *IA : générer du code matplotlib*

3. **Seaborn : visualisations statistiques**
   - Distributions : histplot, kdeplot, boxplot, violinplot
   - Relations : scatterplot, regplot, heatmap
   - Catégorielles : countplot, barplot
   - Pairplot pour exploration multivariée

4. **Plotly : interactivité (survol ~1-2h)**
   - Avantages de l'interactivité
   - Graphiques interactifs de base
   - Quand utiliser plotly vs. matplotlib/seaborn

5. **Bonnes pratiques**
   - Lisibilité : taille des polices, espacement
   - Accessibilité : couleurs, contrastes
   - Éviter les graphiques trompeurs
   - Storytelling visuel

6. **Préparation pour Power BI**
   - Format des données attendu
   - Lien avec Module 4

**Hors scope :** Dashboards (Dash/Streamlit), cartographie, animations

---

## **Chapitre 9 : Chargement vers le cloud**

**Objectif :** Savoir écrire les données transformées vers S3 pour les rendre disponibles.

**Durée estimée : 4-5h**

**Sous-parties :**

1. **Écrire vers S3 avec Python**
   - `boto3` : upload de fichiers
   - Pandas vers S3 : `df.to_csv("s3://...")`
   - Formats recommandés : CSV pour compatibilité, Parquet pour performance

2. **Organisation d'un data lake basique**
   - Structure de dossiers : `raw/`, `processed/`, `final/`
   - Conventions de nommage : dates, versions
   - Ne jamais écraser les données brutes

3. **Documentation du dataset**
   - Métadonnées : colonnes, types, signification
   - Data dictionary
   - 🤖 *IA : générer automatiquement une documentation*

4. **Vérification post-upload**
   - Relire ce qu'on a écrit pour valider
   - Contrôles de cohérence

5. **Bonnes pratiques professionnelles**
   - Versioning des données (mention DVC, Delta Lake — sans approfondir)
   - Traçabilité des transformations

---

## **Chapitre 10 : Cas pratique fil rouge**

**Objectif :** Appliquer l'ensemble de la pipeline sur un dataset réel.

**Durée estimée : 12-15h**

**Sous-parties :**

1. **Contexte et objectifs métier**
   - Présentation du cas et des questions business
   - Définition des livrables attendus

2. **Extraction multi-sources**
   - Données locales (CSV/Excel)
   - Données depuis S3
   - Données API (optionnel selon le cas)

3. **EDA diagnostique**
   - Application du référentiel qualité
   - Documentation des problèmes identifiés

4. **Nettoyage**
   - Stratégies choisies et justifiées
   - Traçabilité des transformations

5. **Structuration et transformation**
   - Jointures, agrégations, feature engineering
   - Dataset final propre

6. **EDA analytique**
   - Exploration des relations
   - Hypothèses formulées

7. **Visualisations exploratoires**
   - Graphiques clés pour répondre aux questions métier

8. **Chargement vers S3**
   - Upload du dataset final
   - Documentation

9. **Présentation**
   - Restitution des insights
   - Justification des choix
   - Limites et recommandations
   - 🤖 *IA : utilisation libre comme assistant tout au long*

---

## **Chapitre 11 : Conclusion et aide-mémoire**

**Objectif :** Synthétiser les apprentissages et fournir une référence rapide.

**Durée estimée : 2-3h**

**Sous-parties :**

1. **Synthèse des compétences acquises**
   - Ce que vous savez faire maintenant
   - Checklist des compétences validées

2. **Erreurs fréquentes à éviter**
   - Top 10 des pièges en data preparation
   - Comment les éviter

3. **Lien avec les modules suivants**
   - Module 3 (ML) : les données préparées alimentent les modèles
   - Module 4 (Power BI) : de l'exploration à la décision

---

### **📋 AIDE-MÉMOIRE — Data Pipeline**

#### **Extraction**
```python
# CSV
df = pd.read_csv("file.csv", encoding="utf-8", sep=";")

# Excel
df = pd.read_excel("file.xlsx", sheet_name="Sheet1")

# JSON
df = pd.read_json("file.json")

# SQL
from sqlalchemy import create_engine
engine = create_engine("postgresql://user:pass@host:5432/db")
df = pd.read_sql("SELECT * FROM table", engine)

# API
import requests
response = requests.get(url, headers={"Authorization": "Bearer TOKEN"})
data = response.json()

# S3
df = pd.read_csv("s3://bucket/path/file.csv")
```

#### **Diagnostic qualité**
```python
df.shape                    # Dimensions
df.dtypes                   # Types
df.info()                   # Résumé
df.describe()               # Stats numériques
df.isnull().sum()           # Valeurs manquantes
df.duplicated().sum()       # Doublons
df["col"].value_counts()    # Distribution catégorielle
```

#### **Nettoyage**
```python
# Valeurs manquantes
df.dropna()                              # Supprimer
df.fillna(value)                         # Imputer
df["col"].fillna(df["col"].median())     # Imputer médiane

# Doublons
df.drop_duplicates()

# Types
df["date"] = pd.to_datetime(df["date"])
df["num"] = pd.to_numeric(df["num"], errors="coerce")

# Texte
df["col"] = df["col"].str.strip().str.lower()
```

#### **Transformation**
```python
# Pivot
df.pivot_table(index="A", columns="B", values="C", aggfunc="sum")

# Melt
pd.melt(df, id_vars=["id"], value_vars=["col1", "col2"])

# Merge
pd.merge(df1, df2, on="key", how="left")

# Concat
pd.concat([df1, df2], axis=0)

# GroupBy
df.groupby("col").agg({"val": ["mean", "sum", "count"]})

# Feature engineering
df["year"] = df["date"].dt.year
df["bin"] = pd.cut(df["age"], bins=[0, 18, 65, 100], labels=["young", "adult", "senior"])
pd.get_dummies(df, columns=["category"])
```

#### **EDA analytique**
```python
# Corrélation
df.corr()
df[["col1", "col2"]].corr()

# Distribution
df["col"].hist()
df["col"].describe()

# Par groupe
df.groupby("segment")["value"].mean()
```

#### **Visualisation**
```python
import matplotlib.pyplot as plt
import seaborn as sns

# Histogram
df["col"].hist(bins=30)

# Boxplot
sns.boxplot(x="category", y="value", data=df)

# Scatter
sns.scatterplot(x="col1", y="col2", data=df)

# Heatmap corrélation
sns.heatmap(df.corr(), annot=True, cmap="coolwarm")

# Pairplot
sns.pairplot(df)
```

#### **Cloud S3**
```python
import boto3

# Lire
df = pd.read_csv("s3://bucket/path/file.csv")

# Écrire
df.to_csv("s3://bucket/path/output.csv", index=False)

# Avec boto3 explicite
s3 = boto3.client("s3")
s3.upload_file("local.csv", "bucket", "path/file.csv")
s3.download_file("bucket", "path/file.csv", "local.csv")
```

#### **Checklist qualité**
- [ ] Dimensions vérifiées (lignes, colonnes)
- [ ] Types de données corrects
- [ ] Valeurs manquantes traitées
- [ ] Doublons supprimés
- [ ] Outliers analysés
- [ ] Formats standardisés
- [ ] Dataset documenté

---

## **Récapitulatif des durées**

| # | Chapitre | Durée |
|---|----------|-------|
| 1 | Pipeline data : vue d'ensemble | 4-5h |
| 2 | Extraction (local, SQL, API, scraping) | 10-12h |
| 3 | Cloud pour la data | 8-10h |
| 4 | EDA diagnostique et qualité | 8-10h |
| 5 | Nettoyage | 10-12h |
| 6 | Structuration et transformation | 8-10h |
| 7 | EDA analytique | 10-12h |
| 8 | Visualisation exploratoire | 8-10h |
| 9 | Chargement vers le cloud | 4-5h |
| 10 | Cas pratique fil rouge | 12-15h |
| 11 | Conclusion et aide-mémoire | 2-3h |
| **TOTAL** | | **~85-105h** |

---

## **Prérequis**

- Python : variables, types, conditions, boucles, fonctions, listes, dictionnaires
- SQL : SELECT, WHERE, JOIN, GROUP BY, ORDER BY
- Excel : manipulation basique de tableaux
- Module 1 complété : Prompt Engineering

---

## **Livrables apprenants**

À la fin du module, chaque apprenant aura :

1. ✅ Un notebook `.ipynb` documenté avec le fil rouge complet
2. ✅ Un dataset nettoyé et structuré uploadé sur S3
3. ✅ Un rapport d'EDA avec visualisations clés
4. ✅ Une documentation du dataset (data dictionary)
5. ✅ L'aide-mémoire comme référence pour la suite

---

## **Éléments hors scope**

- Infrastructure as code (Terraform, CloudFormation)
- Streaming temps réel (Kafka, Kinesis)
- Scraping avancé (Selenium, Scrapy)
- NLP et traitement de texte avancé
- Tests statistiques formels
- Analyse multivariée avancée (PCA, clustering)
- Dashboards interactifs (Dash, Streamlit)
- Feature stores et MLOps
- OAuth2 et authentification complexe

---
