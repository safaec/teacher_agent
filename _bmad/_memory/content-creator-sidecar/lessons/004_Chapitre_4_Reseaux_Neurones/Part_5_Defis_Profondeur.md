# Chapitre 4 : Réseaux de Neurones (Conceptuel)

## Part 5 : Les Défis de la Profondeur — Et Leurs Solutions

**Durée estimée : 1h30**

---

### Objectifs d'apprentissage

À la fin de cette partie, vous serez capable de :
1. **Expliquer** pourquoi les réseaux profonds fonctionnent mieux que les réseaux peu profonds
2. **Identifier** les défis spécifiques aux réseaux profonds (vanishing gradients)
3. **Décrire** les solutions modernes : ReLU, Batch Normalization, Dropout

---

### 🎯 Accroche : Le Paradoxe du Deep Learning

*Nous avons vu que les réseaux profonds peuvent apprendre des représentations hiérarchiques — des bords aux formes aux objets. En théorie, plus c'est profond, mieux c'est.*

*Mais pendant des décennies, les chercheurs n'arrivaient pas à entraîner des réseaux de plus de 2-3 couches. L'information semblait "se perdre" quelque part. Que se passait-il ?*

```
┌─────────────────────────────────────────────────────────────────┐
│                LE PARADOXE (avant 2012)                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   THÉORIE :          Plus de couches = Meilleures features     │
│                       = Meilleures performances                 │
│                                                                 │
│   PRATIQUE :         Plus de 3 couches... le réseau n'apprend  │
│                       RIEN. La loss ne descend pas.            │
│                                                                 │
│   ┌────────────────────────────────────────────────┐           │
│   │         Loss pendant l'entraînement            │           │
│   │                                                │           │
│   │   2 couches : \_____  ← Converge bien         │           │
│   │                                                │           │
│   │   5 couches : ────────  ← Bloqué !            │           │
│   │                                                │           │
│   │   10 couches : ───────────  ← Rien ne se passe│           │
│   │                                                │           │
│   └────────────────────────────────────────────────┘           │
│                                                                 │
│   "L'hiver de l'IA" (1990s-2000s) était en partie dû à cela.  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Question :** D'après ce que nous avons appris sur la backpropagation, avez-vous une idée de ce qui pourrait "bloquer" l'apprentissage dans les couches profondes ?

*(Réponse attendue : Si le gradient devient très petit en remontant les couches, les poids des premières couches ne sont presque pas mis à jour. Ils restent quasi-aléatoires.)*

---

## 5.1 Pourquoi "Deep" Fonctionne (Quand ça marche)

### Les avantages de la profondeur

Avant de parler des problèmes, rappelons pourquoi on VEUT des réseaux profonds :

```
┌─────────────────────────────────────────────────────────────────┐
│          POURQUOI LA PROFONDEUR EST PUISSANTE                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   1. ABSTRACTIONS HIÉRARCHIQUES                                │
│   ═════════════════════════════                                │
│                                                                 │
│   Couche 1 : Pixels → Bords                                    │
│   Couche 2 : Bords → Formes                                    │
│   Couche 3 : Formes → Parties                                  │
│   Couche 4 : Parties → Objets                                  │
│                                                                 │
│   Chaque couche construit sur la précédente.                   │
│   On ne pourrait pas sauter directement de pixels à objets.    │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   2. EFFICACITÉ DES PARAMÈTRES                                 │
│   ════════════════════════════                                 │
│                                                                 │
│   Pour représenter certaines fonctions :                       │
│   • Shallow : 2^n neurones nécessaires                         │
│   • Deep : O(n) neurones suffisent                             │
│                                                                 │
│   Exemple : Multiplier n nombres binaires                      │
│   • 1 couche : exponentiel en n                                │
│   • log(n) couches : polynomial en n                           │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   3. RÉUTILISATION DES FEATURES                                │
│   ═════════════════════════════                                │
│                                                                 │
│   Les features de bas niveau (bords, textures) sont            │
│   RÉUTILISABLES pour différentes tâches :                      │
│   • Reconnaître des chats utilise les mêmes bords que chiens   │
│   • Transfer Learning : pré-entraîner, puis spécialiser        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 5.2 Le Problème : Vanishing Gradients

### Le gradient qui s'évapore

Rappel : Pendant la backpropagation, le gradient est propagé de la sortie vers l'entrée. À chaque couche, il est multiplié par la dérivée de l'activation.

```
┌─────────────────────────────────────────────────────────────────┐
│              LE PROBLÈME DU VANISHING GRADIENT                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Avec SIGMOID comme activation :                              │
│                                                                 │
│   Dérivée de sigmoid : σ'(z) = σ(z) × (1 - σ(z))              │
│   Maximum : 0.25 (quand z = 0)                                 │
│                                                                 │
│   À chaque couche, le gradient est multiplié par ≤ 0.25       │
│                                                                 │
│   ┌─────────────────────────────────────────────────────────┐  │
│   │                                                         │  │
│   │   Gradient à la sortie : 1.0                            │  │
│   │                          │                              │  │
│   │                          × 0.25                         │  │
│   │                          ▼                              │  │
│   │   Après couche 4 :      0.25                            │  │
│   │                          │                              │  │
│   │                          × 0.25                         │  │
│   │                          ▼                              │  │
│   │   Après couche 3 :      0.0625                          │  │
│   │                          │                              │  │
│   │                          × 0.25                         │  │
│   │                          ▼                              │  │
│   │   Après couche 2 :      0.0156                          │  │
│   │                          │                              │  │
│   │                          × 0.25                         │  │
│   │                          ▼                              │  │
│   │   Après couche 1 :      0.0039  ← Quasi-ZÉRO !          │  │
│   │                                                         │  │
│   └─────────────────────────────────────────────────────────┘  │
│                                                                 │
│   En 4 couches : gradient ÷ 256                                │
│   En 10 couches : gradient ÷ 1,000,000+ → ZÉRO                │
│                                                                 │
│   ⚠️ Les premières couches ne reçoivent quasi aucun signal    │
│      d'apprentissage ! Leurs poids restent aléatoires.         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Question :** Si les gradients des premières couches sont quasi-nuls, que se passe-t-il pour ces couches ?

*(Réponse attendue : Elles n'apprennent pas ! Leurs poids restent proches de l'initialisation aléatoire. Seules les dernières couches apprennent, ce qui revient à avoir un réseau peu profond.)*

---

┌─────────────────────────────────────────────────────────────────┐
│ 📖 DÉFINITION : Vanishing Gradient Problem                      │
│                                                                 │
│ Le problème du gradient qui disparaît (vanishing gradient)     │
│ survient dans les réseaux profonds lorsque les gradients       │
│ deviennent exponentiellement petits en se propageant vers      │
│ les premières couches.                                          │
│                                                                 │
│ Causes :                                                        │
│ • Fonctions d'activation saturantes (sigmoid, tanh) dont la    │
│   dérivée est < 1 dans la majeure partie du domaine            │
│ • Multiplication répétée de petites valeurs = explosion vers 0 │
│                                                                 │
│ Conséquences :                                                  │
│ • Les couches profondes (proches de l'entrée) n'apprennent pas │
│ • Le réseau se comporte comme s'il était peu profond           │
│ • L'entraînement stagne (loss ne descend pas)                  │
│                                                                 │
│ Problème symétrique : Exploding Gradients (gradients > 1       │
│ qui explosent vers l'infini) — résolu par gradient clipping.   │
└─────────────────────────────────────────────────────────────────┘

---

### Autres défis des réseaux profonds

```
┌─────────────────────────────────────────────────────────────────┐
│           AUTRES DÉFIS DE LA PROFONDEUR                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   1. COÛT COMPUTATIONNEL                                       │
│   ══════════════════════                                       │
│   • Plus de couches = plus de calculs                          │
│   • Forward pass plus long                                     │
│   • Backward pass plus long                                    │
│   • Besoin de GPUs puissants                                   │
│                                                                 │
│   2. BESOIN DE DONNÉES                                         │
│   ════════════════════                                         │
│   • Plus de paramètres = plus facile de mémoriser              │
│   • Risque d'overfitting sans assez de données                 │
│   • Règle empirique : 10× plus de données que de paramètres    │
│                                                                 │
│   3. INSTABILITÉ D'ENTRAÎNEMENT                               │
│   ═════════════════════════════                                │
│   • Gradients qui explosent (exploding gradients)              │
│   • Loss qui oscille ou diverge                                │
│   • Sensibilité à l'initialisation des poids                   │
│                                                                 │
│   4. OVERFITTING                                               │
│   ══════════════                                               │
│   • Réseau très expressif peut mémoriser le train set          │
│   • Mauvaise généralisation aux nouvelles données              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 5.3 Les Solutions Modernes

### Solution 1 : ReLU — Laisser passer le signal

Nous avons déjà vu ReLU dans la Partie 2, mais maintenant nous comprenons son importance CRITIQUE :

```
┌─────────────────────────────────────────────────────────────────┐
│          RELU : LA SOLUTION AU VANISHING GRADIENT               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   SIGMOID                         RELU                          │
│   ═══════                         ════                          │
│                                                                 │
│   Dérivée : σ'(z) = σ(1-σ)       Dérivée : 1 si z > 0          │
│   Maximum : 0.25                            0 si z ≤ 0          │
│                                                                 │
│   ┌────────────────────┐         ┌────────────────────┐        │
│   │ Gradient × 0.25    │         │ Gradient × 1       │        │
│   │ à chaque couche    │         │ à chaque couche    │        │
│   │                    │         │ (si z > 0)         │        │
│   │ → S'évapore !      │         │ → RESTE INTACT !   │        │
│   └────────────────────┘         └────────────────────┘        │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   COMPARAISON SUR 10 COUCHES :                                 │
│                                                                 │
│   Sigmoid : 0.25^10 = 0.00000095  → Quasi-zéro                │
│   ReLU    : 1^10 = 1               → Gradient intact !         │
│                                                                 │
│   C'est pourquoi ReLU a RÉVOLUTIONNÉ le Deep Learning          │
│   en 2012 (AlexNet).                                           │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

<details>
<summary>🤔 Question Socratique : Mais si la dérivée de ReLU est 0 pour z ≤ 0, n'est-ce pas aussi un problème ?</summary>

### 🔑 Réponse

Excellente observation ! C'est le problème des **"dying ReLU"** (neurones morts).

Si un neurone a z < 0 pour TOUTES les données, son gradient est toujours 0, et il ne peut plus apprendre. Il est "mort".

**Pourquoi ce n'est pas aussi grave que le vanishing gradient :**
1. Seulement ~50% des neurones ont z < 0 à un instant donné (pas tous)
2. Sur différentes données, différents neurones sont actifs
3. Les neurones morts sont "rares" si l'initialisation est bonne

**Solutions si le problème survient :**
- **Leaky ReLU** : f(z) = z si z > 0, sinon 0.01z (petit gradient même si négatif)
- **ELU** : Sortie négative possible, plus lisse
- **Meilleure initialisation** (Xavier, He)

En pratique, ReLU standard fonctionne très bien dans la plupart des cas.

</details>

---

### Solution 2 : Batch Normalization — Stabiliser les données

```
┌─────────────────────────────────────────────────────────────────┐
│           BATCH NORMALIZATION : STABILISATION                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   LE PROBLÈME : Internal Covariate Shift                       │
│   ══════════════════════════════════════                       │
│                                                                 │
│   Pendant l'entraînement, la distribution des activations      │
│   change à chaque couche, à chaque batch.                      │
│                                                                 │
│   Batch 1 : Couche 2 reçoit des valeurs dans [-2, 5]          │
│   Batch 2 : Couche 2 reçoit des valeurs dans [0, 10]          │
│   Batch 3 : Couche 2 reçoit des valeurs dans [-5, 2]          │
│                                                                 │
│   → La couche doit constamment se réajuster !                  │
│   → Entraînement instable et lent                              │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   LA SOLUTION : Normaliser à chaque couche                     │
│   ════════════════════════════════════════                     │
│                                                                 │
│   Après chaque couche, AVANT l'activation :                    │
│                                                                 │
│   1. Calculer moyenne et variance du batch                     │
│      μ = moyenne(activations)                                  │
│      σ² = variance(activations)                                │
│                                                                 │
│   2. Normaliser : x̂ = (x - μ) / √(σ² + ε)                     │
│      → Moyenne ≈ 0, Variance ≈ 1                               │
│                                                                 │
│   3. Rescale avec paramètres appris : y = γx̂ + β             │
│      → Le réseau peut "défaire" la normalisation si utile     │
│                                                                 │
│   RÉSULTAT : Distributions stables → Entraînement stable      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

### Avantages de Batch Normalization

```
┌─────────────────────────────────────────────────────────────────┐
│         POURQUOI BATCH NORMALIZATION EST PUISSANT               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   1. ENTRAÎNEMENT PLUS RAPIDE                                  │
│   • Permet des learning rates plus élevés                      │
│   • Convergence 10-14× plus rapide dans certains cas           │
│                                                                 │
│   2. MOINS SENSIBLE À L'INITIALISATION                         │
│   • Mauvaise initialisation → corrigée par normalisation       │
│   • Plus robuste aux hyperparamètres                           │
│                                                                 │
│   3. EFFET RÉGULARISANT                                        │
│   • Le "bruit" dû au calcul sur mini-batch                     │
│   • Aide à éviter l'overfitting (comme Dropout)                │
│   • Peut parfois remplacer Dropout                             │
│                                                                 │
│   4. PERMET DES RÉSEAUX PLUS PROFONDS                          │
│   • Stabilise le flux de gradients                             │
│   • Essentiel pour ResNet (152 couches !)                      │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   PLACEMENT TYPIQUE :                                          │
│                                                                 │
│   Dense → BatchNorm → ReLU → Dense → BatchNorm → ReLU → ...   │
│                                                                 │
│   (Le placement exact fait débat : avant ou après ReLU ?)      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

┌─────────────────────────────────────────────────────────────────┐
│ 📖 DÉFINITION : Batch Normalization                             │
│                                                                 │
│ La Batch Normalization (Ioffe & Szegedy, 2015) est une         │
│ technique qui normalise les activations de chaque couche       │
│ pour avoir une moyenne proche de 0 et une variance proche de 1.│
│                                                                 │
│ Formules :                                                      │
│ • Normalisation : x̂ = (x - μ_batch) / √(σ²_batch + ε)         │
│ • Transformation : y = γ × x̂ + β                              │
│                                                                 │
│ Où γ et β sont des paramètres APPRIS qui permettent au         │
│ réseau de "défaire" la normalisation si nécessaire.            │
│                                                                 │
│ Pendant l'inférence :                                           │
│ • On utilise μ et σ² calculés sur le training set entier      │
│   (running average pendant l'entraînement)                     │
│ • Pas de dépendance au batch                                   │
│                                                                 │
│ Effet : Stabilise l'entraînement, permet des learning rates    │
│ plus élevés, agit comme régularisateur.                        │
└─────────────────────────────────────────────────────────────────┘

---

### Solution 3 : Dropout — Empêcher la mémorisation

```
┌─────────────────────────────────────────────────────────────────┐
│              DROPOUT : RÉGULARISATION PAR L'OUBLI               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   LE PROBLÈME : Overfitting                                    │
│   ═════════════════════════                                    │
│                                                                 │
│   Un réseau profond peut MÉMORISER le training set au lieu     │
│   d'apprendre des patterns généralisables.                     │
│                                                                 │
│   Train accuracy : 99.9%  }  GAP = Overfitting !               │
│   Test accuracy  : 70%    }                                    │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   LA SOLUTION : Dropout                                        │
│   ════════════════════                                         │
│                                                                 │
│   Pendant l'ENTRAÎNEMENT, à chaque forward pass :              │
│   → "Éteindre" aléatoirement certains neurones                 │
│                                                                 │
│   AVANT Dropout (100% actifs)    AVEC Dropout (50% actifs)    │
│                                                                 │
│        ● ● ● ●                        ● ○ ● ○                  │
│       /│\│/│\│\                      /│   │                    │
│      ● ● ● ● ● ●                    ○ ● ○ ● ● ○                │
│       \│/│\│/│/                        │\│/│                   │
│        ● ● ● ●                        ● ○ ● ●                  │
│                                                                 │
│   ○ = neurone "éteint" (sortie = 0)                           │
│   ● = neurone actif                                            │
│                                                                 │
│   À chaque batch, un DIFFÉRENT ensemble est éteint !          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

### Pourquoi Dropout fonctionne

```
┌─────────────────────────────────────────────────────────────────┐
│              INTUITION DERRIÈRE DROPOUT                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   1. EMPÊCHE LA CO-ADAPTATION                                  │
│   ═══════════════════════════                                  │
│                                                                 │
│   Sans Dropout : Les neurones peuvent "se reposer" sur         │
│   leurs voisins. "Neurone A s'occupe de ça, moi je ne fais rien"│
│                                                                 │
│   Avec Dropout : Chaque neurone doit être utile SEUL car       │
│   ses voisins pourraient être éteints.                         │
│                                                                 │
│   → Features plus robustes et indépendantes                    │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   2. COMME UN ENSEMBLE DE RÉSEAUX                              │
│   ═══════════════════════════════                              │
│                                                                 │
│   Chaque configuration de dropout = un réseau différent        │
│   Avec n neurones et p=0.5, il y a 2^n configurations !       │
│                                                                 │
│   Dropout ≈ Entraîner et moyenner 2^n réseaux                 │
│   (Similaire à Random Forest qui moyenne plusieurs arbres)     │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   PENDANT L'INFÉRENCE (test) :                                 │
│   ══════════════════════════                                   │
│                                                                 │
│   • Dropout est DÉSACTIVÉ (tous les neurones actifs)          │
│   • Les poids sont multipliés par (1-p) pour compenser        │
│     (ou les activations divisées par (1-p) pendant training)   │
│                                                                 │
│   VALEURS TYPIQUES :                                           │
│   • p = 0.2 à 0.5 (20% à 50% des neurones éteints)            │
│   • Couches cachées : p = 0.5                                  │
│   • Couche d'entrée : p = 0.2 (moins agressif)                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

┌─────────────────────────────────────────────────────────────────┐
│ 📖 DÉFINITION : Dropout                                         │
│                                                                 │
│ Dropout (Srivastava et al., 2014) est une technique de         │
│ régularisation qui "éteint" aléatoirement une fraction des     │
│ neurones pendant l'entraînement.                                │
│                                                                 │
│ Fonctionnement :                                                │
│ • Pendant le training : chaque neurone a une probabilité p     │
│   d'être mis à zéro à chaque forward pass                      │
│ • Pendant l'inférence : tous les neurones sont actifs,        │
│   poids multipliés par (1-p) pour compenser                    │
│                                                                 │
│ Effets :                                                        │
│ • Empêche la co-adaptation des neurones                        │
│ • Équivalent approximatif d'un ensemble de réseaux             │
│ • Réduit fortement l'overfitting                               │
│                                                                 │
│ Paramètre p typique :                                           │
│ • 0.5 pour les couches cachées (Hinton's recommendation)       │
│ • 0.1-0.2 pour la couche d'entrée                              │
│                                                                 │
│ Placement : Généralement après l'activation (après ReLU)       │
└─────────────────────────────────────────────────────────────────┘

---

## Récapitulatif : L'Architecture Moderne

```
┌─────────────────────────────────────────────────────────────────┐
│         RECETTE MODERNE D'UN RÉSEAU PROFOND                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   AVANT 2012 (ça ne marchait pas)                              │
│   ════════════════════════════════                             │
│                                                                 │
│   Dense → Sigmoid → Dense → Sigmoid → Dense → Softmax          │
│                                                                 │
│   ❌ Vanishing gradients après 2-3 couches                     │
│   ❌ Entraînement instable                                     │
│   ❌ Overfitting sévère                                        │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   APRÈS 2012 (la révolution Deep Learning)                     │
│   ════════════════════════════════════════                     │
│                                                                 │
│   Dense → BatchNorm → ReLU → Dropout →                         │
│   Dense → BatchNorm → ReLU → Dropout →                         │
│   Dense → BatchNorm → ReLU → Dropout →                         │
│   ...                                                           │
│   Dense → Softmax                                               │
│                                                                 │
│   ✅ Gradients qui passent (ReLU)                              │
│   ✅ Entraînement stable (BatchNorm)                           │
│   ✅ Régularisation (Dropout)                                  │
│   ✅ Réseaux de 10, 50, 100+ couches possibles !              │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   RÉSUMÉ DES SOLUTIONS :                                       │
│                                                                 │
│   ┌─────────────────┬────────────────────────────────────┐     │
│   │ Problème        │ Solution                           │     │
│   ├─────────────────┼────────────────────────────────────┤     │
│   │ Vanishing grad. │ ReLU (dérivée = 1 si z > 0)       │     │
│   │ Instabilité     │ Batch Normalization               │     │
│   │ Overfitting     │ Dropout + plus de données         │     │
│   │ Exploding grad. │ Gradient clipping + BatchNorm     │     │
│   │ Convergence     │ Adam optimizer (learning rate     │     │
│   │ lente           │   adaptatif)                       │     │
│   └─────────────────┴────────────────────────────────────┘     │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🧭 Récapitulatif et Transition

### Termes clés introduits

| Terme | Définition |
|-------|------------|
| **Vanishing Gradient** | Gradient qui s'évapore en remontant les couches |
| **Exploding Gradient** | Gradient qui explose (inverse) |
| **ReLU** | Activation qui laisse passer le gradient (dérivée = 1) |
| **Batch Normalization** | Normalise les activations pour stabilité |
| **Dropout** | Éteint aléatoirement des neurones pour régulariser |
| **Internal Covariate Shift** | Changement de distribution des activations |

### Ce que nous avons appris

1. **Pourquoi Deep > Shallow** : Abstractions hiérarchiques, efficacité, réutilisation des features

2. **Le problème** : Vanishing gradients empêchait l'entraînement de réseaux profonds

3. **Les solutions** :
   - **ReLU** : Laisse passer le gradient (dérivée = 1)
   - **BatchNorm** : Stabilise les distributions
   - **Dropout** : Régularise contre l'overfitting

---

### 🔮 Ce qui vient ensuite

*Nous avons maintenant toutes les pièces du puzzle :*
- *Le neurone (Partie 2)*
- *L'architecture MLP (Partie 3)*
- *La mécanique d'apprentissage (Partie 4)*
- *Les solutions aux défis de la profondeur (Partie 5)*

*Dans la partie suivante, nous allons VOIR tout cela en action avec TensorFlow Playground et écrire notre premier réseau avec Keras. Il est temps de mettre les mains dans le cambouis !*

---

## 📝 Réflexion Métacognitive

1. **Expliquez avec vos propres mots** pourquoi ReLU a été si important pour le Deep Learning.

2. **Connexion avec le Chapitre 3** : Le Dropout ressemble-t-il à une technique que nous avons vue ? (Indice : bagging, Random Forest)

3. **Question de design** : Si vous deviez construire un réseau pour un problème avec peu de données, quelles techniques utiliseriez-vous pour éviter l'overfitting ?

---

*Dans la partie finale, nous allons manipuler un réseau visuellement avec TensorFlow Playground et écrire notre premier code Keras !*
