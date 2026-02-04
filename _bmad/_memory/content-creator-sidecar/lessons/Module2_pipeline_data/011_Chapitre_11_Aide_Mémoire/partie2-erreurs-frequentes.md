# Chapitre 11 : Conclusion et aide-mémoire

**Durée estimée : 2-3h**

---

## Objectifs de ce chapitre

1. **Synthétiser** les compétences acquises tout au long du module
2. **Identifier** les erreurs fréquentes à éviter en situation professionnelle
3. **Connecter** vos apprentissages avec la suite du parcours Data & IA

---

## 2. Erreurs fréquentes à éviter

### 2.1 Le Top 11 des pièges en Data Preparation

Ces erreurs sont commises par des professionnels expérimentés. Maintenant que vous les connaissez, vous pouvez les éviter.

> ⚠️ **Erreur #11 (DATA LEAKAGE)** est particulièrement critique — elle peut invalider tout votre travail ML si vous ne la comprenez pas.

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

# ✅ Bon (Module 2 - analytique)
print(f"Missing: {df['prix'].isna().sum()}")
df['prix_missing'] = df['prix'].isna()  # Flag pour traçabilité
df_clean = df.dropna(subset=['prix'])   # Suppression explicite
```

> ⚠️ **Note importante** : L'imputation par moyenne/médiane (`fillna(df['col'].median())`) est **INTERDITE en Module 2** car elle cause du **data leakage** si les données sont ensuite utilisées pour le ML. Voir Erreur #11.

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

---

#### ❌ Erreur #11 : Imputer les données avant le split train/test (DATA LEAKAGE)

**Le piège** : Utiliser `fillna(df['col'].median())` ou `fillna(df['col'].mean())` sur tout le dataset dans le Module 2, puis utiliser ces données pour entraîner un modèle ML dans le Module 3.

**Conséquence** : **Data leakage** — les statistiques du test set "fuient" dans le train set. Vos métriques de performance sont faussement optimistes. En production, le modèle performe mal.

**Pourquoi c'est grave** :
```
         TOUT LE DATASET
    ┌──────────────────────────┐
    │  median = 100            │  ← Calculée sur TOUT (y compris futur test)
    │  ┌─────────┬─────────┐   │
    │  │  TRAIN  │  TEST   │   │
    │  │  (80%)  │  (20%)  │   │
    │  │ fillna  │ fillna  │   │  ← Les deux utilisent la même médiane
    │  │  =100   │  =100   │   │  ← ⚠️ Info du TEST dans le TRAIN !
    │  └─────────┴─────────┘   │
    └──────────────────────────┘
```

**Solution Module 2** : NE PAS imputer. Utiliser suppression ou flags.

```python
# ❌ INTERDIT en Module 2 (cause data leakage)
df['prix'] = df['prix'].fillna(df['prix'].median())

# ✅ BON en Module 2
df['prix_missing'] = df['prix'].isna()  # Flag
df_clean = df.dropna(subset=['prix'])   # Ou suppression
```

**Solution Module 3** : Imputer APRÈS le split, avec fit() sur train uniquement.

```python
# ✅ BON en Module 3 (avec sklearn)
from sklearn.impute import SimpleImputer
from sklearn.model_selection import train_test_split

X_train, X_test, y_train, y_test = train_test_split(X, y)

imputer = SimpleImputer(strategy='median')
imputer.fit(X_train)           # ← Fit sur TRAIN uniquement
X_train = imputer.transform(X_train)
X_test = imputer.transform(X_test)  # ← Transform avec stats du train
```

**Règle d'or** : Dans Module 2, on **diagnostique** et **documente**. L'imputation statistique appartient au **Module 3**.

---

### ✍️ Exercice 2 : Identifiez vos pièges personnels

Parmi les 11 erreurs ci-dessus, lesquelles avez-vous déjà commises ou êtes-vous le plus susceptible de commettre ?

1. Mon erreur la plus probable : _______________________
2. Comment je vais l'éviter : _______________________
