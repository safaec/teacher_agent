# **Module 2 — Pipeline Data : de la collecte à l'analyse en environnement cloud**

**Durée : 3 semaines (environ 100 heures)**

**Public : profils en reconversion avec bases Python, Excel et SQL (niveau intermédiaire incluant JOIN/GROUP BY)**

---

## **Objectifs globaux**

* Comprendre le cycle de vie complet de la donnée, de l'extraction à l'analyse exploratoire
* Maîtriser l'accès aux données dans un environnement cloud (AWS) et comprendre les alternatives (GCP, Azure)
* Diagnostiquer et améliorer la qualité des données
* Nettoyer, structurer et transformer les données pour l'analyse et le ML
* Réaliser des analyses exploratoires et visualisations pertinentes pour comprendre les phénomènes
* Préparer les données pour la visualisation décisionnelle (Power BI) et le Machine Learning

---

## **Chapitre 1 : Introduction à la donnée dans un projet Data & IA**

**Objectif :** Comprendre le rôle central de la donnée et situer ce module dans la chaîne Data & IA.

**Durée estimée : 3-4h**

**Sous-parties :**

1. Rôle de la donnée dans un projet analytique et IA
2. Importance de la qualité et préparation avant ML
3. Types de données : structurées, semi-structurées, non structurées
4. Position du module dans la pipeline Data & IA
5. Vue d'ensemble des sources de données en entreprise (locales, cloud, APIs, bases de données)

---

## **Chapitre 2 : Qualité des données : concepts et référentiel d'analyse**

**Objectif :** Identifier les dimensions de qualité des données et créer un cadre d'analyse pour guider le nettoyage et la structuration.

**Durée estimée : 4-5h**

**Sous-parties :**

1. Définition de la qualité des données
2. Dimensions clés : complétude, cohérence, exactitude, unicité, fraîcheur
3. Problèmes courants dans les données réelles
4. Indicateurs de qualité et contrôles de base
5. Coût de la mauvaise qualité des données (impact métier)

---

## **Chapitre 3 : Vue d'ensemble de la pipeline data de bout en bout**

**Objectif :** Donner une vision globale de la pipeline et de l'itération entre les étapes, inspirée de CRISP-DM.

**Durée estimée : 3-4h**

**Sous-parties :**

1. Étapes de la pipeline : extraction → EDA diagnostique → nettoyage → structuration → EDA analytique → visualisation exploratoire
2. Notion d'itération et retours en arrière
3. Lien avec les standards professionnels et méthodologie CRISP-DM
4. Importance de la reproductibilité et traçabilité
5. Rôle du cloud dans la pipeline data moderne

---

## **Chapitre 4 : Environnements cloud et accès aux données**

**Objectif :** Comprendre les fondamentaux du cloud computing pour la data et savoir accéder aux données stockées dans le cloud (AWS).

**Durée estimée : 8-10h**

**Sous-parties :**

1. **Introduction au cloud computing pour la data**
   - Pourquoi le cloud ? Avantages vs. environnement local
   - Les trois grands : AWS, GCP, Azure (panorama comparatif)
   - Concepts clés : régions, disponibilité, scalabilité, pay-as-you-go

2. **Types de stockage cloud**
   - Stockage objet (S3) vs. stockage fichier vs. bases de données managées
   - Quand utiliser quoi ? Critères de choix
   - Formats de données courants en cloud (Parquet, CSV, JSON)

3. **Hands-on AWS : Configuration et accès**
   - Création et navigation dans la console AWS
   - IAM : utilisateurs, rôles, policies (principes de sécurité)
   - Configuration des credentials pour accès programmatique

4. **Hands-on AWS : Manipulation de données avec Python**
   - Introduction à boto3
   - Lire et écrire des fichiers depuis/vers S3
   - Intégration pandas + S3 (lecture directe)

5. **Panorama GCP et Azure (conceptuel)**
   - Équivalences : S3 ↔ Cloud Storage ↔ Blob Storage
   - Équivalences : RDS ↔ Cloud SQL ↔ Azure SQL
   - Quand choisir quel cloud ? (critères entreprise)

**Hors scope :** Infrastructure as code (Terraform/CloudFormation), architectures multi-cloud, optimisation des coûts avancée, services serverless (Lambda, Glue)

---

## **Chapitre 5 : Extraction et collecte des données**

**Objectif :** Savoir extraire les données depuis diverses sources (locales, cloud, APIs, bases de données, web) et comprendre leur structure avant toute transformation.

**Durée estimée : 10-12h**

**Sous-parties :**

1. **Extraction depuis fichiers locaux avec pandas**
   - CSV, Excel, JSON : options de lecture et pièges courants
   - Gestion des encodages et séparateurs
   - Lecture de fichiers volumineux (chunking)

2. **Extraction depuis bases de données SQL**
   - Connexion Python à PostgreSQL/MySQL (sqlalchemy, psycopg2)
   - Requêtes SELECT avec filtres, JOIN, GROUP BY
   - Bonnes pratiques : ne pas surcharger la base de production

3. **Extraction depuis S3 (cloud)**
   - Lecture directe pandas depuis S3
   - Gestion des credentials en environnement de développement
   - Patterns courants : data lake, données partitionnées

4. **Extraction via API REST**
   - Principes des APIs REST (endpoints, méthodes HTTP, JSON)
   - Utilisation de la librairie requests
   - Authentification basique (API keys, headers)
   - Pagination et rate limiting
   - Cas pratique : extraction depuis une API publique

5. **Introduction au web scraping (survol)**
   - Quand scraper ? Limites légales et éthiques (robots.txt, CGU)
   - BeautifulSoup : principes de base
   - Démonstration sur un cas simple
   - Alternatives : APIs officielles, datasets publics

6. **Inspection initiale et compréhension des données**
   - Compréhension du contexte métier avant transformation
   - Identification des limites et hypothèses sur les données
   - Documentation de la provenance (data lineage basique)

**Hors scope :** Scraping avancé (Selenium, Scrapy), streaming temps réel (Kafka), OAuth2 complexe, GraphQL

---

## **Chapitre 6 : Analyse exploratoire diagnostique (EDA diagnostique)**

**Objectif :** Détecter anomalies, incohérences et problèmes de qualité afin de guider le nettoyage.

**Durée estimée : 8-10h**

**Sous-parties :**

1. Objectif de l'EDA diagnostique : trouver les problèmes, pas encore comprendre les patterns
2. Statistiques descriptives de base (shape, dtypes, describe, info)
3. Visualisations simples pour identifier anomalies (histogrammes, boxplots)
4. Détection des valeurs manquantes : patterns et visualisation (missingno)
5. Détection des doublons : exacts et quasi-doublons
6. Identification des outliers : méthodes IQR et z-score
7. Différencier bug, bruit et signal métier
8. Documentation des problèmes identifiés (checklist qualité)

**Outils :** pandas, matplotlib, seaborn, missingno

---

## **Chapitre 7 : Nettoyage des données**

**Objectif :** Corriger les problèmes détectés lors de l'EDA diagnostique pour obtenir des données propres et exploitables.

**Durée estimée : 10-12h**

**Sous-parties :**

1. **Gestion des valeurs manquantes**
   - Stratégies : suppression, imputation (moyenne, médiane, mode), flags
   - Quand supprimer vs. imputer ? Critères de décision
   - Imputation contextuelle (par groupe)

2. **Traitement des doublons**
   - Identification et suppression des doublons exacts
   - Gestion des quasi-doublons (fuzzy matching - survol)

3. **Traitement des valeurs aberrantes**
   - Suppression vs. winsorisation vs. conservation
   - Justification métier des choix

4. **Nettoyage des types de données**
   - Conversion dates (parsing, timezones)
   - Conversion numériques (gestion des formats)
   - Standardisation des catégories (strip, lower, mapping)

5. **Nettoyage de texte**
   - Normalisation basique (espaces, casse, caractères spéciaux)
   - Expressions régulières pour extraction/nettoyage

6. **Nettoyage avec SQL**
   - Opérations de nettoyage côté base de données
   - Quand nettoyer en SQL vs. en Python ?

7. **Documentation et traçabilité**
   - Justification des choix en lien avec l'EDA diagnostique
   - Versioning des datasets (bonnes pratiques)

**Hors scope :** NLP avancé, record linkage complexe, data quality frameworks enterprise

---

## **Chapitre 8 : Structuration et transformation des données**

**Objectif :** Mettre en forme les données pour les rendre analytiquement exploitables et compatibles avec le ML et la visualisation.

**Durée estimée : 8-10h**

**Sous-parties :**

1. **Restructuration des DataFrames**
   - Pivot et unpivot (melt)
   - Wide vs. long format : quand utiliser quoi ?

2. **Combinaison de données**
   - Merge (jointures) : inner, left, right, outer
   - Concat : empiler des datasets
   - Gestion des clés et conflits

3. **Agrégations avancées**
   - GroupBy : split-apply-combine
   - Agrégations multiples et fonctions custom
   - Window functions (rolling, expanding)

4. **Feature engineering basique**
   - Création de variables dérivées
   - Binning (discrétisation)
   - Encoding catégoriel : one-hot, label encoding, target encoding (survol)
   - Variables temporelles (extraction jour, mois, année, jour de semaine)

5. **Préparation du dataset final**
   - Workflow complet : données brutes → dataset propre
   - Validation du schema final
   - Export pour analyse, ML et visualisation

**Hors scope :** Feature stores, pipelines sklearn, automated feature engineering

---

## **Chapitre 9 : Analyse exploratoire analytique (EDA analytique)**

**Objectif :** Explorer les données propres pour comprendre les relations, formuler des hypothèses métier et préparer le ML.

**Durée estimée : 10-12h**

**Sous-parties :**

1. **Différence EDA diagnostique vs. analytique**
   - Diagnostique = trouver les problèmes
   - Analytique = comprendre les phénomènes

2. **Analyse univariée approfondie**
   - Distribution de chaque variable
   - Identification des patterns (normalité, skewness, multimodalité)

3. **Analyse bivariée**
   - Relations numériques : corrélations, scatter plots
   - Relations catégorielles : tables de contingence, chi2 (survol)
   - Relations mixtes : boxplots par catégorie, violin plots

4. **Segmentation et analyse par groupes**
   - Patterns par sous-populations
   - Comparaisons entre segments

5. **Formulation d'hypothèses**
   - Passer de l'observation à l'hypothèse
   - Identification des variables candidates pour le ML
   - Questions métier à approfondir

6. **Limites et pensée critique**
   - Corrélation ≠ causalité
   - Biais de sélection et survivorship bias
   - Ce que les données ne disent pas

---

## **Chapitre 10 : Visualisation exploratoire**

**Objectif :** Créer des visualisations pertinentes pour l'analyse exploratoire et préparer la transition vers la visualisation décisionnelle.

**Durée estimée : 8-10h**

**Sous-parties :**

1. **Principes de visualisation**
   - Choisir le bon graphique selon le type de données et la question
   - Visualisation pour comprendre (exploratoire) vs. pour décider (décisionnelle)

2. **Visualisations avec matplotlib**
   - Graphiques de base : line, bar, scatter, hist
   - Personnalisation : titres, labels, couleurs, annotations
   - Subplots et figures composées

3. **Visualisations avec seaborn**
   - Graphiques statistiques : boxplot, violinplot, heatmap
   - Pairplot pour exploration multivariée
   - Styling et palettes

4. **Introduction à plotly (survol)**
   - Avantages de l'interactivité pour l'exploration
   - Graphiques interactifs de base
   - Quand utiliser plotly vs. matplotlib/seaborn ?

5. **Bonnes pratiques**
   - Lisibilité et accessibilité
   - Éviter les graphiques trompeurs
   - Storytelling visuel : guider l'œil du lecteur

6. **Préparation pour Power BI**
   - Format des données attendu
   - Transition vers la visualisation décisionnelle (Module 4)

**Hors scope :** Dashboards interactifs (Dash/Streamlit), cartographie (folium/geopandas), animations

---

## **Chapitre 11 : Apports de l'IA dans la préparation et l'analyse des données**

**Objectif :** Intégrer l'IA comme assistant pour l'exploration, la détection d'anomalies et la documentation des données, en connaissant ses limites.

**Durée estimée : 4-5h**

**Sous-parties :**

1. **IA pour l'exploration des données**
   - Utiliser un LLM pour comprendre un dataset inconnu
   - Génération de questions d'exploration
   - Interprétation assistée des résultats

2. **IA pour la génération de code**
   - Génération de code pandas/SQL avec prompts
   - Revue et validation du code généré
   - Itération et affinage

3. **IA pour la documentation**
   - Génération de data dictionaries
   - Documentation des transformations
   - Création de rapports d'EDA

4. **Limites et risques**
   - Hallucinations sur les données
   - Biais reproduits ou amplifiés
   - Risques de confidentialité (données sensibles)

5. **Positionnement critique**
   - L'IA comme assistant, pas comme oracle
   - Validation humaine obligatoire
   - Lien avec Module 1 (Prompt Engineering)

---

## **Chapitre 12 : Cas pratique fil rouge**

**Objectif :** Mettre en œuvre l'ensemble de la pipeline sur un dataset réel pour consolider les compétences acquises.

**Durée estimée : 12-15h**

**Sous-parties :**

1. **Contexte et objectifs métier**
   - Présentation du cas et des questions business
   - Définition des livrables attendus

2. **Extraction multi-sources**
   - Données locales (CSV/Excel)
   - Données cloud (S3)
   - Données API (si applicable)

3. **EDA diagnostique et nettoyage**
   - Application du référentiel qualité
   - Documentation des problèmes et solutions

4. **Structuration et transformation**
   - Jointures et agrégations
   - Feature engineering pertinent

5. **EDA analytique**
   - Exploration des relations
   - Formulation d'hypothèses

6. **Visualisations exploratoires**
   - Graphiques clés pour répondre aux questions métier
   - Préparation des données pour Power BI

7. **Présentation et justification**
   - Restitution des insights
   - Justification des choix méthodologiques
   - Limites et prochaines étapes

---

## **Chapitre 13 : Conclusion et transition**

**Objectif :** Synthétiser les apprentissages, présenter les bonnes pratiques et préparer les modules suivants.

**Durée estimée : 2-3h**

**Sous-parties :**

1. Synthèse des compétences acquises
2. Bonnes pratiques professionnelles (checklist)
3. Erreurs fréquentes à éviter
4. Lien avec Module 3 (ML et LLM) : les données préparées alimentent les modèles
5. Lien avec Module 4 (Power BI) : de l'exploration à la décision

---

## **Récapitulatif des durées**

| Chapitre | Thème | Durée estimée |
|----------|-------|---------------|
| 1 | Introduction à la donnée | 3-4h |
| 2 | Qualité des données | 4-5h |
| 3 | Pipeline data et CRISP-DM | 3-4h |
| 4 | **Cloud et AWS** | 8-10h |
| 5 | **Extraction (SQL, API, scraping)** | 10-12h |
| 6 | EDA diagnostique | 8-10h |
| 7 | Nettoyage | 10-12h |
| 8 | Structuration et transformation | 8-10h |
| 9 | EDA analytique | 10-12h |
| 10 | Visualisation exploratoire | 8-10h |
| 11 | IA dans la préparation des données | 4-5h |
| 12 | Cas pratique fil rouge | 12-15h |
| 13 | Conclusion et transition | 2-3h |
| **TOTAL** | | **~90-110h** |

---

## **Éléments hors scope (explicitement exclus)**

Pour éviter le scope creep, les éléments suivants sont **explicitement hors scope** de ce module :

- Infrastructure as code (Terraform, CloudFormation)
- Architectures multi-cloud et migration
- Services serverless (Lambda, Glue, Step Functions)
- Data lakes avancés et formats Delta/Iceberg
- Streaming temps réel (Kafka, Kinesis)
- Scraping avancé (Selenium, Scrapy)
- NLP et traitement de texte avancé
- Tests statistiques formels (t-test, ANOVA, chi2 en profondeur)
- Analyse multivariée avancée (PCA, clustering - réservé Module 3)
- Dashboards interactifs (Dash, Streamlit)
- Feature stores et MLOps
- Gouvernance des données enterprise

---

## **Prérequis**

- Python : variables, types, conditions, boucles, fonctions, listes, dictionnaires
- SQL : SELECT, WHERE, JOIN, GROUP BY, ORDER BY
- Excel : manipulation basique de tableaux
- Module 1 complété : Prompt Engineering

---

## **Livrables apprenants**

À la fin du module, chaque apprenant aura :

1. Un notebook Jupyter documenté avec le fil rouge complet
2. Un dataset nettoyé et structuré prêt pour le ML
3. Un rapport d'EDA avec visualisations clés
4. Une extraction fonctionnelle depuis S3 (AWS)
5. Une documentation des choix méthodologiques

---
