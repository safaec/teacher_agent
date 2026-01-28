# Chapitre 11 : Conclusion et aide-mémoire

**Durée estimée : 2-3h**

---

## Objectifs de ce chapitre

1. **Synthétiser** les compétences acquises tout au long du module
2. **Identifier** les erreurs fréquentes à éviter en situation professionnelle
3. **Connecter** vos apprentissages avec la suite du parcours Data & IA

---

## 🎯 Félicitations !

Vous avez terminé le **Module 2 : Pipeline Data**.

En 85-100 heures de travail, vous êtes passé(e) de "Je sais manipuler des données dans Excel" à "Je sais construire une pipeline de données complète, du fichier brut au cloud".

**C'est une transformation significative.** Prenez un moment pour reconnaître ce que vous avez accompli.

---

## 1. Synthèse des compétences acquises

### 1.1 Ce que vous savez faire maintenant

| Chapitre | Compétence clé | Vous savez... |
|----------|----------------|---------------|
| **1** | Vision pipeline | Expliquer ETL vs ELT, situer chaque étape dans CRISP-DM |
| **2** | Extraction | Lire CSV, Excel, JSON, SQL, APIs avec pandas et requests |
| **3** | Cloud | Configurer AWS, lire/écrire sur S3 avec boto3 |
| **4** | Diagnostic | Évaluer la qualité des données sur 5 dimensions |
| **5** | Nettoyage | Traiter missing, doublons, outliers, types, texte |
| **6** | Transformation | Joindre, pivoter, agréger, créer des features |
| **7** | EDA analytique | Analyser distributions, corrélations, formuler hypothèses |
| **8** | Visualisation | Créer des graphiques avec matplotlib, seaborn, plotly |
| **9** | Chargement | Uploader vers S3, organiser un data lake, documenter |
| **10** | Projet complet | Appliquer toute la pipeline sur un cas réel |

---

### 1.2 Checklist des compétences validées

Cochez les compétences que vous maîtrisez avec confiance :

#### Extraction
- [ ] Je sais lire un CSV avec les bons paramètres (encoding, separator)
- [ ] Je sais lire un fichier Excel multi-feuilles
- [ ] Je sais parser du JSON et le convertir en DataFrame
- [ ] Je sais me connecter à une base SQL et exécuter des requêtes
- [ ] Je sais appeler une API REST et gérer la pagination
- [ ] Je sais lire depuis S3 avec pandas

#### Diagnostic & Nettoyage
- [ ] Je sais calculer le taux de valeurs manquantes
- [ ] Je sais détecter et supprimer les doublons
- [ ] Je sais identifier les outliers (IQR, boxplot)
- [ ] Je sais choisir une stratégie d'imputation adaptée
- [ ] Je sais standardiser des colonnes catégorielles
- [ ] Je sais convertir les types de données (dates, numériques)

#### Transformation
- [ ] Je sais faire des jointures (merge) entre DataFrames
- [ ] Je sais utiliser groupby pour agréger des données
- [ ] Je sais pivoter et "melter" un DataFrame
- [ ] Je sais créer des features dérivées (temporelles, catégorielles)

#### Analyse & Visualisation
- [ ] Je sais calculer des statistiques descriptives
- [ ] Je sais interpréter une corrélation
- [ ] Je sais créer des histogrammes, boxplots, scatter plots
- [ ] Je sais choisir le bon type de graphique selon la question

#### Cloud & Documentation
- [ ] Je sais écrire un DataFrame vers S3
- [ ] Je sais choisir entre CSV et Parquet
- [ ] Je sais organiser un data lake (raw/processed/curated)
- [ ] Je sais créer un data dictionary

---

### ✍️ Exercice 1 : Auto-évaluation honnête

Pour chaque catégorie, notez votre niveau de confiance :

| Catégorie | 1-Débutant | 2-En cours | 3-Confiant | 4-Expert |
|-----------|------------|------------|------------|----------|
| Extraction | ⬜ | ⬜ | ⬜ | ⬜ |
| Nettoyage | ⬜ | ⬜ | ⬜ | ⬜ |
| Transformation | ⬜ | ⬜ | ⬜ | ⬜ |
| Visualisation | ⬜ | ⬜ | ⬜ | ⬜ |
| Cloud | ⬜ | ⬜ | ⬜ | ⬜ |

**Action** : Pour chaque catégorie où vous avez noté 1 ou 2, planifiez de refaire les exercices du chapitre correspondant.

---

## 2. Erreurs fréquentes à éviter

### 2.1 Le Top 10 des pièges en Data Preparation

Ces erreurs sont commises par des professionnels expérimentés. Maintenant que vous les connaissez, vous pouvez les éviter.

---

#### ❌ Erreur #1 : Ne pas explorer avant de nettoyer

**Le piège** : Se précipiter sur le nettoyage sans comprendre les données.

**Conséquence** : Vous supprimez des lignes "aberrantes" qui étaient en fait des cas métier valides.

**Solution** : Toujours faire un EDA diagnostique AVANT de modifier quoi que ce soit.

```python
# ❌ Mauvais
df = df[df['montant'] < 10000]  # "Les gros montants sont des erreurs"

# ✅ Bon
print(df['montant'].describe())
print(df[df['montant'] > 10000])  # Examiner avant de décider
# Puis discuter avec le métier
```

---

#### ❌ Erreur #2 : Écraser les données brutes

**Le piège** : Modifier directement le fichier source.

**Conséquence** : Impossible de revenir en arrière si vous découvrez une erreur.

**Solution** : Toujours travailler sur une copie, conserver les originaux dans `/raw/`.

```python
# ❌ Mauvais
df.to_csv('donnees.csv')  # Écrase l'original

# ✅ Bon
df_clean.to_csv('donnees_clean_20240120.csv')  # Nouveau fichier
```

---

#### ❌ Erreur #3 : Ignorer les valeurs manquantes

**Le piège** : Laisser les NaN sans les traiter explicitement.

**Conséquence** : Calculs faux, modèles qui plantent, biais dans l'analyse.

**Solution** : Toujours documenter votre stratégie de traitement.

```python
# ❌ Mauvais
df['prix'].mean()  # Ignore silencieusement les NaN

# ✅ Bon
print(f"Missing: {df['prix'].isna().sum()}")
df['prix'] = df['prix'].fillna(df['prix'].median())  # Choix explicite
```

---

#### ❌ Erreur #4 : Confondre corrélation et causalité

**Le piège** : "Les ventes de glaces augmentent avec les noyades → les glaces causent des noyades !"

**Conséquence** : Recommandations business absurdes, décisions erronées.

**Solution** : Toujours chercher les variables confondantes (ici : la température).

```python
# ❌ Mauvais
print("Corrélation = 0.85, donc A cause B")

# ✅ Bon
print("Corrélation = 0.85, hypothèse à explorer")
# Vérifier les variables tierces, le contexte métier
```

---

#### ❌ Erreur #5 : Utiliser le mauvais type de jointure

**Le piège** : Toujours utiliser `inner join` par défaut.

**Conséquence** : Perte silencieuse de données importantes.

**Solution** : Comprendre les 4 types de join et vérifier le résultat.

```python
# ❌ Mauvais
df_merged = pd.merge(df1, df2, on='id')  # Inner par défaut - perd des lignes !

# ✅ Bon
print(f"df1: {len(df1)}, df2: {len(df2)}")
df_merged = pd.merge(df1, df2, on='id', how='left')
print(f"Après merge: {len(df_merged)}")  # Vérifier !
```

---

#### ❌ Erreur #6 : Ne pas vérifier les types après lecture

**Le piège** : Supposer que pandas devine correctement les types.

**Conséquence** : Dates traitées comme strings, nombres comme objets.

**Solution** : Toujours inspecter `dtypes` et convertir explicitement.

```python
# ❌ Mauvais
df = pd.read_csv('data.csv')
df['date'].mean()  # Erreur ! C'est une string

# ✅ Bon
df = pd.read_csv('data.csv')
print(df.dtypes)
df['date'] = pd.to_datetime(df['date'])
```

---

#### ❌ Erreur #7 : Créer des graphiques sans contexte

**Le piège** : Un graphique sans titre, sans unités, sans source.

**Conséquence** : Le graphique est inutilisable par quelqu'un d'autre.

**Solution** : Toujours ajouter titre, labels, et source.

```python
# ❌ Mauvais
df['ventes'].plot()

# ✅ Bon
import matplotlib.pyplot as plt
df['ventes'].plot()
plt.title('Évolution des ventes mensuelles (2024)')
plt.xlabel('Mois')
plt.ylabel('Ventes (€)')
plt.figtext(0.99, 0.01, 'Source: CRM TechShop', ha='right', fontsize=8)
```

---

#### ❌ Erreur #8 : Ne pas documenter les transformations

**Le piège** : "Je me souviendrai de ce que j'ai fait."

**Conséquence** : Dans 3 mois, personne (y compris vous) ne comprend le code.

**Solution** : Commenter les choix, créer un data dictionary.

```python
# ❌ Mauvais
df['x'] = df['a'] * 0.85 + df['b']

# ✅ Bon
# Calcul du score pondéré : 85% facteur A + facteur B
# Justification : Recommandation équipe analytics (cf. email 2024-01-15)
df['score_pondere'] = df['facteur_a'] * 0.85 + df['facteur_b']
```

---

#### ❌ Erreur #9 : Uploader sans valider

**Le piège** : "L'upload a réussi, c'est bon."

**Conséquence** : Fichier corrompu, colonnes manquantes, types modifiés.

**Solution** : Toujours relire et comparer après upload.

```python
# ❌ Mauvais
df.to_parquet('s3://bucket/data.parquet')
print("Done!")

# ✅ Bon
df.to_parquet('s3://bucket/data.parquet')
df_check = pd.read_parquet('s3://bucket/data.parquet')
assert df_check.shape == df.shape, "Erreur de dimensions !"
print("Validation OK")
```

---

#### ❌ Erreur #10 : Optimiser prématurément

**Le piège** : "Je vais utiliser Spark pour mes 10,000 lignes."

**Conséquence** : Complexité inutile, temps perdu, bugs supplémentaires.

**Solution** : Commencer simple (pandas), scaler si nécessaire.

```python
# ❌ Mauvais (pour 10K lignes)
from pyspark.sql import SparkSession
spark = SparkSession.builder.getOrCreate()
# ... 50 lignes de code Spark

# ✅ Bon
import pandas as pd
df = pd.read_csv('data.csv')  # 2 secondes, ça marche
```

---

### ✍️ Exercice 2 : Identifiez vos pièges personnels

Parmi les 10 erreurs ci-dessus, lesquelles avez-vous déjà commises ou êtes-vous le plus susceptible de commettre ?

1. Mon erreur la plus probable : _______________________
2. Comment je vais l'éviter : _______________________

---

## 3. Lien avec les modules suivants

### 3.1 Module 3 : Machine Learning

Votre dataset propre est le **carburant** des modèles de ML.

```
Module 2 (Data)          →    Module 3 (ML)
─────────────────────────────────────────────
Dataset nettoyé          →    Données d'entraînement
Features engineered      →    Variables prédictives
EDA analytique           →    Sélection de features
Qualité vérifiée         →    Modèle fiable
```

**Ce que vous ferez dans le Module 3** :
- Prédire le churn client (classification)
- Estimer les ventes futures (régression)
- Segmenter les clients automatiquement (clustering)

**Pourquoi la qualité des données est critique** :

> "Garbage in, garbage out."

Un modèle entraîné sur des données mal nettoyées donnera des prédictions fausses. Tout ce que vous avez appris dans ce module est **fondamental** pour le ML.

---

### 3.2 Module 4 : Power BI

Vos visualisations exploratoires deviennent des **dashboards interactifs**.

```
Module 2 (Data)          →    Module 4 (Power BI)
─────────────────────────────────────────────
Matplotlib/Seaborn       →    Visualisations interactives
Dataset final            →    Source de données
EDA analytique           →    KPIs et métriques
Data dictionary          →    Documentation Power BI
```

**Ce que vous ferez dans le Module 4** :
- Créer des dashboards dynamiques
- Permettre aux utilisateurs d'explorer les données
- Automatiser les rapports mensuels

**Le lien direct** : Le fichier `techshop_analytics.parquet` que vous avez créé peut être importé directement dans Power BI.

---

### 3.3 Votre parcours Data & IA

```
┌─────────────────────────────────────────────────────────────────┐
│                    PARCOURS DATA & IA                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  Module 1          Module 2           Module 3       Module 4    │
│  ┌─────────┐      ┌─────────┐       ┌─────────┐    ┌─────────┐  │
│  │ Prompt  │  →   │  DATA   │   →   │   ML    │ →  │Power BI │  │
│  │Engineering│    │PIPELINE │       │         │    │         │  │
│  └─────────┘      └────┬────┘       └─────────┘    └─────────┘  │
│       ✅               ✅                                        │
│                        │                                         │
│                   VOUS ÊTES ICI                                  │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 4. Aide-mémoire complet

Gardez cette référence à portée de main pour vos futurs projets.

---

### 📋 AIDE-MÉMOIRE — Pipeline Data

---

#### 🔹 EXTRACTION

```python
import pandas as pd
import requests
import json

# === CSV ===
df = pd.read_csv("fichier.csv", encoding="utf-8", sep=";")
df = pd.read_csv("fichier.csv", usecols=['col1', 'col2'])  # Colonnes spécifiques
df = pd.read_csv("gros_fichier.csv", chunksize=10000)  # Par morceaux

# === EXCEL ===
df = pd.read_excel("fichier.xlsx", sheet_name="Feuille1")
df = pd.read_excel("fichier.xlsx", sheet_name=None)  # Toutes les feuilles (dict)

# === JSON ===
df = pd.read_json("fichier.json")
with open("fichier.json", 'r') as f:
    data = json.load(f)
df = pd.DataFrame(data)

# === SQL ===
from sqlalchemy import create_engine
engine = create_engine("postgresql://user:pass@host:5432/db")
df = pd.read_sql("SELECT * FROM table WHERE date > '2024-01-01'", engine)

# === API REST ===
response = requests.get(url, headers={"Authorization": "Bearer TOKEN"})
data = response.json()
df = pd.DataFrame(data['results'])

# === S3 ===
df = pd.read_csv("s3://bucket/path/fichier.csv")
df = pd.read_parquet("s3://bucket/path/fichier.parquet")
```

---

#### 🔹 DIAGNOSTIC QUALITÉ

```python
# === INSPECTION DE BASE ===
df.shape                      # (lignes, colonnes)
df.dtypes                     # Types de données
df.info()                     # Résumé complet
df.head(10)                   # Premiers enregistrements
df.describe()                 # Statistiques numériques
df.describe(include='object') # Statistiques catégorielles

# === VALEURS MANQUANTES ===
df.isnull().sum()                    # Comptage par colonne
df.isnull().sum() / len(df) * 100    # Pourcentage
df.isnull().any(axis=1).sum()        # Lignes avec au moins 1 NaN

# === DOUBLONS ===
df.duplicated().sum()                # Nombre de doublons
df[df.duplicated(keep=False)]        # Voir tous les doublons
df.duplicated(subset=['col1']).sum() # Doublons sur colonne spécifique

# === VALEURS UNIQUES ===
df['col'].nunique()                  # Nombre de valeurs uniques
df['col'].value_counts()             # Distribution
df['col'].unique()                   # Liste des valeurs

# === OUTLIERS ===
Q1 = df['col'].quantile(0.25)
Q3 = df['col'].quantile(0.75)
IQR = Q3 - Q1
outliers = df[(df['col'] < Q1 - 1.5*IQR) | (df['col'] > Q3 + 1.5*IQR)]
```

---

#### 🔹 NETTOYAGE

```python
# === VALEURS MANQUANTES ===
df.dropna()                                    # Supprimer lignes avec NaN
df.dropna(subset=['col1', 'col2'])             # Sur colonnes spécifiques
df['col'].fillna(value)                        # Remplir par valeur
df['col'].fillna(df['col'].median())           # Remplir par médiane
df['col'].fillna(df.groupby('cat')['col'].transform('median'))  # Par groupe
df['col'].interpolate()                        # Interpolation

# === DOUBLONS ===
df.drop_duplicates()                           # Supprimer doublons exacts
df.drop_duplicates(subset=['col1'])            # Sur colonne spécifique
df.drop_duplicates(keep='last')                # Garder le dernier

# === TYPES DE DONNÉES ===
df['date'] = pd.to_datetime(df['date'], format='%Y-%m-%d')
df['date'] = pd.to_datetime(df['date'], format='mixed', errors='coerce')
df['num'] = pd.to_numeric(df['num'], errors='coerce')
df['cat'] = df['cat'].astype('category')

# === TEXTE ===
df['col'] = df['col'].str.strip()              # Espaces
df['col'] = df['col'].str.lower()              # Minuscules
df['col'] = df['col'].str.upper()              # Majuscules
df['col'] = df['col'].str.replace('old', 'new')
df['col'] = df['col'].str.extract(r'(\d+)')    # Regex

# === MAPPING / REMPLACEMENT ===
mapping = {'ancien': 'nouveau', 'old': 'new'}
df['col'] = df['col'].map(mapping)
df['col'] = df['col'].replace(mapping)
```

---

#### 🔹 TRANSFORMATION

```python
# === JOINTURES ===
pd.merge(df1, df2, on='key')                   # Inner join (défaut)
pd.merge(df1, df2, on='key', how='left')       # Left join
pd.merge(df1, df2, on='key', how='right')      # Right join
pd.merge(df1, df2, on='key', how='outer')      # Outer join
pd.merge(df1, df2, left_on='a', right_on='b')  # Clés différentes

# === CONCATÉNATION ===
pd.concat([df1, df2], axis=0)                  # Empiler verticalement
pd.concat([df1, df2], axis=1)                  # Côte à côte

# === PIVOT / MELT ===
df.pivot_table(index='row', columns='col', values='val', aggfunc='sum')
pd.melt(df, id_vars=['id'], value_vars=['col1', 'col2'])

# === AGRÉGATION ===
df.groupby('col').agg({'val': ['sum', 'mean', 'count']})
df.groupby(['col1', 'col2']).agg({
    'montant': 'sum',
    'id': 'count'
}).reset_index()

# === FEATURE ENGINEERING ===
# Temporel
df['year'] = df['date'].dt.year
df['month'] = df['date'].dt.month
df['dayofweek'] = df['date'].dt.dayofweek
df['is_weekend'] = df['date'].dt.dayofweek >= 5

# Binning
df['bin'] = pd.cut(df['age'], bins=[0, 18, 65, 100], labels=['jeune', 'adulte', 'senior'])
df['quantile'] = pd.qcut(df['revenu'], q=4, labels=['Q1', 'Q2', 'Q3', 'Q4'])

# Encoding
pd.get_dummies(df, columns=['categorie'])      # One-hot encoding
df['cat_code'] = df['categorie'].astype('category').cat.codes  # Label encoding
```

---

#### 🔹 ANALYSE

```python
# === STATISTIQUES ===
df['col'].mean()                # Moyenne
df['col'].median()              # Médiane
df['col'].std()                 # Écart-type
df['col'].var()                 # Variance
df['col'].mode()                # Mode
df['col'].skew()                # Asymétrie
df['col'].kurtosis()            # Kurtosis

# === CORRÉLATION ===
df.corr()                       # Matrice de corrélation
df[['col1', 'col2']].corr()     # Entre 2 colonnes
df.corr(method='spearman')      # Corrélation de Spearman

# === ANALYSE PAR GROUPE ===
df.groupby('segment')['montant'].describe()
df.groupby('segment').agg({
    'montant': ['mean', 'sum'],
    'id': 'count'
})
```

---

#### 🔹 VISUALISATION

```python
import matplotlib.pyplot as plt
import seaborn as sns

# === MATPLOTLIB ===
fig, ax = plt.subplots(figsize=(10, 6))
df['col'].plot(kind='hist', bins=30, ax=ax)
df.plot(kind='bar', x='cat', y='val', ax=ax)
df.plot(kind='line', x='date', y='val', ax=ax)
df.plot(kind='scatter', x='x', y='y', ax=ax)
plt.title('Titre')
plt.xlabel('Axe X')
plt.ylabel('Axe Y')
plt.savefig('graphique.png', dpi=150)
plt.show()

# === SEABORN ===
sns.histplot(data=df, x='col', bins=30)
sns.boxplot(data=df, x='cat', y='val')
sns.scatterplot(data=df, x='x', y='y', hue='cat')
sns.heatmap(df.corr(), annot=True, cmap='coolwarm')
sns.pairplot(df)
sns.countplot(data=df, x='cat')
sns.barplot(data=df, x='cat', y='val', estimator='mean')
```

---

#### 🔹 CLOUD S3

```python
import boto3
from io import StringIO, BytesIO

# === LECTURE ===
df = pd.read_csv("s3://bucket/path/file.csv")
df = pd.read_parquet("s3://bucket/path/file.parquet")

# === ÉCRITURE DIRECTE ===
df.to_csv("s3://bucket/path/file.csv", index=False)
df.to_parquet("s3://bucket/path/file.parquet", index=False)

# === ÉCRITURE AVEC BOTO3 ===
s3 = boto3.client('s3')

# CSV
csv_buffer = StringIO()
df.to_csv(csv_buffer, index=False)
s3.put_object(Bucket='bucket', Key='path/file.csv', Body=csv_buffer.getvalue())

# Parquet
parquet_buffer = BytesIO()
df.to_parquet(parquet_buffer, index=False)
s3.put_object(Bucket='bucket', Key='path/file.parquet', Body=parquet_buffer.getvalue())

# === STRUCTURE DATA LAKE ===
# s3://bucket/raw/source/YYYY/MM/DD/fichier_original.csv
# s3://bucket/processed/dataset/fichier_clean.parquet
# s3://bucket/curated/analytics/table_finale.parquet
```

---

#### 🔹 CHECKLIST PROJET

```markdown
## Avant de commencer
- [ ] Objectifs business définis
- [ ] Sources de données identifiées
- [ ] Accès aux données confirmé

## Extraction
- [ ] Fichiers chargés sans erreur
- [ ] Types inspectés (dtypes)
- [ ] Premières lignes vérifiées (head)

## Diagnostic
- [ ] Shape documenté
- [ ] Valeurs manquantes quantifiées
- [ ] Doublons détectés
- [ ] Outliers identifiés
- [ ] Incohérences listées

## Nettoyage
- [ ] Stratégie documentée pour chaque problème
- [ ] Transformations tracées
- [ ] Données brutes préservées

## Transformation
- [ ] Jointures validées (vérifier les counts)
- [ ] Features pertinentes créées
- [ ] Types finaux corrects

## Analyse
- [ ] Questions business répondues
- [ ] Visualisations claires
- [ ] Limitations documentées

## Livraison
- [ ] Dataset final uploadé
- [ ] Data dictionary créé
- [ ] Code reproductible
- [ ] Présentation prête
```

---

## 5. Mot de la fin

### Vous êtes prêt(e)

Vous avez maintenant les compétences pour :

1. **Comprendre** le cycle de vie des données en entreprise
2. **Extraire** des données de multiples sources
3. **Diagnostiquer** et **nettoyer** les problèmes de qualité
4. **Transformer** les données brutes en insights
5. **Visualiser** les patterns et communiquer les résultats
6. **Déployer** sur le cloud de manière professionnelle

### Ce qui vous différencie

La plupart des débutants savent ouvrir un fichier CSV.

**Vous**, vous savez :
- Anticiper les problèmes de qualité
- Choisir la bonne stratégie de nettoyage
- Justifier vos décisions avec des données
- Documenter votre travail pour les autres
- Livrer un résultat prêt pour la production

**C'est la différence entre un amateur et un professionnel.**

---

### 🎉 Célébration

Prenez un moment pour :

1. **Relire** votre projet fil rouge du Chapitre 10
2. **Mesurer** le chemin parcouru depuis le début du module
3. **Partager** votre réussite (LinkedIn, collègues, mentors)

---

### Prochaine étape

Rendez-vous au **Module 3 : Machine Learning** pour transformer vos données propres en prédictions actionables.

> *"Les données sont le nouveau pétrole, mais comme le pétrole, elles doivent être raffinées pour avoir de la valeur."*
> — Clive Humby

**Vous savez maintenant raffiner les données. Félicitations !**

---

## 📚 Ressources pour aller plus loin

### Documentation officielle
- [Pandas Documentation](https://pandas.pydata.org/docs/)
- [AWS S3 Developer Guide](https://docs.aws.amazon.com/s3/)
- [Matplotlib Tutorials](https://matplotlib.org/stable/tutorials/)
- [Seaborn Tutorial](https://seaborn.pydata.org/tutorial.html)

### Livres recommandés
- *Python for Data Analysis* — Wes McKinney (créateur de pandas)
- *Data Science from Scratch* — Joel Grus
- *Storytelling with Data* — Cole Nussbaumer Knaflic

### Pratique continue
- Kaggle Datasets : Des datasets réels pour pratiquer
- DataCamp : Exercices interactifs Python/pandas
- LeetCode Database : Exercices SQL
