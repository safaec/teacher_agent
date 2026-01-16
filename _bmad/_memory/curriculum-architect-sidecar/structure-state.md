# Structure State

## Current Course

**Boot Camp Data & Intelligence Artificielle**

## Structured Modules

| Module | Status | Chapters | Lessons |
|--------|--------|----------|---------|
| 1 - Prompt Engineering | Not structured | — | — |
| **2 - Pipeline Data** | ✅ Structured | 11 | 65 |
| 3 - ML & LLMs | Not structured | — | — |
| 4 - Power BI | Not structured | — | — |
| Final Capstone | Not structured | — | — |

---

# 📘 Module 2 — Pipeline Data : de la collecte à l'analyse en cloud

**Durée totale :** 85-105h | **Public :** Reconversion avec bases Python, Excel, SQL

**Méthodologie :** Progressive PBL with scaffolded milestones

**Projet Module :** "Mon Pipeline Data de A à Z"

---

## Chapitre 1 : Pipeline data — vue d'ensemble
**Durée : 4-5h** | **Activité principale :** Concept Map

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 1.1 | Position du module dans le parcours Data & IA | 45min | Théorie + Discussion |
| 1.2 | ETL vs ELT : deux approches | 1h | Théorie + Schéma |
| 1.3 | Les étapes de la pipeline | 1h | Théorie + Concept Map |
| 1.4 | Méthodologie CRISP-DM | 45min | Théorie |
| 1.5 | Sources de données en entreprise | 30min | Discussion |
| — | **🎯 Activité : Concept Map ETL/ELT** | 1h | Groupe |

---

## Chapitre 2 : Extraction des données
**Durée : 10-12h** | **Activité principale :** Guided Lab

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 2.1 | Extraction depuis fichiers avec pandas (CSV, Excel, JSON) | 2h | Théorie + Code |
| 2.2 | Extraction depuis bases SQL (sqlalchemy, read_sql) | 2h | Théorie + Lab |
| 2.3 | Extraction via API REST (requests, pagination) | 2.5h | Théorie + Lab |
| 2.4 | Introduction au web scraping (BeautifulSoup) | 1.5h | Survol + Démo |
| 2.5 | Inspection initiale des données | 1h | Théorie + Pratique |
| — | **🎯 Guided Lab : Extraire CSV, SQL, API** | 2h | Pair work |
| — | **🎯 Mini-projet : Extraire & profiler un dataset réel** | 1h | Individuel |

🤖 *IA : Utiliser un LLM pour comprendre une documentation d'API*

---

## Chapitre 3 : Le Cloud pour la data
**Durée : 8-10h** | **Activité principale :** Hands-on Setup

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 3.1 | Introduction au cloud computing | 1h | Théorie |
| 3.2 | Types de stockage cloud (objet vs fichier vs DB) | 1h | Théorie + Comparaison |
| 3.3 | AWS S3 : concepts (buckets, objets, IAM) | 1.5h | Théorie |
| 3.4 | Hands-on : configuration AWS (compte, credentials) | 1.5h | Lab guidé |
| 3.5 | Hands-on : lire depuis S3 avec Python (boto3, pandas) | 2h | Lab guidé |
| 3.6 | Panorama GCP et Azure (conceptuel) | 1h | Théorie |
| — | **🎯 Lab : Setup AWS + lire depuis S3** | 1h | Checklist validation |

---

## Chapitre 4 : EDA diagnostique et qualité
**Durée : 8-10h** | **Activité principale :** Dataset Autopsy

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 4.1 | Objectif de l'EDA diagnostique | 30min | Théorie |
| 4.2 | Dimensions de la qualité (complétude, unicité, cohérence, exactitude) | 1h | Théorie |
| 4.3 | Statistiques descriptives de diagnostic | 1h | Théorie + Code |
| 4.4 | Détection des valeurs manquantes (missingno) | 1.5h | Théorie + Lab |
| 4.5 | Détection des doublons | 1h | Théorie + Lab |
| 4.6 | Détection des outliers (IQR, Z-score) | 1.5h | Théorie + Lab |
| 4.7 | Visualisations de diagnostic | 1h | Pratique |
| 4.8 | Documentation des problèmes | 30min | Méthodologie |
| — | **🎯 Dataset Autopsy : Analyser un dirty dataset** | 1h | Groupe |

🤖 *IA : Générer du code d'exploration automatiquement*

---

## Chapitre 5 : Nettoyage des données
**Durée : 10-12h** | **Activité principale :** Cleaning Challenge

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 5.1 | Gestion des valeurs manquantes (suppression, imputation) | 2h | Théorie + Lab |
| 5.2 | Traitement des doublons | 1h | Théorie + Lab |
| 5.3 | Traitement des valeurs aberrantes | 1.5h | Théorie + Lab |
| 5.4 | Nettoyage des types de données (dates, numériques) | 2h | Théorie + Lab |
| 5.5 | Nettoyage de texte (regex, normalisation) | 1.5h | Théorie + Lab |
| 5.6 | Nettoyage avec SQL | 1h | Théorie |
| 5.7 | Traçabilité et documentation | 30min | Méthodologie |
| — | **🎯 Cleaning Challenge** | 1.5h | Individuel |
| — | **🎯 Peer Code Review** | 1h | Pair work |
| — | **🎯 Mini-projet : Quality report + cleaning pipeline** | — | Livrable |

🤖 *IA : Suggérer une stratégie de nettoyage adaptée*

---

## Chapitre 6 : Structuration et transformation
**Durée : 8-10h** | **Activité principale :** Puzzle Lab

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 6.1 | Restructuration (pivot, melt, wide vs long) | 2h | Théorie + Lab |
| 6.2 | Combinaison de données (merge, concat) | 2h | Théorie + Lab |
| 6.3 | Agrégations (groupby, agg) | 1.5h | Théorie + Lab |
| 6.4 | Feature engineering basique | 1.5h | Théorie + Lab |
| 6.5 | Création du dataset final | 1h | Pratique |
| — | **🎯 Puzzle Lab : Répondre à des questions métier** | 1.5h | Groupe + Live coding |

🤖 *IA : Générer du code de merge complexe*

---

## Chapitre 7 : EDA analytique
**Durée : 10-12h** | **Activité principale :** Hypothesis Hunt

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 7.1 | Différence EDA diagnostique vs analytique | 30min | Théorie |
| 7.2 | Analyse univariée | 2h | Théorie + Lab |
| 7.3 | Analyse bivariée (corrélation, scatter, contingence) | 2.5h | Théorie + Lab |
| 7.4 | Segmentation et analyse par groupes | 2h | Théorie + Lab |
| 7.5 | Formulation d'hypothèses | 1h | Méthodologie |
| 7.6 | Pensée critique (corrélation ≠ causalité) | 1h | Discussion |
| — | **🎯 Hypothesis Hunt** | 1.5h | Groupe |
| — | **🎯 Discussion : Biais et limites** | 1h | Groupe |
| — | **🎯 Mini-projet : Dataset analysis-ready** | — | Livrable |

🤖 *IA : Interpréter des corrélations, suggérer des analyses*

---

## Chapitre 8 : Visualisation exploratoire
**Durée : 8-10h** | **Activité principale :** Viz Critique

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 8.1 | Principes de visualisation | 1h | Théorie |
| 8.2 | Matplotlib : fondamentaux | 2h | Théorie + Lab |
| 8.3 | Seaborn : visualisations statistiques | 2h | Théorie + Lab |
| 8.4 | Plotly : interactivité (survol) | 1h | Démo |
| 8.5 | Bonnes pratiques (lisibilité, accessibilité) | 1h | Théorie |
| 8.6 | Préparation pour Power BI | 30min | Théorie |
| — | **🎯 Viz Critique : Analyser des graphiques** | 1h | Groupe |
| — | **🎯 Mini-challenge : Best Viz Contest** | 1h | Compétition |

🤖 *IA : Générer du code matplotlib*

---

## Chapitre 9 : Chargement vers le cloud
**Durée : 4-5h** | **Activité principale :** Pipeline Completion

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 9.1 | Écrire vers S3 avec Python | 1h | Théorie + Lab |
| 9.2 | Organisation d'un data lake basique (raw/processed/final) | 1h | Théorie |
| 9.3 | Documentation du dataset (data dictionary) | 45min | Pratique |
| 9.4 | Vérification post-upload | 30min | Pratique |
| 9.5 | Bonnes pratiques professionnelles | 30min | Théorie |
| — | **🎯 Lab : Upload to S3 + Checklist validation** | 1h | Individuel |

🤖 *IA : Générer automatiquement une documentation*

---

## Chapitre 10 : Cas pratique fil rouge
**Durée : 12-15h** | **Activité principale :** MODULE PROJECT

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 10.1 | Contexte et objectifs métier | 1h | Briefing |
| 10.2 | Extraction multi-sources | 2h | Projet |
| 10.3 | EDA diagnostique | 1.5h | Projet |
| 10.4 | Nettoyage | 2h | Projet |
| 10.5 | Structuration et transformation | 2h | Projet |
| 10.6 | EDA analytique | 1.5h | Projet |
| 10.7 | Visualisations exploratoires | 1.5h | Projet |
| 10.8 | Chargement vers S3 | 1h | Projet |
| 10.9 | Préparation présentation | 1.5h | Projet |
| — | **🎯 Checkpoints réguliers** | — | Office hours |
| — | **🎯 Livrable : Notebook + Dataset S3 + Documentation** | — | Évaluation |

🤖 *IA : Utilisation libre comme assistant tout au long*

---

## Chapitre 11 : Conclusion et aide-mémoire
**Durée : 2-3h** | **Activité principale :** Reflection + Demo Day

| Leçon | Titre | Durée | Type |
|-------|-------|-------|------|
| 11.1 | Synthèse des compétences acquises | 30min | Réflexion |
| 11.2 | Erreurs fréquentes à éviter (Top 10) | 30min | Discussion |
| 11.3 | Lien avec les modules suivants (M3 ML, M4 Power BI) | 30min | Théorie |
| — | **🎯 Quiz de validation** | 30min | Individuel |
| — | **🎯 CLASS DEMO DAY** | 1h | Présentations (3min/étudiant) |

---

## Résumé de la structure

| Ch | Titre | Leçons | Activités | Durée |
|----|-------|--------|-----------|-------|
| 1 | Pipeline vue d'ensemble | 5 | 1 | 4-5h |
| 2 | Extraction | 5 | 2 + mini-projet | 10-12h |
| 3 | Cloud (AWS S3) | 6 | 1 | 8-10h |
| 4 | EDA diagnostique | 8 | 1 | 8-10h |
| 5 | Nettoyage | 7 | 2 + mini-projet | 10-12h |
| 6 | Transformation | 5 | 1 | 8-10h |
| 7 | EDA analytique | 6 | 2 + mini-projet | 10-12h |
| 8 | Visualisation | 6 | 2 | 8-10h |
| 9 | Cloud Upload | 5 | 1 | 4-5h |
| 10 | Fil Rouge | 9 | PROJECT | 12-15h |
| 11 | Conclusion | 3 | 2 | 2-3h |
| **Total** | | **65 leçons** | **15+ activités** | **85-105h** |

---

## Learning Blocks (per Instructional Designer)

```
Ch 1-2  → Mini-projet: Extraire & profiler un dataset réel
Ch 3    → Lab: Setup AWS + lire depuis S3
Ch 4-5  → Mini-projet: Quality report + cleaning pipeline
Ch 6-7  → Mini-projet: Dataset analysis-ready
Ch 8-9  → Lab: Visualisations + upload S3
Ch 10   → MODULE PROJECT: Mon Pipeline Data de A à Z
Ch 11   → Reflection + Demo Day
```

---

## Next Action

Module 2 structured. Ready for Content Creator to write lesson content.

Remaining modules to structure:
- Module 1: Prompt Engineering
- Module 3: ML & LLMs
- Module 4: Power BI
- Final Capstone

---

## Session History

- **2026-01-14:** Module 2 structured — 11 chapters, 65 lessons, 15+ activities, preserved Domain Expert structure with Instructional Designer activities integrated
