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

## 🎯 Le Hook : Comment Netflix gère 700 millions d'heures de streaming par jour

En 2024, Netflix a diffusé plus de **700 millions d'heures de contenu par jour** à travers le monde. Imaginez les serveurs nécessaires pour stocker et distribuer autant de données... en temps réel, sans interruption.

La réponse ? **Ils n'ont pas de serveurs.** Du moins, pas les leurs.

Netflix utilise AWS (Amazon Web Services) pour stocker et distribuer ses contenus. Au lieu de construire des data centers coûteux, ils "louent" de la puissance de calcul et du stockage à la demande.

C'est ça, **le cloud** : accéder à des ressources informatiques comme on utilise l'électricité — vous payez ce que vous consommez.

> 💭 **Question Socratique #1** : Si le cloud permet d'accéder à des ressources "illimitées", pourquoi toutes les entreprises ne l'ont-elles pas encore adopté ? Quels freins peuvent exister ?

---

## 3.1 Introduction au cloud computing

### Qu'est-ce que le cloud ?

Le **cloud computing** consiste à accéder à des ressources informatiques (serveurs, stockage, bases de données, etc.) via Internet, sans posséder l'infrastructure physique.

```
┌─────────────────────────────────────────────────────────────┐
│                    AVANT (On-Premise)                       │
├─────────────────────────────────────────────────────────────┤
│  Vous devez :                                               │
│  • Acheter les serveurs physiques                          │
│  • Les installer dans un local climatisé                   │
│  • Les maintenir et les mettre à jour                      │
│  • Prévoir la capacité pour les pics d'activité            │
│  • Gérer la sécurité physique                              │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                    APRÈS (Cloud)                            │
├─────────────────────────────────────────────────────────────┤
│  Vous pouvez :                                              │
│  • Démarrer des ressources en quelques clics               │
│  • Payer uniquement ce que vous utilisez                   │
│  • Augmenter/réduire la capacité à la demande              │
│  • Accéder depuis n'importe où                             │
│  • Déléguer la maintenance au fournisseur                  │
└─────────────────────────────────────────────────────────────┘
```

### Les avantages du cloud

| Avantage | Description | Exemple |
|----------|-------------|---------|
| **Scalabilité** | Ajuster les ressources selon les besoins | Black Friday : x10 serveurs temporaires |
| **Pay-as-you-go** | Payer uniquement ce qu'on consomme | 0€ la nuit si aucun traitement |
| **Disponibilité** | Accès mondial 24/7 | Équipes à Paris, New York, Tokyo |
| **Maintenance déléguée** | Le fournisseur gère l'infra | Plus de serveur en panne à 3h du matin |
| **Innovation rapide** | Nouveaux services constamment | GPU pour ML sans achat matériel |

---

### Les trois grands fournisseurs cloud

Le marché est dominé par trois acteurs majeurs :

| Fournisseur | Part de marché (Q2 2025) | Croissance annuelle | Force principale |
|-------------|--------------------------|---------------------|------------------|
| **AWS** (Amazon) | **30%** | +17.5% | Leader historique, écosystème complet |
| **Azure** (Microsoft) | **20%** | +39% | Intégration Microsoft, IA (OpenAI) |
| **GCP** (Google) | **13%** | +32% | Data/Analytics, IA (Gemini) |

*(Source : [Statista - Cloud Market Share Q2 2025](https://www.statista.com/chart/18819/worldwide-market-share-of-leading-cloud-infrastructure-service-providers/))*

**Tendance 2025 :** Azure et GCP gagnent du terrain grâce à leurs offres IA (OpenAI/Gemini). AWS reste leader mais sa part diminue lentement (33% en 2021 → 30% en 2025).

---

### ✍️ Exercice 3.1 : Cloud ou local ? (10 min)

Pour chaque scénario, indiquez si vous recommanderiez le **cloud** ou une solution **locale (on-premise)**, et justifiez :

| Scénario | Cloud / Local | Justification |
|----------|---------------|---------------|
| Startup avec 3 data scientists et budget limité | _____ | _____ |
| Banque traitant des données ultra-sensibles (régulation stricte) | _____ | _____ |
| Site e-commerce avec pics saisonniers (Noël, soldes) | _____ | _____ |
| Hôpital devant conserver des données 30 ans | _____ | _____ |
| Projet ML expérimental de 3 mois | _____ | _____ |

---

> 💭 **Question Socratique #2** : AWS domine le marché avec 30% de parts, mais Azure croît 2x plus vite (+39% vs +17.5%). À votre avis, AWS risque-t-il de perdre sa position de leader ? Quels facteurs pourraient accélérer ou freiner ce changement ?

---

## 3.2 Types de stockage cloud

### Les trois catégories principales

```
┌──────────────────────────────────────────────────────────────────┐
│                    TYPES DE STOCKAGE CLOUD                       │
├────────────────────┬────────────────────┬────────────────────────┤
│   STOCKAGE OBJET   │  STOCKAGE FICHIER  │   BASES DE DONNÉES     │
│      (S3)          │    (EFS, FSx)      │      MANAGÉES          │
├────────────────────┼────────────────────┼────────────────────────┤
│ • Fichiers plats   │ • Système de       │ • SQL : RDS, Aurora    │
│ • Images, vidéos   │   fichiers partagé │ • NoSQL : DynamoDB     │
│ • Backups          │ • Compatible NFS   │ • Data Warehouse :     │
│ • Data lakes       │ • Multi-accès      │   Redshift             │
├────────────────────┼────────────────────┼────────────────────────┤
│ Accès : API/SDK    │ Accès : montage    │ Accès : requêtes SQL   │
│ Coût : très bas    │ Coût : moyen       │ Coût : selon usage     │
└────────────────────┴────────────────────┴────────────────────────┘
```

### Stockage objet : le standard pour la data

Le **stockage objet** (comme AWS S3) est le plus utilisé en data science car :

- ✅ **Coût très bas** : ~0.023$/Go/mois
- ✅ **Capacité illimitée** : pas de limite de taille
- ✅ **Durabilité** : 99.999999999% (11 neufs !)
- ✅ **Accès programmatique** : API et SDK Python

**Cas d'usage typiques :**
- Stockage de datasets bruts (data lake)
- Backup de bases de données
- Fichiers d'entraînement ML
- Logs et archives

---

### Formats de données recommandés

| Format | Avantages | Inconvénients | Quand l'utiliser |
|--------|-----------|---------------|------------------|
| **CSV** | Universel, lisible | Volumineux, pas de types | Échange, petits fichiers |
| **JSON** | Structures imbriquées | Volumineux | APIs, configs |
| **Parquet** | Compressé, colonnes, types | Moins lisible | **Production, gros volumes** |
| **Avro** | Schéma intégré | Complexe | Streaming |

**Recommandation pour la data science :**
- **Développement** : CSV pour la simplicité
- **Production** : **Parquet** pour les performances (3-10x plus petit que CSV)

---

### ✍️ Exercice 3.2 : Choix du format (10 min)

Quel format choisiriez-vous pour chaque situation ?

| Situation | Format recommandé | Raison |
|-----------|-------------------|--------|
| Envoyer des données à un collègue par email | _____ | _____ |
| Stocker 50 Go de données de ventes sur S3 | _____ | _____ |
| Recevoir des données depuis une API | _____ | _____ |
| Archiver des logs pour analyse future | _____ | _____ |

---

## 3.3 AWS S3 : concepts fondamentaux

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

---

### Régions AWS

AWS possède des data centers dans le monde entier. Choisissez une région proche de vos utilisateurs.

| Région | Code | Localisation |
|--------|------|--------------|
| Europe (Paris) | `eu-west-3` | **🇫🇷 Recommandé pour la France** |
| Europe (Francfort) | `eu-central-1` | Allemagne |
| Europe (Irlande) | `eu-west-1` | Irlande |
| US East | `us-east-1` | Virginie (le moins cher) |

**Important pour le RGPD :** Stocker les données personnelles européennes dans une région européenne.

---

### ✍️ Exercice 3.3 : Anatomie d'un chemin S3 (5 min)

Décomposez les chemins S3 suivants :

**Chemin 1 :** `s3://entreprise-analytics/data/sales/2024/Q4/revenue.parquet`

- Bucket : _____
- Prefix : _____
- Nom du fichier : _____

**Chemin 2 :** `s3://ml-models-prod/classification/customer_churn_v2.pkl`

- Bucket : _____
- Prefix : _____
- Nom du fichier : _____

---

## 3.4 Configuration AWS (Hands-on)

### Étape 1 : Créer un compte AWS

1. Allez sur [aws.amazon.com](https://aws.amazon.com)
2. Cliquez sur "Create an AWS Account"
3. Suivez les étapes (email, carte bancaire requise)
4. Activez le **Free Tier** (12 mois gratuits pour certains services)

**Free Tier S3 :**
- 5 Go de stockage gratuit
- 20 000 requêtes GET gratuites/mois
- 2 000 requêtes PUT gratuites/mois

---

### Étape 2 : Créer un utilisateur IAM

**⚠️ Ne jamais utiliser le compte root pour l'accès programmatique !**

1. Allez dans **IAM** (Identity and Access Management)
2. Créez un nouvel utilisateur
3. Attachez la policy `AmazonS3FullAccess`
4. Générez des **Access Keys**

```
Access Key ID:     AKIAIOSFODNN7EXAMPLE
Secret Access Key: wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY
```

---

### Étape 3 : Configurer les credentials (SÉCURITÉ)

**❌ JAMAIS dans le code :**
```python
# NE FAITES JAMAIS CECI !
aws_access_key = "AKIAIOSFODNN7EXAMPLE"
aws_secret_key = "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"
```

**✅ Option 1 : Fichier de configuration AWS (recommandé)**

Créez le fichier `~/.aws/credentials` :
```ini
[default]
aws_access_key_id = VOTRE_ACCESS_KEY
aws_secret_access_key = VOTRE_SECRET_KEY
```

Et `~/.aws/config` :
```ini
[default]
region = eu-west-3
output = json
```

**✅ Option 2 : Variables d'environnement**
```bash
export AWS_ACCESS_KEY_ID=VOTRE_ACCESS_KEY
export AWS_SECRET_ACCESS_KEY=VOTRE_SECRET_KEY
export AWS_DEFAULT_REGION=eu-west-3
```

**✅ Option 3 : Fichier .env (pour les projets)**
```
# .env (JAMAIS committé sur Git !)
AWS_ACCESS_KEY_ID=VOTRE_ACCESS_KEY
AWS_SECRET_ACCESS_KEY=VOTRE_SECRET_KEY
```

```python
from dotenv import load_dotenv
load_dotenv()  # Charge les variables depuis .env
```

---

### ⚠️ Les erreurs fatales à éviter

| Erreur | Conséquence | Solution |
|--------|-------------|----------|
| Committer des credentials sur Git | Compte compromis en minutes | `.gitignore` + secrets manager |
| Utiliser le compte root | Risque maximal si compromis | Toujours utiliser IAM |
| Permissions trop larges | Accès non autorisés | Principe du moindre privilège |
| Credentials en clair dans le code | Fuite de données | Variables d'environnement |

---

### ✍️ Exercice 3.4 : Sécurité des credentials (10 min)

Un collègue vous montre ce code. Identifiez **3 problèmes de sécurité** :

```python
import boto3

# Configuration
AWS_KEY = "AKIAIOSFODNN7EXAMPLE"
AWS_SECRET = "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"

s3 = boto3.client(
    "s3",
    aws_access_key_id=AWS_KEY,
    aws_secret_access_key=AWS_SECRET
)

# Upload fichier
s3.upload_file("data.csv", "mon-bucket-public", "data.csv")
```

**Problèmes identifiés :**
1. _____
2. _____
3. _____

**Comment corrigeriez-vous ce code ?**

---

> 💭 **Question Socratique #3** : Les grandes entreprises utilisent souvent des "secrets managers" (AWS Secrets Manager, HashiCorp Vault) pour gérer leurs credentials. Pourquoi ne pas simplement utiliser des variables d'environnement partout ?

---

## 3.5 Lire depuis S3 avec Python

### Installation de boto3

```bash
pip install boto3 pandas
```

### Méthode 1 : boto3 direct

```python
import boto3

# Créer un client S3 (utilise ~/.aws/credentials automatiquement)
s3 = boto3.client("s3")

# Télécharger un fichier
s3.download_file(
    Bucket="mon-bucket",
    Key="raw/ventes.csv",
    Filename="ventes_local.csv"
)
```

### Méthode 2 : pandas + S3 (recommandé)

```python
import pandas as pd

# Lecture directe depuis S3 (nécessite s3fs)
# pip install s3fs

df = pd.read_csv("s3://mon-bucket/raw/ventes.csv")

# Lecture Parquet
df = pd.read_parquet("s3://mon-bucket/processed/ventes.parquet")
```

### Méthode 3 : boto3 avec streaming (gros fichiers)

```python
import boto3
import pandas as pd
from io import BytesIO

s3 = boto3.client("s3")

# Récupérer l'objet en mémoire (sans télécharger sur disque)
response = s3.get_object(Bucket="mon-bucket", Key="raw/ventes.csv")
df = pd.read_csv(BytesIO(response["Body"].read()))
```

---

### Lister les fichiers d'un bucket

```python
import boto3

s3 = boto3.client("s3")

# Lister tous les objets d'un bucket
response = s3.list_objects_v2(Bucket="mon-bucket", Prefix="raw/")

for obj in response.get("Contents", []):
    print(f"{obj['Key']} - {obj['Size']} bytes")
```

**Pour plus de 1000 fichiers, utilisez un paginator :**

```python
paginator = s3.get_paginator("list_objects_v2")

for page in paginator.paginate(Bucket="mon-bucket", Prefix="raw/"):
    for obj in page.get("Contents", []):
        print(obj["Key"])
```

*(Source : [Mastering Data Processing on AWS - GeekCafe 2025](https://www.geekcafe.com/blog/2025/09/mastering-data-processing-on-aws-a-python-developers-guide-to-boto3-and-s3))*

---

### ✍️ Exercice 3.5 : Lecture S3 (15 min)

Complétez le code pour lire tous les fichiers CSV d'un dossier S3 et les combiner :

```python
import boto3
import pandas as pd
from io import BytesIO

s3 = boto3.client("s3")
bucket = "data-entreprise"
prefix = "sales/2024/"

# 1. Lister tous les fichiers CSV
response = s3.list_objects_v2(Bucket=_____, Prefix=_____)

# 2. Lire et combiner chaque fichier
dataframes = []
for obj in response.get("Contents", []):
    key = obj["Key"]
    if key.endswith(".csv"):
        print(f"Lecture de {key}...")
        response_file = s3.get_object(Bucket=_____, Key=_____)
        df = pd.read_csv(_____)
        dataframes.append(df)

# 3. Combiner tous les DataFrames
df_final = pd.concat(_____, ignore_index=True)
print(f"Total : {len(df_final)} lignes")
```

---

## 3.6 Écrire vers S3 avec Python

### Méthode 1 : boto3 direct

```python
import boto3

s3 = boto3.client("s3")

# Upload un fichier local
s3.upload_file(
    Filename="ventes_clean.csv",
    Bucket="mon-bucket",
    Key="processed/ventes_clean.csv"
)
```

### Méthode 2 : pandas + S3

```python
import pandas as pd

# Écriture CSV
df.to_csv("s3://mon-bucket/processed/ventes.csv", index=False)

# Écriture Parquet (recommandé)
df.to_parquet("s3://mon-bucket/processed/ventes.parquet")
```

### Méthode 3 : En mémoire (sans fichier temporaire)

```python
import boto3
from io import BytesIO

s3 = boto3.client("s3")

# Créer un buffer en mémoire
buffer = BytesIO()
df.to_parquet(buffer, index=False)
buffer.seek(0)  # Revenir au début du buffer

# Upload
s3.put_object(
    Bucket="mon-bucket",
    Key="processed/ventes.parquet",
    Body=buffer.getvalue()
)
```

---

### Bonnes pratiques d'écriture

| Pratique | Raison |
|----------|--------|
| Utiliser Parquet en production | 3-10x plus compact que CSV |
| Organiser en dossiers par date | Facilite les requêtes (partitioning) |
| Ne jamais écraser raw/ | Garder les données originales |
| Ajouter des métadonnées | Traçabilité (qui, quand, comment) |

---

### ✍️ Exercice 3.6 : Pipeline complet S3 (20 min)

Créez un mini-pipeline qui :
1. Lit un CSV depuis S3 (raw/)
2. Fait un nettoyage simple (supprime les doublons)
3. Sauvegarde en Parquet dans S3 (processed/)

```python
import boto3
import pandas as pd

# Configuration
bucket = "mon-data-lake"
input_key = "raw/clients.csv"
output_key = "processed/clients_clean.parquet"

# 1. Lecture depuis S3
df = pd.read_csv(f"s3://{bucket}/{input_key}")
print(f"Lignes avant nettoyage : {len(df)}")

# 2. Nettoyage (suppression doublons)
df_clean = df._____()
print(f"Lignes après nettoyage : {len(df_clean)}")

# 3. Sauvegarde vers S3
df_clean.to_parquet(f"s3://{_____}/{_____}")
print(f"Fichier sauvegardé : s3://{bucket}/{output_key}")
```

---

## 3.7 Panorama GCP et Azure (conceptuel)

### Équivalences entre clouds

| Fonction | AWS | GCP | Azure |
|----------|-----|-----|-------|
| **Stockage objet** | S3 | Cloud Storage | Blob Storage |
| **Base SQL managée** | RDS | Cloud SQL | Azure SQL |
| **Data Warehouse** | Redshift | BigQuery | Synapse |
| **Serverless compute** | Lambda | Cloud Functions | Azure Functions |
| **ML managé** | SageMaker | Vertex AI | Azure ML |
| **IAM** | IAM | IAM | Azure AD |

### Quand choisir quel cloud ?

| Critère | AWS | Azure | GCP |
|---------|-----|-------|-----|
| **Écosystème Microsoft** | ○ | ★★★ | ○ |
| **Startups / simplicité** | ★★ | ★ | ★★★ |
| **Big Data / Analytics** | ★★ | ★ | ★★★ |
| **IA générative** | ★ | ★★★ (OpenAI) | ★★★ (Gemini) |
| **Services les plus matures** | ★★★ | ★★ | ★★ |
| **Coût stockage** | ★★ | ★★ | ★★★ |

---

### 🔄 Point de vue alternatif : Multi-cloud

> **Alternative Viewpoint** : 89% des entreprises utilisent une stratégie multi-cloud (plusieurs fournisseurs). Pourquoi ? Pour éviter le "vendor lock-in" (dépendance à un seul fournisseur) et bénéficier des forces de chacun. Cependant, le multi-cloud ajoute de la complexité. Pour un débutant, **maîtriser un seul cloud (AWS) est suffisant** — vous pourrez transférer vos compétences facilement.

---

### ✍️ Exercice 3.7 : Équivalences cloud (10 min)

Une entreprise utilise actuellement ces services AWS. Trouvez les équivalents GCP :

| Service AWS | Équivalent GCP |
|-------------|----------------|
| S3 | _____ |
| RDS (PostgreSQL) | _____ |
| Redshift | _____ |
| Lambda | _____ |
| SageMaker | _____ |

---

## 🧠 Réflexion métacognitive

### Auto-évaluation

| Compétence | 1 | 2 | 3 | 4 | 5 |
|------------|---|---|---|---|---|
| Je comprends les avantages du cloud vs local | ○ | ○ | ○ | ○ | ○ |
| Je connais la structure de S3 (buckets, keys) | ○ | ○ | ○ | ○ | ○ |
| Je sais configurer les credentials de manière sécurisée | ○ | ○ | ○ | ○ | ○ |
| Je peux lire/écrire des données depuis S3 avec Python | ○ | ○ | ○ | ○ | ○ |
| Je connais les équivalences AWS/GCP/Azure | ○ | ○ | ○ | ○ | ○ |

### Questions de réflexion

1. **Quel aspect de la sécurité cloud** vous semble le plus important à maîtriser pour un data scientist ?

2. **Si vous deviez expliquer S3 à un non-technicien**, quelle analogie utiliseriez-vous ?

3. **Avez-vous identifié des zones d'ombre** dans votre compréhension du cloud ?

---

## 📚 Résumé du chapitre

### Points clés à retenir

1. **Cloud computing** :
   - Ressources à la demande, pay-as-you-go
   - AWS leader (30%), Azure (20%), GCP (13%)
   - Choisir une région européenne pour le RGPD

2. **AWS S3** :
   - Stockage objet = fichiers plats accessibles par API
   - Structure : `s3://bucket/prefix/fichier.ext`
   - Formats recommandés : CSV (dev), **Parquet (prod)**

3. **Sécurité** :
   - **Jamais de credentials dans le code**
   - Utiliser `~/.aws/credentials` ou variables d'environnement
   - Toujours IAM, jamais le compte root

4. **Python + S3** :
   - `boto3` pour les opérations bas niveau
   - `pandas` + `s3fs` pour lecture/écriture directe
   - Paginators pour lister > 1000 fichiers

---

## 🔗 Sources et références

- [Real Python - Python Boto3 AWS S3](https://realpython.com/python-boto3-aws-s3/)
- [Boto3 Official Documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/s3-examples.html)
- [GeekCafe - Mastering Data Processing on AWS (2025)](https://www.geekcafe.com/blog/2025/09/mastering-data-processing-on-aws-a-python-developers-guide-to-boto3-and-s3)
- [Statista - Cloud Market Share Q2 2025](https://www.statista.com/chart/18819/worldwide-market-share-of-leading-cloud-infrastructure-service-providers/)
- [Canalys - Global Cloud Q1 2025](https://canalys.com/newsroom/global-cloud-q1-2025)

---

## ➡️ Prochain chapitre

**Chapitre 4 : EDA diagnostique et qualité des données** — Vous apprendrez à évaluer la qualité de vos données et identifier les problèmes à corriger.

---

*Module 2 — Pipeline Data | Chapitre 3 sur 11*
