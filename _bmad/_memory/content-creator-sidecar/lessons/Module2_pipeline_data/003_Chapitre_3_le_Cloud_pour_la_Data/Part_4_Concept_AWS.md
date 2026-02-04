# Chapitre 3 — Le Cloud pour la data

**Durée estimée : 8-10 heures**

---

## Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

1. **Expliquer** les avantages du cloud computing et identifier quand l'utiliser plutôt qu'un environnement local
2. **Configurer** un compte AWS et gérer les credentials de manière sécurisée
3. **Lire et écrire** des données depuis/vers AWS S3 en utilisant boto3 et pandas
4. **Comparer** les équivalences entre AWS, GCP et Azure pour le stockage de données

---

## 3.4 AWS S3 : concepts fondamentaux

### Architecture de S3

```
┌─────────────────────────────────────────────────────────┐
│                      AWS S3                             │
├─────────────────────────────────────────────────────────┤
│  ┌─────────────────────────────────────────────────┐   │
│  │              BUCKET: mon-data-lake               │   │
│  │  ┌─────────────────────────────────────────┐    │   │
│  │  │  raw/                                   │    │   │
│  │  │    ├── ventes_2024.csv                 │    │   │
│  │  │    └── clients_export.json             │    │   │
│  │  ├─────────────────────────────────────────┤    │   │
│  │  │  processed/                             │    │   │
│  │  │    └── ventes_clean.parquet            │    │   │
│  │  ├─────────────────────────────────────────┤    │   │
│  │  │  models/                                │    │   │
│  │  │    └── model_v1.pkl                    │    │   │
│  │  └─────────────────────────────────────────┘    │   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

### Vocabulaire essentiel

| Terme | Définition | Analogie |
|-------|------------|----------|
| **Bucket** | Conteneur racine pour vos objets | Un disque dur virtuel |
| **Object** | Un fichier stocké dans un bucket | Un fichier sur ce disque |
| **Key** | Le chemin complet de l'objet | Le chemin du fichier |
| **Prefix** | Un "dossier" (virtuellement) | Un répertoire |
| **Region** | Zone géographique du stockage | Le data center physique |

### Structure des chemins S3

```
s3://mon-bucket/raw/ventes/2024/janvier/ventes.csv
     └── Bucket └── Prefix (chemin) ──────┘└── Fichier
```

**URL complète :** `s3://bucket-name/prefix/filename.ext`
### Régions AWS

AWS possède des data centers dans le monde entier. Choisissez une région proche de vos utilisateurs.

| Région | Code | Localisation |
|--------|------|-------------|
| Europe (Paris) | `eu-west-3` | **🇫🇷 Recommandé pour la France** |
| Europe (Francfort) | `eu-central-1` | Allemagne |
| Europe (Irlande) | `eu-west-1` | Irlande |
| US East | `us-east-1` | Virginie (le moins cher) |

**Important pour le RGPD :** Stocker les données personnelles européennes dans une région européenne.
### ✍️ Exercice 3.4 : Anatomie d'un chemin S3 (5 min)

Décomposez les chemins S3 suivants :

**Chemin 1 :** `s3://entreprise-analytics/data/sales/2024/Q4/revenue.parquet`

- Bucket : _____
- Prefix : _____
- Nom du fichier : _____

**Chemin 2 :** `s3://ml-models-prod/classification/customer_churn_v2.pkl`

- Bucket : _____
- Prefix : _____
- Nom du fichier : _____a