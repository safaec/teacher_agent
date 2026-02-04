# Chapitre 4 — Partie 3 : Le Compromis Biais-Variance

**Durée estimée : 1h30**

## 🎯 Objectifs d'apprentissage

À la fin de cette partie, vous serez capable de :
1. **Expliquer** ce que représentent le biais et la variance d'un modèle
2. **Diagnostiquer** l'underfitting et l'overfitting à partir des performances train/test
3. **Tracer** et interpréter des courbes d'apprentissage (learning curves)
4. **Appliquer** des stratégies pour trouver le bon équilibre

---

## Lien avec les chapitres précédents

Dans le **Chapitre 3 (Partie 3 : Arbres et Random Forest)**, nous avons observé un phénomène troublant : un arbre de décision profond pouvait atteindre 100% d'accuracy sur le train mais seulement 85% sur le test. Nous avons nommé cela **overfitting**.

Maintenant, nous allons comprendre ce phénomène en profondeur avec le cadre théorique du **compromis biais-variance**.

---

## 🌍 Problème Réel : Le dilemme du médecin

Imaginez deux médecins qui diagnostiquent des patients :

**Dr. Prudent** : "Tous mes patients ont un rhume."
- Simple, jamais faux pour les vrais rhumes
- Mais il manque toutes les autres maladies !

**Dr. Paranoia** : "Chaque patient a une maladie unique basée sur ses 47 symptômes exacts."
- Diagnostic ultra-personnalisé
- Mais incapable de reconnaître un rhume classique chez un nouveau patient

**Question :** Lequel de ces deux médecins préféreriez-vous consulter ?

*(Réponse attendue : Aucun des deux n'est idéal ! Le premier est trop simpliste (il manque des cas), le second est trop spécifique (il ne généralise pas).)*

---

Ce dilemme capture l'essence du **compromis biais-variance** en Machine Learning.

---

## 3.1 Décomposer l'Erreur : Biais et Variance

### L'idée fondamentale

L'erreur totale d'un modèle peut être décomposée en trois composantes :

$$\text{Erreur totale} = \text{Biais}^2 + \text{Variance} + \text{Bruit irréductible}$$

```
┌─────────────────────────────────────────────────────────────────────┐
│           DÉCOMPOSITION DE L'ERREUR                                 │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   ERREUR TOTALE = BIAIS² + VARIANCE + BRUIT                         │
│                                                                     │
│   ┌───────────────────────────────────────────────────────────────┐ │
│   │                                                               │ │
│   │   BIAIS                    VARIANCE                           │ │
│   │   ═════                    ════════                           │ │
│   │   Erreur due à des         Erreur due à la                    │ │
│   │   hypothèses trop          sensibilité aux                    │ │
│   │   SIMPLISTES               DONNÉES d'entraînement             │ │
│   │                                                               │ │
│   │   "Le modèle ne peut       "Le modèle change                  │ │
│   │   pas capturer la          beaucoup si on change              │ │
│   │   complexité réelle"       un peu les données"                │ │
│   │                                                               │ │
│   │   → UNDERFITTING           → OVERFITTING                      │ │
│   │                                                               │ │
│   └───────────────────────────────────────────────────────────────┘ │
│                                                                     │
│   BRUIT IRRÉDUCTIBLE : erreur aléatoire inhérente aux données      │
│                        (on ne peut pas l'éliminer)                  │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 3.2 Le Biais : Quand le Modèle est Trop Simple

### Analogie du tireur à l'arc

Imaginez un tireur à l'arc qui vise une cible :

```
┌─────────────────────────────────────────────────────────────────────┐
│           HAUT BIAIS : Le tireur vise systématiquement à côté      │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│                         🎯                                          │
│                        /│\                                          │
│                       / │ \                                         │
│                      /  │  \                                        │
│                     /   │   \                                       │
│                    /    │    \                                      │
│           ●      /     │     \                                      │
│           ●     /      │      \                                     │
│           ●    /       │       \     ← Tous les tirs groupés        │
│                        │              MAIS loin de la cible         │
│                        │                                            │
│   Le tireur a un BIAIS : sa technique est imparfaite,              │
│   il rate systématiquement dans la même direction.                  │
│                                                                     │
│   → Même avec plus d'entraînement, il ratera toujours               │
│     (il doit CHANGER sa technique)                                  │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

**En ML :** Un modèle à haut biais fait des hypothèses trop simplistes. Peu importe combien de données vous lui donnez, il ne peut pas capturer la vraie complexité.

**Exemples :**
- Régression linéaire sur des données en forme de courbe
- Arbre de décision avec `max_depth=1` sur un problème complexe

---

### Symptômes du haut biais (Underfitting)

| Signal | Ce que vous observez |
|--------|---------------------|
| Score train | **Faible** (le modèle ne capte pas les patterns) |
| Score test | **Faible** (également) |
| Écart train-test | **Faible** (les deux sont mauvais) |

```
┌─────────────────────────────────────────────────────────────────────┐
│              UNDERFITTING : DIAGNOSTIC                              │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   Score train : 65%  ← Le modèle n'apprend même pas les données    │
│   Score test  : 62%                                                 │
│   Écart       : 3%   ← Faible écart, mais tout est mauvais !       │
│                                                                     │
│   Le modèle est trop SIMPLE pour la tâche.                         │
│   Il ne peut pas capturer les patterns même sur l'entraînement.    │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

┌─────────────────────────────────────────────────────────────────────┐
│ 📖 DÉFINITION : Biais (Bias)                                        │
│                                                                     │
│ Le biais d'un modèle est l'erreur systématique due à des           │
│ hypothèses trop simplistes. Un modèle à haut biais ne peut pas     │
│ représenter la complexité réelle des données, peu importe la       │
│ quantité de données d'entraînement.                                 │
│                                                                     │
│ Causes :                                                            │
│ • Modèle trop simple (peu de paramètres)                           │
│ • Features insuffisantes ou non pertinentes                        │
│ • Hypothèses incorrectes (ex: linéarité quand relation non-linéaire)│
│                                                                     │
│ Conséquence : UNDERFITTING                                          │
└─────────────────────────────────────────────────────────────────────┘

---

## 3.3 La Variance : Quand le Modèle est Trop Complexe

### Suite de l'analogie du tireur

```
┌─────────────────────────────────────────────────────────────────────┐
│           HAUTE VARIANCE : Le tireur est imprévisible              │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│                         🎯                                          │
│                  ●     /│\     ●                                    │
│                       / │ \                                         │
│             ●        /  │  \        ●                               │
│                     /   │   \                                       │
│                    /    │    \                                      │
│          ●        /     │     \     ●                               │
│                  /      │      \                                    │
│                 /       │       \                                   │
│                         │                                           │
│                                                                     │
│   Les tirs sont DISPERSÉS partout autour de la cible.              │
│   Parfois il touche, parfois il est très loin.                     │
│                                                                     │
│   → Le tireur est trop sensible aux conditions du moment           │
│     (vent, fatigue, etc.)                                          │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

**En ML :** Un modèle à haute variance est trop sensible aux données d'entraînement. Il "mémorise" les particularités des données au lieu d'apprendre les patterns généraux.

**Exemples :**
- Arbre de décision très profond (sans limite)
- K-NN avec K=1
- Réseau de neurones surdimensionné

---

### Symptômes de la haute variance (Overfitting)

| Signal | Ce que vous observez |
|--------|---------------------|
| Score train | **Très élevé** (proche de 100%) |
| Score test | **Plus bas** |
| Écart train-test | **Grand** (c'est le signal d'alerte !) |

```
┌─────────────────────────────────────────────────────────────────────┐
│              OVERFITTING : DIAGNOSTIC                               │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   Score train : 99%  ← Le modèle "mémorise" les données            │
│   Score test  : 75%                                                 │
│   Écart       : 24%  ← GRAND écart = signal d'overfitting !        │
│                                                                     │
│   Le modèle est trop COMPLEXE.                                      │
│   Il a mémorisé le bruit des données d'entraînement.               │
│   Il ne généralise pas aux nouvelles données.                       │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

┌─────────────────────────────────────────────────────────────────────┐
│ 📖 DÉFINITION : Variance                                            │
│                                                                     │
│ La variance d'un modèle mesure à quel point ses prédictions        │
│ changent si on l'entraîne sur un jeu de données différent.         │
│ Un modèle à haute variance est très sensible aux données           │
│ d'entraînement et capte le bruit plutôt que les vrais patterns.    │
│                                                                     │
│ Causes :                                                            │
│ • Modèle trop complexe (trop de paramètres)                        │
│ • Pas assez de données d'entraînement                              │
│ • Pas de régularisation                                             │
│                                                                     │
│ Conséquence : OVERFITTING                                           │
└─────────────────────────────────────────────────────────────────────┘

---

## 3.4 Le Compromis : Trouver le Point Optimal

### Visualiser le compromis

```
┌─────────────────────────────────────────────────────────────────────┐
│           LE COMPROMIS BIAIS-VARIANCE                               │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   Erreur                                                            │
│     │                                                               │
│     │  \                                                            │
│     │   \    Biais²                                                 │
│     │    \                    Variance                              │
│     │     \                    /                                    │
│     │      \                  /                                     │
│     │       \               /                                       │
│     │        \             /                                        │
│     │         \    ●      /     Erreur                              │
│     │          \  OPTIMAL/      Totale                              │
│     │           ──●──●──                                            │
│     │              \  /                                             │
│     └──────────────────────────────────────── Complexité            │
│                                                                     │
│       SIMPLE ←──────────────────────────────→ COMPLEXE              │
│       (Haut Biais,                     (Bas Biais,                  │
│        Basse Variance)                  Haute Variance)             │
│                                                                     │
│       UNDERFITTING                         OVERFITTING              │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

**Question :** Où voudriez-vous que votre modèle se situe sur ce graphique ?

*(Réponse attendue : Au point optimal, là où l'erreur totale est minimale — c'est un équilibre entre biais et variance.)*

---

## 3.5 Diagnostic avec les Courbes d'Apprentissage (Learning Curves)

### Qu'est-ce qu'une learning curve ?

Une courbe d'apprentissage montre comment les performances évoluent en fonction de la **quantité de données d'entraînement**.

```
┌─────────────────────────────────────────────────────────────────────┐
│           LEARNING CURVES : Interpréter les patterns                │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   CAS 1 : UNDERFITTING (Haut Biais)                                 │
│   ─────────────────────────────────                                 │
│                                                                     │
│   Score                                                             │
│     │                                                               │
│     │   ────────────────────────── Train (plat et bas)             │
│     │   ────────────────────────── Test (plat et bas)              │
│     └──────────────────────────────── Nb données                    │
│                                                                     │
│   → Les deux courbes convergent vers un score BAS                   │
│   → Plus de données n'aidera pas !                                  │
│   → Solution : modèle plus complexe                                 │
│                                                                     │
│   ─────────────────────────────────────────────────────────────────│
│                                                                     │
│   CAS 2 : OVERFITTING (Haute Variance)                              │
│   ────────────────────────────────────                              │
│                                                                     │
│   Score                                                             │
│     │   ────────────────────────── Train (très haut)               │
│     │                                                               │
│     │                                                               │
│     │            ───────────────── Test (plus bas)                 │
│     │       ↑ GRAND ÉCART                                          │
│     └──────────────────────────────── Nb données                    │
│                                                                     │
│   → Grand écart persistant entre train et test                      │
│   → Plus de données POURRAIT aider                                  │
│   → Solution : simplifier le modèle ou régulariser                  │
│                                                                     │
│   ─────────────────────────────────────────────────────────────────│
│                                                                     │
│   CAS 3 : BON ÉQUILIBRE                                             │
│   ─────────────────────────────                                     │
│                                                                     │
│   Score                                                             │
│     │   ────────────────────────── Train                           │
│     │   ────────────────────────── Test                            │
│     │   (courbes proches et hautes)                                │
│     └──────────────────────────────── Nb données                    │
│                                                                     │
│   → Les courbes convergent vers un score ÉLEVÉ                      │
│   → Écart faible entre train et test                                │
│   → Le modèle généralise bien !                                     │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 3.6 Stratégies pour Trouver l'Équilibre

### Si vous avez un HAUT BIAIS (Underfitting)

```
┌─────────────────────────────────────────────────────────────────────┐
│           CORRIGER L'UNDERFITTING                                   │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   1. AUGMENTER LA COMPLEXITÉ DU MODÈLE                              │
│      • Passer de régression linéaire à polynomiale                 │
│      • Augmenter max_depth des arbres                               │
│      • Ajouter des couches/neurones au réseau                       │
│                                                                     │
│   2. AJOUTER DES FEATURES                                           │
│      • Feature engineering (créer de nouvelles variables)          │
│      • Features polynomiales (x², x³, interactions)                │
│                                                                     │
│   3. DIMINUER LA RÉGULARISATION                                     │
│      • Réduire le paramètre alpha/lambda                           │
│                                                                     │
│   ⚠️ CE QUI NE MARCHERA PAS :                                       │
│      • Ajouter plus de données (le modèle est limité par design)   │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

### Si vous avez une HAUTE VARIANCE (Overfitting)

```
┌─────────────────────────────────────────────────────────────────────┐
│           CORRIGER L'OVERFITTING                                    │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   1. SIMPLIFIER LE MODÈLE                                           │
│      • Réduire max_depth des arbres                                 │
│      • Moins de features                                            │
│      • Moins de paramètres                                          │
│                                                                     │
│   2. AJOUTER DE LA RÉGULARISATION                                   │
│      • L1 (Lasso) ou L2 (Ridge) pour les modèles linéaires         │
│      • min_samples_split, min_samples_leaf pour les arbres         │
│      • Dropout pour les réseaux de neurones                         │
│                                                                     │
│   3. AJOUTER PLUS DE DONNÉES                                        │
│      • Plus d'exemples d'entraînement                              │
│      • Data augmentation si possible                                │
│                                                                     │
│   4. TECHNIQUES D'ENSEMBLE                                          │
│      • Random Forest au lieu d'un seul arbre                        │
│      • Bagging (moyenne de plusieurs modèles)                       │
│                                                                     │
│   5. EARLY STOPPING                                                 │
│      • Arrêter l'entraînement avant le sur-apprentissage           │
│                                                                     │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 3.7 Récapitulatif : Guide de Diagnostic

```
┌─────────────────────────────────────────────────────────────────────┐
│           GUIDE DE DIAGNOSTIC RAPIDE                                │
├─────────────────────────────────────────────────────────────────────┤
│                                                                     │
│   OBSERVATION          │ DIAGNOSTIC      │ ACTION                   │
│   ─────────────────────┼─────────────────┼─────────────────────────│
│                        │                 │                          │
│   Train BAS            │                 │ Modèle plus complexe     │
│   Test BAS             │ UNDERFITTING    │ Plus de features         │
│   Écart FAIBLE         │ (Haut Biais)    │ Moins de régularisation  │
│                        │                 │                          │
│   ─────────────────────┼─────────────────┼─────────────────────────│
│                        │                 │                          │
│   Train ÉLEVÉ          │                 │ Simplifier le modèle     │
│   Test BAS             │ OVERFITTING     │ Plus de régularisation   │
│   Écart GRAND          │ (Haute Variance)│ Plus de données          │
│                        │                 │                          │
│   ─────────────────────┼─────────────────┼─────────────────────────│
│                        │                 │                          │
│   Train ÉLEVÉ          │                 │                          │
│   Test ÉLEVÉ           │ BON ÉQUILIBRE   │ Continuer !             │
│   Écart FAIBLE         │ ✓               │                          │
│                        │                 │                          │
└─────────────────────────────────────────────────────────────────────┘
```

---

## 🧠 Réflexion Métacognitive

Avant de passer à la suite :

1. **Pouvez-vous expliquer** pourquoi un arbre de décision profond a haute variance ?

2. **Si un modèle** a 65% sur train et 63% sur test, quel est le problème probable ?

3. **La régularisation** réduit la variance — mais quel effet a-t-elle sur le biais ?

---

## 📝 Résumé

| Concept | Définition | Symptôme | Solution |
|---------|------------|----------|----------|
| **Biais** | Hypothèses trop simplistes | Train et test bas | Plus de complexité |
| **Variance** | Sensibilité aux données | Grand écart train-test | Régularisation, plus de données |
| **Underfitting** | Modèle trop simple | Ne capte pas les patterns | Augmenter complexité |
| **Overfitting** | Modèle trop complexe | Mémorise le bruit | Simplifier, régulariser |

**Formule clé :**
$$\text{Erreur} = \text{Biais}^2 + \text{Variance} + \text{Bruit irréductible}$$

---

## ➡️ Prochaine partie

Dans la **Partie 4 : Validation Croisée**, nous allons apprendre à évaluer nos modèles de manière plus robuste avec le K-Fold Cross-Validation.

**Question de transition :** Comment être sûr que notre score de test n'est pas juste un coup de chance dû à un split particulier des données ?
