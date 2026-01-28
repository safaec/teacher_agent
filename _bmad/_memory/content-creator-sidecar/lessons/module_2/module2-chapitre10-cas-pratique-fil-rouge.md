# Chapitre 10 : Cas pratique fil rouge

**Durée estimée : 12-15h**

---

## Objectifs d'apprentissage

À la fin de ce chapitre, vous aurez :

1. **Appliqué** l'intégralité de la pipeline data sur un cas réel
2. **Produit** un dataset nettoyé et documenté, uploadé sur S3
3. **Présenté** des insights business basés sur votre analyse

---

## 🎯 L'accroche : Votre premier projet data professionnel

Vous venez d'être recruté(e) comme **Data Analyst Junior** chez **TechShop**, un e-commerçant d'électronique en pleine croissance.

Votre manager vous accueille avec ces mots :

> "Bienvenue ! On a un problème : nos données sont partout. Le marketing a ses fichiers Excel, les ventes exportent du CRM, et on a des données historiques sur S3 que personne n'a jamais nettoyées. Le CEO veut un rapport sur nos performances Q4, mais personne ne peut répondre à des questions simples comme 'Quel est notre produit le plus rentable ?' ou 'Quels clients risquent de partir ?'
>
> Ta mission : construire une pipeline de données propre. Tu as 2 semaines."

Selon Gartner, les entreprises perdent en moyenne **$15 millions par an** à cause de données de mauvaise qualité. Chez TechShop, ce problème se traduit par des campagnes marketing mal ciblées, des stocks mal gérés, et des décisions basées sur des intuitions plutôt que des faits.

**C'est exactement le type de situation que vous saurez résoudre après ce module.**

*(Source : [Data Ladder - Retail Data Quality Issues](https://dataladder.com/common-data-quality-issues-in-the-retail-industry-and-how-to-fix-them/))*

---

## 1. Contexte et objectifs métier

### 1.1 Présentation de TechShop

**TechShop** est un e-commerçant français spécialisé dans l'électronique grand public :
- Smartphones, laptops, accessoires, audio
- 45,000 clients actifs
- 150,000 commandes/an
- CA : 12M€

**Problème business** : L'équipe dirigeante prépare le budget 2025 et a besoin de réponses data-driven.

### 1.2 Les questions business à résoudre

| # | Question business | Impact |
|---|-------------------|--------|
| 1 | Quelles catégories de produits génèrent le plus de revenus et de marge ? | Stratégie produit |
| 2 | Existe-t-il des patterns saisonniers dans les ventes ? | Gestion des stocks |
| 3 | Comment se répartissent nos clients par segment (valeur, fréquence) ? | Marketing ciblé |
| 4 | Quels sont les indicateurs de risque de churn (perte de clients) ? | Rétention |
| 5 | Quelle est la qualité actuelle de nos données ? | Gouvernance |

### 1.3 Les livrables attendus

À la fin de ce projet, vous devrez produire :

- [ ] **Dataset final** : `techshop_analytics.parquet` uploadé sur S3
- [ ] **Data dictionary** : Documentation complète du dataset
- [ ] **Notebook** : Code reproductible avec commentaires
- [ ] **Rapport EDA** : 5-7 visualisations clés avec insights
- [ ] **Présentation** : Réponses aux 5 questions business

---

## 2. Extraction multi-sources

### 2.1 Les sources de données disponibles

TechShop dispose de 4 sources de données :

```
📁 Sources TechShop
├── 📄 ventes_q4_2024.csv          # Transactions (CRM export)
├── 📊 catalogue_produits.xlsx     # Catalogue (Marketing)
├── 🌐 API reviews                 # Avis clients (simulée)
└── ☁️ S3: historique_clients     # Données historiques
```

### 2.2 Source 1 : Transactions (CSV)

Le CRM exporte les ventes au format CSV. **Attention : ce fichier contient des problèmes de qualité volontaires** — comme dans la vraie vie.

```python
# Créez ce fichier ou téléchargez-le depuis votre formateur
# Pour l'exercice, nous allons le simuler

import pandas as pd
import numpy as np
from datetime import datetime, timedelta

np.random.seed(42)

# Génération de données réalistes avec problèmes de qualité
n_transactions = 5000

# Dates Q4 2024
dates = pd.date_range('2024-10-01', '2024-12-31', periods=n_transactions)

# Données de base
ventes_raw = pd.DataFrame({
    'transaction_id': range(1, n_transactions + 1),
    'date': np.random.choice(dates, n_transactions),
    'client_id': np.random.randint(1000, 5000, n_transactions),
    'produit_id': np.random.choice(['PHONE-001', 'PHONE-002', 'LAPTOP-001', 'LAPTOP-002',
                                     'AUDIO-001', 'AUDIO-002', 'ACC-001', 'ACC-002'], n_transactions),
    'quantite': np.random.choice([1, 1, 1, 2, 2, 3], n_transactions),
    'prix_unitaire': np.random.choice([29.99, 49.99, 199.99, 599.99, 899.99, 1299.99], n_transactions),
    'canal': np.random.choice(['web', 'mobile', 'Web', 'MOBILE', 'app'], n_transactions),  # Inconsistances !
    'region': np.random.choice(['IDF', 'PACA', 'Ile-de-France', 'paca', 'ARA', None], n_transactions),  # Doublons + NULL
})

# Ajout de problèmes de qualité réalistes

# 1. Doublons (3% des transactions)
doublons = ventes_raw.sample(n=150, random_state=42)
ventes_raw = pd.concat([ventes_raw, doublons], ignore_index=True)

# 2. Valeurs manquantes (5% client_id, 8% region)
mask_client = np.random.random(len(ventes_raw)) < 0.05
ventes_raw.loc[mask_client, 'client_id'] = np.nan

mask_region = np.random.random(len(ventes_raw)) < 0.08
ventes_raw.loc[mask_region, 'region'] = None

# 3. Outliers (quelques transactions suspectes)
ventes_raw.loc[np.random.choice(ventes_raw.index, 5), 'quantite'] = 999
ventes_raw.loc[np.random.choice(ventes_raw.index, 3), 'prix_unitaire'] = -50.0

# 4. Formats de date mixtes (certaines lignes)
ventes_raw['date'] = ventes_raw['date'].astype(str)
ventes_raw.loc[100:110, 'date'] = ventes_raw.loc[100:110, 'date'].str.replace('-', '/')

# Sauvegarde
ventes_raw.to_csv('ventes_q4_2024.csv', index=False)
print(f"✅ Fichier ventes_q4_2024.csv créé : {len(ventes_raw)} lignes")
```

---

### ✍️ Exercice 2.1 : Chargez les transactions

```python
# TODO: Chargez le fichier CSV et faites une première inspection

import pandas as pd

# 1. Chargez le fichier
df_ventes = pd.read_csv('_______________')

# 2. Premiers réflexes
print("Shape:", _______________)
print("\nTypes:\n", _______________)
print("\nAperçu:\n", _______________)
print("\nValeurs manquantes:\n", _______________)
```

**Questions à vous poser** :
- Combien de lignes et colonnes ?
- Y a-t-il des valeurs manquantes ? Où ?
- Les types de données sont-ils corrects ?

---

### 2.3 Source 2 : Catalogue produits (Excel)

Le marketing maintient le catalogue dans Excel avec plusieurs feuilles.

```python
# Génération du catalogue produits
catalogue = pd.DataFrame({
    'produit_id': ['PHONE-001', 'PHONE-002', 'LAPTOP-001', 'LAPTOP-002',
                   'AUDIO-001', 'AUDIO-002', 'ACC-001', 'ACC-002'],
    'nom_produit': ['iPhone 15', 'Samsung S24', 'MacBook Air M3', 'ThinkPad X1',
                    'AirPods Pro', 'Sony WH-1000XM5', 'Coque iPhone', 'Chargeur USB-C'],
    'categorie': ['Smartphones', 'Smartphones', 'Laptops', 'Laptops',
                  'Audio', 'Audio', 'Accessoires', 'Accessoires'],
    'cout_achat': [750, 650, 1100, 950, 180, 250, 5, 12],
    'prix_vente': [999, 899, 1499, 1299, 279, 379, 29.99, 49.99],
    'stock_actuel': [150, 200, 80, 45, 500, 120, 2000, 1500]
})

categories = pd.DataFrame({
    'categorie': ['Smartphones', 'Laptops', 'Audio', 'Accessoires'],
    'responsable': ['Marie D.', 'Jean P.', 'Sophie L.', 'Marc T.'],
    'objectif_ca_q4': [2000000, 1500000, 500000, 200000]
})

# Sauvegarde Excel multi-feuilles
with pd.ExcelWriter('catalogue_produits.xlsx') as writer:
    catalogue.to_excel(writer, sheet_name='Produits', index=False)
    categories.to_excel(writer, sheet_name='Categories', index=False)

print("✅ Fichier catalogue_produits.xlsx créé")
```

---

### ✍️ Exercice 2.2 : Chargez le catalogue Excel

```python
# TODO: Chargez les deux feuilles du fichier Excel

# 1. Chargez la feuille "Produits"
df_produits = pd.read_excel('catalogue_produits.xlsx', sheet_name='_______________')

# 2. Chargez la feuille "Categories"
df_categories = pd.read_excel('catalogue_produits.xlsx', sheet_name='_______________')

# 3. Inspectez
print("Produits:", df_produits.shape)
print(df_produits.head())

print("\nCategories:", df_categories.shape)
print(df_categories.head())
```

---

### 2.4 Source 3 : Avis clients (API simulée)

Dans la vraie vie, vous appelleriez une API. Ici, nous simulons la réponse.

```python
# Simulation de données API (avis clients)
import json

avis_api = []
for produit in ['PHONE-001', 'PHONE-002', 'LAPTOP-001', 'LAPTOP-002',
                'AUDIO-001', 'AUDIO-002', 'ACC-001', 'ACC-002']:
    n_avis = np.random.randint(50, 200)
    for i in range(n_avis):
        avis_api.append({
            'produit_id': produit,
            'note': np.random.choice([1, 2, 3, 4, 4, 5, 5, 5]),  # Distribution réaliste
            'date_avis': str(pd.Timestamp('2024-10-01') + timedelta(days=np.random.randint(0, 90))),
            'verifie': np.random.choice([True, True, True, False])  # 75% vérifiés
        })

# Sauvegarde JSON (comme une réponse API)
with open('avis_clients.json', 'w') as f:
    json.dump(avis_api, f)

print(f"✅ Fichier avis_clients.json créé : {len(avis_api)} avis")
```

---

### ✍️ Exercice 2.3 : Chargez les données JSON

```python
# TODO: Chargez le fichier JSON et convertissez en DataFrame

import json
import pandas as pd

# 1. Chargez le JSON
with open('avis_clients.json', 'r') as f:
    data_avis = json.load(f)

# 2. Convertissez en DataFrame
df_avis = pd.DataFrame(_______________)

# 3. Inspectez
print("Shape:", df_avis.shape)
print(df_avis.head())
print("\nDistribution des notes:\n", df_avis['note'].value_counts().sort_index())
```

---

### 2.5 Source 4 : Données historiques (S3 simulé)

Pour l'exercice, nous simulons la lecture depuis S3 avec un fichier local.

```python
# Simulation de données historiques clients (comme si elles venaient de S3)
clients_historique = pd.DataFrame({
    'client_id': range(1000, 5000),
    'date_inscription': pd.date_range('2020-01-01', periods=4000, freq='D'),
    'email': [f'client{i}@email.com' for i in range(1000, 5000)],
    'segment_initial': np.random.choice(['nouveau', 'standard', 'premium'], 4000),
    'source_acquisition': np.random.choice(['google', 'facebook', 'direct', 'referral'], 4000)
})

# Sauvegarde en Parquet (format cloud)
clients_historique.to_parquet('historique_clients.parquet', index=False)
print("✅ Fichier historique_clients.parquet créé")

# En production, vous liriez depuis S3 :
# df_clients = pd.read_parquet("s3://techshop-datalake/raw/clients/historique_clients.parquet")
```

---

### ✍️ Exercice 2.4 : Chargez le fichier Parquet

```python
# TODO: Chargez le fichier Parquet

df_clients = pd.read_parquet('_______________')

print("Shape:", df_clients.shape)
print(df_clients.dtypes)
print(df_clients.head())
```

---

### 2.6 Récapitulatif des données extraites

Après extraction, vous devriez avoir :

| DataFrame | Source | Lignes | Colonnes | Format |
|-----------|--------|--------|----------|--------|
| df_ventes | CRM | ~5,150 | 8 | CSV |
| df_produits | Marketing | 8 | 6 | Excel |
| df_categories | Marketing | 4 | 3 | Excel |
| df_avis | API | ~1,000 | 4 | JSON |
| df_clients | S3 | 4,000 | 5 | Parquet |

> 🤔 **Question Socratique** : Avant même de regarder les données en détail, quels problèmes anticipez-vous lors de la jointure entre ces sources ? Pensez aux clés de jointure et aux formats.

---

## 3. EDA diagnostique

### 3.1 Objectif : Trouver les problèmes

Maintenant que vous avez extrait les données, il faut diagnostiquer leur qualité **avant** de les analyser.

### 3.2 Diagnostic des transactions

```python
# Script de diagnostic complet pour df_ventes

print("=" * 50)
print("DIAGNOSTIC QUALITÉ : TRANSACTIONS")
print("=" * 50)

# 1. Dimensions
print(f"\n📊 Dimensions: {df_ventes.shape[0]} lignes, {df_ventes.shape[1]} colonnes")

# 2. Types de données
print("\n📋 Types de données:")
print(df_ventes.dtypes)

# 3. Valeurs manquantes
print("\n❓ Valeurs manquantes:")
missing = df_ventes.isnull().sum()
missing_pct = (missing / len(df_ventes) * 100).round(2)
missing_df = pd.DataFrame({'Manquantes': missing, 'Pourcentage': missing_pct})
print(missing_df[missing_df['Manquantes'] > 0])

# 4. Doublons
doublons = df_ventes.duplicated().sum()
print(f"\n🔄 Doublons exacts: {doublons} ({doublons/len(df_ventes)*100:.2f}%)")

# 5. Valeurs uniques (colonnes catégorielles)
print("\n🏷️ Valeurs uniques:")
for col in ['canal', 'region']:
    print(f"  {col}: {df_ventes[col].unique()}")

# 6. Statistiques numériques (outliers potentiels)
print("\n📈 Statistiques numériques:")
print(df_ventes[['quantite', 'prix_unitaire']].describe())

# 7. Valeurs aberrantes
print("\n⚠️ Valeurs suspectes:")
print(f"  Quantités > 100: {(df_ventes['quantite'] > 100).sum()}")
print(f"  Prix négatifs: {(df_ventes['prix_unitaire'] < 0).sum()}")
```

---

### ✍️ Exercice 3.1 : Complétez le diagnostic

Exécutez le script ci-dessus et répondez aux questions :

1. **Combien de doublons avez-vous trouvés ?** ___________
2. **Quel pourcentage de `client_id` est manquant ?** ___________
3. **Quels problèmes voyez-vous dans la colonne `canal` ?** ___________
4. **Quels problèmes voyez-vous dans la colonne `region` ?** ___________
5. **Combien de lignes ont des quantités suspectes (>100) ?** ___________

---

### 3.3 Visualisations de diagnostic

```python
import matplotlib.pyplot as plt
import seaborn as sns

fig, axes = plt.subplots(2, 2, figsize=(12, 10))

# 1. Valeurs manquantes
ax1 = axes[0, 0]
missing_pct = (df_ventes.isnull().sum() / len(df_ventes) * 100)
missing_pct.plot(kind='bar', ax=ax1, color='coral')
ax1.set_title('Pourcentage de valeurs manquantes par colonne')
ax1.set_ylabel('% manquant')
ax1.axhline(y=5, color='red', linestyle='--', label='Seuil 5%')
ax1.legend()

# 2. Distribution des quantités (outliers)
ax2 = axes[0, 1]
df_ventes['quantite'].plot(kind='hist', bins=50, ax=ax2, color='steelblue')
ax2.set_title('Distribution des quantités (attention aux outliers)')
ax2.set_xlabel('Quantité')

# 3. Boxplot prix unitaire
ax3 = axes[1, 0]
df_ventes.boxplot(column='prix_unitaire', ax=ax3)
ax3.set_title('Boxplot prix unitaire')

# 4. Répartition des canaux (problème de standardisation)
ax4 = axes[1, 1]
df_ventes['canal'].value_counts().plot(kind='bar', ax=ax4, color='mediumseagreen')
ax4.set_title('Canaux de vente (noter les doublons)')

plt.tight_layout()
plt.savefig('diagnostic_qualite.png', dpi=150)
plt.show()

print("✅ Visualisations de diagnostic sauvegardées")
```

---

### 3.4 Checklist qualité

Documentez vos findings dans une checklist :

```markdown
# Checklist Qualité - TechShop Ventes Q4

## Complétude
- [ ] client_id : ~5% manquant → Stratégie : imputation ou suppression ?
- [ ] region : ~8% manquant → Stratégie : ?

## Unicité
- [ ] ~150 doublons exacts détectés → À supprimer

## Cohérence
- [ ] canal : 5 valeurs pour 3 canaux (web/Web, mobile/MOBILE/app)
- [ ] region : variations (IDF/Ile-de-France, PACA/paca)

## Exactitude
- [ ] 5 lignes avec quantite = 999 → Outliers ou erreurs ?
- [ ] 3 lignes avec prix_unitaire < 0 → Erreurs certaines

## Fraîcheur
- [ ] Dates : vérifier le format mixte (- et /)
```

> 🤔 **Question Socratique** : Pour les 5 lignes avec quantité = 999, comment décideriez-vous s'il s'agit d'erreurs à corriger ou de vraies commandes en gros ? Quelle information supplémentaire demanderiez-vous au métier ?

---

## 4. Nettoyage

### 4.1 Stratégie de nettoyage

Basé sur le diagnostic, voici le plan :

| Problème | Stratégie | Justification |
|----------|-----------|---------------|
| Doublons | Supprimer | Erreur d'export |
| client_id manquant | Conserver avec flag | Transactions valides |
| region manquant | Imputer "Inconnu" | Analyse possible par région |
| canal inconsistant | Standardiser (lower) | Harmonisation |
| region inconsistant | Mapper vers standard | Harmonisation |
| quantite = 999 | Remplacer par médiane | Erreur probable |
| prix < 0 | Supprimer | Erreur certaine |
| dates format mixte | Convertir avec coercion | Standardisation |

---

### 4.2 Code de nettoyage

```python
# Copie pour préserver l'original
df_ventes_clean = df_ventes.copy()

print("Nettoyage en cours...")
print(f"Avant : {len(df_ventes_clean)} lignes")

# 1. Supprimer les doublons
df_ventes_clean = df_ventes_clean.drop_duplicates()
print(f"Après suppression doublons : {len(df_ventes_clean)} lignes")

# 2. Supprimer les lignes avec prix négatif
df_ventes_clean = df_ventes_clean[df_ventes_clean['prix_unitaire'] >= 0]
print(f"Après suppression prix négatifs : {len(df_ventes_clean)} lignes")

# 3. Traiter les quantités aberrantes
mediane_qte = df_ventes_clean[df_ventes_clean['quantite'] < 100]['quantite'].median()
df_ventes_clean.loc[df_ventes_clean['quantite'] > 100, 'quantite'] = mediane_qte
print(f"Quantités aberrantes remplacées par médiane ({mediane_qte})")

# 4. Standardiser le canal
df_ventes_clean['canal'] = df_ventes_clean['canal'].str.lower()
# Mapper 'app' vers 'mobile'
df_ventes_clean['canal'] = df_ventes_clean['canal'].replace({'app': 'mobile'})
print(f"Canaux standardisés : {df_ventes_clean['canal'].unique()}")

# 5. Standardiser les régions
region_mapping = {
    'IDF': 'Ile-de-France',
    'Ile-de-France': 'Ile-de-France',
    'PACA': 'PACA',
    'paca': 'PACA',
    'ARA': 'Auvergne-Rhône-Alpes',
    None: 'Inconnu'
}
df_ventes_clean['region'] = df_ventes_clean['region'].map(region_mapping)
df_ventes_clean['region'] = df_ventes_clean['region'].fillna('Inconnu')
print(f"Régions standardisées : {df_ventes_clean['region'].unique()}")

# 6. Flag pour client_id manquant
df_ventes_clean['client_inconnu'] = df_ventes_clean['client_id'].isna()
print(f"Clients inconnus flaggés : {df_ventes_clean['client_inconnu'].sum()}")

# 7. Convertir les dates
df_ventes_clean['date'] = pd.to_datetime(df_ventes_clean['date'], format='mixed', errors='coerce')
dates_invalides = df_ventes_clean['date'].isna().sum()
print(f"Dates converties ({dates_invalides} invalides)")

# 8. Créer le montant total
df_ventes_clean['montant_total'] = df_ventes_clean['quantite'] * df_ventes_clean['prix_unitaire']

print(f"\n✅ Nettoyage terminé : {len(df_ventes_clean)} lignes finales")
```

---

### ✍️ Exercice 4.1 : Validez le nettoyage

Après le nettoyage, vérifiez que les problèmes sont résolus :

```python
# TODO: Vérifiez chaque point

# 1. Plus de doublons ?
print("Doublons restants:", df_ventes_clean.duplicated().sum())

# 2. Plus de prix négatifs ?
print("Prix négatifs:", (df_ventes_clean['prix_unitaire'] < 0).sum())

# 3. Canaux standardisés ?
print("Canaux uniques:", df_ventes_clean['canal'].unique())

# 4. Régions standardisées ?
print("Régions uniques:", df_ventes_clean['region'].unique())

# 5. Pas de quantités > 100 ?
print("Quantités > 100:", (df_ventes_clean['quantite'] > 100).sum())
```

---

## 5. Structuration et transformation

### 5.1 Jointures entre sources

Maintenant, combinons nos datasets pour créer une vue unifiée.

```python
# 1. Joindre les ventes avec le catalogue produits
df_analyse = df_ventes_clean.merge(
    df_produits[['produit_id', 'nom_produit', 'categorie', 'cout_achat', 'prix_vente']],
    on='produit_id',
    how='left'
)

print(f"Après jointure produits : {df_analyse.shape}")

# 2. Joindre avec les catégories
df_analyse = df_analyse.merge(
    df_categories[['categorie', 'responsable', 'objectif_ca_q4']],
    on='categorie',
    how='left'
)

print(f"Après jointure catégories : {df_analyse.shape}")

# 3. Agréger les avis par produit (note moyenne)
avis_agg = df_avis.groupby('produit_id').agg({
    'note': 'mean',
    'verifie': 'sum'  # Nombre d'avis vérifiés
}).rename(columns={'note': 'note_moyenne', 'verifie': 'nb_avis_verifies'}).reset_index()

df_analyse = df_analyse.merge(avis_agg, on='produit_id', how='left')

print(f"Après jointure avis : {df_analyse.shape}")

# 4. Joindre avec l'historique clients (pour les clients connus)
df_analyse = df_analyse.merge(
    df_clients[['client_id', 'date_inscription', 'segment_initial', 'source_acquisition']],
    on='client_id',
    how='left'
)

print(f"Après jointure clients : {df_analyse.shape}")
print(f"\nColonnes finales : {list(df_analyse.columns)}")
```

---

### 5.2 Feature engineering

Créons des variables dérivées utiles pour l'analyse :

```python
# 1. Marge par transaction
df_analyse['marge_unitaire'] = df_analyse['prix_vente'] - df_analyse['cout_achat']
df_analyse['marge_totale'] = df_analyse['marge_unitaire'] * df_analyse['quantite']

# 2. Variables temporelles
df_analyse['mois'] = df_analyse['date'].dt.month
df_analyse['semaine'] = df_analyse['date'].dt.isocalendar().week
df_analyse['jour_semaine'] = df_analyse['date'].dt.day_name()
df_analyse['is_weekend'] = df_analyse['date'].dt.dayofweek >= 5

# 3. Ancienneté client (pour les clients connus)
df_analyse['anciennete_jours'] = (df_analyse['date'] - df_analyse['date_inscription']).dt.days

# 4. Segment de valeur transaction
df_analyse['segment_transaction'] = pd.cut(
    df_analyse['montant_total'],
    bins=[0, 50, 200, 500, float('inf')],
    labels=['Petit', 'Moyen', 'Gros', 'Très gros']
)

print("✅ Features créées :")
print(df_analyse[['marge_totale', 'mois', 'jour_semaine', 'anciennete_jours', 'segment_transaction']].head())
```

---

### ✍️ Exercice 5.1 : Créez des features supplémentaires

```python
# TODO: Créez ces features additionnelles

# 1. Panier moyen par client (montant moyen des transactions d'un client)
# Indice : groupby + transform

df_analyse['panier_moyen_client'] = df_analyse.groupby('client_id')['montant_total'].transform('_______________')

# 2. Fréquence d'achat client (nombre de transactions par client)
df_analyse['freq_achat_client'] = df_analyse.groupby('client_id')['transaction_id'].transform('_______________')

# 3. Part du canal (quelle proportion des ventes vient de ce canal ?)
# Pour chaque catégorie, calculez le % du montant par canal

# Vérification
print(df_analyse[['client_id', 'panier_moyen_client', 'freq_achat_client']].head(10))
```

---

### 5.3 Dataset final

```python
# Sélection des colonnes pertinentes pour l'analyse
colonnes_finales = [
    # Identifiants
    'transaction_id', 'date', 'client_id',
    # Produit
    'produit_id', 'nom_produit', 'categorie', 'responsable',
    # Transaction
    'quantite', 'prix_unitaire', 'montant_total', 'canal', 'region',
    # Rentabilité
    'cout_achat', 'marge_unitaire', 'marge_totale',
    # Qualité
    'note_moyenne', 'nb_avis_verifies',
    # Client
    'segment_initial', 'source_acquisition', 'anciennete_jours',
    # Temporel
    'mois', 'semaine', 'jour_semaine', 'is_weekend',
    # Comportement
    'segment_transaction', 'client_inconnu'
]

df_final = df_analyse[colonnes_finales].copy()

print(f"✅ Dataset final : {df_final.shape}")
print(f"Colonnes : {len(colonnes_finales)}")
```

> 🤔 **Question Socratique** : Nous avons créé beaucoup de features. Toutes sont-elles vraiment nécessaires ? Comment décideriez-vous lesquelles garder pour un modèle de machine learning versus un dashboard BI ?

---

## 6. EDA analytique

### 6.1 Répondre aux questions business

Maintenant que nos données sont propres, répondons aux questions du CEO.

### Question 1 : Quelles catégories génèrent le plus de revenus et de marge ?

```python
# Analyse par catégorie
cat_analysis = df_final.groupby('categorie').agg({
    'montant_total': 'sum',
    'marge_totale': 'sum',
    'transaction_id': 'count',
    'objectif_ca_q4': 'first'
}).rename(columns={'transaction_id': 'nb_transactions'})

cat_analysis['marge_pct'] = (cat_analysis['marge_totale'] / cat_analysis['montant_total'] * 100).round(1)
cat_analysis['atteinte_objectif'] = (cat_analysis['montant_total'] / cat_analysis['objectif_ca_q4'] * 100).round(1)

print("📊 Performance par catégorie Q4 2024 :")
print(cat_analysis.sort_values('montant_total', ascending=False))
```

---

### Question 2 : Patterns saisonniers ?

```python
# Analyse temporelle
temps_analysis = df_final.groupby(['mois', 'semaine']).agg({
    'montant_total': 'sum',
    'transaction_id': 'count'
}).reset_index()

# Par jour de semaine
jour_analysis = df_final.groupby('jour_semaine')['montant_total'].sum()
jour_order = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
jour_analysis = jour_analysis.reindex(jour_order)

print("📅 CA par jour de semaine :")
print(jour_analysis)

# Weekend vs semaine
weekend_analysis = df_final.groupby('is_weekend')['montant_total'].agg(['sum', 'mean', 'count'])
print("\n📅 Weekend vs Semaine :")
print(weekend_analysis)
```

---

### Question 3 : Segmentation clients

```python
# RFM simplifié (Récence, Fréquence, Montant)
client_rfm = df_final[~df_final['client_inconnu']].groupby('client_id').agg({
    'date': 'max',  # Dernière commande
    'transaction_id': 'count',  # Fréquence
    'montant_total': 'sum'  # Montant total
}).rename(columns={
    'date': 'derniere_commande',
    'transaction_id': 'frequence',
    'montant_total': 'montant_cumule'
})

# Calcul récence (jours depuis dernière commande)
date_reference = df_final['date'].max()
client_rfm['recence_jours'] = (date_reference - client_rfm['derniere_commande']).dt.days

# Segmentation simple
def segment_client(row):
    if row['montant_cumule'] > 1000 and row['frequence'] >= 3:
        return 'VIP'
    elif row['montant_cumule'] > 500:
        return 'Fidèle'
    elif row['recence_jours'] > 60:
        return 'À risque'
    else:
        return 'Standard'

client_rfm['segment'] = client_rfm.apply(segment_client, axis=1)

print("👥 Distribution des segments clients :")
print(client_rfm['segment'].value_counts())
print(f"\n💰 CA moyen par segment :")
print(client_rfm.groupby('segment')['montant_cumule'].mean().round(2))
```

---

### ✍️ Exercice 6.1 : Analyse approfondie

Répondez aux questions 4 et 5 avec votre propre code :

```python
# Question 4 : Quels sont les indicateurs de risque de churn ?
# Indice : Analysez les clients "À risque" - qu'ont-ils en commun ?

# TODO: Votre analyse ici
# Suggestions :
# - Source d'acquisition des clients à risque vs VIP
# - Catégorie de produits achetés
# - Canal utilisé


# Question 5 : Qualité des données
# Calculez un "score de qualité" pour le dataset final

# TODO: Complétez
completude = (1 - df_final.isnull().sum().sum() / df_final.size) * 100
print(f"Score de complétude : {completude:.1f}%")

# Ajoutez d'autres métriques...
```

---

## 7. Visualisations exploratoires

### 7.1 Dashboard analytique

```python
import matplotlib.pyplot as plt
import seaborn as sns

fig, axes = plt.subplots(2, 3, figsize=(16, 10))

# 1. CA par catégorie (barres)
ax1 = axes[0, 0]
cat_ca = df_final.groupby('categorie')['montant_total'].sum().sort_values(ascending=True)
cat_ca.plot(kind='barh', ax=ax1, color='steelblue')
ax1.set_title('CA par catégorie (Q4 2024)')
ax1.set_xlabel('Chiffre d\'affaires (€)')

# 2. Évolution temporelle (ligne)
ax2 = axes[0, 1]
temps = df_final.groupby(df_final['date'].dt.date)['montant_total'].sum()
temps.plot(ax=ax2, color='forestgreen')
ax2.set_title('Évolution du CA quotidien')
ax2.set_xlabel('Date')
ax2.set_ylabel('CA (€)')

# 3. Répartition par canal (pie)
ax3 = axes[0, 2]
canal_ca = df_final.groupby('canal')['montant_total'].sum()
canal_ca.plot(kind='pie', ax=ax3, autopct='%1.1f%%', startangle=90)
ax3.set_title('Répartition CA par canal')
ax3.set_ylabel('')

# 4. Marge par catégorie (barres empilées)
ax4 = axes[1, 0]
margin_data = df_final.groupby('categorie').agg({
    'cout_achat': lambda x: (x * df_final.loc[x.index, 'quantite']).sum(),
    'marge_totale': 'sum'
})
margin_data.plot(kind='bar', stacked=True, ax=ax4, color=['coral', 'mediumseagreen'])
ax4.set_title('Coût vs Marge par catégorie')
ax4.set_xlabel('')
ax4.legend(['Coût', 'Marge'])
ax4.tick_params(axis='x', rotation=45)

# 5. Distribution des notes produits
ax5 = axes[1, 1]
note_by_cat = df_final.groupby('categorie')['note_moyenne'].mean()
note_by_cat.plot(kind='bar', ax=ax5, color='goldenrod')
ax5.set_title('Note moyenne par catégorie')
ax5.set_ylim(0, 5)
ax5.axhline(y=4, color='green', linestyle='--', alpha=0.7)
ax5.set_xlabel('')
ax5.tick_params(axis='x', rotation=45)

# 6. Heatmap jour/heure
ax6 = axes[1, 2]
heatmap_data = df_final.pivot_table(
    values='montant_total',
    index='jour_semaine',
    columns='mois',
    aggfunc='sum'
)
jour_order = ['Monday', 'Tuesday', 'Wednesday', 'Thursday', 'Friday', 'Saturday', 'Sunday']
heatmap_data = heatmap_data.reindex(jour_order)
sns.heatmap(heatmap_data, annot=True, fmt='.0f', cmap='YlOrRd', ax=ax6)
ax6.set_title('CA par jour et mois')

plt.tight_layout()
plt.savefig('dashboard_techshop.png', dpi=150, bbox_inches='tight')
plt.show()

print("✅ Dashboard sauvegardé : dashboard_techshop.png")
```

---

### ✍️ Exercice 7.1 : Créez vos propres visualisations

Créez au moins 2 visualisations supplémentaires pour répondre aux questions business :

```python
# TODO: Visualisation 1 - Segmentation clients
# Idée : scatter plot Fréquence vs Montant, coloré par segment


# TODO: Visualisation 2 - Analyse du churn
# Idée : comparaison des caractéristiques clients "À risque" vs "VIP"

```

---

## 8. Chargement vers S3

### 8.1 Préparation de l'upload

```python
from datetime import datetime

# Métadonnées
date_str = datetime.now().strftime("%Y%m%d")
nom_fichier = f"techshop_analytics_{date_str}.parquet"

# Chemin S3 (simulé localement pour l'exercice)
chemin_local = f"./{nom_fichier}"
chemin_s3 = f"s3://techshop-datalake/curated/analytics/{nom_fichier}"

print(f"Fichier à uploader : {nom_fichier}")
print(f"Destination S3 : {chemin_s3}")
```

### 8.2 Upload avec validation

```python
# Sauvegarde locale (simulation S3)
df_final.to_parquet(chemin_local, index=False)

# Validation
df_verif = pd.read_parquet(chemin_local)

assert df_verif.shape == df_final.shape, "Erreur dimensions"
assert list(df_verif.columns) == list(df_final.columns), "Erreur colonnes"

print(f"✅ Fichier validé : {chemin_local}")
print(f"   Lignes : {len(df_verif)}")
print(f"   Colonnes : {len(df_verif.columns)}")

# En production avec boto3 :
"""
import boto3
from io import BytesIO

s3 = boto3.client('s3')
buffer = BytesIO()
df_final.to_parquet(buffer, index=False)

s3.put_object(
    Bucket='techshop-datalake',
    Key=f'curated/analytics/{nom_fichier}',
    Body=buffer.getvalue(),
    Metadata={
        'created_at': datetime.now().isoformat(),
        'source': 'Module 2 - Projet Fil Rouge',
        'rows': str(len(df_final)),
        'columns': str(len(df_final.columns))
    }
)
"""
```

---

### 8.3 Documentation du dataset

```python
# Génération automatique du data dictionary
data_dictionary = f"""
# Data Dictionary : {nom_fichier}

## Métadonnées
- **Date de création** : {datetime.now().strftime("%Y-%m-%d %H:%M")}
- **Source** : Projet fil rouge Module 2
- **Lignes** : {len(df_final):,}
- **Colonnes** : {len(df_final.columns)}

## Description des colonnes

| Colonne | Type | Description |
|---------|------|-------------|
"""

for col in df_final.columns:
    dtype = str(df_final[col].dtype)
    # Description automatique basée sur le nom
    desc = col.replace('_', ' ').title()
    data_dictionary += f"| {col} | {dtype} | {desc} |\n"

data_dictionary += f"""
## Transformations appliquées
1. Suppression de {150} doublons
2. Standardisation des canaux (web, mobile)
3. Standardisation des régions (5 valeurs)
4. Correction des quantités aberrantes (>100)
5. Suppression des prix négatifs
6. Jointure avec catalogue, catégories, avis, clients
7. Création de 10+ features dérivées

## Sources de données
- ventes_q4_2024.csv (CRM)
- catalogue_produits.xlsx (Marketing)
- avis_clients.json (API)
- historique_clients.parquet (S3)
"""

# Sauvegarde
with open('data_dictionary_techshop.md', 'w') as f:
    f.write(data_dictionary)

print("✅ Data dictionary créé : data_dictionary_techshop.md")
```

---

## 9. Présentation et synthèse

### 9.1 Template de présentation

Structurez votre présentation finale :

```markdown
# 📊 Analyse TechShop Q4 2024
## Rapport Data Analytics

---

### 1. Contexte et objectifs
- Mission : Construire une pipeline de données propre
- Sources : 4 (CRM, Marketing, API, S3)
- Période : Q4 2024

---

### 2. Qualité des données
- **Avant nettoyage** : X problèmes identifiés
- **Après nettoyage** : Score de complétude Y%
- **Principales corrections** : [liste]

---

### 3. Réponses aux questions business

#### Q1 : Catégories les plus rentables
[Votre insight + chiffres clés]

#### Q2 : Patterns saisonniers
[Votre insight + chiffres clés]

#### Q3 : Segmentation clients
[Votre insight + chiffres clés]

#### Q4 : Indicateurs de churn
[Votre insight + chiffres clés]

#### Q5 : État de la qualité données
[Votre insight + chiffres clés]

---

### 4. Visualisations clés
[2-3 graphiques les plus impactants]

---

### 5. Recommandations
1. [Action 1]
2. [Action 2]
3. [Action 3]

---

### 6. Limites et prochaines étapes
- Ce que l'analyse ne dit pas
- Données manquantes
- Analyses futures suggérées
```

---

### ✍️ Exercice 9.1 : Préparez votre présentation

Complétez le template ci-dessus avec vos propres résultats. Points à inclure :

1. **3 chiffres clés** que le CEO doit retenir
2. **1 visualisation** la plus impactante
3. **2 recommandations** actionnables
4. **1 limitation** importante à mentionner

---

## 🧠 Réflexion métacognitive finale

### Ce que vous avez accompli

Félicitations ! Vous venez de réaliser un projet data complet :

- [x] Extraction de 4 sources différentes (CSV, Excel, JSON, Parquet)
- [x] Diagnostic qualité avec checklist
- [x] Nettoyage de 5+ types de problèmes
- [x] Jointures et transformations
- [x] Feature engineering (10+ variables)
- [x] EDA analytique avec réponses business
- [x] Visualisations professionnelles
- [x] Upload et documentation

### Auto-évaluation finale

| Compétence | Avant module | Après projet |
|------------|--------------|--------------|
| Extraction multi-sources | ⬜⬜⬜⬜⬜ | ⬜⬜⬜⬜⬜ |
| Diagnostic qualité | ⬜⬜⬜⬜⬜ | ⬜⬜⬜⬜⬜ |
| Nettoyage données | ⬜⬜⬜⬜⬜ | ⬜⬜⬜⬜⬜ |
| Transformations | ⬜⬜⬜⬜⬜ | ⬜⬜⬜⬜⬜ |
| EDA analytique | ⬜⬜⬜⬜⬜ | ⬜⬜⬜⬜⬜ |
| Visualisation | ⬜⬜⬜⬜⬜ | ⬜⬜⬜⬜⬜ |
| Cloud S3 | ⬜⬜⬜⬜⬜ | ⬜⬜⬜⬜⬜ |
| Documentation | ⬜⬜⬜⬜⬜ | ⬜⬜⬜⬜⬜ |

### Questions de réflexion

1. **Quelle étape vous a demandé le plus de temps ?** Pourquoi ?

2. **Si vous deviez refaire ce projet, que feriez-vous différemment ?**

3. **Quelle compétence souhaitez-vous approfondir ?**

4. **Comment allez-vous utiliser ces compétences dans votre futur métier ?**

---

## 📋 Checklist des livrables

Avant de soumettre votre projet, vérifiez :

- [ ] **Notebook complet** avec tout le code exécutable
- [ ] **Dataset final** : `techshop_analytics_YYYYMMDD.parquet`
- [ ] **Data dictionary** : `data_dictionary_techshop.md`
- [ ] **Dashboard** : `dashboard_techshop.png`
- [ ] **Présentation** : Réponses aux 5 questions business
- [ ] **Auto-évaluation** : Tableau complété

---

## 🔗 Connexion avec la suite du parcours

Ce projet vous a préparé pour :

- **Module 3 (Machine Learning)** : Votre dataset `techshop_analytics.parquet` est prêt pour entraîner des modèles de prédiction (churn, recommandation)

- **Module 4 (Power BI)** : Les visualisations créées ici peuvent être transformées en dashboard interactif

**Vous savez maintenant transformer des données brutes en insights actionnables. C'est le cœur du métier de Data Analyst.**

---

## 📚 Ressources complémentaires

- [Data Ladder - Retail Data Quality Issues](https://dataladder.com/common-data-quality-issues-in-the-retail-industry-and-how-to-fix-them/)
- [Integrate.io - E-Commerce Data Pipelines](https://www.integrate.io/blog/data-pipelines-e-commerce-industry/)
- [SPD Technology - Data Analytics in Retail 2025](https://spd.tech/data/data-analytics-in-retail-making-data-work-for-your-business-in-2025/)
