# Chapitre 9 : Chargement vers le cloud

**Durée estimée : 4-5h**

---

## Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

1. **Écrire** des données depuis Python vers AWS S3 en utilisant boto3 et pandas
2. **Organiser** un data lake avec une structure professionnelle (raw/processed/final)
3. **Documenter** un dataset avec un data dictionary complet

---

## 🎯 L'accroche : Le paradoxe du data scientist isolé

Imaginez ce scénario : vous avez passé trois jours à nettoyer un dataset de 2 millions de lignes. Vos transformations sont parfaites. Vos colonnes sont impeccables. Votre EDA révèle des insights précieux.

Puis votre ordinateur plante.

Tout est perdu.

Ou pire encore : votre collègue vous demande le dataset pour son modèle de machine learning. Vous lui envoyez le fichier par email — 500 Mo. Il échoue. Vous essayez WeTransfer. Ça prend 45 minutes. Le lendemain, il vous dit que le fichier est corrompu.

**C'est exactement pourquoi les entreprises utilisent le cloud.**

Selon AWS, les organisations qui centralisent leurs données sur S3 réduisent de 70% le temps passé à chercher et partager des fichiers. *(Source : AWS Well-Architected Framework, 2024)*

> 🤔 **Question Socratique** : Pourquoi pensez-vous que l'email reste si populaire pour partager des données en entreprise, malgré ses limitations évidentes ? Quels comportements humains ou organisationnels expliquent cette résistance au changement ?

---

## Rappel : Ce que vous savez déjà

Au Chapitre 3, vous avez appris à **lire** depuis S3 :

```python
import pandas as pd

# Lecture directe depuis S3
df = pd.read_csv("s3://mon-bucket/data/ventes.csv")
```

Vous connaissez également boto3 pour les opérations de base et la structure des chemins S3 (`s3://bucket/prefix/fichier.ext`).

**Maintenant, nous allons faire l'inverse : écrire vers le cloud.**

---

## 1. Écrire vers S3 avec Python

### 1.1 Trois méthodes pour charger vos données

Il existe trois approches principales pour écrire des DataFrames vers S3. Chacune a ses avantages.

#### Méthode 1 : Pandas natif avec URI S3

La plus simple — pandas gère tout en arrière-plan :

```python
import pandas as pd

# Votre DataFrame nettoyé
df_clean = pd.DataFrame({
    'client_id': [1, 2, 3],
    'montant': [150.0, 230.5, 89.0],
    'date': ['2024-01-15', '2024-01-16', '2024-01-17']
})

# Écriture directe vers S3
df_clean.to_csv("s3://mon-bucket/processed/ventes_clean.csv", index=False)
```

**Avantage** : Simplicité maximale, une seule ligne de code.

**Limitation** : Nécessite la librairie `s3fs` installée, qui peut créer des conflits de dépendances dans certains environnements.

---

#### Méthode 2 : boto3 avec buffer mémoire

Plus de contrôle, pas de dépendance supplémentaire :

```python
import boto3
from io import StringIO

# Créer le client S3
s3_client = boto3.client('s3')

# Convertir le DataFrame en CSV dans un buffer mémoire
csv_buffer = StringIO()
df_clean.to_csv(csv_buffer, index=False)

# Uploader vers S3
s3_client.put_object(
    Bucket='mon-bucket',
    Key='processed/ventes_clean.csv',
    Body=csv_buffer.getvalue()
)

print("Upload réussi !")
```

**Avantage** : Contrôle total, fonctionne partout où boto3 est installé.

**Quand l'utiliser** : Environnements avec contraintes de dépendances, ou quand vous avez besoin d'options avancées (encryption, metadata).

---

#### Méthode 3 : AWS SDK for pandas (awswrangler)

La solution "enterprise-grade" recommandée par AWS :

```python
import awswrangler as wr

# Écriture simple
wr.s3.to_csv(df_clean, "s3://mon-bucket/processed/ventes_clean.csv", index=False)

# Avec partitionnement (pour les gros datasets)
wr.s3.to_csv(
    df_clean,
    "s3://mon-bucket/processed/ventes/",
    index=False,
    dataset=True,
    partition_cols=['annee', 'mois']
)
```

**Avantage** : Fonctionnalités avancées (partitionnement, intégration Glue Catalog, gestion automatique des types).

**Installation** : `pip install awswrangler`

*(Source : [AWS SDK for pandas Documentation](https://aws-sdk-pandas.readthedocs.io/en/stable/tutorials/003%20-%20Amazon%20S3.html))*

---

### ✍️ Exercice pratique 1 : Votre premier upload

Choisissez la méthode 2 (boto3) et uploadez un DataFrame de test vers S3.

```python
# TODO: Complétez ce code
import boto3
from io import StringIO
import pandas as pd

# 1. Créez un DataFrame avec 5 lignes de données fictives
df_test = pd.DataFrame({
    # Ajoutez vos colonnes ici
})

# 2. Créez le client S3
s3_client = _______________

# 3. Créez le buffer et convertissez
csv_buffer = _______________
df_test.to_csv(_______________, index=False)

# 4. Uploadez vers votre bucket (remplacez par votre bucket)
s3_client.put_object(
    Bucket='_______________',
    Key='test/mon_premier_upload.csv',
    Body=_______________
)
```

> 💡 **Indice** : Relisez la méthode 2 ci-dessus. Chaque blanc correspond à un élément que vous avez vu.

---

### 1.2 CSV vs Parquet : Le choix qui change tout

Jusqu'ici, nous avons utilisé le format CSV. Mais pour le cloud, un autre format domine : **Parquet**.

| Critère | CSV | Parquet |
|---------|-----|---------|
| Lisibilité humaine | ✅ Oui (texte) | ❌ Non (binaire) |
| Taille fichier | 100% | **15-30%** (compression) |
| Vitesse de lecture | Lente | **2x plus rapide** |
| Coût S3/Athena | Élevé | **Très réduit** |
| Types de données | Perdus | ✅ Préservés |

**Chiffres clés** : Selon AWS, Parquet est "2x plus rapide à décharger et consomme jusqu'à 6x moins de stockage sur S3 par rapport aux formats texte". Une étude sur AWS Athena montre que les requêtes sur Parquet coûtent **$3.65/an** contre **$2,000/an** pour les mêmes données en CSV. *(Source : [AWS Athena Parquet vs CSV](https://www.linkedin.com/pulse/aws-athena-parquet-vs-csv-ahmed-fayed))*

> 🤔 **Question Socratique** : Si Parquet est tellement supérieur, pourquoi le CSV existe-t-il encore ? Dans quels contextes précis le CSV reste-t-il le meilleur choix ?

---

#### Écrire en Parquet

```python
# Avec pandas natif
df_clean.to_parquet("s3://mon-bucket/processed/ventes_clean.parquet", index=False)

# Avec awswrangler (recommandé pour gros volumes)
import awswrangler as wr
wr.s3.to_parquet(df_clean, "s3://mon-bucket/processed/ventes_clean.parquet")

# Avec boto3 + buffer (pour contrôle total)
import boto3
from io import BytesIO  # Attention : BytesIO pour binaire, pas StringIO

parquet_buffer = BytesIO()
df_clean.to_parquet(parquet_buffer, index=False)

s3_client = boto3.client('s3')
s3_client.put_object(
    Bucket='mon-bucket',
    Key='processed/ventes_clean.parquet',
    Body=parquet_buffer.getvalue()
)
```

**Règle pratique** :
- **CSV** : Fichiers < 100 Mo, besoin d'inspection manuelle, échange avec non-techniciens
- **Parquet** : Fichiers > 100 Mo, pipelines automatisés, requêtes analytiques

---

### ✍️ Exercice pratique 2 : Comparez les tailles

```python
import pandas as pd
import os

# Créez un DataFrame de 100,000 lignes
import numpy as np
np.random.seed(42)

df_large = pd.DataFrame({
    'id': range(100000),
    'valeur': np.random.randn(100000),
    'categorie': np.random.choice(['A', 'B', 'C', 'D'], 100000),
    'date': pd.date_range('2024-01-01', periods=100000, freq='T')
})

# Sauvegardez en local dans les deux formats
df_large.to_csv('test_comparaison.csv', index=False)
df_large.to_parquet('test_comparaison.parquet', index=False)

# Comparez les tailles
taille_csv = os.path.getsize('test_comparaison.csv') / (1024 * 1024)  # Mo
taille_parquet = os.path.getsize('test_comparaison.parquet') / (1024 * 1024)  # Mo

print(f"Taille CSV    : {taille_csv:.2f} Mo")
print(f"Taille Parquet: {taille_parquet:.2f} Mo")
print(f"Réduction     : {((taille_csv - taille_parquet) / taille_csv) * 100:.1f}%")
```

**Résultat attendu** : Le fichier Parquet devrait être 3 à 5 fois plus petit.

---

## 2. Organisation d'un data lake basique

### 2.1 Le problème du "data swamp"

Un data lake mal organisé devient un "data swamp" (marécage de données) : personne ne sait ce qui s'y trouve, où le trouver, ni si les données sont fiables.

**Symptômes du data swamp** :
- "C'est dans quel dossier déjà, les données clients ?"
- "Cette version est-elle la bonne ? Il y a sales_v2, sales_final, sales_FINAL_v3..."
- "Qui a modifié ce fichier ? Quand ?"

La solution : **l'architecture en couches** (Medallion Architecture).

---

### 2.2 Les trois zones essentielles

L'industrie a convergé vers un standard : trois zones distinctes avec des règles strictes.

```
s3://mon-datalake/
├── raw/                    # 🥉 Bronze : données brutes
│   ├── source_crm/
│   │   └── 2024/01/15/
│   │       └── clients_export_20240115.csv
│   ├── source_erp/
│   └── source_api/
│
├── processed/              # 🥈 Silver : données nettoyées
│   ├── clients/
│   │   └── clients_clean.parquet
│   └── ventes/
│       └── ventes_clean.parquet
│
└── curated/                # 🥇 Gold : données prêtes à l'emploi
    ├── analytics/
    │   └── dashboard_ventes.parquet
    └── ml/
        └── features_clients.parquet
```

*(Source : [AWS Data Lake Foundation](https://docs.aws.amazon.com/whitepapers/latest/building-data-lakes/data-lake-foundation.html), [Microsoft Data Lake Zones](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/scenarios/cloud-scale-analytics/best-practices/data-lake-zones))*

---

#### Zone RAW (Bronze) — Les règles d'or

| Règle | Pourquoi |
|-------|----------|
| **Jamais modifier** les fichiers | Traçabilité, possibilité de reconstruire |
| **Conserver le format d'origine** | CSV reste CSV, JSON reste JSON |
| **Inclure la date d'ingestion** | `/source/YYYY/MM/DD/fichier.ext` |
| **Aucune transformation** | La zone raw est un miroir de la source |

```python
# Exemple : ingestion d'un fichier brut
from datetime import datetime

aujourd_hui = datetime.now().strftime("%Y/%m/%d")
chemin_raw = f"s3://mon-datalake/raw/source_crm/{aujourd_hui}/clients_export.csv"

# Le fichier est copié tel quel, sans modification
s3_client.upload_file('clients_export.csv', 'mon-datalake',
                      f'raw/source_crm/{aujourd_hui}/clients_export.csv')
```

---

#### Zone PROCESSED (Silver) — Données nettoyées

C'est ici que vit le résultat de votre travail des Chapitres 4-6 :

- Valeurs manquantes traitées
- Doublons supprimés
- Types corrigés
- Format optimisé (Parquet recommandé)

```python
# Après nettoyage, on écrit dans processed/
chemin_processed = "s3://mon-datalake/processed/clients/clients_clean.parquet"
df_clean.to_parquet(chemin_processed, index=False)
```

---

#### Zone CURATED (Gold) — Prête pour l'analyse

Données agrégées, jointes, enrichies — directement utilisables par :
- Les analystes BI (tableaux de bord)
- Les data scientists (features pour ML)
- Les outils de reporting

```python
# Exemple : table agrégée pour dashboard
df_dashboard = df_ventes.groupby(['region', 'mois']).agg({
    'montant': 'sum',
    'nb_transactions': 'count'
}).reset_index()

chemin_curated = "s3://mon-datalake/curated/analytics/ventes_par_region.parquet"
df_dashboard.to_parquet(chemin_curated, index=False)
```

> 🤔 **Question Socratique** : Dans votre projet fil rouge, quelles données iraient dans chaque zone ? Prenez 2 minutes pour esquisser mentalement la structure de votre propre data lake.

---

### 📌 Point de vue alternatif : La simplicité avant tout

Certains praticiens argumentent que l'architecture Medallion est **over-engineering** pour les petites équipes. L'argument :

> "Avec 3 data engineers et des datasets de quelques Go, maintenir 3 zones distinctes crée plus de friction que de valeur. Un simple dossier `raw/` et `clean/` suffit."
> — *Pragmatic Data Engineering (2024)*

**Quand simplifier** :
- Équipe < 5 personnes
- Datasets < 10 Go
- Pas de contraintes réglementaires strictes

**Quand structurer** :
- Équipe > 10 personnes ou croissance prévue
- Datasets > 100 Go
- Secteurs réglementés (finance, santé)
- Multiples consommateurs des données

---

### ✍️ Exercice pratique 3 : Concevez votre structure

Pour votre projet fil rouge, dessinez la structure de dossiers S3 que vous utiliseriez.

```
s3://[votre-bucket]/
├── raw/
│   └── [quelles sources ?]
│
├── processed/
│   └── [quels datasets nettoyés ?]
│
└── curated/
    └── [quelles tables finales ?]
```

Répondez aux questions :
1. Quelles sont vos sources de données brutes ?
2. Quel format choisirez-vous pour processed/ ? Pourquoi ?
3. Qui consommera les données de curated/ ?

---

## 3. Documentation du dataset

### 3.1 Pourquoi documenter est crucial

Un dataset non documenté est un dataset inutilisable dans 6 mois — même par vous-même.

**Le syndrome du "je me souviendrai"** :
- Que signifie la colonne `flag_x` ?
- Pourquoi y a-t-il des valeurs négatives dans `montant` ?
- Les dates sont-elles en UTC ou en heure locale ?

### 3.2 Le Data Dictionary

Un data dictionary décrit chaque colonne de votre dataset :

```markdown
# Data Dictionary : clients_clean.parquet

## Métadonnées générales
- **Dernière mise à jour** : 2024-01-20
- **Source** : CRM Salesforce (extraction quotidienne)
- **Responsable** : equipe-data@entreprise.com
- **Lignes** : 45,230
- **Colonnes** : 12

## Description des colonnes

| Colonne | Type | Description | Valeurs possibles | Notes |
|---------|------|-------------|-------------------|-------|
| client_id | int64 | Identifiant unique client | 1-999999 | Clé primaire |
| nom | string | Nom complet | Texte libre | Nettoyé (majuscules) |
| email | string | Email principal | format@email.com | Validé par regex |
| date_inscription | datetime | Date création compte | 2015-01-01 à aujourd'hui | Timezone UTC |
| segment | category | Segment marketing | 'premium', 'standard', 'nouveau' | Dérivé du CA annuel |
| ca_annuel | float64 | Chiffre d'affaires 12 mois | 0.0 - 999999.99 | En euros, peut être 0 |
| nb_commandes | int64 | Nombre de commandes | 0 - 9999 | Période 12 mois |
| actif | bool | Client actif ? | True/False | True si commande < 6 mois |

## Transformations appliquées
1. Emails invalides → remplacés par NULL (2.3% des lignes)
2. Doublons sur email → conservé le plus récent (154 lignes supprimées)
3. CA négatifs → corrigés via jointure avec table remboursements
```

---

### 3.3 Générer automatiquement avec l'IA

Vous pouvez utiliser un LLM pour générer un premier draft :

```python
# Prompt pour générer un data dictionary
prompt = f"""
Génère un data dictionary en markdown pour ce DataFrame :

Colonnes : {list(df_clean.columns)}
Types : {df_clean.dtypes.to_dict()}
Statistiques : {df_clean.describe().to_dict()}
Valeurs uniques (catégories) : {df_clean.select_dtypes('object').nunique().to_dict()}

Format demandé : tableau markdown avec Colonne, Type, Description, Valeurs possibles, Notes
"""
```

> 🤖 **Astuce IA** : Copiez ce prompt dans Claude ou ChatGPT avec vos vraies données pour obtenir un draft en 30 secondes. Vous n'aurez plus qu'à vérifier et compléter.

---

### ✍️ Exercice pratique 4 : Documentez un dataset

Prenez un des DataFrames que vous avez nettoyé dans les chapitres précédents et créez son data dictionary.

Minimum requis :
- [ ] Métadonnées générales (date, source, responsable)
- [ ] Tableau des colonnes avec type et description
- [ ] Au moins 3 transformations documentées

---

## 4. Vérification post-upload

### 4.1 Ne jamais faire confiance à l'upload

Règle cardinale : **toujours vérifier ce qu'on a écrit**.

Les problèmes courants :
- Fichier corrompu pendant le transfert
- Mauvais encodage
- Colonnes manquantes
- Types modifiés

### 4.2 Checklist de vérification

```python
import pandas as pd

# 1. Relire le fichier uploadé
chemin_s3 = "s3://mon-bucket/processed/clients_clean.parquet"
df_verification = pd.read_parquet(chemin_s3)

# 2. Vérifier les dimensions
assert df_verification.shape == df_clean.shape, "Erreur : dimensions différentes !"

# 3. Vérifier les colonnes
assert list(df_verification.columns) == list(df_clean.columns), "Erreur : colonnes différentes !"

# 4. Vérifier les types
for col in df_clean.columns:
    assert df_verification[col].dtype == df_clean[col].dtype, f"Erreur type : {col}"

# 5. Vérifier un échantillon de valeurs
assert df_verification['client_id'].sum() == df_clean['client_id'].sum(), "Erreur : somme ID différente"

print("✅ Vérification réussie : le fichier S3 est identique au DataFrame source")
```

---

### 4.3 Script de validation réutilisable

```python
def valider_upload_s3(df_source, chemin_s3):
    """
    Valide qu'un fichier uploadé sur S3 correspond au DataFrame source.

    Args:
        df_source: DataFrame original
        chemin_s3: Chemin S3 du fichier uploadé

    Returns:
        bool: True si validation réussie

    Raises:
        AssertionError: Si une vérification échoue
    """
    # Détecter le format
    if chemin_s3.endswith('.parquet'):
        df_uploaded = pd.read_parquet(chemin_s3)
    elif chemin_s3.endswith('.csv'):
        df_uploaded = pd.read_csv(chemin_s3)
    else:
        raise ValueError(f"Format non supporté : {chemin_s3}")

    # Vérifications
    checks = {
        "Nombre de lignes": df_uploaded.shape[0] == df_source.shape[0],
        "Nombre de colonnes": df_uploaded.shape[1] == df_source.shape[1],
        "Noms des colonnes": list(df_uploaded.columns) == list(df_source.columns),
    }

    # Rapport
    for check, result in checks.items():
        status = "✅" if result else "❌"
        print(f"{status} {check}")

    if all(checks.values()):
        print("\n✅ Validation complète réussie")
        return True
    else:
        raise AssertionError("Échec de la validation")

# Utilisation
valider_upload_s3(df_clean, "s3://mon-bucket/processed/clients_clean.parquet")
```

> 🤔 **Question Socratique** : Cette fonction de validation vérifie les dimensions et les colonnes, mais pas le contenu exact de chaque cellule. Pourquoi ce compromis est-il acceptable en pratique ? Dans quels cas voudriez-vous une vérification plus stricte ?

---

## 5. Bonnes pratiques professionnelles

### 5.1 Règle d'or : Immutabilité des données brutes

**Ne jamais écraser les données brutes.** Jamais.

```python
# ❌ MAUVAIS : écrase le fichier original
df_clean.to_csv("s3://bucket/raw/clients.csv")  # DANGER !

# ✅ BON : écrit dans une zone différente
df_clean.to_parquet("s3://bucket/processed/clients_clean.parquet")
```

Pourquoi ? Si vous découvrez une erreur dans votre nettoyage dans 3 mois, vous pourrez reconstruire depuis les données brutes.

---

### 5.2 Conventions de nommage

| Élément | Convention | Exemple |
|---------|------------|---------|
| Dates dans les noms | `YYYYMMDD` ou `YYYY-MM-DD` | `ventes_20240120.parquet` |
| Versions | `_v1`, `_v2` ou timestamp | `clients_v2.parquet` |
| Environnements | Préfixe ou bucket séparé | `dev/`, `prod/` |
| Pas d'espaces | Underscores ou tirets | `mon_dataset.parquet` |

```python
from datetime import datetime

# Génération automatique du nom avec date
date_str = datetime.now().strftime("%Y%m%d")
nom_fichier = f"clients_clean_{date_str}.parquet"
# Résultat : clients_clean_20240120.parquet
```

---

### 5.3 Versioning des données (aperçu)

Pour les projets avancés, des outils permettent de versionner les données comme on versionne le code :

| Outil | Description | Cas d'usage |
|-------|-------------|-------------|
| **DVC** (Data Version Control) | Git pour les données | Projets ML, reproductibilité |
| **Delta Lake** | Format de table versionné | Grands volumes, Spark |
| **LakeFS** | Git-like pour data lakes | Branches de données |

**Nous n'approfondirons pas ces outils** dans ce module, mais sachez qu'ils existent pour les projets à grande échelle.

---

### 5.4 Traçabilité : documenter le "quand" et le "comment"

Chaque fichier uploadé devrait avoir une trace de :
- **Quand** : date et heure de création
- **Comment** : script ou notebook utilisé
- **Par qui** : auteur de la transformation
- **Depuis quoi** : fichiers sources

```python
# Ajouter des métadonnées lors de l'upload avec boto3
import json
from datetime import datetime

metadata = {
    'created_at': datetime.now().isoformat(),
    'created_by': 'safae@entreprise.com',
    'source_files': 'raw/crm/clients_20240115.csv',
    'script': 'notebooks/nettoyage_clients.ipynb',
    'transformations': 'missing_filled,duplicates_removed,types_converted'
}

s3_client.put_object(
    Bucket='mon-bucket',
    Key='processed/clients_clean.parquet',
    Body=parquet_buffer.getvalue(),
    Metadata={k: str(v) for k, v in metadata.items()}  # S3 metadata = strings only
)
```

---

### ✍️ Exercice pratique 5 : Upload complet avec bonnes pratiques

Réalisez un upload complet en appliquant toutes les bonnes pratiques :

```python
# TODO: Complétez ce script d'upload professionnel

import pandas as pd
import boto3
from io import BytesIO
from datetime import datetime

# 1. Votre DataFrame nettoyé (utilisez vos données du fil rouge)
df_clean = pd.read_csv("votre_fichier_nettoye.csv")

# 2. Générez le nom avec date
date_str = _______________
nom_fichier = f"________________{date_str}.parquet"

# 3. Définissez le chemin S3 (zone processed/)
chemin_s3 = f"s3://votre-bucket/processed/{nom_fichier}"

# 4. Créez les métadonnées
metadata = {
    'created_at': _______________,
    'source_files': '_______________',
    'transformations': '_______________'
}

# 5. Uploadez avec boto3
buffer = BytesIO()
df_clean.to_parquet(buffer, index=False)

s3_client = boto3.client('s3')
s3_client.put_object(
    Bucket='_______________',
    Key=f'processed/{nom_fichier}',
    Body=buffer.getvalue(),
    Metadata=metadata
)

# 6. Validez l'upload
# (utilisez la fonction valider_upload_s3)

print(f"✅ Upload réussi : {chemin_s3}")
```

---

## 🔄 Récapitulatif : Le workflow complet

```
┌─────────────────┐
│   DataFrame     │
│    nettoyé      │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Choix format   │
│  CSV / Parquet  │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Upload S3      │
│  (zone processed)│
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Vérification   │
│  post-upload    │
└────────┬────────┘
         │
         ▼
┌─────────────────┐
│  Documentation  │
│  data dictionary│
└─────────────────┘
```

---

## 🧠 Réflexion métacognitive

Avant de terminer ce chapitre, prenez un moment pour réfléchir à votre apprentissage :

### Questions d'auto-évaluation

1. **Quelle partie de ce chapitre vous a semblé la plus claire ?**
   - L'écriture vers S3 avec différentes méthodes ?
   - L'organisation du data lake ?
   - La documentation ?

2. **Quelle partie reste floue ou nécessite plus de pratique ?**
   - Notez-la et prévoyez d'y revenir.

3. **Pouvez-vous expliquer à un collègue...**
   - ... pourquoi Parquet est préférable au CSV pour le cloud ?
   - ... ce que signifie "ne jamais écraser les données brutes" ?
   - ... les trois zones d'un data lake ?

4. **Sur une échelle de 1 à 5, comment évaluez-vous votre confiance pour :**

| Compétence | 1 (Pas du tout) → 5 (Très confiant) |
|------------|-------------------------------------|
| Écrire un DataFrame vers S3 | ⬜ ⬜ ⬜ ⬜ ⬜ |
| Choisir entre CSV et Parquet | ⬜ ⬜ ⬜ ⬜ ⬜ |
| Organiser un data lake | ⬜ ⬜ ⬜ ⬜ ⬜ |
| Documenter un dataset | ⬜ ⬜ ⬜ ⬜ ⬜ |
| Valider un upload | ⬜ ⬜ ⬜ ⬜ ⬜ |

---

## 🔗 Connexion avec le projet fil rouge

Dans le **Chapitre 10 (Cas pratique)**, vous appliquerez tout ce que vous avez appris :

- Vous uploaderez votre dataset final nettoyé vers S3
- Vous utiliserez le format Parquet pour les données volumineuses
- Vous créerez un data dictionary complet
- Vous organiserez vos fichiers selon l'architecture raw/processed/curated

**Préparez-vous** : identifiez dès maintenant quel bucket S3 vous utiliserez et quelle structure de dossiers vous adopterez.

---

## 📚 Sources et références

- [AWS SDK for pandas Documentation](https://aws-sdk-pandas.readthedocs.io/en/stable/tutorials/003%20-%20Amazon%20S3.html)
- [AWS Data Lake Foundation Whitepaper](https://docs.aws.amazon.com/whitepapers/latest/building-data-lakes/data-lake-foundation.html)
- [Microsoft Data Lake Zones Best Practices](https://learn.microsoft.com/en-us/azure/cloud-adoption-framework/scenarios/cloud-scale-analytics/best-practices/data-lake-zones)
- [Parquet vs CSV Performance Comparison](https://www.linkedin.com/pulse/aws-athena-parquet-vs-csv-ahmed-fayed)
- [Data Lake Organization Best Practices](https://www.upsolver.com/blog/best-practices-to-organize-your-data-lake-and-drain-the-data-swamp)

---

## Aide-mémoire du chapitre

```python
# === ÉCRITURE S3 ===

# Méthode 1 : Pandas natif
df.to_csv("s3://bucket/path/file.csv", index=False)
df.to_parquet("s3://bucket/path/file.parquet", index=False)

# Méthode 2 : boto3 + buffer
from io import StringIO, BytesIO
import boto3

s3 = boto3.client('s3')

# CSV
csv_buffer = StringIO()
df.to_csv(csv_buffer, index=False)
s3.put_object(Bucket='bucket', Key='path/file.csv', Body=csv_buffer.getvalue())

# Parquet
parquet_buffer = BytesIO()
df.to_parquet(parquet_buffer, index=False)
s3.put_object(Bucket='bucket', Key='path/file.parquet', Body=parquet_buffer.getvalue())

# Méthode 3 : awswrangler
import awswrangler as wr
wr.s3.to_csv(df, "s3://bucket/path/file.csv", index=False)
wr.s3.to_parquet(df, "s3://bucket/path/file.parquet")

# === STRUCTURE DATA LAKE ===
# s3://bucket/raw/source/YYYY/MM/DD/fichier_original.csv
# s3://bucket/processed/dataset/fichier_clean.parquet
# s3://bucket/curated/analytics/table_finale.parquet

# === VALIDATION ===
df_check = pd.read_parquet("s3://bucket/path/file.parquet")
assert df_check.shape == df.shape
```
