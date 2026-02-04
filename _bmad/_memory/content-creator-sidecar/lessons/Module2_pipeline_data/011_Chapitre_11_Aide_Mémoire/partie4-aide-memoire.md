# Chapitre 11 : Conclusion et aide-mémoire

**Durée estimée : 2-3h**

---

## Objectifs de ce chapitre

1. **Synthétiser** les compétences acquises tout au long du module
2. **Identifier** les erreurs fréquentes à éviter en situation professionnelle
3. **Connecter** vos apprentissages avec la suite du parcours Data & IA

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

> ⚠️ **ATTENTION DATA LEAKAGE** : Les techniques d'imputation (fillna avec median/mean) sont **INTERDITES en Module 2**. Elles seront enseignées correctement dans le **Module 3 (ML)** avec `SimpleImputer` et fit/transform séparés.

```python
# === VALEURS MANQUANTES (Module 2 - techniques autorisées) ===
df.dropna()                                    # Supprimer lignes avec NaN
df.dropna(subset=['col1', 'col2'])             # Sur colonnes spécifiques
df['col'].fillna("Inconnu")                    # Valeur constante explicite (OK)
df['col_missing'] = df['col'].isna()           # Flag pour traçabilité (OK)

# ❌ INTERDIT en Module 2 (cause data leakage si données utilisées en ML)
# df['col'].fillna(df['col'].median())         # ← NE PAS FAIRE
# df['col'].fillna(df['col'].mean())           # ← NE PAS FAIRE
# df['col'].interpolate()                      # ← NE PAS FAIRE

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
