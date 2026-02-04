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

## 3.5 Configuration AWS (Hands-on)

### Étape 1 : Créer un compte AWS

1. Allez sur [aws.amazon.com](https://aws.amazon.com)
2. Cliquez sur "Create an AWS Account"
3. Suivez les étapes (email, carte bancaire requise)
4. Activez le **Free Tier** (12 mois gratuits pour certains services)

**Free Tier S3 :**
- 5 Go de stockage gratuit
- 20 000 requêtes GET gratuites/mois
- 2 000 requêtes PUT gratuites/mois

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
### Étape 3 : Configurer les credentials (SÉCURITÉ)

**❌ JAMAIS dans le code :**
# ❌ NE FAITES JAMAIS CECI ! (montré comme mauvais exemple)
# aws_access_key = "AKIAIOSFODNN7EXAMPLE"
# aws_secret_key = "wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY"

print("⚠️ Ne jamais mettre les credentials directement dans le code !")
**✅ Fichier de configuration AWS (recommandé)**

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
### ⚠️ Les erreurs fatales à éviter

| Erreur | Conséquence | Solution |
|--------|-------------|----------|
| Committer des credentials sur Git | Compte compromis en minutes | `.gitignore` + secrets manager |
| Utiliser le compte root | Risque maximal si compromis | Toujours utiliser IAM |
| Permissions trop larges | Accès non autorisés | Principe du moindre privilège |
| Credentials en clair dans le code | Fuite de données | Variables d'environnement |