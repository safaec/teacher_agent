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

En 1999, la NASA a perdu la sonde Mars Climate Orbiter — un investissement de **125 millions de dollars** — à cause d'une erreur de données. Un système utilisait des mesures en **livres-force** tandis qu'un autre attendait des **newtons**. Personne n'a vérifié la cohérence des unités.

La sonde s'est désintégrée dans l'atmosphère martienne.

Ce n'était pas un bug de code. C'était un **problème de qualité des données**.

Avant de nettoyer, transformer ou analyser vos données, vous devez d'abord **diagnostiquer** ce qui ne va pas. C'est l'objet de ce chapitre.

> 💭 **Question Socratique #1** : À votre avis, pourquoi les équipes de la NASA — composées d'ingénieurs brillants — n'ont-elles pas détecté cette incohérence d'unités ? Quel processus aurait pu prévenir ce désastre ?

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

---

### ✍️ Exercice 4.1 : Diagnostic ou Analytique ? (5 min)

Classez chaque action dans la bonne catégorie :

| Action | Diagnostique | Analytique |
|--------|--------------|------------|
| Calculer la corrélation entre prix et ventes | ○ | ○ |
| Compter les valeurs manquantes par colonne | ○ | ○ |
| Identifier les segments de clients | ○ | ○ |
| Vérifier si les dates sont au bon format | ○ | ○ |
| Analyser la distribution des âges | ○ | ○ |
| Chercher les doublons dans les emails | ○ | ○ |

---

## 4.2 Les 6 dimensions de la qualité des données

### Framework standardisé

La qualité des données s'évalue selon **6 dimensions principales** reconnues par l'industrie :

```
┌─────────────────────────────────────────────────────────────────────┐
│                    DIMENSIONS DE QUALITÉ                            │
├─────────────────┬─────────────────┬─────────────────────────────────┤
│  COMPLÉTUDE     │   UNICITÉ       │         COHÉRENCE               │
│  (Completeness) │   (Uniqueness)  │       (Consistency)             │
│                 │                 │                                 │
│  Toutes les     │  Pas de         │  Mêmes valeurs dans             │
│  données sont   │  doublons       │  différents systèmes            │
│  présentes ?    │                 │                                 │
├─────────────────┼─────────────────┼─────────────────────────────────┤
│  EXACTITUDE     │   VALIDITÉ      │         FRAÎCHEUR               │
│  (Accuracy)     │   (Validity)    │       (Timeliness)              │
│                 │                 │                                 │
│  Les données    │  Format et      │  Données à jour,                │
│  reflètent la   │  règles         │  pas obsolètes                  │
│  réalité ?      │  respectés ?    │                                 │
└─────────────────┴─────────────────┴─────────────────────────────────┘
```

*(Source : [IBM - Data Quality Dimensions](https://www.ibm.com/think/topics/data-quality-dimensions))*

---

### 1. Complétude (Completeness)

**Question :** Toutes les données requises sont-elles présentes ?

**Exemples de problèmes :**
- Client sans adresse email
- Commande sans date de livraison
- Produit sans prix

**Métrique :** `% de valeurs non-nulles`

```python
# Calculer la complétude par colonne
completude = (1 - df.isnull().mean()) * 100
print(completude.sort_values())
```

---

### 2. Unicité (Uniqueness)

**Question :** Y a-t-il des enregistrements en double ?

**Exemples de problèmes :**
- Même client enregistré 3 fois
- Même transaction comptée deux fois
- Doublons dus à des imports multiples

**Métrique :** `% d'enregistrements uniques`

```python
# Taux d'unicité
unicite = (1 - df.duplicated().mean()) * 100
print(f"Unicité : {unicite:.2f}%")
```

---

### 3. Cohérence (Consistency)

**Question :** Les données sont-elles logiquement cohérentes ?

**Exemples de problèmes :**
- Date de naissance > date d'inscription
- Ville "Paris" avec code postal "69000" (Lyon)
- Total ≠ somme des lignes

**Métrique :** `% de règles de cohérence respectées`

```python
# Vérifier une règle de cohérence
incoherents = df[df["date_naissance"] > df["date_inscription"]]
print(f"Incohérences : {len(incoherents)} lignes")
```

---

### 4. Exactitude (Accuracy)

**Question :** Les données reflètent-elles la réalité ?

**Exemples de problèmes :**
- Âge de 150 ans (erreur de saisie)
- Température de -100°C à Paris
- Salaire négatif

**Métrique :** `% de valeurs dans les plages attendues`

```python
# Vérifier les valeurs aberrantes
ages_invalides = df[(df["age"] < 0) | (df["age"] > 120)]
print(f"Âges invalides : {len(ages_invalides)}")
```

---

### 5. Validité (Validity)

**Question :** Les données respectent-elles le format attendu ?

**Exemples de problèmes :**
- Email sans "@"
- Code postal avec des lettres
- Date au format "31/13/2024"

**Métrique :** `% de valeurs conformes au format`

```python
# Vérifier le format email (regex simple)
import re
pattern = r'^[\w\.-]+@[\w\.-]+\.\w+$'
emails_invalides = df[~df["email"].str.match(pattern, na=False)]
print(f"Emails invalides : {len(emails_invalides)}")
```

---

### 6. Fraîcheur (Timeliness)

**Question :** Les données sont-elles à jour ?

**Exemples de problèmes :**
- Adresse d'un client déménagé il y a 2 ans
- Prix catalogue de 2019
- Statut de commande non mis à jour

**Métrique :** `Âge moyen des données`

```python
from datetime import datetime

# Calculer l'âge des données
df["anciennete_jours"] = (datetime.now() - pd.to_datetime(df["date_maj"])).dt.days
print(f"Âge moyen : {df['anciennete_jours'].mean():.0f} jours")
```

---

### ✍️ Exercice 4.2 : Identifier les dimensions (10 min)

Pour chaque problème, identifiez la dimension de qualité concernée :

| Problème | Dimension |
|----------|-----------|
| 15% des clients n'ont pas de numéro de téléphone | _____ |
| Le même produit apparaît 3 fois avec des prix différents | _____ |
| Un client a une date de naissance en 2050 | _____ |
| Le code postal contient des caractères spéciaux | _____ |
| L'adresse d'un entrepôt fermé depuis 2 ans est encore active | _____ |
| Le total de la facture ne correspond pas à la somme des lignes | _____ |

---

> 💭 **Question Socratique #2** : Une entreprise peut-elle avoir des données 100% complètes mais de mauvaise qualité ? Donnez un exemple concret.

---

## 4.3 Statistiques descriptives de diagnostic

### Les commandes essentielles

```python
import pandas as pd

# Charger les données
df = pd.read_csv("ventes.csv")

# 1. Dimensions
print(f"📊 Shape : {df.shape[0]} lignes × {df.shape[1]} colonnes")

# 2. Types de données
print("\n📝 Types :")
print(df.dtypes)

# 3. Aperçu
print("\n👀 Premières lignes :")
print(df.head())

# 4. Résumé complet
print("\n📋 Info :")
df.info()

# 5. Statistiques numériques
print("\n📈 Describe :")
print(df.describe())

# 6. Statistiques catégorielles
print("\n🏷️ Catégories :")
for col in df.select_dtypes(include="object").columns:
    print(f"\n{col}: {df[col].nunique()} valeurs uniques")
    print(df[col].value_counts().head())
```

---

### Tableau de référence

| Commande | Information | Utilité diagnostique |
|----------|-------------|---------------------|
| `df.shape` | (lignes, colonnes) | Volume attendu ? |
| `df.dtypes` | Types par colonne | Types corrects ? |
| `df.head()` | 5 premières lignes | Aperçu visuel |
| `df.info()` | Résumé + mémoire | Valeurs non-null |
| `df.describe()` | Stats numériques | Min/max aberrants ? |
| `df.nunique()` | Valeurs uniques | Cardinalité |
| `df.value_counts()` | Distribution | Catégories inattendues ? |

---

### 🤖 IA : Générer du code d'exploration automatiquement

**Prompt efficace pour un LLM :**

```
J'ai un DataFrame pandas avec ces colonnes :
- client_id (int)
- nom (str)
- email (str)
- age (int)
- date_inscription (str)
- montant_achats (float)

Génère-moi un code Python complet pour :
1. Vérifier les types de données
2. Identifier les valeurs manquantes
3. Détecter les doublons potentiels
4. Chercher les outliers numériques
5. Valider les formats (email, dates)

Inclus des commentaires explicatifs.
```

---

### ✍️ Exercice 4.3 : Exploration initiale (15 min)

Analysez ce DataFrame et identifiez les problèmes potentiels :

```python
import pandas as pd
import numpy as np

# Données simulées
df = pd.DataFrame({
    'id': [1, 2, 3, 4, 5, 5],
    'nom': ['Alice', 'Bob', 'Charlie', 'David', 'Eve', 'Eve'],
    'age': [25, 150, 35, -5, 28, 28],
    'email': ['alice@test.com', 'bob', 'charlie@test.com', None, 'eve@test.com', 'eve@test.com'],
    'salaire': [50000, 55000, np.nan, 60000, 45000, 45000],
    'date_inscription': ['2024-01-15', '2024-02-30', '2024-03-10', '2024-04-05', '2024-05-12', '2024-05-12']
})

# Votre exploration
print("1. Shape :", df.shape)
print("\n2. Types :\n", df.dtypes)
print("\n3. Valeurs manquantes :\n", df.isnull().sum())
print("\n4. Doublons :", df.duplicated().sum())
print("\n5. Stats numériques :\n", df.describe())

# Questions :
# a) Combien de problèmes de COMPLÉTUDE identifiez-vous ?
# b) Combien de problèmes d'EXACTITUDE ?
# c) Combien de problèmes de VALIDITÉ ?
# d) Combien de problèmes d'UNICITÉ ?
```

**Réponses :**
- a) Complétude : _____
- b) Exactitude : _____
- c) Validité : _____
- d) Unicité : _____

---

## 4.4 Détection des valeurs manquantes

### Comprendre les patterns de missing

Les valeurs manquantes ne sont pas toutes égales. Il existe trois types :

| Type | Définition | Exemple |
|------|------------|---------|
| **MCAR** (Missing Completely At Random) | Manque aléatoire, aucun pattern | Erreur de saisie ponctuelle |
| **MAR** (Missing At Random) | Manque lié à d'autres variables | Revenus manquants surtout chez les jeunes |
| **MNAR** (Missing Not At Random) | Manque lié à la valeur elle-même | Revenus élevés non déclarés volontairement |

**Pourquoi c'est important ?** Le type de missing influence la stratégie de traitement (Chapitre 5).

---

### Quantifier les valeurs manquantes

```python
# Nombre de valeurs manquantes par colonne
print(df.isnull().sum())

# Pourcentage de valeurs manquantes
print((df.isnull().mean() * 100).round(2))

# Lignes avec au moins une valeur manquante
lignes_incompletes = df[df.isnull().any(axis=1)]
print(f"Lignes incomplètes : {len(lignes_incompletes)} ({len(lignes_incompletes)/len(df)*100:.1f}%)")
```

---

### Visualiser avec missingno

```python
# pip install missingno
import missingno as msno
import matplotlib.pyplot as plt

# Matrice de valeurs manquantes
msno.matrix(df)
plt.title("Pattern des valeurs manquantes")
plt.show()

# Heatmap de corrélation entre missing
msno.heatmap(df)
plt.title("Corrélation entre valeurs manquantes")
plt.show()
```

**Interprétation de la matrice :**
- Blanc = valeur manquante
- Noir = valeur présente
- Patterns verticaux = colonnes problématiques
- Patterns horizontaux = lignes problématiques

---

### ✍️ Exercice 4.4 : Analyse des missing (15 min)

```python
import pandas as pd
import numpy as np

# Données avec patterns de missing
np.random.seed(42)
n = 1000

df = pd.DataFrame({
    'age': np.random.randint(18, 80, n),
    'revenu': np.random.randint(20000, 100000, n),
    'education': np.random.choice(['Bac', 'Licence', 'Master', 'Doctorat'], n)
})

# Introduire des missing avec pattern
# Les jeunes (< 25 ans) ont souvent des revenus manquants
mask_jeunes = df['age'] < 25
df.loc[mask_jeunes & (np.random.random(n) < 0.4), 'revenu'] = np.nan

# L'éducation est parfois manquante aléatoirement
df.loc[np.random.random(n) < 0.05, 'education'] = np.nan

# Analyse
print("Valeurs manquantes par colonne :")
print(df.isnull().sum())

print("\nPourcentage de revenus manquants par tranche d'âge :")
df['tranche_age'] = pd.cut(df['age'], bins=[18, 25, 35, 50, 80], labels=['18-25', '26-35', '36-50', '51+'])
print(df.groupby('tranche_age')['revenu'].apply(lambda x: x.isnull().mean() * 100))

# Questions :
# 1. Le missing sur 'revenu' est-il MCAR, MAR ou MNAR ?
# 2. Le missing sur 'education' est-il MCAR, MAR ou MNAR ?
# 3. Quelle stratégie de traitement suggéreriez-vous pour chaque cas ?
```

---

## 4.5 Détection des doublons

### Types de doublons

| Type | Définition | Exemple |
|------|------------|---------|
| **Exact** | Lignes 100% identiques | Import dupliqué |
| **Quasi-doublon** | Même entité, petites différences | "Jean Dupont" vs "Jean DUPONT" |
| **Doublon partiel** | Même clé, valeurs différentes | Même client avec 2 adresses |

---

### Détecter les doublons exacts

```python
# Nombre de doublons
print(f"Doublons exacts : {df.duplicated().sum()}")

# Voir les doublons
doublons = df[df.duplicated(keep=False)]  # keep=False montre tous les doublons
print(doublons.sort_values(by=list(df.columns)))

# Doublons sur une clé spécifique
doublons_email = df[df.duplicated(subset=['email'], keep=False)]
print(f"Emails dupliqués : {doublons_email['email'].nunique()}")
```

---

### Identifier les clés candidates

```python
# Vérifier si une colonne peut servir de clé unique
def verifier_cle_unique(df, colonnes):
    """Vérifie si un ensemble de colonnes peut servir de clé unique."""
    doublons = df.duplicated(subset=colonnes)
    if doublons.sum() == 0:
        print(f"✅ {colonnes} peut servir de clé unique")
    else:
        print(f"❌ {colonnes} a {doublons.sum()} doublons")

# Tests
verifier_cle_unique(df, ['id'])
verifier_cle_unique(df, ['email'])
verifier_cle_unique(df, ['nom', 'date_inscription'])
```

---

### ✍️ Exercice 4.5 : Chasse aux doublons (10 min)

```python
import pandas as pd

df = pd.DataFrame({
    'client_id': [1, 2, 3, 4, 5, 1],
    'nom': ['Alice Martin', 'Bob Dupont', 'Charlie Brown', 'David Lee', 'Eve Wilson', 'Alice MARTIN'],
    'email': ['alice@test.com', 'bob@test.com', 'charlie@test.com', 'david@test.com', 'eve@test.com', 'alice@test.com'],
    'date_achat': ['2024-01-15', '2024-01-16', '2024-01-17', '2024-01-18', '2024-01-19', '2024-01-20'],
    'montant': [100, 200, 150, 300, 250, 100]
})

# Votre analyse
# 1. Combien de doublons exacts ?
print("Doublons exacts :", _____)

# 2. Combien de doublons sur client_id ?
print("Doublons client_id :", _____)

# 3. Combien de doublons sur email ?
print("Doublons email :", _____)

# 4. Y a-t-il un quasi-doublon sur le nom ? Comment le détecteriez-vous ?
```

---

> 💭 **Question Socratique #3** : Si deux lignes ont le même email mais des noms légèrement différents ("Jean Dupont" vs "jean dupont"), est-ce un doublon à supprimer ou deux entrées légitimes ? Comment décideriez-vous ?

---

## 4.6 Détection des outliers

### Qu'est-ce qu'un outlier ?

Un **outlier** (valeur aberrante) est une observation qui s'écarte significativement des autres.

**Attention :** Un outlier n'est pas toujours une erreur !
- ❌ Erreur : Âge de 200 ans (impossible)
- ✅ Valeur extrême légitime : Salaire de 500k€ (rare mais réel)

---

### Méthode 1 : IQR (Interquartile Range)

```python
def detecter_outliers_iqr(series, multiplicateur=1.5):
    """Détecte les outliers avec la méthode IQR."""
    Q1 = series.quantile(0.25)
    Q3 = series.quantile(0.75)
    IQR = Q3 - Q1

    borne_inf = Q1 - multiplicateur * IQR
    borne_sup = Q3 + multiplicateur * IQR

    outliers = series[(series < borne_inf) | (series > borne_sup)]
    return outliers, borne_inf, borne_sup

# Application
outliers, b_inf, b_sup = detecter_outliers_iqr(df['salaire'])
print(f"Bornes : [{b_inf:.0f}, {b_sup:.0f}]")
print(f"Outliers : {len(outliers)} ({len(outliers)/len(df)*100:.1f}%)")
```

---

### Méthode 2 : Z-Score

```python
from scipy import stats

def detecter_outliers_zscore(series, seuil=3):
    """Détecte les outliers avec le Z-score."""
    z_scores = stats.zscore(series.dropna())
    outliers_mask = abs(z_scores) > seuil
    return series.dropna()[outliers_mask]

# Application
outliers = detecter_outliers_zscore(df['salaire'])
print(f"Outliers (|Z| > 3) : {len(outliers)}")
```

**Interprétation du Z-score :**
- |Z| > 2 : Valeur inhabituelle (~5% des données)
- |Z| > 3 : Valeur très rare (~0.3%)
- |Z| > 4 : Valeur extrêmement rare

---

### Comparaison des méthodes

| Méthode | Avantages | Inconvénients | Quand l'utiliser |
|---------|-----------|---------------|------------------|
| **IQR** | Robuste, pas d'hypothèse | Conservateur | Données asymétriques |
| **Z-Score** | Simple, interprétable | Sensible aux extrêmes | Données normales |

---

### ✍️ Exercice 4.6 : Détection d'outliers (15 min)

```python
import pandas as pd
import numpy as np

# Données de salaires avec outliers
np.random.seed(42)
salaires = np.concatenate([
    np.random.normal(50000, 10000, 950),  # Salaires normaux
    np.array([150000, 200000, -5000, 300000, 250000])  # Outliers
])
df = pd.DataFrame({'salaire': salaires})

# 1. Méthode IQR
Q1 = df['salaire'].quantile(0.25)
Q3 = df['salaire'].quantile(0.75)
IQR = Q3 - Q1
borne_inf = _____
borne_sup = _____

outliers_iqr = df[(df['salaire'] < borne_inf) | (df['salaire'] > borne_sup)]
print(f"Outliers IQR : {len(outliers_iqr)}")

# 2. Méthode Z-Score
from scipy import stats
z_scores = stats.zscore(df['salaire'])
outliers_zscore = df[abs(z_scores) > 3]
print(f"Outliers Z-Score : {len(outliers_zscore)}")

# Questions :
# a) Pourquoi les deux méthodes donnent-elles des résultats différents ?
# b) Le salaire de -5000 est-il une erreur ou une valeur légitime ?
# c) Quelle méthode préférez-vous ici et pourquoi ?
```

---

## 4.7 Visualisations de diagnostic

### Histogrammes pour les distributions

```python
import matplotlib.pyplot as plt

fig, axes = plt.subplots(1, 3, figsize=(15, 4))

# Distribution normale
df['age'].hist(bins=30, ax=axes[0], edgecolor='black')
axes[0].set_title('Distribution des âges')

# Distribution avec outliers visibles
df['salaire'].hist(bins=30, ax=axes[1], edgecolor='black')
axes[1].set_title('Distribution des salaires')

# Distribution catégorielle
df['categorie'].value_counts().plot(kind='bar', ax=axes[2])
axes[2].set_title('Distribution des catégories')

plt.tight_layout()
plt.show()
```

---

### Boxplots pour les outliers

```python
import seaborn as sns

fig, axes = plt.subplots(1, 2, figsize=(12, 5))

# Boxplot simple
sns.boxplot(y=df['salaire'], ax=axes[0])
axes[0].set_title('Boxplot des salaires')

# Boxplot par catégorie
sns.boxplot(x='departement', y='salaire', data=df, ax=axes[1])
axes[1].set_title('Salaires par département')
plt.xticks(rotation=45)

plt.tight_layout()
plt.show()
```

**Lecture d'un boxplot :**
- Boîte = Q1 à Q3 (50% des données)
- Ligne centrale = Médiane
- Moustaches = 1.5 × IQR
- Points = Outliers

---

### Heatmap pour les valeurs manquantes

```python
import seaborn as sns
import matplotlib.pyplot as plt

# Créer une matrice de missing (True/False)
missing_matrix = df.isnull()

# Heatmap
plt.figure(figsize=(12, 8))
sns.heatmap(missing_matrix, cbar=True, yticklabels=False, cmap='viridis')
plt.title('Carte des valeurs manquantes')
plt.show()
```

---

### ✍️ Exercice 4.7 : Dashboard de diagnostic (20 min)

Créez un dashboard de diagnostic complet pour un DataFrame :

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Données simulées
np.random.seed(42)
df = pd.DataFrame({
    'age': np.concatenate([np.random.randint(18, 70, 950), [150, -5, 200]]),
    'revenu': np.concatenate([np.random.normal(50000, 15000, 900), [np.nan]*100]),
    'categorie': np.random.choice(['A', 'B', 'C', None], 1000, p=[0.3, 0.3, 0.3, 0.1])
})

# Dashboard
fig, axes = plt.subplots(2, 2, figsize=(14, 10))

# 1. Distribution des âges avec outliers
axes[0, 0].hist(df['age'].dropna(), bins=30, edgecolor='black')
axes[0, 0].set_title('Distribution des âges')
axes[0, 0].axvline(df['age'].median(), color='red', linestyle='--', label='Médiane')

# 2. Boxplot des revenus
sns.boxplot(y=df['revenu'], ax=axes[0, 1])
axes[0, 1].set_title('Boxplot des revenus')

# 3. Valeurs manquantes par colonne
missing_pct = (df.isnull().mean() * 100)
missing_pct.plot(kind='bar', ax=axes[1, 0], color='coral')
axes[1, 0].set_title('% Valeurs manquantes')
axes[1, 0].set_ylabel('Pourcentage')

# 4. Distribution des catégories
df['categorie'].value_counts(dropna=False).plot(kind='pie', ax=axes[1, 1], autopct='%1.1f%%')
axes[1, 1].set_title('Distribution des catégories')

plt.tight_layout()
plt.show()

# Questions :
# 1. Combien d'outliers identifiez-vous dans les âges ?
# 2. Quel pourcentage de revenus est manquant ?
# 3. Quel problème voyez-vous dans les catégories ?
```

---

## 4.8 Documentation des problèmes

### Créer une checklist de qualité

```python
def generer_rapport_qualite(df):
    """Génère un rapport de qualité complet."""
    rapport = []

    # 1. Complétude
    for col in df.columns:
        missing_pct = df[col].isnull().mean() * 100
        if missing_pct > 0:
            rapport.append({
                'Dimension': 'Complétude',
                'Colonne': col,
                'Problème': f'{missing_pct:.1f}% manquant',
                'Priorité': 'Haute' if missing_pct > 20 else 'Moyenne'
            })

    # 2. Unicité
    doublons = df.duplicated().sum()
    if doublons > 0:
        rapport.append({
            'Dimension': 'Unicité',
            'Colonne': 'Toutes',
            'Problème': f'{doublons} doublons exacts',
            'Priorité': 'Haute'
        })

    # 3. Exactitude (numériques)
    for col in df.select_dtypes(include=[np.number]).columns:
        Q1, Q3 = df[col].quantile([0.25, 0.75])
        IQR = Q3 - Q1
        outliers = df[(df[col] < Q1 - 1.5*IQR) | (df[col] > Q3 + 1.5*IQR)][col]
        if len(outliers) > 0:
            rapport.append({
                'Dimension': 'Exactitude',
                'Colonne': col,
                'Problème': f'{len(outliers)} outliers détectés',
                'Priorité': 'Moyenne'
            })

    return pd.DataFrame(rapport)

# Générer le rapport
rapport = generer_rapport_qualite(df)
print(rapport)
```

---

### Priorisation des problèmes

| Priorité | Critères | Action |
|----------|----------|--------|
| **🔴 Haute** | >20% missing, doublons, erreurs critiques | Corriger immédiatement |
| **🟡 Moyenne** | 5-20% missing, outliers à vérifier | Corriger si temps disponible |
| **🟢 Basse** | <5% missing, problèmes mineurs | Documenter pour plus tard |

---

### ✍️ Exercice 4.8 : Rapport de qualité (15 min)

Créez un rapport de qualité pour ce DataFrame et priorisez les corrections :

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    'id': [1, 2, 3, 4, 5, 1, 7, 8, 9, 10],
    'nom': ['Alice', 'Bob', np.nan, 'David', 'Eve', 'Alice', 'Grace', 'Henry', np.nan, 'Julia'],
    'age': [25, 30, 35, 150, 28, 25, 42, -5, 38, 45],
    'email': ['a@t.com', 'b@t.com', 'c@t.com', 'd@t.com', 'invalid', 'a@t.com', 'g@t.com', 'h@t.com', 'i@t.com', 'j@t.com'],
    'salaire': [50000, 55000, np.nan, np.nan, 45000, 50000, 70000, 62000, np.nan, 80000]
})

# Votre rapport
problemes = []

# Complétude
# ...

# Unicité
# ...

# Exactitude
# ...

# Validité (email)
# ...

# Classez les problèmes par priorité
```

---

## 🧠 Réflexion métacognitive

### Auto-évaluation

| Compétence | 1 | 2 | 3 | 4 | 5 |
|------------|---|---|---|---|---|
| Je distingue EDA diagnostique et analytique | ○ | ○ | ○ | ○ | ○ |
| Je connais les 6 dimensions de qualité | ○ | ○ | ○ | ○ | ○ |
| Je sais détecter les valeurs manquantes | ○ | ○ | ○ | ○ | ○ |
| Je sais identifier les doublons | ○ | ○ | ○ | ○ | ○ |
| Je sais détecter les outliers (IQR, Z-score) | ○ | ○ | ○ | ○ | ○ |
| Je peux créer un rapport de qualité priorisé | ○ | ○ | ○ | ○ | ○ |

### Questions de réflexion

1. **Quelle dimension de qualité** vous semble la plus difficile à évaluer ? Pourquoi ?

2. **Comment expliqueriez-vous** l'importance de l'EDA diagnostique à un manager non-technique ?

3. **Dans votre futur métier**, quel type de problème de qualité pensez-vous rencontrer le plus souvent ?

---

## 📚 Résumé du chapitre

### Points clés à retenir

1. **EDA Diagnostique ≠ Analytique** :
   - Diagnostique = trouver les problèmes (avant nettoyage)
   - Analytique = comprendre les patterns (après nettoyage)

2. **6 dimensions de qualité** :
   - Complétude, Unicité, Cohérence
   - Exactitude, Validité, Fraîcheur

3. **Outils de détection** :
   - Missing : `isnull()`, `missingno`
   - Doublons : `duplicated()`
   - Outliers : IQR (robuste), Z-score (distributions normales)

4. **Documentation** :
   - Toujours créer une checklist de qualité
   - Prioriser : Haute > Moyenne > Basse

---

## 🔗 Sources et références

- [IBM - Data Quality Dimensions](https://www.ibm.com/think/topics/data-quality-dimensions)
- [Atlan - Data Quality Dimensions 2025](https://atlan.com/data-quality-dimensions/)
- [Monte Carlo - 6 Data Quality Dimensions](https://www.montecarlodata.com/blog-6-data-quality-dimensions-examples/)
- [dbt Labs - Data Quality Dimensions](https://www.getdbt.com/blog/data-quality-dimensions)

---

## ➡️ Prochain chapitre

**Chapitre 5 : Nettoyage des données** — Vous apprendrez à corriger les problèmes identifiés : traiter les valeurs manquantes, supprimer les doublons, et gérer les outliers.

---

*Module 2 — Pipeline Data | Chapitre 4 sur 11*
