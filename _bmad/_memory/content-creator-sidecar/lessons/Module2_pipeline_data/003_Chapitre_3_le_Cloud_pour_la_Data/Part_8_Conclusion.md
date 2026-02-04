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
   * Ressources à la demande, pay-as-you-go
   * AWS leader (30%), Azure (20%), GCP (13%)
   * Choisir une région européenne pour le RGPD

2. **AWS S3** :
   * Stockage objet = fichiers plats accessibles par API
   * Structure : `s3://bucket/prefix/fichier.ext`
   * Formats recommandés : CSV (dev), **Parquet (prod)**

3. **Sécurité** :
   * **Jamais de credentials dans le code**
   * Utiliser `~/.aws/credentials` ou variables d'environnement
   * Toujours IAM, jamais le compte root

4. **Python + S3** :
   * `boto3` pour les opérations bas niveau
   * `pandas` + `s3fs` pour lecture/écriture directe
   * Paginators pour lister > 1000 fichiers

---

## 🔗 Sources et références

* [Real Python - Python Boto3 AWS S3](https://realpython.com/python-boto3-aws-s3/)
* [Boto3 Official Documentation](https://boto3.amazonaws.com/v1/documentation/api/latest/guide/s3-examples.html)
* [GeekCafe - Mastering Data Processing on AWS (2025)](https://www.geekcafe.com/blog/2025/09/mastering-data-processing-on-aws-a-python-developers-guide-to-boto3-and-s3)
* [Statista - Cloud Market Share Q2 2025](https://www.statista.com/chart/18819/worldwide-market-share-of-leading-cloud-infrastructure-service-providers/)
* [Canalys - Global Cloud Q1 2025](https://canalys.com/newsroom/global-cloud-q1-2025)

---

## ➡️ Prochain chapitre

**Chapitre 4 : EDA diagnostique et qualité des données** — Vous apprendrez à évaluer la qualité de vos données et identifier les problèmes à corriger.

---

*Module 2 — Pipeline Data | Chapitre 3 sur 11*
