# Chapitre 9 : Chargement vers le cloud

**Durée estimée : 4-5h**

---

## Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

1. **Écrire** des données depuis Python vers AWS S3 en utilisant boto3 et pandas
2. **Organiser** un data lake avec une structure professionnelle (raw/processed/final)
3. **Documenter** un dataset avec un data dictionary complet

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
# df.to_csv("s3://bucket/path/file.csv", index=False)
# df.to_parquet("s3://bucket/path/file.parquet", index=False)

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
