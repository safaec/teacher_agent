# Chapitre 9 : Chargement vers le cloud

**Durée estimée : 4-5h**

---

## Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

1. **Écrire** des données depuis Python vers AWS S3 en utilisant boto3 et pandas
2. **Organiser** un data lake avec une structure professionnelle (raw/processed/final)
3. **Documenter** un dataset avec un data dictionary complet

---

## 9.1. Organisation d'un data lake basique

### 1.1 Le problème du "data swamp"

Un data lake mal organisé devient un "data swamp" (marécage de données) : personne ne sait ce qui s'y trouve, où le trouver, ni si les données sont fiables.

**Symptômes du data swamp** :

- "C'est dans quel dossier déjà, les données clients ?"
- "Cette version est-elle la bonne ? Il y a sales_v2, sales_final, sales_FINAL_v3..."
- "Qui a modifié ce fichier ? Quand ?"

La solution : **l'architecture en couches** (Medallion Architecture).

---

### 1.2 Les trois zones essentielles

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
