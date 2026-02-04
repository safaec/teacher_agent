# Guide Projet Data Pipeline

Bienvenue dans ce guide méthodologique pour réaliser votre projet data pipeline de bout en bout.

---

## Objectif

Ce guide vous accompagne à travers toutes les étapes d'un projet data professionnel, de la définition du sujet jusqu'à la présentation des résultats.

---

## Structure du guide

Le projet est divisé en **8 phases** + des ressources complémentaires :

```
📁 Guide_Projet_Data_Pipeline/
│
├── 📄 README.md                          ← Vous êtes ici
│
├── 🎯 PHASES DU PROJET
│   ├── 00_Introduction_et_Cadrage.md     ← Choisir son sujet, définir les questions
│   ├── 01_Extraction_Multi_Sources.md    ← Collecter les données
│   ├── 02_EDA_Diagnostique.md            ← Diagnostiquer la qualité
│   ├── 03_Nettoyage_Donnees.md           ← Nettoyer les problèmes
│   ├── 04_Transformation_Feature_Eng.md  ← Enrichir les données
│   ├── 05_EDA_Analytique.md              ← Analyser et répondre aux questions
│   ├── 06_Visualisation_Storytelling.md  ← Visualiser et raconter
│   ├── 07_Chargement_Documentation.md    ← Exporter et documenter
│   └── 08_Presentation_Synthese.md       ← Préparer les livrables finaux
│
└── 📚 RESSOURCES
    ├── Annexe_Idees_Sujets.md            ← Idées de sujets par domaine
    └── Grille_Evaluation.md              ← Critères de notation
```

---

## Workflow visuel

```
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   PHASE 0   │     │   PHASE 1   │     │   PHASE 2   │
│   Cadrage   │ ──▶ │  Extraction │ ──▶ │  Diagnostic │
│             │     │             │     │             │
└─────────────┘     └─────────────┘     └─────────────┘
                                               │
       ┌───────────────────────────────────────┘
       ▼
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   PHASE 3   │     │   PHASE 4   │     │   PHASE 5   │
│  Nettoyage  │ ──▶ │ Transforma- │ ──▶ │   Analyse   │
│             │     │    tion     │     │             │
└─────────────┘     └─────────────┘     └─────────────┘
                                               │
       ┌───────────────────────────────────────┘
       ▼
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│   PHASE 6   │     │   PHASE 7   │     │   PHASE 8   │
│ Visualisa-  │ ──▶ │   Documen-  │ ──▶ │ Présenta-   │
│    tion     │     │   tation    │     │    tion     │
└─────────────┘     └─────────────┘     └─────────────┘
```

---

## Comment utiliser ce guide

### 1. Lisez d'abord les bases

- Commencez par ce README
- Parcourez la Phase 0 pour comprendre le cadrage
- Consultez l'Annexe pour des idées de sujets

### 2. Avancez phase par phase

- Suivez l'ordre des phases
- Complétez les checklists de chaque phase
- Ne passez à la suivante que si la phase actuelle est validée

### 3. Utilisez l'IA comme assistant

- Chaque phase contient des prompts IA prêts à l'emploi
- L'IA peut vous aider à brainstormer, débugger, interpréter

### 4. Documentez au fur et à mesure

- Complétez les templates proposés
- Ne remettez pas la documentation à la fin

---

## Chaque fichier contient

- **Objectif** : Ce que vous devez accomplir
- **Checklists** : Actions à cocher au fur et à mesure
- **Templates** : Documents à compléter
- **Prompts IA** : Suggestions d'utilisation de l'IA générative
- **Questions de réflexion** : Pour approfondir votre compréhension
- **Critères de validation** : Comment savoir si l'étape est réussie

---

## Livrables attendus

À la fin du projet, vous devez produire :

| Livrable | Format | Vérifié |
|----------|--------|---------|
| Dataset final nettoyé | .parquet ou .csv | ☐ |
| Data Dictionary | .md | ☐ |
| Notebook documenté | .ipynb | ☐ |
| Visualisations (5-7 min) | .png | ☐ |
| Présentation/Rapport | .pdf ou .pptx | ☐ |

---

## Notation

Le projet est évalué sur **100 points** répartis comme suit :

| Phase | Points |
|-------|--------|
| Cadrage | 10 |
| Extraction | 10 |
| Diagnostic | 10 |
| Nettoyage | 15 |
| Transformation | 15 |
| Analyse | 15 |
| Visualisation | 10 |
| Documentation | 10 |
| Utilisation IA | 5 |
| **Total** | **100** |

Voir [Grille_Evaluation.md](Grille_Evaluation.md) pour les critères détaillés.

---

## Conseils pour réussir

### À faire

- ✅ Choisir un sujet qui vous passionne
- ✅ Avancer progressivement, phase par phase
- ✅ Documenter au fur et à mesure
- ✅ Utiliser l'IA de façon stratégique
- ✅ Valider chaque étape avant de passer à la suivante

### À éviter

- ❌ Commencer sans avoir cadré le projet
- ❌ Choisir un sujet trop ambitieux
- ❌ Négliger la documentation
- ❌ Remettre les visualisations à la fin
- ❌ Oublier de mentionner les limites

---

## Besoin d'aide ?

1. **Consultez le guide** de la phase concernée
2. **Utilisez les prompts IA** fournis
3. **Relisez les exemples** et templates
4. **Posez vos questions** à votre formateur

---

**Bon projet !**

---

*Guide créé pour le Module 2 : Data Pipeline*
