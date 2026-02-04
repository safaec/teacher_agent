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

## 3.3 Panorama GCP et Azure (conceptuel)

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

### ✍️ Exercice 3.3 : Équivalences cloud (10 min)

Une entreprise utilise actuellement ces services AWS. Trouvez les équivalents GCP :

| Service AWS | Équivalent GCP |
|-------------|----------------|
| S3 | _____ |
| RDS (PostgreSQL) | _____ |
| Redshift | _____ |
| Lambda | _____ |
| SageMaker | _____ |

---