# Chapitre 6 — Structuration et transformation

**Durée estimée : 8-10 heures**

---

## Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

1. **Restructurer** des DataFrames avec pivot et melt selon les besoins d'analyse
2. **Combiner** des sources de données avec merge et concat en choisissant la bonne méthode
3. **Agréger** des données avec groupby et créer des statistiques par groupe
4. **Créer** de nouvelles variables pertinentes (feature engineering) pour l'analyse et le ML

---

## 🎯 Le Hook : Comment Spotify crée 10 000 features pour vous connaître

Spotify ne stocke pas simplement "vous avez écouté cette chanson". À partir de vos données d'écoute brutes, ils créent plus de **10 000 features** par utilisateur :

- Nombre d'écoutes par genre, par heure, par jour
- Tempo moyen préféré
- Diversité musicale (ratio nouveaux artistes / artistes habituels)
- Patterns saisonniers (musique de Noël en décembre ?)

Ces features transformées alimentent ensuite l'algorithme de recommandation.

La donnée brute ("user_123 a écouté track_456 à 14h32") devient une **représentation riche** de vos goûts.

C'est le pouvoir de la **transformation des données**.

> 💭 **Question Socratique #1** : Si Spotify stocke 10 000 features par utilisateur pour 500 millions d'utilisateurs, cela représente des milliards de données. Est-ce vraiment nécessaire de tout stocker, ou pourrait-on recalculer à la demande ?

---

## 6.1 Restructuration des DataFrames

### Wide vs Long : deux visions des mêmes données

```
FORMAT WIDE (large)                    FORMAT LONG (long)
┌────────┬─────┬─────┬─────┐          ┌────────┬───────┬────────┐
│ Client │ Jan │ Fev │ Mar │          │ Client │ Mois  │ Ventes │
├────────┼─────┼─────┼─────┤          ├────────┼───────┼────────┤
│ Alice  │ 100 │ 150 │ 120 │    ↔     │ Alice  │ Jan   │ 100    │
│ Bob    │ 200 │ 180 │ 220 │          │ Alice  │ Fev   │ 150    │
└────────┴─────┴─────┴─────┘          │ Alice  │ Mar   │ 120    │
                                      │ Bob    │ Jan   │ 200    │
1 ligne par client                    │ Bob    │ Fev   │ 180    │
Colonnes = périodes                   │ Bob    │ Mar   │ 220    │
                                      └────────┴───────┴────────┘
                                      1 ligne par observation
```

### Quand utiliser quel format ?

| Format | Avantages | Cas d'usage |
|--------|-----------|-------------|
| **Wide** | Lisible, compact | Tableaux de reporting, Excel |
| **Long** | Flexible, analyses faciles | Visualisation, ML, base de données |

---

### Pivot : Long → Wide

```python
# Données en format long
df_long = pd.DataFrame({
    'client': ['Alice', 'Alice', 'Alice', 'Bob', 'Bob', 'Bob'],
    'mois': ['Jan', 'Fev', 'Mar', 'Jan', 'Fev', 'Mar'],
    'ventes': [100, 150, 120, 200, 180, 220]
})

# Pivot vers format wide
df_wide = df_long.pivot(
    index='client',     # Devient les lignes
    columns='mois',     # Devient les colonnes
    values='ventes'     # Les valeurs à répartir
)
print(df_wide)
```

**Résultat :**
```
mois    Fev  Jan  Mar
client
Alice   150  100  120
Bob     180  200  220
```

---

### Pivot Table : avec agrégation

Quand il y a plusieurs valeurs par combinaison, utilisez `pivot_table` :

```python
df = pd.DataFrame({
    'client': ['Alice', 'Alice', 'Alice', 'Bob', 'Bob'],
    'categorie': ['A', 'A', 'B', 'A', 'B'],
    'montant': [100, 150, 200, 300, 250]
})

# Pivot avec somme des montants
df_pivot = pd.pivot_table(
    df,
    index='client',
    columns='categorie',
    values='montant',
    aggfunc='sum',           # Fonction d'agrégation
    fill_value=0             # Remplacer NaN par 0
)
print(df_pivot)
```

---

### ✍️ Exercice 6.1 : Pivot de ventes (10 min)

```python
import pandas as pd

# Ventes par région et trimestre
df = pd.DataFrame({
    'region': ['Nord', 'Nord', 'Nord', 'Nord', 'Sud', 'Sud', 'Sud', 'Sud'],
    'trimestre': ['Q1', 'Q2', 'Q3', 'Q4', 'Q1', 'Q2', 'Q3', 'Q4'],
    'ventes': [1000, 1200, 1100, 1500, 800, 900, 850, 1000]
})

# 1. Créez un tableau pivot : régions en lignes, trimestres en colonnes
df_pivot = df.pivot(
    index='_____',
    columns='_____',
    values='_____'
)
print("Pivot simple :")
print(df_pivot)

# 2. Ajoutez une colonne 'Total' avec la somme des trimestres
df_pivot['Total'] = df_pivot._____
print("\nAvec total :")
print(df_pivot)

# 3. Quelle région a le meilleur Q4 ?
# Réponse : _____
```

---

### Melt : Wide → Long

`melt` est l'inverse de `pivot` :

```python
# Données en format wide
df_wide = pd.DataFrame({
    'client': ['Alice', 'Bob'],
    'Jan': [100, 200],
    'Fev': [150, 180],
    'Mar': [120, 220]
})

# Melt vers format long
df_long = pd.melt(
    df_wide,
    id_vars=['client'],           # Colonnes à garder fixes
    value_vars=['Jan', 'Fev', 'Mar'],  # Colonnes à "fondre"
    var_name='mois',              # Nom de la nouvelle colonne de variables
    value_name='ventes'           # Nom de la nouvelle colonne de valeurs
)
print(df_long)
```

**Résultat :**
```
   client mois  ventes
0   Alice  Jan     100
1     Bob  Jan     200
2   Alice  Fev     150
3     Bob  Fev     180
4   Alice  Mar     120
5     Bob  Mar     220
```

---

### ✍️ Exercice 6.2 : Melt pour visualisation (10 min)

```python
import pandas as pd

# Données de performance au format wide
df_wide = pd.DataFrame({
    'employe': ['Alice', 'Bob', 'Charlie'],
    'score_2022': [85, 78, 92],
    'score_2023': [88, 82, 90],
    'score_2024': [91, 85, 95]
})

print("Format wide :")
print(df_wide)

# Transformez en format long pour pouvoir créer un graphique de l'évolution
df_long = pd.melt(
    df_wide,
    id_vars=['_____'],
    value_vars=['_____', '_____', '_____'],
    var_name='_____',
    value_name='_____'
)

# Nettoyez la colonne année pour extraire uniquement l'année
df_long['annee'] = df_long['annee'].str.replace('score_', '').astype(int)

print("\nFormat long (prêt pour visualisation) :")
print(df_long)

# Maintenant vous pouvez facilement faire :
# sns.lineplot(data=df_long, x='annee', y='score', hue='employe')
```

---

> 💭 **Question Socratique #2** : Pourquoi les bibliothèques de visualisation comme Seaborn préfèrent-elles généralement le format long ? Quel avantage cela apporte-t-il pour créer des graphiques ?

---

## 6.2 Combinaison de données

### Merge : joindre sur une clé

```
┌──────────────┐     ┌──────────────┐
│   clients    │     │   commandes  │
├──────────────┤     ├──────────────┤
│ client_id    │ ←─→ │ client_id    │
│ nom          │     │ produit      │
│ ville        │     │ montant      │
└──────────────┘     └──────────────┘
```

```python
clients = pd.DataFrame({
    'client_id': [1, 2, 3],
    'nom': ['Alice', 'Bob', 'Charlie'],
    'ville': ['Paris', 'Lyon', 'Marseille']
})

commandes = pd.DataFrame({
    'commande_id': [101, 102, 103, 104],
    'client_id': [1, 1, 2, 4],  # Note: client 4 n'existe pas dans clients
    'montant': [100, 150, 200, 50]
})

# Inner join (intersection)
df_inner = pd.merge(clients, commandes, on='client_id', how='inner')

# Left join (tous les clients)
df_left = pd.merge(clients, commandes, on='client_id', how='left')

# Right join (toutes les commandes)
df_right = pd.merge(clients, commandes, on='client_id', how='right')

# Outer join (union)
df_outer = pd.merge(clients, commandes, on='client_id', how='outer')
```

---

### Types de jointures visualisés

```
       INNER                LEFT                RIGHT               OUTER
    ┌─────────┐          ┌─────────┐          ┌─────────┐          ┌─────────┐
    │    A    │          │    A    │          │         │          │    A    │
    │  ┌───┐  │          │  ┌───┐  │          │  ┌───┐  │          │  ┌───┐  │
    │  │ X │  │          │  │ X │  │          │  │ X │  │          │  │ X │  │
    │  └───┘  │          │  └───┘  │          │  └───┘  │          │  └───┘  │
    │    B    │          │         │          │    B    │          │    B    │
    └─────────┘          └─────────┘          └─────────┘          └─────────┘

   Seulement ce        Tout A +            Tout B +             Tout A + Tout B
   qui matche          matchs de B         matchs de A
```

---

### Cas courants de merge

```python
# Clés différentes dans les deux tables
df = pd.merge(
    clients,
    commandes,
    left_on='id_client',   # Nom dans clients
    right_on='customer_id',  # Nom dans commandes
    how='left'
)

# Merge sur plusieurs colonnes
df = pd.merge(
    ventes,
    objectifs,
    on=['annee', 'region'],
    how='left'
)

# Suffixes pour colonnes en conflit
df = pd.merge(
    df_2023,
    df_2024,
    on='produit',
    suffixes=('_2023', '_2024')
)
```

---

### ✍️ Exercice 6.3 : Jointures multiples (15 min)

```python
import pandas as pd

# Trois tables à joindre
clients = pd.DataFrame({
    'client_id': [1, 2, 3, 4],
    'nom': ['Alice', 'Bob', 'Charlie', 'David'],
    'segment': ['Premium', 'Standard', 'Premium', 'Standard']
})

commandes = pd.DataFrame({
    'commande_id': [101, 102, 103, 104, 105],
    'client_id': [1, 1, 2, 5, 3],  # client 5 n'existe pas
    'produit_id': [10, 20, 10, 30, 20],
    'quantite': [2, 1, 5, 3, 2]
})

produits = pd.DataFrame({
    'produit_id': [10, 20, 30],
    'nom_produit': ['Widget A', 'Widget B', 'Widget C'],
    'prix': [50, 75, 100]
})

# Étape 1 : Joindre commandes et clients (left join sur commandes)
df = pd.merge(commandes, clients, on='_____', how='_____')

# Étape 2 : Joindre avec produits
df = pd.merge(df, produits, on='_____', how='_____')

# Étape 3 : Calculer le montant total par commande
df['montant'] = df['_____'] * df['_____']

# Étape 4 : Afficher les commandes du segment Premium
print("Commandes Premium :")
print(df[df['_____'] == 'Premium'])

# Question : La commande 104 (client_id=5) apparaît-elle dans le résultat ?
# Pourquoi ?
```

---

### 🤖 IA : Générer du code de merge complexe

**Prompt efficace :**

```
J'ai 4 tables :
- clients (client_id, nom, region)
- commandes (commande_id, client_id, date, statut)
- lignes_commande (ligne_id, commande_id, produit_id, quantite)
- produits (produit_id, nom, categorie, prix)

Génère le code pandas pour créer un DataFrame avec :
- Une ligne par combinaison client/produit
- Les colonnes : nom_client, region, nom_produit, categorie, total_quantite, total_montant
- Uniquement les commandes avec statut='livré'
- Trié par total_montant décroissant
```

---

### Concat : empiler des DataFrames

```python
# Empiler verticalement (ajout de lignes)
df_2024 = pd.concat([df_janvier, df_fevrier, df_mars], axis=0)

# Empiler horizontalement (ajout de colonnes)
df_complet = pd.concat([df_infos, df_scores, df_adresses], axis=1)

# Avec réinitialisation de l'index
df_2024 = pd.concat([df_q1, df_q2, df_q3, df_q4], ignore_index=True)

# Avec identification de l'origine
df_all = pd.concat(
    [df_paris, df_lyon, df_marseille],
    keys=['Paris', 'Lyon', 'Marseille']
)
```

---

### Merge vs Concat : quelle différence ?

| Critère | Merge | Concat |
|---------|-------|--------|
| **Utilisation** | Joindre sur une clé | Empiler sans condition |
| **Colonnes** | Peuvent être différentes | Doivent correspondre (axis=0) |
| **Analogie SQL** | JOIN | UNION |
| **Cas typique** | Clients + Commandes | Janvier + Février + Mars |

---

### ✍️ Exercice 6.4 : Concat de fichiers mensuels (10 min)

```python
import pandas as pd
import numpy as np

# Simuler des fichiers mensuels
def creer_donnees_mois(mois, n=100):
    return pd.DataFrame({
        'date': pd.date_range(f'2024-{mois:02d}-01', periods=n, freq='D')[:n],
        'ventes': np.random.randint(100, 500, n),
        'region': np.random.choice(['Nord', 'Sud', 'Est', 'Ouest'], n)
    })

df_jan = creer_donnees_mois(1)
df_fev = creer_donnees_mois(2)
df_mar = creer_donnees_mois(3)

print(f"Janvier : {len(df_jan)} lignes")
print(f"Février : {len(df_fev)} lignes")
print(f"Mars : {len(df_mar)} lignes")

# 1. Concaténer les trois mois
df_q1 = pd.concat([_____, _____, _____], ignore_index=True)
print(f"\nQ1 total : {len(df_q1)} lignes")

# 2. Ajouter une colonne pour identifier le mois d'origine
df_jan['mois'] = 'Janvier'
df_fev['mois'] = 'Février'
df_mar['mois'] = 'Mars'
df_q1_tagged = pd.concat([df_jan, df_fev, df_mar], ignore_index=True)

# 3. Calculer les ventes totales par mois
print("\nVentes par mois :")
print(df_q1_tagged.groupby('mois')['ventes'].sum())
```

---

> 💭 **Question Socratique #3** : Vous devez combiner des données clients provenant de deux systèmes différents (CRM et e-commerce). Les deux ont une colonne "client_id" mais les ID ne sont pas les mêmes. Comment aborderiez-vous ce problème ?

---

## 6.3 Agrégations avec GroupBy

### Le paradigme Split-Apply-Combine

```
        DONNÉES ORIGINALES
        ┌──────────────────┐
        │ Region  Ventes   │
        │ Nord    100      │
        │ Sud     150      │
        │ Nord    120      │
        │ Sud     180      │
        │ Nord    90       │
        └──────────────────┘
              │ SPLIT
              ▼
    ┌─────────────┬─────────────┐
    │   Nord      │    Sud      │
    │   100       │    150      │
    │   120       │    180      │
    │   90        │             │
    └─────────────┴─────────────┘
              │ APPLY (sum)
              ▼
    ┌─────────────┬─────────────┐
    │   310       │    330      │
    └─────────────┴─────────────┘
              │ COMBINE
              ▼
        ┌──────────────────┐
        │ Region  Total    │
        │ Nord    310      │
        │ Sud     330      │
        └──────────────────┘
```

---

### Syntaxe de base

```python
# Agrégation simple
df.groupby('region')['ventes'].sum()

# Plusieurs colonnes de groupement
df.groupby(['region', 'annee'])['ventes'].sum()

# Plusieurs fonctions d'agrégation
df.groupby('region')['ventes'].agg(['sum', 'mean', 'count', 'std'])

# Agrégations différentes par colonne
df.groupby('region').agg({
    'ventes': 'sum',
    'clients': 'nunique',
    'montant': ['mean', 'max']
})
```

---

### Fonctions d'agrégation courantes

| Fonction | Description |
|----------|-------------|
| `sum()` | Somme |
| `mean()` | Moyenne |
| `median()` | Médiane |
| `count()` | Nombre de valeurs non-null |
| `size()` | Nombre total de lignes |
| `nunique()` | Nombre de valeurs uniques |
| `min()`, `max()` | Minimum, Maximum |
| `std()`, `var()` | Écart-type, Variance |
| `first()`, `last()` | Première, Dernière valeur |

---

### Agrégations nommées (pandas ≥ 0.25)

```python
# Syntaxe moderne avec noms explicites
df_agg = df.groupby('region').agg(
    total_ventes=('ventes', 'sum'),
    moyenne_ventes=('ventes', 'mean'),
    nb_transactions=('ventes', 'count'),
    nb_clients=('client_id', 'nunique')
).reset_index()
```

---

### ✍️ Exercice 6.5 : Analyse par groupe (15 min)

```python
import pandas as pd
import numpy as np

# Données de ventes
np.random.seed(42)
df = pd.DataFrame({
    'date': pd.date_range('2024-01-01', periods=1000, freq='D'),
    'region': np.random.choice(['Nord', 'Sud', 'Est', 'Ouest'], 1000),
    'categorie': np.random.choice(['Électronique', 'Vêtements', 'Alimentation'], 1000),
    'montant': np.random.randint(10, 500, 1000),
    'client_id': np.random.randint(1, 200, 1000)
})
df['mois'] = df['date'].dt.month

# 1. Ventes totales par région
print("Ventes par région :")
print(df.groupby('_____')['_____']._____())

# 2. Ventes moyennes par catégorie et région
print("\nVentes moyennes par catégorie et région :")
print(df.groupby(['_____', '_____'])['montant']._____().unstack())

# 3. Nombre de clients uniques par région
print("\nClients uniques par région :")
print(df.groupby('region')['_____']._____())

# 4. Top 3 des mois par ventes totales
print("\nTop 3 mois :")
ventes_mois = df.groupby('mois')['montant'].sum().sort_values(ascending=False)
print(ventes_mois.head(3))

# 5. Rapport complet par région
rapport = df.groupby('region').agg(
    total_ventes=('montant', '_____'),
    moyenne_ventes=('montant', '_____'),
    nb_transactions=('montant', '_____'),
    nb_clients=('client_id', '_____'),
    panier_moyen=('montant', 'mean')
).round(2)
print("\nRapport complet :")
print(rapport)
```

---

### Fonctions personnalisées

```python
# Fonction lambda
df.groupby('region')['ventes'].apply(lambda x: x.max() - x.min())

# Fonction définie
def coefficient_variation(x):
    """Écart-type / moyenne (mesure de dispersion relative)"""
    return x.std() / x.mean() if x.mean() != 0 else 0

df.groupby('region')['ventes'].apply(coefficient_variation)

# Plusieurs valeurs de retour
def stats_completes(x):
    return pd.Series({
        'min': x.min(),
        'max': x.max(),
        'range': x.max() - x.min(),
        'cv': x.std() / x.mean() if x.mean() != 0 else 0
    })

df.groupby('region')['ventes'].apply(stats_completes)
```

---

## 6.4 Feature Engineering basique

### Qu'est-ce que le Feature Engineering ?

Le **feature engineering** consiste à créer de nouvelles variables (features) à partir des données existantes pour améliorer l'analyse ou les performances des modèles ML.

```
Données brutes                     Features créées
┌─────────────────┐               ┌─────────────────────────┐
│ date_naissance  │   →           │ age                     │
│                 │               │ generation (Baby Boom...)│
│ date_achat      │   →           │ jour_semaine            │
│                 │               │ est_weekend             │
│                 │               │ mois                    │
│ montant         │   →           │ montant_log             │
│                 │               │ est_gros_achat          │
└─────────────────┘               └─────────────────────────┘
```

---

### Variables dérivées (calculs)

```python
# Calculs simples
df['marge'] = df['prix_vente'] - df['cout']
df['taux_marge'] = df['marge'] / df['prix_vente'] * 100

# Ratios
df['panier_moyen'] = df['ca_total'] / df['nb_commandes']
df['taux_conversion'] = df['achats'] / df['visites'] * 100

# Combinaisons
df['score_total'] = df['score_a'] * 0.4 + df['score_b'] * 0.6
```

---

### Variables temporelles

```python
# À partir d'une date
df['annee'] = df['date'].dt.year
df['mois'] = df['date'].dt.month
df['jour'] = df['date'].dt.day
df['jour_semaine'] = df['date'].dt.dayofweek  # 0=lundi
df['nom_jour'] = df['date'].dt.day_name()
df['semaine'] = df['date'].dt.isocalendar().week
df['trimestre'] = df['date'].dt.quarter

# Variables binaires temporelles
df['est_weekend'] = df['jour_semaine'].isin([5, 6]).astype(int)
df['est_debut_mois'] = (df['jour'] <= 7).astype(int)
df['est_fin_annee'] = df['mois'].isin([11, 12]).astype(int)

# Calcul d'âge
from datetime import datetime
df['age'] = (datetime.now() - df['date_naissance']).dt.days // 365

# Ancienneté en mois
df['anciennete_mois'] = (datetime.now() - df['date_inscription']).dt.days // 30
```

---

### ✍️ Exercice 6.6 : Feature engineering temporel (15 min)

```python
import pandas as pd
import numpy as np

# Données de commandes
df = pd.DataFrame({
    'commande_id': range(1, 1001),
    'date_commande': pd.date_range('2024-01-01', periods=1000, freq='4H'),
    'client_id': np.random.randint(1, 100, 1000),
    'montant': np.random.randint(20, 500, 1000)
})

# 1. Extraire les composantes temporelles
df['annee'] = df['date_commande'].dt._____
df['mois'] = df['date_commande'].dt._____
df['jour_semaine'] = df['date_commande'].dt._____
df['heure'] = df['date_commande'].dt._____

# 2. Créer des features binaires
df['est_weekend'] = df['jour_semaine'].isin([5, 6]).astype(int)
df['est_soiree'] = (df['heure'] >= 18).astype(int)
df['est_nuit'] = ((df['heure'] >= 22) | (df['heure'] <= 6)).astype(int)

# 3. Analyser les patterns
print("Commandes par jour de semaine :")
print(df.groupby('jour_semaine')['commande_id'].count())

print("\nMontant moyen : weekend vs semaine")
print(df.groupby('est_weekend')['montant'].mean())

print("\nCommandes par tranche horaire (matin/après-midi/soir/nuit) :")
df['tranche'] = pd.cut(df['heure'], bins=[0, 6, 12, 18, 24], labels=['Nuit', 'Matin', 'Après-midi', 'Soir'])
print(df.groupby('tranche')['commande_id'].count())
```

---

### Binning / Discrétisation

Transformer une variable continue en catégories :

```python
# Binning avec pd.cut (intervalles égaux)
df['tranche_age'] = pd.cut(
    df['age'],
    bins=[0, 25, 35, 50, 65, 100],
    labels=['18-25', '26-35', '36-50', '51-65', '65+']
)

# Binning avec pd.qcut (quantiles)
df['quartile_salaire'] = pd.qcut(
    df['salaire'],
    q=4,
    labels=['Q1', 'Q2', 'Q3', 'Q4']
)

# Binning personnalisé
def categoriser_montant(montant):
    if montant < 50:
        return 'Petit'
    elif montant < 200:
        return 'Moyen'
    else:
        return 'Gros'

df['taille_commande'] = df['montant'].apply(categoriser_montant)
```

---

### Encoding catégoriel

```python
# One-Hot Encoding (créer des colonnes binaires)
df_encoded = pd.get_dummies(df, columns=['region', 'categorie'], prefix=['reg', 'cat'])

# Label Encoding (convertir en nombres)
df['region_code'] = df['region'].astype('category').cat.codes

# Mapping manuel
mapping_region = {'Nord': 0, 'Sud': 1, 'Est': 2, 'Ouest': 3}
df['region_code'] = df['region'].map(mapping_region)
```

**Quand utiliser quoi ?**

| Méthode | Quand l'utiliser |
|---------|------------------|
| One-Hot | Peu de catégories (< 10), pas d'ordre |
| Label | Catégories ordonnées (petit < moyen < grand) |
| Target | Modèles qui gèrent bien (arbres) |

---

### ✍️ Exercice 6.7 : Pipeline de feature engineering (20 min)

```python
import pandas as pd
import numpy as np

# Données clients
np.random.seed(42)
df = pd.DataFrame({
    'client_id': range(1, 501),
    'date_naissance': pd.date_range('1960-01-01', periods=500, freq='20D'),
    'date_inscription': pd.date_range('2020-01-01', periods=500, freq='2D'),
    'region': np.random.choice(['Nord', 'Sud', 'Est', 'Ouest'], 500),
    'nb_commandes': np.random.randint(0, 50, 500),
    'montant_total': np.random.randint(0, 5000, 500)
})

print("Données initiales :")
print(df.head())

# 1. Calculer l'âge
from datetime import datetime
df['age'] = (datetime.now() - df['date_naissance']).dt.days // 365

# 2. Créer des tranches d'âge
df['tranche_age'] = pd.cut(df['age'], bins=[0, 30, 45, 60, 100], labels=['_____', '_____', '_____', '_____'])

# 3. Calculer l'ancienneté en mois
df['anciennete_mois'] = (datetime.now() - df['date_inscription']).dt.days // 30

# 4. Calculer le panier moyen (attention à la division par 0)
df['panier_moyen'] = np.where(df['nb_commandes'] > 0, df['montant_total'] / df['nb_commandes'], 0)

# 5. Créer un flag "client actif" (au moins 5 commandes)
df['est_actif'] = (df['nb_commandes'] >= 5).astype(int)

# 6. Créer des quartiles de montant total
df['segment_valeur'] = pd.qcut(df['montant_total'], q=4, labels=['Bronze', 'Silver', 'Gold', 'Platinum'])

# 7. One-hot encoding de la région
df_final = pd.get_dummies(df, columns=['region'], prefix='reg')

print("\nDataset final :")
print(df_final.head())
print(f"\nNombre de features : {len(df_final.columns)}")
```

---

## 6.5 Création du dataset final

### Workflow complet

```python
def preparer_dataset(df_raw):
    """Pipeline complet de préparation des données."""
    df = df_raw.copy()

    # 1. Nettoyage
    df = df.drop_duplicates()
    df = df.dropna(subset=['client_id', 'date'])

    # 2. Conversion de types
    df['date'] = pd.to_datetime(df['date'])
    df['montant'] = pd.to_numeric(df['montant'], errors='coerce')

    # 3. Feature engineering
    df['annee'] = df['date'].dt.year
    df['mois'] = df['date'].dt.month
    df['est_weekend'] = df['date'].dt.dayofweek.isin([5, 6]).astype(int)

    # 4. Agrégations
    df_client = df.groupby('client_id').agg(
        nb_commandes=('commande_id', 'count'),
        montant_total=('montant', 'sum'),
        montant_moyen=('montant', 'mean'),
        derniere_commande=('date', 'max')
    ).reset_index()

    # 5. Encoding
    df_client = pd.get_dummies(df_client, columns=['segment'])

    return df_client

# Utilisation
df_final = preparer_dataset(df_raw)
```

---

### Validation du schéma

```python
def valider_schema(df, schema_attendu):
    """Vérifie que le DataFrame correspond au schéma attendu."""
    erreurs = []

    for col, dtype in schema_attendu.items():
        if col not in df.columns:
            erreurs.append(f"Colonne manquante : {col}")
        elif not df[col].dtype == dtype:
            erreurs.append(f"Type incorrect pour {col}: {df[col].dtype} (attendu: {dtype})")

    if erreurs:
        print("❌ Erreurs de schéma :")
        for e in erreurs:
            print(f"  - {e}")
        return False
    else:
        print("✅ Schéma validé")
        return True

# Schéma attendu
schema = {
    'client_id': 'int64',
    'nb_commandes': 'int64',
    'montant_total': 'float64',
    'montant_moyen': 'float64'
}

valider_schema(df_final, schema)
```

---

### Export pour analyse et ML

```python
# Export CSV (pour compatibilité)
df_final.to_csv('dataset_clients.csv', index=False)

# Export Parquet (recommandé pour performance)
df_final.to_parquet('dataset_clients.parquet', index=False)

# Export avec métadonnées
metadata = {
    'date_creation': str(datetime.now()),
    'nb_lignes': len(df_final),
    'colonnes': list(df_final.columns),
    'source': 'pipeline_v1.0'
}

import json
with open('dataset_metadata.json', 'w') as f:
    json.dump(metadata, f, indent=2)
```

---

## 🧠 Réflexion métacognitive

### Auto-évaluation

| Compétence | 1 | 2 | 3 | 4 | 5 |
|------------|---|---|---|---|---|
| Je sais utiliser pivot et melt | ○ | ○ | ○ | ○ | ○ |
| Je maîtrise les différents types de merge | ○ | ○ | ○ | ○ | ○ |
| Je peux créer des agrégations avec groupby | ○ | ○ | ○ | ○ | ○ |
| Je sais créer des features temporelles | ○ | ○ | ○ | ○ | ○ |
| Je peux encoder des variables catégorielles | ○ | ○ | ○ | ○ | ○ |

### Questions de réflexion

1. **Quel type de merge** utilisez-vous le plus souvent dans votre pratique ?

2. **Quelles features** créeriez-vous à partir d'une date de naissance ?

3. **Comment décideriez-vous** entre one-hot encoding et label encoding ?

---

## 📚 Résumé du chapitre

### Points clés à retenir

1. **Restructuration** :
   - `pivot` : Long → Wide (pour reporting)
   - `melt` : Wide → Long (pour visualisation)

2. **Combinaison** :
   - `merge` : Jointure sur clé (inner, left, right, outer)
   - `concat` : Empilement (vertical ou horizontal)

3. **Agrégation** :
   - `groupby` + `agg` pour statistiques par groupe
   - Fonctions : sum, mean, count, nunique...

4. **Feature Engineering** :
   - Variables calculées (ratios, différences)
   - Variables temporelles (année, mois, jour_semaine)
   - Binning (cut, qcut)
   - Encoding (get_dummies, cat.codes)

---

## ➡️ Prochain chapitre

**Chapitre 7 : EDA analytique** — Vous apprendrez à comprendre vos données propres, identifier des patterns et formuler des hypothèses.

---

*Module 2 — Pipeline Data | Chapitre 6 sur 11*
