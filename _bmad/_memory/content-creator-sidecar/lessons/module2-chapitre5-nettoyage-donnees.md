# Chapitre 5 — Nettoyage des données

**Durée estimée : 10-12 heures**

---

## Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

1. **Appliquer** différentes stratégies de traitement des valeurs manquantes (suppression, imputation)
2. **Identifier et supprimer** les doublons en préservant les informations pertinentes
3. **Traiter** les valeurs aberrantes selon le contexte métier
4. **Nettoyer** les types de données (dates, numériques, texte) pour les rendre exploitables

---

## 🎯 Le Hook : La startup qui a perdu 2 millions à cause de doublons

En 2021, une startup e-commerce française a lancé une campagne marketing ciblée. Ils ont envoyé un code promo de 50€ à leurs "100 000 meilleurs clients". Le problème ? Leur base contenait **40% de doublons**.

Résultat : 40 000 personnes ont reçu plusieurs codes. Les plus malins ont cumulé les réductions. Perte totale : **2 millions d'euros**.

Le nettoyage des données n'est pas un luxe académique. C'est une **nécessité business**.

> 💭 **Question Socratique #1** : Cette erreur aurait-elle pu être évitée par une simple requête SQL de dé-duplication ? Ou le problème était-il plus profond (processus, culture, outils) ?

---

## 5.1 Gestion des valeurs manquantes

### Les trois stratégies principales

```
┌─────────────────────────────────────────────────────────────────────┐
│              STRATÉGIES DE TRAITEMENT DES MISSING                   │
├─────────────────────┬─────────────────────┬─────────────────────────┤
│     SUPPRESSION     │     IMPUTATION      │        FLAG             │
│                     │                     │                         │
│  Supprimer lignes   │  Remplacer par      │  Créer une colonne      │
│  ou colonnes        │  une valeur         │  indicatrice            │
│                     │  estimée            │                         │
├─────────────────────┼─────────────────────┼─────────────────────────┤
│  • dropna()         │  • fillna(valeur)   │  • isna().astype(int)   │
│  • Simple mais      │  • Moyenne/médiane  │  • Préserve             │
│    perte de données │  • Par groupe       │    l'information        │
└─────────────────────┴─────────────────────┴─────────────────────────┘
```

---

### Stratégie 1 : Suppression

#### Quand supprimer ?

| Situation | Action recommandée |
|-----------|-------------------|
| < 5% de missing dans une colonne | Supprimer les lignes concernées |
| > 50% de missing dans une colonne | Supprimer la colonne entière |
| Données MCAR (manque aléatoire) | Suppression acceptable |
| Donnée critique pour l'analyse | Ne pas supprimer, imputer |

#### Code pandas

```python
# Supprimer les lignes avec au moins une valeur manquante
df_clean = df.dropna()

# Supprimer les lignes où une colonne spécifique est manquante
df_clean = df.dropna(subset=['email', 'client_id'])

# Supprimer les colonnes avec plus de 50% de missing
seuil = len(df) * 0.5
df_clean = df.dropna(axis=1, thresh=seuil)

# Supprimer uniquement si TOUTES les valeurs sont manquantes
df_clean = df.dropna(how='all')
```

---

### ✍️ Exercice 5.1 : Décision de suppression (10 min)

Pour chaque colonne, décidez si vous supprimez les lignes, la colonne, ou si vous imputez :

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    'id': range(1000),
    'nom': ['Client_' + str(i) for i in range(1000)],
    'age': np.where(np.random.random(1000) < 0.03, np.nan, np.random.randint(18, 70, 1000)),
    'email': np.where(np.random.random(1000) < 0.02, np.nan, ['email_' + str(i) + '@test.com' for i in range(1000)]),
    'revenu': np.where(np.random.random(1000) < 0.25, np.nan, np.random.randint(20000, 100000, 1000)),
    'notes_internes': np.where(np.random.random(1000) < 0.80, np.nan, ['Note_' + str(i) for i in range(1000)])
})

# Analyse
print("Pourcentage de missing par colonne :")
print((df.isnull().mean() * 100).round(1))

# Décisions :
# - id : _____
# - nom : _____
# - age (~3% missing) : _____
# - email (~2% missing) : _____
# - revenu (~25% missing) : _____
# - notes_internes (~80% missing) : _____
```

---

### Stratégie 2 : Imputation

#### Imputation simple

```python
# Imputation par valeur fixe
df['categorie'].fillna('Inconnu', inplace=True)

# Imputation par moyenne (numériques)
df['age'].fillna(df['age'].mean(), inplace=True)

# Imputation par médiane (plus robuste aux outliers)
df['revenu'].fillna(df['revenu'].median(), inplace=True)

# Imputation par mode (catégorielles)
df['ville'].fillna(df['ville'].mode()[0], inplace=True)
```

#### Imputation contextuelle (par groupe)

```python
# Imputer le salaire par la médiane du département
df['salaire'] = df.groupby('departement')['salaire'].transform(
    lambda x: x.fillna(x.median())
)

# Imputer l'âge par la moyenne de la catégorie client
df['age'] = df.groupby('categorie_client')['age'].transform(
    lambda x: x.fillna(x.mean())
)
```

#### Forward/Backward fill (séries temporelles)

```python
# Propager la dernière valeur connue
df['temperature'].fillna(method='ffill', inplace=True)

# Propager la prochaine valeur connue
df['temperature'].fillna(method='bfill', inplace=True)

# Interpolation linéaire
df['temperature'] = df['temperature'].interpolate(method='linear')
```

---

### 🤖 IA : Suggérer une stratégie adaptée au contexte

**Prompt efficace :**

```
J'ai un DataFrame avec ces colonnes et pourcentages de missing :
- client_id : 0%
- age : 8%
- revenu : 35%
- date_inscription : 2%
- score_credit : 45%

Contexte : Modèle de prédiction de défaut de paiement
Contraintes : Le revenu et le score_credit sont importants pour le modèle

Suggère une stratégie de traitement des missing pour chaque colonne,
avec le code pandas correspondant et une justification.
```

---

### Stratégie 3 : Flag (indicateur)

Parfois, le fait qu'une donnée soit manquante **est une information en soi**.

```python
# Créer une colonne indicatrice
df['revenu_manquant'] = df['revenu'].isna().astype(int)

# Puis imputer le revenu
df['revenu'].fillna(df['revenu'].median(), inplace=True)

# Maintenant vous avez les deux informations :
# - revenu imputé
# - indicateur si c'était manquant
```

**Cas d'usage :** Un client qui ne déclare pas son revenu peut avoir un profil différent de celui qui le déclare.

---

### ✍️ Exercice 5.2 : Imputation complète (15 min)

```python
import pandas as pd
import numpy as np

# Données avec patterns réalistes
np.random.seed(42)
df = pd.DataFrame({
    'client_id': range(500),
    'age': np.where(np.random.random(500) < 0.1, np.nan, np.random.randint(20, 70, 500)),
    'departement': np.random.choice(['Paris', 'Lyon', 'Marseille', 'Bordeaux'], 500),
    'salaire': np.where(np.random.random(500) < 0.2, np.nan, np.random.randint(25000, 80000, 500)),
    'anciennete_mois': np.where(np.random.random(500) < 0.05, np.nan, np.random.randint(1, 120, 500))
})

# Ajuster les salaires par département
salaire_moyen = {'Paris': 55000, 'Lyon': 45000, 'Marseille': 42000, 'Bordeaux': 43000}
for dept, moyenne in salaire_moyen.items():
    mask = (df['departement'] == dept) & df['salaire'].notna()
    df.loc[mask, 'salaire'] = df.loc[mask, 'salaire'] * (moyenne / 50000)

print("Avant nettoyage :")
print(df.isnull().sum())

# Votre travail :
# 1. Imputer l'âge par la médiane globale
df['age'] = _____

# 2. Imputer le salaire par la médiane du département
df['salaire'] = df.groupby('departement')['salaire'].transform(_____)

# 3. Supprimer les lignes où anciennete_mois est manquant (< 5%)
df = df._____

# 4. Créer un flag pour les salaires qui étaient manquants (avant imputation)
# Hint : il faut le faire AVANT l'imputation, refaites l'exercice dans le bon ordre

print("\nAprès nettoyage :")
print(df.isnull().sum())
```

---

> 💭 **Question Socratique #2** : Imputer les revenus par la moyenne peut-il créer un biais si les revenus manquants ne sont pas aléatoires ? Par exemple, si les hauts revenus refusent plus souvent de déclarer ?

---

## 5.2 Traitement des doublons

### Identification complète

```python
# Nombre de doublons exacts
print(f"Doublons exacts : {df.duplicated().sum()}")

# Voir toutes les lignes dupliquées (y compris les originaux)
doublons = df[df.duplicated(keep=False)]
print(doublons.sort_values(by=list(df.columns)))

# Doublons sur une clé spécifique
doublons_email = df[df.duplicated(subset=['email'], keep=False)]
print(f"Emails en double : {len(doublons_email)}")
```

### Suppression des doublons

```python
# Supprimer les doublons exacts (garder la première occurrence)
df_clean = df.drop_duplicates()

# Garder la dernière occurrence
df_clean = df.drop_duplicates(keep='last')

# Supprimer TOUTES les occurrences (y compris l'original)
df_clean = df.drop_duplicates(keep=False)

# Dé-dupliquer sur des colonnes spécifiques
df_clean = df.drop_duplicates(subset=['email'])

# Dé-dupliquer en gardant la ligne avec le montant le plus élevé
df_clean = df.sort_values('montant', ascending=False).drop_duplicates(subset=['client_id'])
```

---

### Gestion des quasi-doublons

Les quasi-doublons sont plus difficiles à détecter : même entité avec des variations mineures.

```python
# Normaliser avant de comparer
df['nom_normalise'] = df['nom'].str.lower().str.strip()
df['email_normalise'] = df['email'].str.lower().str.strip()

# Détecter les quasi-doublons
quasi_doublons = df[df.duplicated(subset=['nom_normalise', 'email_normalise'], keep=False)]
print(f"Quasi-doublons : {len(quasi_doublons)}")

# Fusionner les quasi-doublons (garder les infos les plus complètes)
df_clean = df.sort_values(['nom_normalise', 'date_maj'], ascending=[True, False])
df_clean = df_clean.drop_duplicates(subset=['nom_normalise', 'email_normalise'], keep='first')
```

---

### ✍️ Exercice 5.3 : Dé-duplication intelligente (15 min)

```python
import pandas as pd

# Données avec différents types de doublons
df = pd.DataFrame({
    'id': [1, 2, 3, 4, 5, 6, 7, 8],
    'nom': ['Alice Martin', 'Bob Dupont', 'alice martin', 'Charlie Brown',
            'Bob Dupont', 'David Lee', 'ALICE MARTIN', 'Eve Wilson'],
    'email': ['alice@test.com', 'bob@test.com', 'alice@test.com', 'charlie@test.com',
              'bob@test.com', 'david@test.com', 'alice@test.com', 'eve@test.com'],
    'date_inscription': ['2024-01-15', '2024-01-16', '2024-02-01', '2024-01-17',
                         '2024-03-01', '2024-01-18', '2024-03-15', '2024-01-19'],
    'montant_total': [500, 1200, 300, 800, 1500, 600, 200, 900]
})

print("Données originales :")
print(df)

# Étape 1 : Normaliser le nom et l'email
df['nom_norm'] = df['nom']._____._____._____
df['email_norm'] = df['email']._____

# Étape 2 : Identifier les doublons sur nom_norm + email_norm
doublons = df[df.duplicated(subset=['nom_norm', 'email_norm'], keep=False)]
print(f"\nDoublons identifiés : {len(doublons)}")
print(doublons)

# Étape 3 : Garder l'enregistrement avec le montant_total le plus élevé
df_clean = df.sort_values('montant_total', ascending=False)
df_clean = df_clean.drop_duplicates(subset=['nom_norm', 'email_norm'], keep='first')

# Étape 4 : Nettoyer les colonnes temporaires
df_clean = df_clean.drop(columns=['nom_norm', 'email_norm'])

print(f"\nRésultat : {len(df_clean)} lignes uniques")
print(df_clean)

# Question : Pourquoi avons-nous gardé la ligne avec le montant le plus élevé ?
# Auriez-vous fait un autre choix ? Lequel ?
```

---

## 5.3 Traitement des valeurs aberrantes

### Les trois approches

| Approche | Description | Quand l'utiliser |
|----------|-------------|------------------|
| **Suppression** | Retirer les outliers | Erreurs évidentes (âge négatif) |
| **Winsorisation** | Ramener aux bornes | Conserver toutes les lignes |
| **Conservation** | Ne rien faire | Outlier = information réelle |

---

### Suppression conditionnelle

```python
# Supprimer les valeurs impossibles
df = df[df['age'] >= 0]
df = df[df['age'] <= 120]

# Supprimer selon IQR
Q1, Q3 = df['salaire'].quantile([0.25, 0.75])
IQR = Q3 - Q1
borne_inf = Q1 - 1.5 * IQR
borne_sup = Q3 + 1.5 * IQR

df_clean = df[(df['salaire'] >= borne_inf) & (df['salaire'] <= borne_sup)]
print(f"Lignes supprimées : {len(df) - len(df_clean)}")
```

---

### Winsorisation (capping)

```python
# Ramener les valeurs extrêmes aux bornes
def winsorize(series, lower_percentile=0.01, upper_percentile=0.99):
    """Winsorise une série aux percentiles spécifiés."""
    lower = series.quantile(lower_percentile)
    upper = series.quantile(upper_percentile)
    return series.clip(lower=lower, upper=upper)

df['salaire_winsorized'] = winsorize(df['salaire'])

# Avec scipy
from scipy.stats.mstats import winsorize as scipy_winsorize
df['salaire_winsorized'] = scipy_winsorize(df['salaire'], limits=[0.01, 0.01])
```

---

### Décision métier : Supprimer ou conserver ?

```python
def analyser_outlier(df, colonne, valeur):
    """Aide à décider si un outlier doit être conservé."""
    ligne = df[df[colonne] == valeur]

    print(f"=== Analyse de l'outlier: {colonne} = {valeur} ===")
    print(f"\nContexte de la ligne :")
    print(ligne)

    print(f"\nStatistiques de la colonne :")
    print(df[colonne].describe())

    print(f"\nQuestions à se poser :")
    print("1. Cette valeur est-elle physiquement possible ?")
    print("2. Y a-t-il une explication métier ?")
    print("3. Comment a-t-elle été collectée ?")
    print("4. Quel impact sur l'analyse si on la garde/supprime ?")

# Exemple
analyser_outlier(df, 'salaire', 500000)
```

---

### ✍️ Exercice 5.4 : Traitement d'outliers (15 min)

```python
import pandas as pd
import numpy as np

# Données avec outliers variés
np.random.seed(42)
df = pd.DataFrame({
    'employe_id': range(100),
    'age': np.concatenate([np.random.randint(22, 65, 97), [-5, 150, 35]]),
    'salaire': np.concatenate([np.random.normal(50000, 10000, 96), [500000, -1000, 48000, 52000]]),
    'heures_travaillees': np.concatenate([np.random.normal(40, 5, 98), [168, 200]])
})

# Analyse
print("Statistiques descriptives :")
print(df.describe())

# Votre travail :

# 1. L'âge de -5 : Erreur ou valeur légitime ?
# Action : _____
df = df[df['age'] >= _____]

# 2. L'âge de 150 : Erreur ou valeur légitime ?
# Action : _____
df = df[df['age'] <= _____]

# 3. Le salaire de 500000 : CEO ou erreur de saisie ?
# Décision : Sans contexte supplémentaire, on winsorise
df['salaire'] = df['salaire'].clip(upper=df['salaire'].quantile(0.99))

# 4. Le salaire de -1000 : Erreur évidente
df = df[df['salaire'] >= _____]

# 5. Les heures travaillées > 168 (heures dans une semaine)
# Action : _____

print(f"\nLignes restantes : {len(df)}")
```

---

> 💭 **Question Socratique #3** : Un data scientist supprime tous les outliers de son dataset avant d'entraîner un modèle de détection de fraude. Voyez-vous le problème avec cette approche ?

---

## 5.4 Nettoyage des types de données

### Conversion des dates

```python
# Conversion basique
df['date'] = pd.to_datetime(df['date'])

# Avec format spécifique
df['date'] = pd.to_datetime(df['date'], format='%d/%m/%Y')

# Gestion des erreurs
df['date'] = pd.to_datetime(df['date'], errors='coerce')  # NaT pour les erreurs

# Formats courants
formats = {
    'ISO': '%Y-%m-%d',           # 2024-01-15
    'FR': '%d/%m/%Y',            # 15/01/2024
    'US': '%m/%d/%Y',            # 01/15/2024
    'Texte': '%d %B %Y',         # 15 janvier 2024
    'Datetime': '%Y-%m-%d %H:%M:%S'  # 2024-01-15 14:30:00
}
```

#### Extraction de composantes

```python
# Extraire des composantes temporelles
df['annee'] = df['date'].dt.year
df['mois'] = df['date'].dt.month
df['jour'] = df['date'].dt.day
df['jour_semaine'] = df['date'].dt.dayofweek  # 0=lundi
df['trimestre'] = df['date'].dt.quarter
df['semaine'] = df['date'].dt.isocalendar().week
```

---

### Conversion des numériques

```python
# String vers numérique
df['prix'] = pd.to_numeric(df['prix'], errors='coerce')

# Gestion des formats français (virgule décimale)
df['montant'] = df['montant'].str.replace(',', '.').str.replace(' ', '')
df['montant'] = pd.to_numeric(df['montant'])

# Gestion des symboles monétaires
df['prix'] = df['prix'].str.replace('€', '').str.replace(' ', '')
df['prix'] = pd.to_numeric(df['prix'])

# Pourcentages
df['taux'] = df['taux'].str.replace('%', '')
df['taux'] = pd.to_numeric(df['taux']) / 100
```

---

### ✍️ Exercice 5.5 : Conversion de types (15 min)

```python
import pandas as pd

# Données avec formats variés
df = pd.DataFrame({
    'date_fr': ['15/01/2024', '28/02/2024', '31/04/2024', '10/03/2024'],  # Note: 31 avril n'existe pas
    'prix_fr': ['1 234,56 €', '987,00 €', '45,99 €', '2 500,00 €'],
    'taux': ['5,5%', '20%', '10%', '7,5%'],
    'quantite': ['100', '50', 'vingt', '75']  # Note: 'vingt' n'est pas convertible
})

print("Avant conversion :")
print(df.dtypes)

# 1. Convertir les dates (gérer l'erreur du 31 avril)
df['date_clean'] = pd.to_datetime(df['date_fr'], format='%d/%m/%Y', errors='_____')

# 2. Convertir les prix
df['prix_clean'] = df['prix_fr'].str.replace(' ', '').str.replace('€', '').str.replace(',', '.')
df['prix_clean'] = pd.to_numeric(df['prix_clean'])

# 3. Convertir les taux en décimales
df['taux_clean'] = df['taux'].str.replace(',', '.').str.replace('%', '')
df['taux_clean'] = pd.to_numeric(df['taux_clean']) / 100

# 4. Convertir les quantités (gérer 'vingt')
df['quantite_clean'] = pd.to_numeric(df['quantite'], errors='_____')

print("\nAprès conversion :")
print(df[['date_clean', 'prix_clean', 'taux_clean', 'quantite_clean']])
print(df.dtypes)

# Question : Comment traiteriez-vous la ligne avec 'vingt' ?
```

---

## 5.5 Nettoyage de texte

### Opérations de base

```python
# Supprimer les espaces
df['nom'] = df['nom'].str.strip()

# Convertir en minuscules/majuscules
df['email'] = df['email'].str.lower()
df['pays'] = df['pays'].str.upper()

# Remplacer des caractères
df['telephone'] = df['telephone'].str.replace(' ', '').str.replace('-', '')

# Supprimer les accents
import unicodedata
def supprimer_accents(texte):
    if pd.isna(texte):
        return texte
    return ''.join(
        c for c in unicodedata.normalize('NFD', texte)
        if unicodedata.category(c) != 'Mn'
    )
df['ville'] = df['ville'].apply(supprimer_accents)
```

---

### Expressions régulières

```python
import re

# Extraire un pattern
df['code_postal'] = df['adresse'].str.extract(r'(\d{5})')

# Valider un format email
pattern_email = r'^[\w\.-]+@[\w\.-]+\.\w+$'
df['email_valide'] = df['email'].str.match(pattern_email)

# Nettoyer les caractères spéciaux
df['nom_clean'] = df['nom'].str.replace(r'[^a-zA-ZÀ-ÿ\s-]', '', regex=True)

# Extraire des nombres d'un texte
df['montant'] = df['description'].str.extract(r'(\d+[\.,]?\d*)')[0]
```

---

### Standardisation des catégories

```python
# Problème courant : variations d'écriture
print(df['pays'].unique())
# ['France', 'france', 'FRANCE', 'FR', 'Francia', 'Frankreich']

# Solution : mapping
mapping_pays = {
    'france': 'France',
    'fr': 'France',
    'francia': 'France',
    'frankreich': 'France',
    'allemagne': 'Allemagne',
    'de': 'Allemagne',
    'germany': 'Allemagne'
}

df['pays_clean'] = df['pays'].str.lower().map(mapping_pays).fillna(df['pays'])
```

---

### ✍️ Exercice 5.6 : Nettoyage de texte complet (20 min)

```python
import pandas as pd

# Données textuelles désordonnées
df = pd.DataFrame({
    'nom': ['  Jean DUPONT  ', 'marie-claire Martin', 'PIERRE durand', 'émilie Côté'],
    'email': ['Jean.Dupont@Gmail.COM', 'marie@test.fr', 'PIERRE123@yahoo.fr', 'emilie@test'],
    'telephone': ['06 12 34 56 78', '0687654321', '+33 6 11 22 33 44', '06-99-88-77-66'],
    'adresse': ['12 rue de Paris, 75001 PARIS', '5 avenue Lyon 69001', 'Marseille', '10 bd Bordeaux 33000']
})

print("Avant nettoyage :")
print(df)

# 1. Normaliser les noms (strip, title case)
df['nom_clean'] = df['nom']._____._____

# 2. Normaliser les emails (lower, strip)
df['email_clean'] = df['email']._____._____

# 3. Valider les emails (contient @ et .)
df['email_valide'] = df['email_clean'].str.contains('@') & df['email_clean'].str.contains(r'\.')

# 4. Nettoyer les téléphones (garder uniquement les chiffres)
df['tel_clean'] = df['telephone'].str.replace(r'[^\d]', '', regex=True)
# Normaliser au format français (commencer par 0)
df['tel_clean'] = df['tel_clean'].str.replace('^33', '0', regex=True)

# 5. Extraire le code postal de l'adresse
df['code_postal'] = df['adresse'].str.extract(r'(\d{5})')

print("\nAprès nettoyage :")
print(df[['nom_clean', 'email_clean', 'email_valide', 'tel_clean', 'code_postal']])
```

---

## 5.6 Nettoyage avec SQL

### Quand nettoyer côté SQL ?

| Situation | Côté SQL | Côté Python |
|-----------|----------|-------------|
| Filtrer des millions de lignes | ✅ | ❌ |
| Dé-dupliquer avant extraction | ✅ | ○ |
| Transformations complexes | ○ | ✅ |
| Jointures avec d'autres tables | ✅ | ○ |
| Nettoyage itératif/exploratoire | ❌ | ✅ |

### Fonctions SQL de nettoyage

```sql
-- Supprimer les espaces
SELECT TRIM(nom) AS nom_clean FROM clients;

-- Convertir la casse
SELECT UPPER(pays) AS pays, LOWER(email) AS email FROM clients;

-- Remplacer les NULL
SELECT COALESCE(telephone, 'Non renseigné') AS telephone FROM clients;

-- Remplacer des valeurs
SELECT REPLACE(telephone, ' ', '') AS telephone FROM clients;

-- Extraire une sous-chaîne
SELECT SUBSTRING(code_postal, 1, 2) AS departement FROM clients;

-- Dé-dupliquer
SELECT DISTINCT email FROM clients;

-- Dé-dupliquer avec ROW_NUMBER (garder le plus récent)
WITH ranked AS (
    SELECT *,
           ROW_NUMBER() OVER (PARTITION BY email ORDER BY date_maj DESC) as rn
    FROM clients
)
SELECT * FROM ranked WHERE rn = 1;
```

---

### ✍️ Exercice 5.7 : SQL vs Python (10 min)

Pour chaque opération, indiquez si vous la feriez côté **SQL** ou côté **Python** :

| Opération | SQL | Python | Justification |
|-----------|-----|--------|---------------|
| Filtrer les clients actifs sur 10M de lignes | ○ | ○ | _____ |
| Calculer l'âge à partir de la date de naissance | ○ | ○ | _____ |
| Imputer les valeurs manquantes par la médiane | ○ | ○ | _____ |
| Joindre avec la table des commandes | ○ | ○ | _____ |
| Appliquer une regex complexe | ○ | ○ | _____ |
| Supprimer les doublons exacts | ○ | ○ | _____ |

---

## 5.7 Traçabilité et documentation

### Pourquoi documenter ?

1. **Reproductibilité** : Pouvoir refaire exactement le même nettoyage
2. **Audit** : Justifier les choix auprès des stakeholders
3. **Collaboration** : Permettre à d'autres de comprendre vos décisions
4. **Debugging** : Identifier d'où vient un problème

### Template de documentation

```python
# === DOCUMENTATION DU NETTOYAGE ===
# Date : 2025-01-15
# Auteur : Votre nom
# Dataset : clients_export_2024.csv
# Lignes initiales : 10,543

nettoyage_log = {
    'date': '2025-01-15',
    'fichier_source': 'clients_export_2024.csv',
    'lignes_initiales': 10543,
    'operations': []
}

# 1. Suppression des doublons
doublons_avant = df.duplicated().sum()
df = df.drop_duplicates()
nettoyage_log['operations'].append({
    'etape': 1,
    'operation': 'Suppression doublons exacts',
    'lignes_supprimees': doublons_avant,
    'lignes_restantes': len(df)
})

# 2. Traitement des valeurs manquantes
missing_avant = df['email'].isnull().sum()
df = df.dropna(subset=['email'])
nettoyage_log['operations'].append({
    'etape': 2,
    'operation': 'Suppression lignes sans email',
    'lignes_supprimees': missing_avant,
    'justification': 'Email requis pour contact client'
})

# 3. Correction des âges invalides
ages_invalides = len(df[(df['age'] < 0) | (df['age'] > 120)])
df = df[(df['age'] >= 0) & (df['age'] <= 120)]
nettoyage_log['operations'].append({
    'etape': 3,
    'operation': 'Suppression âges hors plage [0-120]',
    'lignes_supprimees': ages_invalides
})

# Résumé
nettoyage_log['lignes_finales'] = len(df)
nettoyage_log['taux_retention'] = f"{len(df)/10543*100:.1f}%"

# Sauvegarder le log
import json
with open('nettoyage_log.json', 'w') as f:
    json.dump(nettoyage_log, f, indent=2)
```

---

### ✍️ Exercice 5.8 : Documentation complète (15 min)

Complétez le log de nettoyage pour ce pipeline :

```python
import pandas as pd
import numpy as np

# Données initiales
df = pd.DataFrame({
    'id': [1, 2, 3, 4, 5, 5, 7, 8],
    'nom': ['Alice', 'Bob', None, 'David', 'Eve', 'Eve', 'Grace', 'Henry'],
    'age': [25, 30, 35, -5, 28, 28, 150, 42],
    'email': ['a@t.com', 'b@t.com', 'c@t.com', 'd@t.com', 'e@t.com', 'e@t.com', 'g@t.com', 'h@t.com']
})

lignes_initiales = len(df)
log = []

# Étape 1 : Doublons
n_doublons = df.duplicated().sum()
df = df.drop_duplicates()
log.append(f"Étape 1: Supprimé {_____} doublons")

# Étape 2 : Noms manquants
n_missing = df['nom'].isnull().sum()
df = df.dropna(subset=['nom'])
log.append(f"Étape 2: Supprimé {_____} lignes sans nom")

# Étape 3 : Âges invalides
n_invalides = len(df[(df['age'] < 0) | (df['age'] > 120)])
df = df[(df['age'] >= 0) & (df['age'] <= 120)]
log.append(f"Étape 3: Supprimé {_____} âges invalides")

# Résumé
print("=== LOG DE NETTOYAGE ===")
for entry in log:
    print(entry)
print(f"\nLignes initiales : {lignes_initiales}")
print(f"Lignes finales : {len(df)}")
print(f"Taux de rétention : {len(df)/lignes_initiales*100:.1f}%")
```

---

## 🧠 Réflexion métacognitive

### Auto-évaluation

| Compétence | 1 | 2 | 3 | 4 | 5 |
|------------|---|---|---|---|---|
| Je sais choisir entre suppression, imputation et flag | ○ | ○ | ○ | ○ | ○ |
| Je peux identifier et supprimer les doublons | ○ | ○ | ○ | ○ | ○ |
| Je sais traiter les outliers selon le contexte | ○ | ○ | ○ | ○ | ○ |
| Je peux convertir les types (dates, numériques) | ○ | ○ | ○ | ○ | ○ |
| Je sais nettoyer du texte (strip, regex) | ○ | ○ | ○ | ○ | ○ |
| Je documente mes choix de nettoyage | ○ | ○ | ○ | ○ | ○ |

### Questions de réflexion

1. **Quelle stratégie de nettoyage** vous semble la plus risquée (celle où on peut perdre de l'information) ?

2. **Comment justifieriez-vous** vos choix de nettoyage à un manager non-technique ?

3. **Quel est le piège principal** à éviter lors du nettoyage de données ?

---

## 📚 Résumé du chapitre

### Points clés à retenir

1. **Valeurs manquantes** :
   - Suppression si < 5% et MCAR
   - Imputation par médiane/moyenne (numérique) ou mode (catégoriel)
   - Flag pour préserver l'information du missing

2. **Doublons** :
   - Normaliser avant de comparer (lower, strip)
   - `drop_duplicates(subset=[...], keep='first'/'last')`

3. **Outliers** :
   - Suppression pour erreurs évidentes
   - Winsorisation pour conserver toutes les lignes
   - **Toujours justifier par le contexte métier**

4. **Types de données** :
   - `pd.to_datetime()` avec `errors='coerce'`
   - `pd.to_numeric()` pour les conversions
   - `str.replace()` et regex pour le texte

5. **Documentation** :
   - Toujours logger chaque opération
   - Justifier les choix
   - Calculer le taux de rétention

---

## ➡️ Prochain chapitre

**Chapitre 6 : Structuration et transformation** — Vous apprendrez à restructurer vos données (pivot, melt), les combiner (merge, concat), et créer de nouvelles variables (feature engineering).

---

*Module 2 — Pipeline Data | Chapitre 5 sur 11*
