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

* ✅ **Coût très bas** : ~0.023$/Go/mois
* ✅ **Capacité illimitée** : pas de limite de taille
* ✅ **Durabilité** : 99.999999999% (11 neufs !)
* ✅ **Accès programmatique** : API et SDK Python

**Cas d'usage typiques :**

* Stockage de datasets bruts (data lake)
* Backup de bases de données
* Fichiers d'entraînement ML
* Logs et archives

---

### Formats de données recommandés

| Format | Avantages | Inconvénients | Quand l'utiliser |
|--------|-----------|---------------|------------------|
| **CSV** | Universel, lisible | Volumineux, pas de types | Échange, petits fichiers |
| **JSON** | Structures imbriquées | Volumineux | APIs, configs |
| **Parquet** | Compressé, colonnes, types | Moins lisible | **Production, gros volumes** |
| **Avro** | Schéma intégré | Complexe | Streaming |

**Recommandation pour la data science :**

* **Développement** : CSV pour la simplicité
* **Production** : **Parquet** pour les performances (3-10x plus petit que CSV)

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
