# Chapitre 2 — Extraction des données (sources locales)

**Durée estimée : 10-12 heures**

---

## Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

1. **Extraire** des données depuis différents formats de fichiers (CSV, Excel, JSON) en utilisant les paramètres appropriés de pandas
2. **Connecter** Python à une base de données SQL et exécuter des requêtes pour récupérer des données dans un DataFrame
3. **Consommer** des APIs REST en gérant l'authentification, la pagination et les limites de requêtes
4. **Évaluer** quand le web scraping est approprié et appliquer les principes éthiques et légaux

---

## 🎯 Le Hook : L'API qui a fait tomber un serveur

En 2019, un data scientist junior a lancé un script Python pour extraire des données d'une API publique. Sans pagination ni rate limiting, son script a envoyé **50 000 requêtes en 30 secondes**. Résultat : l'API a planté, son adresse IP a été bannie, et il a reçu une lettre d'avertissement du service juridique de l'entreprise.

Ce qu'il ignorait : **l'extraction de données est un art qui demande du respect** — respect des serveurs, des limites, et des règles.

Dans ce chapitre, vous apprendrez à extraire des données **efficacement et éthiquement**, que ce soit depuis un simple fichier CSV ou une API complexe.

> 💭 **Question Socratique #1** : Pourquoi pensez-vous que les APIs imposent des limites de requêtes (rate limiting) ? Est-ce uniquement pour protéger leurs serveurs, ou y a-t-il d'autres raisons ?

---

## 2.1 Extraction depuis fichiers avec pandas

### Introduction à pandas

**pandas** est LA bibliothèque Python pour la manipulation de données. Son objet central est le **DataFrame** : un tableau à deux dimensions avec des lignes et des colonnes nommées.

```python
import pandas as pd

# Créer un DataFrame simple
df = pd.DataFrame({
    'nom': ['Alice', 'Bob', 'Charlie'],
    'age': [25, 30, 35],
    'ville': ['Paris', 'Lyon', 'Marseille']
})
```

### Lecture de fichiers CSV

Le CSV (Comma-Separated Values) est le format le plus courant pour l'échange de données.

#### Syntaxe de base

```python
# Lecture simple
df = pd.read_csv("fichier.csv")

# Avec paramètres courants
df = pd.read_csv(
    "fichier.csv",
    encoding="utf-8",      # Encodage des caractères
    sep=";",               # Séparateur (virgule par défaut)
    decimal=","            # Séparateur décimal français
)
```

#### Paramètres essentiels

| Paramètre | Usage | Exemple |
|-----------|-------|---------|
| `encoding` | Gérer les accents | `"utf-8"`, `"latin-1"`, `"cp1252"` |
| `sep` | Changer le séparateur | `";"`, `"\t"` (tabulation) |
| `decimal` | Séparateur décimal | `","` pour les fichiers français |
| `header` | Ligne d'en-tête | `0` (défaut), `None` si pas d'en-tête |
| `usecols` | Sélectionner des colonnes | `["col1", "col2"]` ou `[0, 2, 5]` |
| `nrows` | Limiter les lignes lues | `1000` pour un aperçu |
| `skiprows` | Ignorer des lignes | `[0, 1]` ou `5` |
| `na_values` | Valeurs à traiter comme NaN | `["N/A", "Missing", "-"]` |

---

### ✍️ Exercice 2.1 : Diagnostic d'encodage (10 min)

Vous recevez un fichier CSV et obtenez cette erreur :
```
UnicodeDecodeError: 'utf-8' codec can't decode byte 0xe9 in position 42
```

1. Que signifie cette erreur ?
2. Quels encodages testeriez-vous en premier pour un fichier français ?
3. Écrivez le code pour lire le fichier avec l'encodage `latin-1` et un séparateur point-virgule.

**Votre code :**
```python
# Complétez :
df = pd.read_csv(_____)
```

---

### Lecture de fichiers Excel

Excel est omniprésent en entreprise. pandas le gère avec `read_excel()`.

```python
# Installation nécessaire
# pip install openpyxl

# Lecture simple
df = pd.read_excel("fichier.xlsx")

# Lecture d'une feuille spécifique
df = pd.read_excel("fichier.xlsx", sheet_name="Ventes2024")

# Lecture de plusieurs feuilles (retourne un dictionnaire)
dfs = pd.read_excel("fichier.xlsx", sheet_name=["Ventes", "Clients"])
# Accès : dfs["Ventes"], dfs["Clients"]

# Lecture de toutes les feuilles
dfs = pd.read_excel("fichier.xlsx", sheet_name=None)
```

#### Paramètres spécifiques Excel

| Paramètre | Usage | Exemple |
|-----------|-------|---------|
| `sheet_name` | Nom ou index de la feuille | `"Sheet1"`, `0`, `[0, 1]`, `None` |
| `header` | Ligne d'en-tête | `0`, `[0, 1]` pour multi-index |
| `skiprows` | Ignorer les premières lignes | `3` si métadonnées en haut |
| `engine` | Moteur de lecture | `"openpyxl"` (xlsx), `"xlrd"` (xls) |

---

### Lecture de fichiers JSON

JSON (JavaScript Object Notation) est le format standard des APIs et du web.

```python
# JSON plat (tableau d'objets)
df = pd.read_json("fichier.json")

# JSON imbriqué - nécessite normalisation
import json

with open("fichier.json") as f:
    data = json.load(f)

# Si structure imbriquée
df = pd.json_normalize(data, record_path="items")
```

#### Exemple de JSON imbriqué

```json
{
  "entreprise": "TechCorp",
  "employes": [
    {"nom": "Alice", "departement": {"nom": "IT", "etage": 3}},
    {"nom": "Bob", "departement": {"nom": "RH", "etage": 2}}
  ]
}
```

```python
# Normalisation
df = pd.json_normalize(
    data["employes"],
    meta=["nom"],
    record_prefix="dept_"
)
```

---

> 💭 **Question Socratique #2** : Les fichiers Excel peuvent contenir des formules, des graphiques, et des mises en forme. Que pensez-vous qu'il arrive à ces éléments quand vous lisez le fichier avec pandas ? Sont-ils préservés ?

---

### Lecture de fichiers volumineux (Chunking)

Quand un fichier dépasse la mémoire disponible, le **chunking** est la solution.

#### Le problème

```python
# ❌ Ceci peut faire planter votre machine avec un fichier de 10 Go
df = pd.read_csv("enorme_fichier.csv")
```

#### La solution : chunksize

```python
# ✅ Lecture par morceaux de 100 000 lignes
chunks = pd.read_csv("enorme_fichier.csv", chunksize=100_000)

# Traitement itératif
resultats = []
for chunk in chunks:
    # Traitement sur chaque chunk
    resultat = chunk.groupby("categorie")["ventes"].sum()
    resultats.append(resultat)

# Combinaison des résultats
final = pd.concat(resultats).groupby(level=0).sum()
```

#### Quand utiliser le chunking ?

| Taille du fichier | RAM disponible | Recommandation |
|-------------------|----------------|----------------|
| < 500 Mo | 8 Go | Lecture directe |
| 500 Mo - 2 Go | 8 Go | `usecols` + types optimisés |
| > 2 Go | 8 Go | **Chunking obligatoire** |
| > 10 Go | Toute RAM | Considérer Dask ou Spark |

*(Source : [pandas Documentation - Scaling](https://pandas.pydata.org/docs/user_guide/scale.html))*

---

### ✍️ Exercice 2.2 : Chunking pratique (15 min)

Vous avez un fichier `ventes_2024.csv` de 5 Go contenant des colonnes : `date`, `produit`, `categorie`, `quantite`, `prix_unitaire`, `client_id`.

Votre objectif : calculer le chiffre d'affaires total par catégorie.

1. Pourquoi ne pouvez-vous pas simplement faire `pd.read_csv("ventes_2024.csv")` ?
2. Complétez le code ci-dessous :

```python
# Votre code
chunks = pd.read_csv(
    "ventes_2024.csv",
    chunksize=_____,        # Combien de lignes par chunk ?
    usecols=_____           # Quelles colonnes sont nécessaires ?
)

ca_par_categorie = []
for chunk in chunks:
    # Calculez le CA (quantite * prix_unitaire) par catégorie
    chunk["ca"] = _____
    resultat = _____
    ca_par_categorie.append(resultat)

# Combinez les résultats
final = _____
```

---

### 🔄 Point de vue alternatif : pandas vs alternatives

> **Alternative Viewpoint** : Pour les très gros fichiers, certains experts recommandent d'abandonner pandas au profit de **Dask** (qui parallelise les opérations) ou **Polars** (écrit en Rust, très rapide). Cependant, pandas reste le standard de l'industrie et la courbe d'apprentissage de ces alternatives peut ralentir un débutant. Maîtrisez d'abord pandas, puis explorez les alternatives si nécessaire.

*(Source : [KDnuggets - Handle Large Datasets](https://www.kdnuggets.com/how-to-handle-large-datasets-in-python-even-if-youre-a-beginner))*

---

## 2.2 Extraction depuis bases de données SQL

### Pourquoi extraire depuis SQL ?

En entreprise, les données de production vivent dans des bases de données relationnelles (PostgreSQL, MySQL, SQL Server, Oracle). Savoir s'y connecter depuis Python est **essentiel**.

### Architecture de connexion

```
┌──────────────┐      ┌─────────────┐      ┌──────────────────┐
│    Python    │ ──→  │  SQLAlchemy │ ──→  │  Base de données │
│   (pandas)   │      │   (moteur)  │      │   (PostgreSQL)   │
└──────────────┘      └─────────────┘      └──────────────────┘
```

### Configuration de la connexion

```python
from sqlalchemy import create_engine

# Format de l'URL de connexion
# dialect+driver://username:password@host:port/database

# PostgreSQL
engine = create_engine("postgresql://user:motdepasse@localhost:5432/mabase")

# MySQL
engine = create_engine("mysql+pymysql://user:motdepasse@localhost:3306/mabase")

# SQLite (fichier local)
engine = create_engine("sqlite:///ma_base.db")
```

---

### ⚠️ Sécurité : Ne jamais hardcoder les credentials !

```python
# ❌ JAMAIS CECI (visible dans le code, committé sur Git)
engine = create_engine("postgresql://admin:SuperSecret123@prod.company.com/db")

# ✅ Utiliser des variables d'environnement
import os

DB_USER = os.environ.get("DB_USER")
DB_PASS = os.environ.get("DB_PASS")
DB_HOST = os.environ.get("DB_HOST")
DB_NAME = os.environ.get("DB_NAME")

engine = create_engine(f"postgresql://{DB_USER}:{DB_PASS}@{DB_HOST}/{DB_NAME}")
```

**Fichier `.env` (jamais committé) :**
```
DB_USER=monuser
DB_PASS=monmotdepasse
DB_HOST=localhost
DB_NAME=mabase
```

---

### Lecture avec pandas

```python
import pandas as pd
from sqlalchemy import create_engine

engine = create_engine("postgresql://user:pass@localhost/db")

# Requête simple
df = pd.read_sql("SELECT * FROM clients", engine)

# Requête avec filtres
df = pd.read_sql("""
    SELECT client_id, nom, email, date_inscription
    FROM clients
    WHERE date_inscription >= '2024-01-01'
    ORDER BY date_inscription DESC
""", engine)

# Jointures
df = pd.read_sql("""
    SELECT c.nom, c.email, COUNT(o.order_id) as nb_commandes
    FROM clients c
    LEFT JOIN orders o ON c.client_id = o.client_id
    GROUP BY c.client_id, c.nom, c.email
    HAVING COUNT(o.order_id) > 5
""", engine)
```

---

### ✍️ Exercice 2.3 : Requête SQL optimisée (15 min)

Vous devez extraire les ventes du dernier trimestre pour analyse. La table `ventes` contient 50 millions de lignes.

**Mauvaise approche :**
```python
# ❌ Charge TOUTES les données puis filtre en Python
df = pd.read_sql("SELECT * FROM ventes", engine)
df = df[df["date"] >= "2024-10-01"]
```

**Pourquoi c'est mauvais ?** _____

**Votre mission :** Réécrivez la requête pour :
1. Filtrer les dates côté SQL (pas Python)
2. Sélectionner uniquement les colonnes nécessaires : `date`, `produit_id`, `quantite`, `montant`
3. Limiter à 1 million de lignes maximum (sécurité)

```python
# Votre requête optimisée
df = pd.read_sql("""
    _____
""", engine)
```

---

> 💭 **Question Socratique #3** : Quand devriez-vous faire les transformations côté SQL (dans la requête) versus côté Python (après extraction) ? Quels critères utiliseriez-vous pour décider ?

---

## 2.3 Extraction via API REST

### Qu'est-ce qu'une API REST ?

Une **API** (Application Programming Interface) est un contrat qui définit comment deux systèmes communiquent. **REST** est un style architectural basé sur HTTP.

```
┌──────────────┐                      ┌──────────────┐
│    Client    │  ──── Requête ────→  │   Serveur    │
│   (Python)   │  ←─── Réponse ────   │    (API)     │
└──────────────┘                      └──────────────┘
```

### Les méthodes HTTP

| Méthode | Action | Exemple |
|---------|--------|---------|
| **GET** | Lire des données | Récupérer la liste des produits |
| **POST** | Créer des données | Ajouter un nouveau client |
| **PUT** | Mettre à jour (complet) | Modifier toutes les infos d'un client |
| **PATCH** | Mettre à jour (partiel) | Modifier uniquement l'email |
| **DELETE** | Supprimer | Supprimer un produit |

**Pour l'extraction de données, vous utiliserez principalement GET.**

---

### La bibliothèque requests

```python
import requests

# Requête GET simple
response = requests.get("https://api.example.com/products")

# Vérifier le statut
print(response.status_code)  # 200 = OK

# Récupérer les données JSON
data = response.json()

# Convertir en DataFrame
df = pd.DataFrame(data)
```

### Codes de statut HTTP importants

| Code | Signification | Action |
|------|---------------|--------|
| 200 | OK | Continuer |
| 201 | Created | Ressource créée |
| 400 | Bad Request | Vérifier vos paramètres |
| 401 | Unauthorized | Vérifier l'authentification |
| 403 | Forbidden | Vous n'avez pas les droits |
| 404 | Not Found | L'URL est incorrecte |
| **429** | **Too Many Requests** | **Rate limit atteint !** |
| 500 | Server Error | Problème côté serveur |

---

### Authentification par API Key

La plupart des APIs nécessitent une clé d'authentification.

```python
# Via header (méthode la plus courante)
headers = {
    "Authorization": "Bearer VOTRE_CLE_API",
    "Content-Type": "application/json"
}
response = requests.get("https://api.example.com/data", headers=headers)

# Via paramètre URL (moins sécurisé)
params = {
    "api_key": "VOTRE_CLE_API",
    "limit": 100
}
response = requests.get("https://api.example.com/data", params=params)
```

---

### ✍️ Exercice 2.4 : Première requête API (10 min)

Utilisez l'API publique JSONPlaceholder pour récupérer les 10 premiers posts.

**URL de base :** `https://jsonplaceholder.typicode.com`
**Endpoint :** `/posts`

```python
import requests
import pandas as pd

# 1. Faites la requête GET
response = _____

# 2. Vérifiez que le statut est 200
if response._____ == 200:
    # 3. Récupérez les données JSON
    data = _____

    # 4. Convertissez en DataFrame et affichez les 5 premières lignes
    df = _____
    print(df.head())
else:
    print(f"Erreur : {response.status_code}")
```

---

### Pagination : le défi des grandes quantités

Les APIs limitent le nombre de résultats par requête (souvent 100 ou 1000). Pour tout récupérer, vous devez **paginer**.

#### Types de pagination

| Type | Fonctionnement | Exemple |
|------|----------------|---------|
| **Offset/Limit** | Position + nombre | `?offset=100&limit=50` |
| **Page/Size** | Numéro de page | `?page=3&size=50` |
| **Cursor** | Pointeur vers le suivant | `?cursor=abc123` |

#### Exemple : Pagination offset/limit

```python
def extraire_toutes_donnees(base_url, limit=100):
    """Extrait toutes les données d'une API paginée."""
    all_data = []
    offset = 0

    while True:
        # Construire l'URL avec pagination
        url = f"{base_url}?offset={offset}&limit={limit}"
        response = requests.get(url, headers=headers)

        if response.status_code != 200:
            print(f"Erreur : {response.status_code}")
            break

        data = response.json()

        # Si pas de données, on a tout récupéré
        if not data:
            break

        all_data.extend(data)
        offset += limit

        print(f"Récupéré {len(all_data)} enregistrements...")

    return pd.DataFrame(all_data)
```

*(Source : [Speakeasy - Pagination Best Practices](https://www.speakeasy.com/api-design/pagination))*

---

### Rate Limiting : respecter les limites

Les APIs imposent des limites pour protéger leurs serveurs. **Les ignorer peut vous faire bannir.**

```python
import time

def extraire_avec_rate_limit(urls, requests_per_second=2):
    """Extrait des données en respectant le rate limit."""
    delay = 1 / requests_per_second
    all_data = []

    for url in urls:
        response = requests.get(url, headers=headers)

        if response.status_code == 429:  # Rate limit atteint
            # Vérifier si l'API indique combien attendre
            retry_after = response.headers.get("Retry-After", 60)
            print(f"Rate limit ! Attente de {retry_after} secondes...")
            time.sleep(int(retry_after))
            response = requests.get(url, headers=headers)  # Réessayer

        if response.status_code == 200:
            all_data.append(response.json())

        time.sleep(delay)  # Pause entre chaque requête

    return all_data
```

---

### 🤖 Utiliser un LLM pour comprendre une documentation d'API

Quand vous découvrez une nouvelle API, sa documentation peut être complexe. **Un LLM peut vous aider** :

**Prompt efficace :**
```
Voici la documentation de l'API X. J'ai besoin de :
1. Récupérer tous les utilisateurs actifs
2. Filtrer par date de création > 2024-01-01
3. Paginer les résultats

Génère-moi le code Python avec requests pour faire cela.
Inclus la gestion des erreurs et du rate limiting.
```

---

### ✍️ Exercice 2.5 : Extraction paginée complète (20 min)

L'API JSONPlaceholder a 100 posts au total. Écrivez un script qui :
1. Récupère tous les posts par lots de 10
2. Attend 0.5 seconde entre chaque requête
3. Affiche la progression
4. Retourne un DataFrame final

**Note :** Cette API ne supporte pas vraiment la pagination, mais simulez-la avec `_start` et `_limit`.

```python
import requests
import pandas as pd
import time

def extraire_tous_posts():
    base_url = "https://jsonplaceholder.typicode.com/posts"
    all_posts = []
    start = 0
    limit = 10

    while start < 100:  # On sait qu'il y a 100 posts
        # Votre code ici
        url = f"{base_url}?_start={_____}&_limit={_____}"

        response = _____

        if response.status_code == 200:
            posts = response.json()
            all_posts._____(posts)
            print(f"Récupéré posts {start} à {start + len(posts)}")

        start += _____
        time.sleep(_____)  # Respecter le rate limit

    return pd.DataFrame(all_posts)

# Test
df = extraire_tous_posts()
print(f"\nTotal : {len(df)} posts")
```

---

> 💭 **Question Socratique #4** : Certaines APIs offrent des webhooks (notifications push) au lieu de devoir faire des requêtes régulières (pull). Quels seraient les avantages et inconvénients de chaque approche pour un pipeline de données ?

---

## 2.4 Introduction au web scraping

### ⚠️ Avertissement important

Le web scraping est un outil puissant mais **juridiquement sensible**. Avant de scraper :

1. ✅ Vérifiez s'il existe une **API officielle**
2. ✅ Lisez les **CGU (Conditions Générales d'Utilisation)**
3. ✅ Respectez le fichier **robots.txt**
4. ✅ N'extrayez **jamais de données personnelles** sans base légale

### Le cadre légal français (CNIL, 2025)

La CNIL a publié en juin 2025 des directives claires :

> « Le traitement ne pourra pas entrer dans les attentes raisonnables des personnes si son responsable n'exclut pas de la collecte les sites qui s'opposent clairement au moissonnage par l'intermédiaire des protocoles d'exclusion robots.txt ou de CAPTCHA. »

**Amendes possibles :** Jusqu'à **20 millions d'euros** ou 4% du chiffre d'affaires mondial (RGPD).

**Exemple réel :** KASPR a été condamné à **240 000 €** pour avoir scrapé LinkedIn sans consentement.

*(Source : [CNIL - Web Scraping Guidance 2025](https://www.cnil.fr/en/legal-basis-legitimate-interests-focus-sheet-measures-implement-case-data-collection-web-scraping))*

---

### Vérifier robots.txt

Avant tout scraping, consultez `robots.txt` à la racine du site :

```
https://www.exemple.com/robots.txt
```

**Exemple de robots.txt :**
```
User-agent: *
Disallow: /admin/
Disallow: /private/
Crawl-delay: 10

User-agent: GPTBot
Disallow: /
```

**Interprétation :**
- Tous les bots (`*`) peuvent accéder au site sauf `/admin/` et `/private/`
- Ils doivent attendre **10 secondes** entre les requêtes
- GPTBot (OpenAI) est **totalement bloqué**

---

### BeautifulSoup : les bases

**Installation :**
```bash
pip install beautifulsoup4 requests
```

**Exemple simple :**
```python
import requests
from bs4 import BeautifulSoup

# Récupérer la page
url = "https://quotes.toscrape.com"  # Site de test
response = requests.get(url)

# Parser le HTML
soup = BeautifulSoup(response.content, "html.parser")

# Trouver des éléments
titre = soup.find("title").text
print(f"Titre : {titre}")

# Trouver tous les éléments d'une classe
quotes = soup.find_all("span", class_="text")
for quote in quotes[:3]:
    print(quote.text)
```

### Sélecteurs courants

| Méthode | Usage | Exemple |
|---------|-------|---------|
| `find()` | Premier élément | `soup.find("h1")` |
| `find_all()` | Tous les éléments | `soup.find_all("p")` |
| `select()` | Sélecteur CSS | `soup.select("div.content > p")` |
| `.text` | Texte de l'élément | `element.text` |
| `.get()` | Attribut | `link.get("href")` |

---

### ✍️ Exercice 2.6 : Scraping éthique (15 min)

Le site `https://quotes.toscrape.com` est conçu pour pratiquer le scraping.

1. Vérifiez d'abord son `robots.txt` : que dit-il ?
2. Extrayez les 5 premières citations avec leurs auteurs

```python
import requests
from bs4 import BeautifulSoup
import time

# 1. Vérifier robots.txt (manuellement ou via code)
robots_url = "https://quotes.toscrape.com/robots.txt"
print(requests.get(robots_url).text)

# 2. Scraper les citations
url = "https://quotes.toscrape.com"
response = requests.get(url)
soup = BeautifulSoup(response.content, "html.parser")

# Trouver les citations (classe "quote")
citations = soup.find_all("div", class_="_____")

for citation in citations[:5]:
    texte = citation.find("span", class_="_____").text
    auteur = citation.find("small", class_="_____").text
    print(f'"{texte}" - {auteur}')
```

---

### 🔄 Point de vue alternatif : Scraping vs APIs vs Datasets publics

> **Alternative Viewpoint** : Avant de scraper, posez-vous ces questions :
> 1. **L'API existe-t-elle ?** La plupart des grands sites ont des APIs officielles (Twitter, Reddit, etc.)
> 2. **Le dataset existe-t-il ?** Kaggle, data.gouv.fr, et autres plateformes offrent des données déjà extraites
> 3. **Puis-je demander ?** Contacter directement l'entreprise peut aboutir à un accès officiel
>
> Le scraping devrait être le **dernier recours**, pas le premier réflexe.

---

## 2.5 Inspection initiale des données

### Les premiers réflexes

Après avoir extrait vos données, **ne vous lancez jamais directement dans l'analyse**. D'abord, inspectez.

```python
# Les 5 commandes essentielles
df.shape        # Dimensions (lignes, colonnes)
df.dtypes       # Types de données
df.head()       # Premiers enregistrements
df.info()       # Résumé complet
df.describe()   # Statistiques numériques
```

### Exemple complet d'inspection

```python
import pandas as pd

# Après extraction
df = pd.read_csv("ventes.csv")

# 1. Dimensions
print(f"📊 Dimensions : {df.shape[0]} lignes × {df.shape[1]} colonnes")

# 2. Types de données
print("\n📝 Types de données :")
print(df.dtypes)

# 3. Aperçu
print("\n👀 Aperçu des données :")
print(df.head())

# 4. Résumé mémoire
print("\n💾 Informations mémoire :")
df.info()

# 5. Statistiques numériques
print("\n📈 Statistiques :")
print(df.describe())
```

---

### Checklist d'inspection initiale

| Vérification | Commande | Question |
|--------------|----------|----------|
| Nombre de lignes | `df.shape[0]` | Est-ce cohérent avec la source ? |
| Nombre de colonnes | `df.shape[1]` | Manque-t-il des colonnes ? |
| Types de données | `df.dtypes` | Les dates sont-elles reconnues ? |
| Valeurs nulles | `df.isnull().sum()` | Combien de données manquantes ? |
| Doublons | `df.duplicated().sum()` | Y a-t-il des lignes en double ? |
| Valeurs uniques | `df["col"].nunique()` | Combien de catégories ? |

---

### Documentation de la provenance (Data Lineage)

**Toujours documenter d'où viennent vos données !**

```python
# Exemple de métadonnées à conserver
metadata = {
    "source": "API CRM v2.3",
    "date_extraction": "2025-01-14 10:30:00",
    "nb_enregistrements": len(df),
    "colonnes": list(df.columns),
    "filtres_appliques": "date >= 2024-01-01",
    "responsable": "votre_nom@entreprise.com"
}

# Sauvegarder avec les données
import json
with open("ventes_metadata.json", "w") as f:
    json.dump(metadata, f, indent=2)
```

---

### ✍️ Exercice 2.7 : Inspection complète (15 min)

Vous recevez un fichier `clients.csv`. Réalisez l'inspection complète et répondez aux questions.

```python
import pandas as pd

# Simulation de données
df = pd.DataFrame({
    'client_id': [1, 2, 3, 4, 5, 5],  # Doublon !
    'nom': ['Alice', 'Bob', 'Charlie', None, 'Eve', 'Eve'],
    'email': ['alice@test.com', 'bob@test.com', None, 'david@test.com', 'eve@test.com', 'eve@test.com'],
    'age': [25, 30, 'trente-cinq', 40, 28, 28],  # Erreur de type !
    'date_inscription': ['2024-01-15', '2024-02-20', '2024-03-10', '2024-04-05', '2024-05-12', '2024-05-12']
})

# Votre inspection
print("1. Dimensions :", _____)
print("2. Types :\n", _____)
print("3. Valeurs nulles :\n", _____)
print("4. Doublons :", _____)

# Questions :
# a) Combien de valeurs manquantes au total ?
# b) Quel problème voyez-vous dans la colonne 'age' ?
# c) Combien de doublons exacts y a-t-il ?
```

---

## 🧠 Réflexion métacognitive

### Auto-évaluation

| Compétence | 1 | 2 | 3 | 4 | 5 |
|------------|---|---|---|---|---|
| Je sais lire des CSV/Excel/JSON avec les bons paramètres | ○ | ○ | ○ | ○ | ○ |
| Je peux utiliser le chunking pour les gros fichiers | ○ | ○ | ○ | ○ | ○ |
| Je sais connecter Python à une base SQL | ○ | ○ | ○ | ○ | ○ |
| Je peux consommer une API REST avec pagination | ○ | ○ | ○ | ○ | ○ |
| Je connais les règles éthiques/légales du scraping | ○ | ○ | ○ | ○ | ○ |
| Je sais inspecter des données avant analyse | ○ | ○ | ○ | ○ | ○ |

### Questions de réflexion

1. **Quelle méthode d'extraction vous semble la plus utile** pour votre futur métier ? Pourquoi ?

2. **Quelle partie vous a semblé la plus difficile ?** Comment pourriez-vous approfondir ce sujet ?

3. **Si vous deviez extraire des données demain** pour un projet professionnel, quelles questions poseriez-vous d'abord ?

---

## 📚 Résumé du chapitre

### Points clés à retenir

1. **Fichiers avec pandas** :
   - `read_csv()`, `read_excel()`, `read_json()` avec les bons paramètres
   - Chunking pour les fichiers > 2 Go
   - Toujours spécifier l'encodage pour les fichiers français

2. **Bases SQL** :
   - SQLAlchemy + `pd.read_sql()` pour la connexion
   - **Jamais de credentials dans le code** (variables d'environnement)
   - Filtrer côté SQL, pas côté Python

3. **APIs REST** :
   - Méthodes HTTP : GET pour lire, POST pour créer
   - Pagination : offset/limit ou cursor
   - **Toujours respecter le rate limiting** (429 = attendre)

4. **Web scraping** :
   - Vérifier robots.txt et CGU **avant** de scraper
   - RGPD : risque jusqu'à 20M€ ou 4% du CA
   - Préférer APIs officielles et datasets publics

5. **Inspection** :
   - `shape`, `dtypes`, `head()`, `info()`, `describe()`
   - Documenter la provenance (data lineage)

---

## 🔗 Sources et références

- [pandas Documentation - Scaling to Large Datasets](https://pandas.pydata.org/docs/user_guide/scale.html)
- [KDnuggets - Handle Large Datasets in Python](https://www.kdnuggets.com/how-to-handle-large-datasets-in-python-even-if-youre-a-beginner)
- [Speakeasy - API Pagination Best Practices](https://www.speakeasy.com/api-design/pagination)
- [CNIL - Web Scraping Guidance (June 2025)](https://www.cnil.fr/en/legal-basis-legitimate-interests-focus-sheet-measures-implement-case-data-collection-web-scraping)
- [Medium - Web Scraping GDPR Risks](https://medium.com/deep-tech-insights/web-scraping-in-2025-the-20-million-gdpr-mistake-you-cant-afford-to-make-07a3ce240f4f)

---

## ➡️ Prochain chapitre

**Chapitre 3 : Le Cloud pour la data** — Vous apprendrez à utiliser AWS S3 pour lire et écrire des données dans le cloud avec Python et boto3.

---

*Module 2 — Pipeline Data | Chapitre 2 sur 11*
