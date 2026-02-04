# Grille d'Évaluation - Projet Data Pipeline

Cette grille détaille les critères d'évaluation pour chaque phase du projet.

**Total : 100 points**

---

## Vue d'ensemble de la notation

| Phase | Points | Poids |
|-------|--------|-------|
| Phase 0 : Cadrage | /10 | 10% |
| Phase 1 : Extraction | /10 | 10% |
| Phase 2 : Diagnostic qualité | /10 | 10% |
| Phase 3 : Nettoyage | /15 | 15% |
| Phase 4 : Transformation | /15 | 15% |
| Phase 5 : Analyse | /15 | 15% |
| Phase 6 : Visualisation | /10 | 10% |
| Phase 7 : Documentation | /10 | 10% |
| Utilisation IA | /5 | 5% |
| **TOTAL** | **/100** | **100%** |

---

## Phase 0 : Cadrage (10 points)

### Critères détaillés

| Critère | 0-2 pts | 3-4 pts | 5 pts |
|---------|---------|---------|-------|
| **Choix du sujet** (5 pts) | Sujet vague, non réalisable | Sujet clair mais classique | Sujet original, bien délimité, motivé |
| **Questions business** (5 pts) | < 3 questions ou floues | 3-4 questions claires | 5 questions SMART avec impact identifié |

### Checklist d'évaluation

- [ ] Le sujet est clairement défini
- [ ] Le contexte/la problématique est expliqué
- [ ] 3-5 questions business sont formulées
- [ ] Les questions sont spécifiques et mesurables
- [ ] Les sources de données sont identifiées

### Note Phase 0 : _____ / 10

---

## Phase 1 : Extraction (10 points)

### Critères détaillés

| Critère | 0-2 pts | 3-4 pts | 5 pts |
|---------|---------|---------|-------|
| **Diversité des sources** (5 pts) | 1 seule source/format | 2 sources, 2 formats | 3+ sources, 3+ formats variés |
| **Qualité du chargement** (5 pts) | Erreurs non résolues | Chargement fonctionnel | Types corrects, encodage maîtrisé |

### Checklist d'évaluation

- [ ] Au moins 2 sources de données différentes
- [ ] Au moins 2 formats différents (CSV, Excel, JSON, API...)
- [ ] Données chargées sans erreur
- [ ] Types de données vérifiés
- [ ] Tableau récapitulatif des données complété

### Note Phase 1 : _____ / 10

---

## Phase 2 : Diagnostic qualité (10 points)

### Critères détaillés

| Critère | 0-3 pts | 4-6 pts | 7-10 pts |
|---------|---------|---------|----------|
| **Exhaustivité du diagnostic** (10 pts) | 1-2 dimensions analysées | 3-4 dimensions | 5 dimensions + rapport complet |

### Les 5 dimensions à analyser

| Dimension | Analysée ? | Problèmes identifiés ? |
|-----------|------------|------------------------|
| Complétude | ☐ | ☐ |
| Unicité | ☐ | ☐ |
| Cohérence | ☐ | ☐ |
| Exactitude | ☐ | ☐ |
| Fraîcheur | ☐ | ☐ |

### Checklist d'évaluation

- [ ] Valeurs manquantes analysées et quantifiées
- [ ] Doublons détectés et comptés
- [ ] Incohérences de format identifiées
- [ ] Outliers et valeurs aberrantes repérés
- [ ] Rapport de diagnostic complété
- [ ] Problèmes priorisés

### Note Phase 2 : _____ / 10

---

## Phase 3 : Nettoyage (15 points)

### Critères détaillés

| Critère | 0-4 pts | 5-9 pts | 10-15 pts |
|---------|---------|---------|-----------|
| **Traitement des problèmes** (8 pts) | Problèmes non traités | Principaux problèmes traités | Tous problèmes traités correctement |
| **Documentation** (4 pts) | Pas de log | Log partiel | Log complet avec justifications |
| **Validation** (3 pts) | Pas de vérification | Vérification basique | Comparatif avant/après détaillé |

### Checklist d'évaluation

- [ ] Doublons supprimés
- [ ] Valeurs aberrantes traitées
- [ ] Formats standardisés
- [ ] Valeurs manquantes gérées (avec stratégie justifiée)
- [ ] Types de données corrigés
- [ ] Log de nettoyage complété
- [ ] Tableau comparatif avant/après
- [ ] Données originales préservées

### Note Phase 3 : _____ / 15

---

## Phase 4 : Transformation (15 points)

### Critères détaillés

| Critère | 0-4 pts | 5-9 pts | 10-15 pts |
|---------|---------|---------|-----------|
| **Jointures** (5 pts) | 1 source seulement | 2-3 sources combinées | 3+ sources, jointures optimisées |
| **Feature Engineering** (7 pts) | 0-2 features créées | 5-10 features | 10+ features variées et pertinentes |
| **Documentation dataset** (3 pts) | Pas de documentation | Liste des colonnes | Description complète |

### Types de features attendues

| Type de feature | Créée ? | Exemples |
|-----------------|---------|----------|
| Temporelles | ☐ | mois, jour_semaine, is_weekend |
| Agrégées | ☐ | moyenne_par_client, total_par_categorie |
| Calculées | ☐ | marge, ratio, pourcentage |
| Catégorielles | ☐ | segment, tranche |
| Indicateurs | ☐ | is_nouveau, a_retour |

### Checklist d'évaluation

- [ ] Sources combinées avec jointures appropriées
- [ ] Features temporelles créées
- [ ] Features agrégées calculées
- [ ] Features métier pertinentes ajoutées
- [ ] Dataset final documenté
- [ ] Colonnes ordonnées logiquement

### Note Phase 4 : _____ / 15

---

## Phase 5 : Analyse (15 points)

### Critères détaillés

| Critère | 0-4 pts | 5-9 pts | 10-15 pts |
|---------|---------|---------|-----------|
| **Couverture des questions** (6 pts) | < 3 questions traitées | Toutes questions traitées | + findings inattendus |
| **Quantification** (5 pts) | Insights sans chiffres | Chiffres présents | Chiffres percutants et contextualisés |
| **Recommandations** (4 pts) | Pas de recommandations | Recommandations présentes | Recommandations actionnables et priorisées |

### Checklist d'évaluation

- [ ] Chaque question business a une analyse dédiée
- [ ] Insights quantifiés avec chiffres clés
- [ ] Limites des analyses mentionnées
- [ ] Recommandations formulées
- [ ] Conclusions validées
- [ ] Découvertes inattendues documentées

### Note Phase 5 : _____ / 15

---

## Phase 6 : Visualisation (10 points)

### Critères détaillés

| Critère | 0-2 pts | 3-4 pts | 5 pts |
|---------|---------|---------|-------|
| **Quantité et variété** (3 pts) | < 3 visualisations | 5-7 visualisations | 7+ visualisations variées |
| **Qualité du design** (4 pts) | Mauvaises pratiques | Propre et lisible | Professionnel et impactant |
| **Storytelling** (3 pts) | Pas de fil conducteur | Récit présent | Récit captivant et structuré |

### Types de visualisations attendues

| Type | Présent ? | Utilisé pour |
|------|-----------|--------------|
| Distribution | ☐ | _______________ |
| Comparaison | ☐ | _______________ |
| Évolution | ☐ | _______________ |
| Composition | ☐ | _______________ |
| Corrélation | ☐ | _______________ |

### Checklist d'évaluation

- [ ] Au moins 5 visualisations produites
- [ ] Chaque graphique a un titre clair
- [ ] Axes labellisés avec unités
- [ ] Légendes présentes si nécessaire
- [ ] Pas de mauvaises pratiques (3D, axes tronqués...)
- [ ] Insights mis en évidence
- [ ] Cohérence visuelle (couleurs, polices)

### Note Phase 6 : _____ / 10

---

## Phase 7 : Documentation (10 points)

### Critères détaillés

| Critère | 0-2 pts | 3-4 pts | 5 pts |
|---------|---------|---------|-------|
| **Data Dictionary** (4 pts) | Absent | Partiel | Complet et détaillé |
| **Code/Notebook** (3 pts) | Non documenté | Partiellement commenté | Bien structuré et commenté |
| **Reproductibilité** (3 pts) | Non reproductible | Partiellement | Entièrement reproductible |

### Checklist d'évaluation

- [ ] Dataset final exporté dans le bon format
- [ ] Data Dictionary complet
- [ ] Toutes colonnes documentées
- [ ] Transformations tracées
- [ ] Notebook structuré et commenté
- [ ] Code exécutable de bout en bout

### Note Phase 7 : _____ / 10

---

## Utilisation de l'IA (5 points)

### Critères détaillés

| Critère | 0-1 pt | 2-3 pts | 4-5 pts |
|---------|--------|---------|---------|
| **Intégration de l'IA** (5 pts) | Non utilisée ou mal utilisée | Utilisée ponctuellement | Utilisée stratégiquement tout au long |

### Checklist d'évaluation

- [ ] IA utilisée pour le cadrage/brainstorming
- [ ] IA utilisée pour le diagnostic/interprétation
- [ ] IA utilisée pour les décisions de nettoyage
- [ ] IA utilisée pour le feature engineering
- [ ] IA utilisée pour la formulation des insights
- [ ] IA utilisée pour la documentation

### Preuves d'utilisation

| Phase | Prompt utilisé ? | Utilité démontrée ? |
|-------|------------------|---------------------|
| Cadrage | ☐ | ☐ |
| Extraction | ☐ | ☐ |
| Diagnostic | ☐ | ☐ |
| Nettoyage | ☐ | ☐ |
| Transformation | ☐ | ☐ |
| Analyse | ☐ | ☐ |
| Visualisation | ☐ | ☐ |
| Documentation | ☐ | ☐ |

### Note Utilisation IA : _____ / 5

---

## Récapitulatif des notes

| Phase | Note | Max |
|-------|------|-----|
| Phase 0 : Cadrage | _____ | /10 |
| Phase 1 : Extraction | _____ | /10 |
| Phase 2 : Diagnostic | _____ | /10 |
| Phase 3 : Nettoyage | _____ | /15 |
| Phase 4 : Transformation | _____ | /15 |
| Phase 5 : Analyse | _____ | /15 |
| Phase 6 : Visualisation | _____ | /10 |
| Phase 7 : Documentation | _____ | /10 |
| Utilisation IA | _____ | /5 |
| **TOTAL** | **_____** | **/100** |

---

## Barème de conversion

| Note /100 | Appréciation |
|-----------|--------------|
| 90-100 | Excellent - Travail de qualité professionnelle |
| 80-89 | Très bien - Maîtrise solide des compétences |
| 70-79 | Bien - Bonnes compétences, quelques points à améliorer |
| 60-69 | Satisfaisant - Bases acquises, axes de progression |
| 50-59 | Passable - Compétences partielles |
| < 50 | Insuffisant - Travail à reprendre |

---

## Commentaires généraux

### Points forts du projet

```
_________________________________________________________________
_________________________________________________________________
_________________________________________________________________
```

### Axes d'amélioration

```
_________________________________________________________________
_________________________________________________________________
_________________________________________________________________
```

### Observations complémentaires

```
_________________________________________________________________
_________________________________________________________________
_________________________________________________________________
```

---

## Signature

**Évaluateur** : _______________________

**Date** : _______________________

---

*Cette grille fait partie du workflow "Projet Data Pipeline".*
