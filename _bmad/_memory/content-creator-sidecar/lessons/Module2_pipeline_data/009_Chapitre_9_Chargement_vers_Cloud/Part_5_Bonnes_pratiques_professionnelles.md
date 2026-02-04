# Chapitre 9 : Chargement vers le cloud

**Durée estimée : 4-5h**

---

## Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

1. **Écrire** des données depuis Python vers AWS S3 en utilisant boto3 et pandas
2. **Organiser** un data lake avec une structure professionnelle (raw/processed/final)
3. **Documenter** un dataset avec un data dictionary complet

---

## 9.5. Bonnes pratiques professionnelles

### 5.1 Règle d'or : Immutabilité des données brutes

**Ne jamais écraser les données brutes.** Jamais.

```python
# ❌ MAUVAIS : écrase le fichier original
# df_clean.to_csv("s3://bucket/raw/clients.csv")  # DANGER !

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
print(f"Nom du fichier : {nom_fichier}")
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
