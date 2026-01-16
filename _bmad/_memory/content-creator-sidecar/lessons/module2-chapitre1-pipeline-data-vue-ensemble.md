# Chapitre 1 — Pipeline Data : Vue d'ensemble

**Durée estimée : 4-5 heures**

---

## Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

1. **Analyser** le cycle de vie complet des données et expliquer pourquoi la qualité des données conditionne la réussite de tout projet Data & IA
2. **Comparer** les approches ETL et ELT en identifiant les critères de choix appropriés selon le contexte
3. **Appliquer** la méthodologie CRISP-DM pour structurer un projet de data pipeline de manière professionnelle

---

## 🎯 Le Hook : Le mystère des 3 trillions de dollars

Imaginez : chaque année, les entreprises américaines perdent collectivement **plus de 3,1 trillions de dollars** à cause de données de mauvaise qualité. C'est l'équivalent du PIB de la France entière... qui disparaît dans des erreurs, des doublons, et des valeurs manquantes.

*(Source : IBM Research on Data Quality Costs)*

Mais voici le mystère : **pourquoi des entreprises dotées de data scientists brillants et de technologies de pointe échouent-elles encore ?**

La réponse se trouve dans ce que vous allez apprendre : **la pipeline de données**. Car même le meilleur algorithme de machine learning ne peut pas compenser des données corrompues à la source.

> 💭 **Question Socratique #1** : Selon vous, à quelle étape du processus les problèmes de données causent-ils le plus de dégâts — lors de la collecte, du stockage, ou de l'analyse ? Pourquoi ?

<details>
<summary>📖 Réponse suggérée</summary>

**Les problèmes causent le plus de dégâts lors de la collecte.**

Pourquoi ? C'est l'effet de propagation : une erreur à la source se multiplie à chaque étape suivante.

- **Lors de la collecte** : Si vous récupérez des données incorrectes, incomplètes ou mal formatées dès le départ, tout le reste du pipeline est compromis. C'est le principe "Garbage In, Garbage Out".
- **Lors du stockage** : Les erreurs à ce niveau (mauvais format, perte de données) sont graves mais plus facilement détectables.
- **Lors de l'analyse** : Les erreurs ici affectent les conclusions, mais les données sources restent intactes — on peut corriger et recommencer.

**Analogie** : C'est comme construire une maison. Une erreur dans les fondations (collecte) est catastrophique et coûteuse à corriger. Une erreur dans la peinture (analyse) est facilement réparable.

</details>

---

## 1.1 Position du module dans le parcours Data & IA

### Où en êtes-vous ?

Rappelons votre parcours de formation :

```
Module 1 (Prompt Engineering) → [VOUS ÊTES ICI] Module 2 (Data) → Module 3 (ML) → Module 4 (Visualisation)
```

Vous avez appris à communiquer efficacement avec l'IA dans le Module 1. Maintenant, vous allez comprendre **ce qui alimente cette IA** : les données.

### Le principe fondamental

Pensez à une recette de cuisine. Vous pouvez avoir le meilleur chef du monde (l'algorithme), la cuisine la mieux équipée (l'infrastructure cloud), mais si vos ingrédients sont avariés... le plat sera immangeable.

C'est exactement ce qui se passe en Data Science :

> **"Garbage In, Garbage Out"** (GIGO) — Si vous entrez des données de mauvaise qualité, vous obtiendrez des résultats de mauvaise qualité.

### L'impact chiffré de la qualité des données

| Métrique | Impact |
|----------|--------|
| Coût annuel moyen par entreprise | **12,9 millions $** de pertes dues à la mauvaise qualité *(Gartner)* |
| Temps perdu par employé | **50%** du temps passé à corriger des erreurs *(Gartner)* |
| Projets IA abandonnés d'ici 2026 | **60%** faute de données AI-ready *(Gartner)* |
| Perte de revenus | **20-30%** du chiffre d'affaires perdu par inefficacités data *(Gartner)* |

*(Sources : Gartner Cross-Industry Research, IBM Data Quality Studies)*

---

### ✍️ Exercice 1.1 : Réflexion personnelle (5 min)

Pensez à une situation où vous avez personnellement rencontré un problème de données (Excel corrompu, formulaire mal rempli, données manquantes...).

1. Décrivez brièvement la situation
2. Quel impact cela a-t-il eu sur votre travail ?
3. Comment avez-vous résolu le problème ?

*Notez vos réponses — elles vous serviront de référence tout au long du module.*

<details>
<summary>📖 Exemple de réponse</summary>

**Situation exemple** : Un fichier Excel de suivi des ventes comportait des dates dans différents formats (JJ/MM/AAAA, MM-JJ-AAAA, texte libre).

**Impact** :
- Impossible de trier chronologiquement
- Les graphiques Excel affichaient des erreurs
- 2 heures perdues à comprendre pourquoi les totaux mensuels étaient faux

**Résolution** :
- Création d'une colonne de date normalisée
- Utilisation d'une formule pour convertir tous les formats
- Mise en place d'une validation de données pour empêcher les nouveaux problèmes

**Leçon retenue** : Les problèmes de format de données sont invisibles jusqu'à ce qu'on essaie de les utiliser. La validation à l'entrée est essentielle.

</details>

---

> 💭 **Question Socratique #2** : Si 60% des projets IA échouent faute de données de qualité, pourquoi les entreprises n'investissent-elles pas massivement dans la préparation des données avant de se lancer dans le ML ?

<details>
<summary>📖 Réponse suggérée</summary>

Plusieurs facteurs expliquent ce paradoxe :

1. **Le "glamour" du ML vs le "travail ingrat" des données**
   - Les algorithmes de machine learning sont perçus comme innovants et excitants
   - Le nettoyage de données est vu comme ennuyeux et peu valorisant
   - Les dirigeants préfèrent annoncer "nous utilisons l'IA" que "nous avons nettoyé nos données"

2. **Sous-estimation systématique**
   - La règle des 80/20 : 80% du temps d'un projet data est consacré à la préparation des données
   - Les entreprises planifient souvent l'inverse (80% modélisation, 20% données)

3. **Problème de visibilité**
   - Un modèle ML qui prédit bien est visible et mesurable
   - La qualité des données est invisible jusqu'à ce qu'elle pose problème

4. **Pression du marché**
   - "La concurrence utilise l'IA, nous devons faire pareil — vite !"
   - Les fondamentaux sont sacrifiés pour la vitesse

**Leçon clé** : Les entreprises qui réussissent en data science investissent d'abord dans l'infrastructure data, puis dans les modèles.

</details>

---

## 1.2 ETL vs ELT : Deux philosophies, deux époques

### L'approche classique : ETL (Extract → Transform → Load)

**ETL** signifie :
- **E**xtract : Récupérer les données depuis diverses sources
- **T**ransform : Nettoyer et transformer les données
- **L**oad : Charger les données transformées dans l'entrepôt

```
[Sources] → EXTRACT → TRANSFORM → LOAD → [Data Warehouse]
                         ↑
              (Traitement AVANT stockage)
```

**Caractéristiques :**
- Les données sont nettoyées **avant** d'arriver dans l'entrepôt
- Nécessite de définir les transformations à l'avance
- Adapté aux **data warehouses** traditionnels (structures rigides)
- Dominant dans les années 1990-2010

### L'approche moderne : ELT (Extract → Load → Transform)

**ELT** inverse le processus :
- **E**xtract : Récupérer les données
- **L**oad : Charger les données **brutes** dans le stockage cloud
- **T**ransform : Transformer après, directement dans le cloud

```
[Sources] → EXTRACT → LOAD → TRANSFORM → [Data Lake/Lakehouse]
                               ↑
                (Traitement APRÈS stockage)
```

**Caractéristiques :**
- Les données brutes sont préservées (on peut toujours revenir en arrière)
- Transformations flexibles selon les besoins
- Exploite la puissance de calcul du cloud
- Dominant depuis 2015+

---

### 📊 Le marché en 2025 : Les chiffres clés

| Indicateur | Valeur |
|------------|--------|
| Marché mondial des outils ETL/ELT | **14,76 milliards $** (croissance 26,8%/an) |
| Part du cloud dans les déploiements | **65%** |
| Entreprises avec stratégie multi-cloud | **89%** |
| Adoption de l'architecture medallion (Bronze/Silver/Gold) | **68%** des entreprises cloud-first |

*(Source : Integrate.io ETL/ELT Statistics 2025, Gartner Cloud Predictions)*

---

### Quand choisir ETL vs ELT ?

| Critère | ETL | ELT |
|---------|-----|-----|
| **Infrastructure** | Data warehouse on-premise | Data lake cloud |
| **Volume de données** | Modéré | Très élevé |
| **Besoins de transformation** | Bien définis à l'avance | Évolutifs |
| **Conformité réglementaire** | Strict (banque, santé) | Flexible |
| **Coût initial** | Élevé | Faible (pay-as-you-go) |
| **Exemple secteur** | Banque traditionnelle | Startup tech |

---

### 🔄 Point de vue alternatif : La convergence ETL/ELT

> **Alternative Viewpoint** : Certains experts affirment que le débat ETL vs ELT devient obsolète. Avec les architectures **lakehouse** modernes (comme Databricks ou Snowflake), les deux approches coexistent au sein d'une même plateforme unifiée. L'important n'est plus *où* transformer, mais *comment* garantir la qualité à chaque étape.

*(Source : Pure Storage Technical Blog 2025)*

---

### ✍️ Exercice 1.2 : Analyse de cas (10 min)

Pour chaque scénario, indiquez si vous recommanderiez **ETL** ou **ELT**, et justifiez votre choix :

**Cas A** : Une banque française doit produire des rapports réglementaires quotidiens pour l'AMF. Les formats sont stricts et ne changent jamais.

Votre réponse : _____ Justification : _____

**Cas B** : Une startup e-commerce veut analyser le comportement de ses utilisateurs. Elle ne sait pas encore quelles métriques seront les plus utiles.

Votre réponse : _____ Justification : _____

**Cas C** : Un hôpital doit croiser des données patients de 15 systèmes différents pour la recherche clinique, tout en respectant le RGPD.

Votre réponse : _____ Justification : _____

<details>
<summary>📖 Réponses</summary>

**Cas A — Banque française / rapports AMF**
- **Réponse : ETL**
- **Justification** :
  - Formats stricts et immuables → transformations définies à l'avance
  - Réglementation financière stricte → traçabilité complète requise
  - Données sensibles → contrôle maximal avant stockage
  - Le secteur bancaire utilise traditionnellement des data warehouses on-premise

**Cas B — Startup e-commerce / analyse comportementale**
- **Réponse : ELT**
- **Justification** :
  - Besoins exploratoires ("ne sait pas encore") → flexibilité requise
  - Startup = budget limité → cloud pay-as-you-go
  - Besoin d'itérer rapidement → transformer après permet de tester différentes approches
  - Volume de données utilisateurs potentiellement massif → scalabilité cloud

**Cas C — Hôpital / recherche clinique RGPD**
- **Réponse : ETL (ou hybride)**
- **Justification** :
  - RGPD impose des règles strictes sur les données de santé → pseudonymisation/anonymisation AVANT stockage
  - Données sensibles médicales → transformation préalable pour conformité
  - Cependant, pour la recherche, une approche hybride est possible : ETL pour la conformité initiale, puis ELT dans un environnement sécurisé pour l'analyse
  - Les hôpitaux ont souvent des infrastructures legacy → ETL plus compatible

</details>

---

> 💭 **Question Socratique #3** : Si 65% du marché migre vers le cloud et l'ELT, pourquoi le secteur bancaire (28% du marché ETL) reste-t-il attaché à l'approche traditionnelle ? Est-ce uniquement une question de réglementation, ou y a-t-il d'autres facteurs ?

<details>
<summary>📖 Réponse suggérée</summary>

**Ce n'est pas uniquement une question de réglementation.** Plusieurs facteurs entrent en jeu :

1. **Réglementation stricte (facteur principal)**
   - Les banques sont soumises à des exigences de conformité (Bâle III, RGPD, lois anti-blanchiment)
   - Les régulateurs exigent de savoir exactement où sont les données et comment elles sont transformées
   - L'ETL offre cette traçabilité car les transformations sont documentées avant le stockage

2. **Systèmes legacy (infrastructure existante)**
   - Les grandes banques ont investi des milliards dans des data warehouses Oracle, Teradata, etc.
   - Migrer vers le cloud représente un coût et un risque énormes
   - "If it ain't broke, don't fix it" — surtout avec des systèmes critiques

3. **Aversion au risque culturelle**
   - Le secteur bancaire valorise la stabilité et la prévisibilité
   - Les nouvelles technologies sont adoptées lentement, après avoir été prouvées ailleurs

4. **Exigences de performance garantie**
   - Les banques ont besoin de temps de réponse garantis (SLA stricts)
   - Les systèmes on-premise offrent un contrôle total sur les performances

5. **Souveraineté des données**
   - Certains pays exigent que les données financières restent sur le territoire national
   - Le cloud public pose des questions de juridiction

**Conclusion** : La réglementation est un facteur majeur, mais l'inertie des systèmes legacy et la culture d'aversion au risque sont tout aussi déterminantes.

</details>

---

## 1.3 Les étapes de la pipeline de données

### Vue d'ensemble du flux

Voici le parcours complet que suivent vos données :

```
┌─────────────┐    ┌─────────────────┐    ┌────────────┐    ┌───────────────┐
│ EXTRACTION  │ →  │ EDA DIAGNOSTIQUE│ →  │ NETTOYAGE  │ →  │ STRUCTURATION │
│ (Chap. 2-3) │    │   (Chap. 4)     │    │ (Chap. 5)  │    │   (Chap. 6)   │
└─────────────┘    └─────────────────┘    └────────────┘    └───────────────┘
                                                                    ↓
┌─────────────┐    ┌─────────────────┐    ┌────────────┐    ┌───────────────┐
│ CHARGEMENT  │ ←  │ VISUALISATION   │ ←  │    EDA     │ ←  │ TRANSFORMATION│
│  (Chap. 9)  │    │   (Chap. 8)     │    │ ANALYTIQUE │    │   (Chap. 6)   │
└─────────────┘    └─────────────────┘    │ (Chap. 7)  │    └───────────────┘
                                          └────────────┘
```

### Description de chaque étape

| Étape | Objectif | Question clé |
|-------|----------|--------------|
| **Extraction** | Récupérer les données depuis leurs sources | *D'où viennent mes données ?* |
| **EDA Diagnostique** | Identifier les problèmes de qualité | *Qu'est-ce qui ne va pas ?* |
| **Nettoyage** | Corriger les problèmes identifiés | *Comment réparer ?* |
| **Structuration** | Organiser pour l'analyse | *Quelle forme finale ?* |
| **EDA Analytique** | Comprendre les patterns | *Que disent les données ?* |
| **Visualisation** | Communiquer les insights | *Comment montrer ?* |
| **Chargement** | Rendre disponible | *Où stocker le résultat ?* |

### La notion d'itération

**Important** : Ce processus n'est **pas linéaire**. En réalité, vous ferez de nombreux allers-retours :

- Pendant le nettoyage, vous découvrirez de nouveaux problèmes → retour à l'EDA diagnostique
- Pendant l'analyse, vous aurez besoin de nouvelles variables → retour à la structuration
- Après visualisation, vous identifierez des anomalies → retour au nettoyage

> C'est normal. C'est même souhaitable. Chaque itération améliore la qualité finale.

---

### ✍️ Exercice 1.3 : Mapping des chapitres (5 min)

Vous recevez un fichier CSV contenant les ventes d'une entreprise. Le fichier a 50 000 lignes et vous suspectez des problèmes de qualité.

Numérotez les actions suivantes dans l'ordre où vous les effectueriez :

- [ ] Créer un graphique des ventes par mois
- [ ] Télécharger le fichier depuis le serveur
- [ ] Remplacer les valeurs manquantes par la médiane
- [ ] Vérifier combien de lignes ont des valeurs nulles
- [ ] Fusionner avec le fichier des produits pour avoir les catégories
- [ ] Uploader le fichier nettoyé sur S3

<details>
<summary>📖 Réponse</summary>

**Ordre correct :**

| Ordre | Action | Étape de la pipeline |
|-------|--------|---------------------|
| **1** | Télécharger le fichier depuis le serveur | Extraction |
| **2** | Vérifier combien de lignes ont des valeurs nulles | EDA Diagnostique |
| **3** | Remplacer les valeurs manquantes par la médiane | Nettoyage |
| **4** | Fusionner avec le fichier des produits pour avoir les catégories | Structuration |
| **5** | Créer un graphique des ventes par mois | Visualisation |
| **6** | Uploader le fichier nettoyé sur S3 | Chargement |

**Explication du raisonnement :**
1. On ne peut rien faire sans d'abord **extraire** les données
2. Avant de nettoyer, il faut **diagnostiquer** les problèmes (valeurs nulles)
3. Le **nettoyage** corrige les problèmes identifiés
4. La **structuration** enrichit les données (fusion avec catégories)
5. La **visualisation** permet de communiquer les insights
6. Le **chargement** rend les données disponibles pour d'autres utilisateurs

</details>

---

## 1.4 Méthodologie CRISP-DM

### Le standard de l'industrie

**CRISP-DM** (Cross-Industry Standard Process for Data Mining) est la méthodologie la plus utilisée en data science depuis sa création en 1999.

> Une étude de 2009 l'a qualifiée de **"standard de facto pour les projets de data mining"**, et les sondages KDnuggets de 2002 à 2014 confirment qu'elle reste la méthodologie dominante.

*(Source : Wikipedia - CRISP-DM, DataScience-PM.com)*

### Les 6 phases de CRISP-DM

```
        ┌─────────────────────┐
        │  1. COMPRÉHENSION   │
        │      MÉTIER         │
        └──────────┬──────────┘
                   ↓
        ┌─────────────────────┐
        │  2. COMPRÉHENSION   │←──────────────┐
        │     DES DONNÉES     │               │
        └──────────┬──────────┘               │
                   ↓                          │
        ┌─────────────────────┐               │
        │  3. PRÉPARATION     │               │
        │     DES DONNÉES     │──────→ Itération
        └──────────┬──────────┘               │
                   ↓                          │
        ┌─────────────────────┐               │
        │  4. MODÉLISATION    │───────────────┘
        └──────────┬──────────┘
                   ↓
        ┌─────────────────────┐
        │  5. ÉVALUATION      │
        └──────────┬──────────┘
                   ↓
        ┌─────────────────────┐
        │  6. DÉPLOIEMENT     │
        └─────────────────────┘
```

### Correspondance avec notre module

| Phase CRISP-DM | Chapitres du Module 2 |
|----------------|----------------------|
| Compréhension métier | Chapitre 1 (contexte) |
| Compréhension des données | Chapitres 2-4 (extraction, EDA diagnostique) |
| Préparation des données | Chapitres 5-6 (nettoyage, structuration) |
| Modélisation | *Module 3 (ML)* |
| Évaluation | Chapitre 7 (EDA analytique) |
| Déploiement | Chapitres 8-9 (visualisation, chargement) |

---

### Forces et limites de CRISP-DM

**Forces :**
- ✅ Étapes logiques et faciles à comprendre
- ✅ Vocabulaire partagé entre équipes
- ✅ Applicable à tous les secteurs et outils
- ✅ Flexible et adaptable

**Limites :**
- ⚠️ Structure linéaire parfois rigide pour les projets agiles
- ⚠️ Ne définit pas les rôles d'équipe
- ⚠️ Manque de structure de communication avec les parties prenantes

*(Source : DataScience-PM Research 2025)*

---

### 🔄 Point de vue alternatif : CRISP-DM + Agile

> **Alternative Viewpoint** : Les équipes data science les plus performantes combinent CRISP-DM avec des pratiques agiles comme Scrum ou Kanban. CRISP-DM fournit le cadre méthodologique (les phases), tandis que l'agilité gère l'exécution (sprints, priorités, feedback continu).

*(Source : DataScience-PM Best Practices 2025)*

---

### ✍️ Exercice 1.4 : Application CRISP-DM (15 min)

**Contexte** : Une chaîne de supermarchés vous demande de prédire quels produits seront en rupture de stock la semaine prochaine.

Pour chaque phase CRISP-DM, listez 2-3 questions que vous poseriez ou actions que vous feriez :

1. **Compréhension métier** :
   - Question 1 : _____
   - Question 2 : _____

2. **Compréhension des données** :
   - Action 1 : _____
   - Action 2 : _____

3. **Préparation des données** :
   - Action 1 : _____
   - Action 2 : _____

4. **Évaluation** :
   - Critère 1 : _____
   - Critère 2 : _____

<details>
<summary>📖 Réponse suggérée</summary>

**1. Compréhension métier :**
- Quel est le coût d'une rupture de stock vs le coût du surstockage ?
- Quels produits sont prioritaires (marge élevée, produits essentiels) ?
- Quel délai d'anticipation est nécessaire (1 jour, 3 jours, 1 semaine) ?
- Qui utilisera les prédictions et comment ?

**2. Compréhension des données :**
- Inventorier les sources disponibles : historique des ventes, stocks actuels, commandes fournisseurs, calendrier promotionnel
- Vérifier la qualité : données manquantes, erreurs de saisie, granularité temporelle
- Explorer les patterns : saisonnalité, effet des promotions, jour de la semaine

**3. Préparation des données :**
- Nettoyer les anomalies (ventes négatives, stocks négatifs)
- Créer des features : moyenne mobile des ventes, jours depuis dernière livraison, indicateur promotion
- Joindre les données produits (catégorie, fournisseur, délai de livraison)
- Gérer les nouveaux produits sans historique

**4. Évaluation :**
- **Précision** : % de ruptures correctement prédites
- **Rappel** : % des vraies ruptures détectées (critique !)
- **Faux positifs** : coût de commander trop vs coût de rupture
- **Horizon temporel** : le modèle prédit-il assez tôt pour agir ?
- **Test A/B** : comparaison avec la méthode actuelle sur un échantillon de magasins

</details>

---

> 💭 **Question Socratique #4** : CRISP-DM date de 1999, soit avant l'explosion du Big Data et du cloud. Est-il encore pertinent aujourd'hui, ou devrait-il être remplacé par une méthodologie plus moderne ? Quels éléments ajouteriez-vous ?

<details>
<summary>📖 Réponse suggérée</summary>

**CRISP-DM reste pertinent, mais mérite des ajouts pour l'ère moderne.**

**Pourquoi il reste pertinent :**
- Les 6 phases sont universelles et intemporelles (comprendre le métier, les données, préparer, modéliser, évaluer, déployer)
- La logique itérative reflète toujours la réalité des projets data
- Le vocabulaire partagé facilite la communication entre équipes

**Ce qu'on pourrait ajouter pour 2025+ :**

1. **Phase "DataOps / MLOps"**
   - Intégration continue des données (CI/CD pour les pipelines)
   - Monitoring des modèles en production
   - Gestion du drift (dérive des données/modèles)

2. **Phase "Éthique & Gouvernance"**
   - Évaluation des biais dans les données et modèles
   - Conformité RGPD / respect de la vie privée
   - Explicabilité des modèles (XAI)

3. **Phase "Scalabilité Cloud"**
   - Architecture distribuée (Spark, Dask)
   - Gestion des coûts cloud
   - Orchestration (Airflow, Prefect)

4. **Collaboration temps réel**
   - Intégration avec les outils agiles (Scrum, Kanban)
   - Feature stores partagés
   - Versioning des données et modèles (DVC, MLflow)

**Conclusion** : CRISP-DM est une fondation solide. Plutôt que de le remplacer, on l'enrichit avec des pratiques modernes comme MLOps et DataOps.

</details>

---

## 1.5 Sources de données en entreprise

### Panorama des sources

En entreprise, vos données peuvent provenir de multiples endroits :

```
┌─────────────────────────────────────────────────────────────────┐
│                    SOURCES DE DONNÉES                           │
├─────────────────┬─────────────────┬─────────────────────────────┤
│   FICHIERS      │   BASES DE      │        EXTERNES             │
│   LOCAUX        │   DONNÉES       │                             │
├─────────────────┼─────────────────┼─────────────────────────────┤
│ • CSV           │ • PostgreSQL    │ • APIs REST                 │
│ • Excel         │ • MySQL         │ • Web scraping              │
│ • JSON          │ • SQL Server    │ • Cloud (S3, GCS, Azure)    │
│ • XML           │ • MongoDB       │ • Données gouvernementales  │
│ • Parquet       │ • Oracle        │ • APIs partenaires          │
└─────────────────┴─────────────────┴─────────────────────────────┘
```

### Ce que vous apprendrez

| Source | Chapitre | Compétences acquises |
|--------|----------|---------------------|
| Fichiers (CSV, Excel, JSON) | Chapitre 2 | `pd.read_csv()`, `pd.read_excel()`, `pd.read_json()` |
| Bases SQL | Chapitre 2 | `sqlalchemy`, `pd.read_sql()` |
| APIs REST | Chapitre 2 | `requests`, authentification, pagination |
| Web scraping | Chapitre 2 | `BeautifulSoup` (bases uniquement) |
| Cloud AWS S3 | Chapitre 3 | `boto3`, lecture/écriture S3 |

---

### ✍️ Exercice 1.5 : Identification des sources (10 min)

Pour chaque besoin métier, identifiez la ou les sources de données les plus probables :

| Besoin métier | Source(s) probable(s) |
|---------------|----------------------|
| Historique des ventes des 3 dernières années | _____ |
| Cours de bourse en temps réel | _____ |
| Liste des clients avec emails | _____ |
| Données météo historiques | _____ |
| Rapports financiers exportés par un collègue | _____ |
| Catalogue produits d'un concurrent | _____ |

<details>
<summary>📖 Réponses</summary>

| Besoin métier | Source(s) probable(s) | Justification |
|---------------|----------------------|---------------|
| Historique des ventes des 3 dernières années | **Base de données SQL** (ERP, CRM) ou **Data Warehouse** | Données transactionnelles structurées, stockées dans les systèmes internes |
| Cours de bourse en temps réel | **API REST** (Yahoo Finance, Alpha Vantage, Bloomberg) | Données externes mises à jour en continu, accessibles via API |
| Liste des clients avec emails | **Base de données SQL** (CRM) ou **Export CSV/Excel** | Données internes, souvent dans Salesforce, HubSpot ou base CRM maison |
| Données météo historiques | **API externe** (OpenWeatherMap, Météo-France) ou **Données gouvernementales** (data.gouv.fr) | Données publiques disponibles via API ou téléchargement |
| Rapports financiers exportés par un collègue | **Fichiers Excel/CSV** (local ou partagé) | Export manuel depuis un système comptable |
| Catalogue produits d'un concurrent | **Web scraping** (avec précautions légales/éthiques) ou **Achat de données** | Données externes non-accessibles autrement ; attention au respect des CGU |

**Note importante** : Pour le web scraping, toujours vérifier :
- Les conditions d'utilisation du site
- Le fichier robots.txt
- Les réglementations locales (RGPD, etc.)

</details>

---

## 🧠 Réflexion métacognitive

Avant de terminer ce chapitre, prenez un moment pour réfléchir à votre apprentissage :

### Auto-évaluation

Pour chaque concept, évaluez votre niveau de confiance (1 = pas du tout confiant, 5 = très confiant) :

| Concept | 1 | 2 | 3 | 4 | 5 |
|---------|---|---|---|---|---|
| J'ai compris pourquoi la qualité des données est critique | ○ | ○ | ○ | ○ | ○ |
| Je peux expliquer la différence ETL vs ELT | ○ | ○ | ○ | ○ | ○ |
| Je connais les étapes d'une pipeline de données | ○ | ○ | ○ | ○ | ○ |
| Je peux appliquer CRISP-DM à un nouveau projet | ○ | ○ | ○ | ○ | ○ |
| Je sais identifier les sources de données appropriées | ○ | ○ | ○ | ○ | ○ |

### Questions de réflexion

1. **Quel concept vous a semblé le plus difficile ?** Pourquoi pensez-vous que c'était difficile ?

2. **Quel concept vous a le plus surpris ?** (statistiques, approches, etc.)

3. **Comment allez-vous utiliser ces connaissances** dans votre futur métier ?

---

## 📚 Résumé du chapitre

### Points clés à retenir

1. **La qualité des données est non négociable** : 12,9M$ de pertes annuelles moyennes par entreprise, 60% des projets IA échouent faute de données de qualité

2. **ETL vs ELT** :
   - ETL = transformer avant de stocker (classique, data warehouse)
   - ELT = stocker puis transformer (moderne, cloud, data lake)
   - 65% du marché migre vers le cloud et l'ELT

3. **La pipeline n'est pas linéaire** : attendez-vous à de nombreuses itérations entre les étapes

4. **CRISP-DM reste le standard** : 6 phases, flexible, applicable à tout projet data

5. **Les sources sont multiples** : fichiers, bases SQL, APIs, cloud — vous apprendrez à toutes les maîtriser

---

## 🔗 Sources et références

- [IBM Research on Data Quality Costs](https://www.ibm.com/think/insights/data-quality-issues)
- [Gartner Data Quality Statistics](https://www.acceldata.io/blog/the-hidden-cost-of-poor-data-quality-governance-adm-turns-risk-into-revenue)
- [Integrate.io ETL vs ELT Statistics 2025](https://www.integrate.io/blog/elt-vs-etl-comparison-statistics/)
- [Pure Storage ETL vs ELT Guide 2025](https://blog.purestorage.com/purely-technical/etl-vs-elt/)
- [DataScience-PM CRISP-DM Guide](https://www.datascience-pm.com/crisp-dm-2/)
- [Wikipedia - CRISP-DM](https://en.wikipedia.org/wiki/Cross-industry_standard_process_for_data_mining)

---

## ➡️ Prochain chapitre

**Chapitre 2 : Extraction des données (sources locales)** — Vous apprendrez à récupérer des données depuis des fichiers CSV, Excel, JSON, des bases SQL, et des APIs REST.

---

*Module 2 — Pipeline Data | Chapitre 1 sur 11*
