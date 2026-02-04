# Chapitre 4 : Réseaux de Neurones (Conceptuel)

## Part 4 : La Mécanique d'Apprentissage — Le Cerveau

**Durée estimée : 2h30**

---

### Objectifs d'apprentissage

À la fin de cette partie, vous serez capable de :
1. **Expliquer** le cycle complet d'apprentissage d'un réseau de neurones
2. **Décrire** le rôle de la fonction de perte (loss function)
3. **Illustrer** le concept de gradient descent avec l'analogie de la montagne
4. **Résumer** le principe de la rétropropagation (backpropagation) sans formules complexes

---

### 🎯 Accroche : La Machine à Deviner

*Dans les parties précédentes, nous avons construit une architecture MLP : des couches de neurones connectés, avec des poids et des fonctions d'activation. Mais pour l'instant, ces poids sont aléatoires. Le réseau est comme un bébé qui ne sait rien.*

**Situation :** Vous montrez une photo du chiffre "3" au réseau. Avec des poids aléatoires, voici ce qu'il répond :

```
┌─────────────────────────────────────────────────────────────────┐
│            RÉSEAU NON ENTRAÎNÉ (Poids aléatoires)               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Image du "3"  ───►  [MLP avec poids aléatoires]               │
│                                                                 │
│                       Prédictions :                             │
│                       P(0) = 0.12   │▓▓                         │
│                       P(1) = 0.08   │▓                          │
│                       P(2) = 0.15   │▓▓                         │
│                       P(3) = 0.09   │▓          ← Devrait être  │
│                       P(4) = 0.11   │▓▓            le plus haut!│
│                       P(5) = 0.10   │▓▓                         │
│                       P(6) = 0.14   │▓▓                         │
│                       P(7) = 0.08   │▓                          │
│                       P(8) = 0.07   │▓                          │
│                       P(9) = 0.06   │▓                          │
│                                                                 │
│   Le réseau "devine" au hasard !                               │
│   Il prédit "2" avec 15%... alors que c'est un "3".            │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Question :** Comment faire pour que le réseau améliore ses prédictions ? Que faudrait-il modifier ?

*(Réponse attendue : Il faut modifier les poids ! Si les poids changent, les calculs changent, et donc les prédictions changent. Il faut trouver les "bons" poids qui donnent les bonnes réponses.)*

---

## 4.1 Forward Pass — La Prédiction

### Le flux de l'information

Le **Forward Pass** (passage avant) est simplement le processus de calcul de la prédiction :

```
┌─────────────────────────────────────────────────────────────────┐
│                    FORWARD PASS                                 │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   DONNÉES          COUCHE 1         COUCHE 2         SORTIE    │
│   D'ENTRÉE                                                      │
│                                                                 │
│      x            h₁ = f(W₁·x + b₁)  h₂ = f(W₂·h₁ + b₂)   ŷ   │
│                                                                 │
│   ┌─────┐        ┌─────────────┐    ┌─────────────┐    ┌─────┐ │
│   │ 0.5 │   ──►  │ Multiplier  │──► │ Multiplier  │──► │0.09 │ │
│   │ 0.3 │        │ par poids   │    │ par poids   │    │0.15 │ │
│   │ 0.8 │        │ + biais     │    │ + biais     │    │0.12 │ │
│   │ ... │        │ + ReLU      │    │ + ReLU      │    │ ... │ │
│   └─────┘        └─────────────┘    └─────────────┘    └─────┘ │
│                                                                 │
│   ═══════════════════════════════════════════════════════════  │
│                                                                 │
│   L'information circule de GAUCHE à DROITE (forward)           │
│                                                                 │
│   À chaque couche :                                            │
│   1. Somme pondérée : z = W · entrée + b                       │
│   2. Activation : h = f(z)  (ReLU ou autre)                    │
│   3. Passer h à la couche suivante                             │
│                                                                 │
│   Le réseau fait une "DEVINETTE" (ŷ = y-hat = prédiction)      │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Question :** Après le Forward Pass, nous avons une prédiction (ŷ). Nous connaissons aussi la vraie réponse (y). Que pouvons-nous faire avec ces deux informations ?

*(Réponse attendue : Nous pouvons COMPARER la prédiction à la réalité ! Si ŷ = 0.09 pour la classe "3" mais que y = 1 (c'est vraiment un "3"), alors le réseau se trompe. On peut mesurer à quel point il se trompe.)*

---

┌─────────────────────────────────────────────────────────────────┐
│ 📖 DÉFINITION : Forward Pass (Propagation Avant)                │
│                                                                 │
│ Le Forward Pass est le processus par lequel les données        │
│ d'entrée traversent le réseau couche par couche pour           │
│ produire une prédiction.                                        │
│                                                                 │
│ À chaque couche :                                               │
│ 1. Calcul de la somme pondérée : z = W·x + b                   │
│ 2. Application de la fonction d'activation : h = f(z)          │
│ 3. Transmission à la couche suivante                           │
│                                                                 │
│ Le résultat final (après la couche de sortie) est la           │
│ prédiction du réseau, notée ŷ (y-hat).                         │
│                                                                 │
│ Pendant l'entraînement, le Forward Pass est suivi du calcul    │
│ de la loss et du Backward Pass (backpropagation).              │
└─────────────────────────────────────────────────────────────────┘

---

## 4.2 La Fonction de Perte (Loss Function)

### "À quel point on se trompe ?"

La **Loss** (perte) est un nombre qui mesure l'écart entre la prédiction et la réalité.

```
┌─────────────────────────────────────────────────────────────────┐
│                  LA FONCTION DE PERTE                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   PRÉDICTION (ŷ)          RÉALITÉ (y)           LOSS (L)       │
│                                                                 │
│   P(3) = 0.09      vs     Vrai label = 3       L = ???         │
│   (9% de chance)          (100% certain)                       │
│                                                                 │
│   ═══════════════════════════════════════════════════════════  │
│                                                                 │
│   OBJECTIF DU JEU :                                            │
│                                                                 │
│   ┌─────────────────────────────────────────────────────────┐  │
│   │                                                         │  │
│   │   MINIMISER LA LOSS                                     │  │
│   │                                                         │  │
│   │   Loss haute = Prédictions mauvaises 😞                │  │
│   │   Loss basse = Prédictions bonnes 😊                   │  │
│   │   Loss = 0   = Prédictions parfaites ⭐                │  │
│   │                                                         │  │
│   └─────────────────────────────────────────────────────────┘  │
│                                                                 │
│   L'entraînement = trouver les poids qui minimisent la Loss    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

### Les deux Loss les plus courantes

```
┌─────────────────────────────────────────────────────────────────┐
│         LOSS POUR RÉGRESSION vs CLASSIFICATION                  │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   1. MSE — Mean Squared Error (Régression)                     │
│   ════════════════════════════════════════                     │
│                                                                 │
│   Formule : L = (1/n) × Σ(ŷᵢ - yᵢ)²                           │
│                                                                 │
│   Intuition : Moyenne des erreurs au carré                     │
│                                                                 │
│   Exemple :                                                     │
│   • Prédiction : 245,000 €                                     │
│   • Réalité : 250,000 €                                        │
│   • Erreur : 5,000 €                                           │
│   • Loss : (5,000)² = 25,000,000                               │
│                                                                 │
│   ⚠️ Le carré pénalise fortement les grosses erreurs           │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   2. CROSS-ENTROPY — (Classification)                          │
│   ═══════════════════════════════════                          │
│                                                                 │
│   Formule : L = -Σ yᵢ × log(ŷᵢ)                               │
│                                                                 │
│   Intuition : Pénalise quand on est "confiant mais faux"       │
│                                                                 │
│   Exemple (classification du "3") :                            │
│   • Prédiction : P(3) = 0.09 (9%)                             │
│   • Réalité : C'est un 3 (y₃ = 1)                             │
│   • Loss : -1 × log(0.09) = 2.41 (élevé !)                    │
│                                                                 │
│   Si le réseau avait prédit P(3) = 0.99 :                     │
│   • Loss : -1 × log(0.99) = 0.01 (très bas !)                 │
│                                                                 │
│   ⚠️ Plus on est confiant ET faux, plus la loss explose        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

┌─────────────────────────────────────────────────────────────────┐
│ 📖 DÉFINITION : Fonction de Perte (Loss Function)               │
│                                                                 │
│ La fonction de perte (ou fonction de coût) est une fonction    │
│ mathématique qui mesure l'écart entre les prédictions du       │
│ modèle (ŷ) et les valeurs réelles (y).                         │
│                                                                 │
│ Propriétés :                                                    │
│ • Toujours positive (ou nulle si prédiction parfaite)          │
│ • Plus elle est basse, meilleur est le modèle                  │
│ • Doit être différentiable (pour le gradient descent)          │
│                                                                 │
│ Loss courantes :                                                │
│ • Régression : MSE (Mean Squared Error), MAE                   │
│ • Classification binaire : Binary Cross-Entropy                │
│ • Classification multi-classe : Categorical Cross-Entropy      │
│                                                                 │
│ L'objectif de l'entraînement est de trouver les paramètres     │
│ (poids et biais) qui MINIMISENT la loss sur les données        │
│ d'entraînement, tout en généralisant aux nouvelles données.    │
└─────────────────────────────────────────────────────────────────┘

---

## 4.3 Gradient Descent — Descendre la Montagne

### L'intuition fondamentale

Nous voulons minimiser la Loss. Mais comment trouver les poids optimaux parmi des millions de possibilités ?

**L'analogie de la montagne :**

```
┌─────────────────────────────────────────────────────────────────┐
│            GRADIENT DESCENT : L'ANALOGIE DE LA MONTAGNE         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Imaginez que vous êtes au sommet d'une montagne,             │
│   les yeux bandés, et vous devez descendre au point le plus bas│
│                                                                 │
│                          VOUS ÊTES ICI                          │
│                              ↓                                  │
│                             /\                                  │
│                            /  \                                 │
│                           /    \                                │
│                          /      \         /\                    │
│                         /        \       /  \                   │
│                        /          \     /    \                  │
│                       /            \   /      \                 │
│                      /              \_/        \                │
│                     /                           \               │
│   ──────────────────────────────────────────────────────────   │
│                              ↑                                  │
│                        OBJECTIF (Loss minimale)                 │
│                                                                 │
│   STRATÉGIE : À chaque pas...                                  │
│   1. Tâtez le sol autour de vous (calculer le gradient)        │
│   2. Identifiez la direction qui descend le plus               │
│   3. Faites un pas dans cette direction                        │
│   4. Répétez jusqu'à être au fond                              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Question :** Dans cette analogie, qu'est-ce qui correspond aux "poids" du réseau ?

*(Réponse attendue : Votre POSITION sur la montagne ! Chaque position possible correspond à un ensemble de valeurs pour les poids. Bouger = changer les poids. Le fond de la vallée = les poids optimaux qui minimisent la loss.)*

---

### Le gradient : la direction de la pente

```
┌─────────────────────────────────────────────────────────────────┐
│              LE GRADIENT INDIQUE LA DIRECTION                   │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   GRADIENT = "Dans quelle direction la loss AUGMENTE le plus?" │
│                                                                 │
│                    Loss                                         │
│                     ↑                                           │
│                     │      /                                    │
│                     │     / ← Gradient pointe vers le HAUT     │
│                     │    /                                      │
│                     │   ●  ← Position actuelle                 │
│                     │  /                                        │
│                     │ /                                         │
│                     └──────────────────► Poids                  │
│                                                                 │
│   ═══════════════════════════════════════════════════════════  │
│                                                                 │
│   POUR DESCENDRE : Aller dans la direction OPPOSÉE au gradient │
│                                                                 │
│   Nouveau_poids = Ancien_poids - (learning_rate × gradient)    │
│                                                                 │
│   Le "-" est crucial : on va dans le sens CONTRAIRE            │
│   du gradient pour DESCENDRE la loss                           │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

### Le Learning Rate : la taille des pas

```
┌─────────────────────────────────────────────────────────────────┐
│              LEARNING RATE : TAILLE DES PAS                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   LEARNING RATE TROP PETIT                                     │
│   ════════════════════════                                     │
│                                                                 │
│         ●..●..●..●..●..●..●..●..●.._                           │
│        /                             \                          │
│       /                               ● Très lent !            │
│                                                                 │
│   • Convergence très lente (des heures au lieu de minutes)    │
│   • Peut rester bloqué dans un minimum local                  │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   LEARNING RATE TROP GRAND                                     │
│   ════════════════════════                                     │
│                                                                 │
│         ●                              ●                        │
│        / \                            / \                       │
│       /   ●                          ●   \                      │
│      /     \                        /     \                     │
│             ●────────────────────●                             │
│                   Oscille !                                     │
│                                                                 │
│   • Saute par-dessus le minimum                               │
│   • Peut diverger (loss qui explose)                          │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   LEARNING RATE OPTIMAL                                        │
│   ═════════════════════                                        │
│                                                                 │
│         ●                                                       │
│        / \                                                      │
│       /   ●                                                     │
│      /     \                                                    │
│     /       ●                                                   │
│              \                                                  │
│               ●                                                 │
│                ●  ← Convergence rapide et stable               │
│                                                                 │
│   Valeurs typiques : 0.001, 0.01, 0.1                          │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

<details>
<summary>🤔 Question Socratique : Pourquoi ne pas simplement calculer directement la position du minimum (les poids optimaux) ?</summary>

### 🔑 Réponse

C'est une excellente question ! Pour certains problèmes simples (régression linéaire), on PEUT effectivement calculer la solution exacte.

Mais pour les réseaux de neurones, c'est impossible pour plusieurs raisons :

1. **Trop de paramètres** : Un réseau peut avoir des millions de poids. Résoudre directement un système à millions de variables est computationnellement infaisable.

2. **Non-linéarité** : Les fonctions d'activation (ReLU, sigmoid) rendent le problème non-linéaire. Il n'y a pas de formule fermée.

3. **Surface complexe** : La "montagne" de la loss n'est pas une simple cuvette. Elle a des bosses, des plateaux, des vallées locales. Il n'y a pas UN seul minimum.

4. **Stochasticité** : On travaille avec des mini-batches (échantillons), donc le "terrain" change légèrement à chaque étape.

Le Gradient Descent est une méthode **itérative** : on fait des petits pas répétés plutôt qu'un grand saut. C'est moins "élégant" mais ça fonctionne même pour des problèmes immenses.

</details>

---

┌─────────────────────────────────────────────────────────────────┐
│ 📖 DÉFINITION : Gradient Descent (Descente de Gradient)         │
│                                                                 │
│ Le Gradient Descent est un algorithme d'optimisation itératif  │
│ qui ajuste les paramètres (poids et biais) dans la direction   │
│ opposée au gradient de la fonction de perte.                    │
│                                                                 │
│ Formule de mise à jour :                                        │
│ θ_nouveau = θ_ancien - η × ∇L(θ)                               │
│                                                                 │
│ Où :                                                            │
│ • θ = paramètres (poids et biais)                              │
│ • η (eta) = learning rate (taux d'apprentissage)               │
│ • ∇L = gradient de la loss par rapport aux paramètres          │
│                                                                 │
│ Variantes :                                                     │
│ • Batch GD : Calcule le gradient sur TOUTES les données       │
│ • Stochastic GD (SGD) : Un seul exemple à la fois             │
│ • Mini-batch GD : Un petit groupe d'exemples (le plus utilisé)│
│                                                                 │
│ Le gradient pointe vers la montée ; le "-" nous fait descendre.│
└─────────────────────────────────────────────────────────────────┘

---

┌─────────────────────────────────────────────────────────────────┐
│ 📖 DÉFINITION : Learning Rate (Taux d'Apprentissage)            │
│                                                                 │
│ Le learning rate (η) est un hyperparamètre qui contrôle        │
│ la taille des pas lors du Gradient Descent.                     │
│                                                                 │
│ Effets :                                                        │
│ • Trop petit → Apprentissage très lent, peut rester bloqué    │
│ • Trop grand → Instabilité, oscillations, peut diverger       │
│ • Optimal → Convergence rapide et stable                      │
│                                                                 │
│ Valeurs typiques : 0.001 à 0.1                                 │
│                                                                 │
│ Techniques avancées :                                           │
│ • Learning rate schedules : réduire η au fil du temps         │
│ • Adaptive methods : Adam, RMSprop ajustent η automatiquement │
│                                                                 │
│ C'est souvent l'hyperparamètre le plus important à régler.     │
└─────────────────────────────────────────────────────────────────┘

---

## 4.4 Backpropagation — "Qui est responsable ?"

### Le défi : des millions de poids

Après le Forward Pass, on a une Loss. On sait qu'il faut ajuster les poids pour réduire cette Loss.

**Mais comment savoir QUEL poids ajuster et de COMBIEN ?**

```
┌─────────────────────────────────────────────────────────────────┐
│              LE PROBLÈME DE L'ATTRIBUTION                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Entrée ──► [C1: 1000 poids] ──► [C2: 5000 poids] ──► Sortie  │
│                                                        │       │
│                                                        ▼       │
│                                                   Loss = 2.4   │
│                                                                 │
│   QUESTION : Parmi ces 6000 poids...                           │
│   • Lesquels sont responsables de l'erreur ?                   │
│   • Faut-il augmenter ou diminuer chacun ?                     │
│   • De combien ?                                               │
│                                                                 │
│   C'est comme demander : "Dans une équipe de 6000 personnes,   │
│   qui a causé l'échec du projet ?"                             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

### La solution : propager l'erreur en arrière

```
┌─────────────────────────────────────────────────────────────────┐
│              BACKPROPAGATION : LE CONCEPT                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   FORWARD PASS (→)                                             │
│   ════════════════                                             │
│   Entrée → Couche 1 → Couche 2 → Sortie → Loss                 │
│                                                                 │
│   BACKWARD PASS (←)    "Backpropagation"                       │
│   ═════════════════                                            │
│   Entrée ← Couche 1 ← Couche 2 ← Sortie ← Loss                 │
│      ↑         ↑          ↑         ↑                          │
│      │         │          │         │                          │
│   "Quelle    "Quelle    "Quelle   "Quelle                      │
│    part de   part de    part de    part de                     │
│    l'erreur  l'erreur   l'erreur   l'erreur                    │
│    est la    est la     est la     vient de                    │
│    mienne?"  mienne?"   mienne?"   moi?"                       │
│                                                                 │
│   ═══════════════════════════════════════════════════════════  │
│                                                                 │
│   PRINCIPE :                                                   │
│                                                                 │
│   1. On commence par la sortie (où on connaît l'erreur)        │
│   2. On "remonte" l'erreur couche par couche                   │
│   3. Chaque poids reçoit sa "part de responsabilité"          │
│   4. Cette part = le GRADIENT de la loss par rapport au poids  │
│                                                                 │
│   C'est la règle de la chaîne (chain rule) du calcul différentiel│
│   appliquée automatiquement.                                   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Question :** Pourquoi faut-il aller "en arrière" (de la sortie vers l'entrée) plutôt que "en avant" ?

*(Réponse attendue : Parce que l'erreur est connue à la SORTIE (c'est là qu'on compare à y). Pour savoir quelle part de cette erreur revient à chaque poids, on doit "remonter la chaîne" des calculs.)*

---

### Illustration simplifiée

```
┌─────────────────────────────────────────────────────────────────┐
│          BACKPROPAGATION : EXEMPLE SIMPLIFIÉ                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Un réseau très simple : x → [w₁] → h → [w₂] → ŷ             │
│                                                                 │
│   FORWARD PASS :                                               │
│   x = 2                                                        │
│   h = w₁ × x = 0.5 × 2 = 1                                    │
│   ŷ = w₂ × h = 0.3 × 1 = 0.3                                  │
│   y = 1 (vraie valeur)                                        │
│   Loss = (ŷ - y)² = (0.3 - 1)² = 0.49                         │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   BACKWARD PASS (intuition sans formules) :                    │
│                                                                 │
│   "Qui est responsable de Loss = 0.49 ?"                       │
│                                                                 │
│   → ŷ était trop bas (0.3 au lieu de 1)                       │
│   → Pour augmenter ŷ, on pourrait augmenter w₂                │
│      (car ŷ = w₂ × h, et h > 0)                               │
│   → Mais h dépend de w₁... donc w₁ aussi est responsable      │
│                                                                 │
│   Le backprop calcule PRÉCISÉMENT :                            │
│   • ∂Loss/∂w₂ = combien la loss change si on modifie w₂       │
│   • ∂Loss/∂w₁ = combien la loss change si on modifie w₁       │
│                                                                 │
│   Puis on met à jour :                                         │
│   w₂_nouveau = w₂ - learning_rate × (∂Loss/∂w₂)               │
│   w₁_nouveau = w₁ - learning_rate × (∂Loss/∂w₁)               │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

┌─────────────────────────────────────────────────────────────────┐
│ 📖 DÉFINITION : Backpropagation (Rétropropagation)              │
│                                                                 │
│ La rétropropagation est l'algorithme qui calcule le gradient   │
│ de la fonction de perte par rapport à chaque poids du réseau,  │
│ en propageant l'erreur de la sortie vers l'entrée.             │
│                                                                 │
│ Étapes :                                                        │
│ 1. Forward Pass : calculer la prédiction ŷ                     │
│ 2. Calculer la Loss : L(ŷ, y)                                  │
│ 3. Backward Pass : pour chaque poids, calculer ∂L/∂w           │
│ 4. Mise à jour : w = w - η × ∂L/∂w                             │
│                                                                 │
│ Mathématiquement, c'est une application efficace de la         │
│ règle de la chaîne (chain rule) du calcul différentiel.        │
│                                                                 │
│ Note historique : Inventée par Paul Werbos (1974), popularisée │
│ par Rumelhart, Hinton & Williams (1986), c'est l'algorithme    │
│ qui a permis l'essor des réseaux de neurones.                   │
│                                                                 │
│ Les frameworks modernes (PyTorch, TensorFlow) calculent        │
│ le backprop automatiquement ("automatic differentiation").      │
└─────────────────────────────────────────────────────────────────┘

---

### Le cycle complet d'apprentissage

```
┌─────────────────────────────────────────────────────────────────┐
│         LE CYCLE D'ENTRAÎNEMENT COMPLET                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│                    ┌──────────────────┐                        │
│                    │  1. FORWARD PASS │                        │
│                    │  Données → ŷ     │                        │
│                    └────────┬─────────┘                        │
│                             │                                   │
│                             ▼                                   │
│                    ┌──────────────────┐                        │
│                    │  2. CALCUL LOSS  │                        │
│                    │  L(ŷ, y)         │                        │
│                    └────────┬─────────┘                        │
│                             │                                   │
│                             ▼                                   │
│                    ┌──────────────────┐                        │
│                    │  3. BACKWARD PASS│                        │
│                    │  Calcul gradients│                        │
│                    └────────┬─────────┘                        │
│                             │                                   │
│                             ▼                                   │
│                    ┌──────────────────┐                        │
│                    │  4. MISE À JOUR  │                        │
│                    │  w = w - η×grad  │                        │
│                    └────────┬─────────┘                        │
│                             │                                   │
│                             │                                   │
│                    ┌────────▼─────────┐                        │
│                    │    RÉPÉTER       │                        │
│                    │  jusqu'à ce que  │                        │
│                    │  Loss soit basse │                        │
│                    └──────────────────┘                        │
│                                                                 │
│   Ce cycle s'exécute des milliers ou millions de fois !        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 4.5 Epochs et Batches — Organiser l'Entraînement

### Le problème de la mémoire

Si vous avez 60,000 images (comme MNIST), vous ne pouvez pas toutes les charger en mémoire GPU pour calculer le gradient d'un coup.

**Solution : les mini-batches**

```
┌─────────────────────────────────────────────────────────────────┐
│              BATCH, EPOCH, ITERATION                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   VOS DONNÉES : 60,000 images                                  │
│                                                                 │
│   ┌─────────────────────────────────────────────────────────┐  │
│   │ ■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■■│  │
│   └─────────────────────────────────────────────────────────┘  │
│                                                                 │
│   BATCH SIZE = 32 (exemple)                                    │
│   ════════════════════════                                     │
│                                                                 │
│   ┌────┐ ┌────┐ ┌────┐ ┌────┐        ┌────┐                   │
│   │ 32 │ │ 32 │ │ 32 │ │ 32 │  ...   │ 32 │  = 1875 batches   │
│   └────┘ └────┘ └────┘ └────┘        └────┘                   │
│     │      │      │      │             │                       │
│     ▼      ▼      ▼      ▼             ▼                       │
│   1 update 1 update 1 update         1 update                  │
│   des poids des poids                 des poids                │
│                                                                 │
│   ═══════════════════════════════════════════════════════════  │
│                                                                 │
│   1 EPOCH = Passage complet sur TOUTES les données             │
│   ════════════════════════════════════════════════             │
│                                                                 │
│   60,000 images ÷ 32 batch_size = 1875 iterations par epoch   │
│                                                                 │
│   Entraînement typique : 10-100 epochs                        │
│   = 18,750 à 187,500 mises à jour des poids !                 │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

### Pourquoi les mini-batches ?

```
┌─────────────────────────────────────────────────────────────────┐
│         AVANTAGES DES MINI-BATCHES                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   1. MÉMOIRE                                                   │
│   • On ne charge que 32 images à la fois (pas 60,000)          │
│   • Permet d'utiliser des GPUs avec mémoire limitée            │
│                                                                 │
│   2. VITESSE                                                   │
│   • Mises à jour fréquentes (1875 fois par epoch, pas 1)       │
│   • Convergence plus rapide en temps réel                      │
│                                                                 │
│   3. RÉGULARISATION                                            │
│   • Le gradient "bruité" (différent à chaque batch)            │
│   • Aide à éviter les minima locaux                            │
│   • Effet régularisant (comme le dropout)                      │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   TAILLES TYPIQUES DE BATCH :                                  │
│                                                                 │
│   • 32-64 : Standard pour la plupart des tâches               │
│   • 128-256 : Pour gros datasets, GPUs puissants              │
│   • 8-16 : Pour très gros modèles (transformers)              │
│                                                                 │
│   Puissances de 2 (32, 64, 128...) pour efficacité GPU        │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

┌─────────────────────────────────────────────────────────────────┐
│ 📖 DÉFINITIONS : Epoch, Batch, Iteration                        │
│                                                                 │
│ BATCH (ou Mini-Batch) :                                        │
│ Un sous-ensemble des données d'entraînement utilisé pour       │
│ calculer le gradient et mettre à jour les poids une fois.      │
│ Taille typique : 32, 64, 128, 256                              │
│                                                                 │
│ ITERATION (ou Step) :                                          │
│ Une mise à jour des poids du réseau, correspondant au          │
│ traitement d'un batch. Nombre d'iterations par epoch =         │
│ taille_dataset / batch_size.                                    │
│                                                                 │
│ EPOCH :                                                         │
│ Un passage complet sur l'ensemble des données d'entraînement.  │
│ Après 1 epoch, chaque exemple a été vu exactement une fois.    │
│ L'entraînement typique dure 10 à 100+ epochs.                  │
│                                                                 │
│ Relation :                                                      │
│ iterations_totales = epochs × (dataset_size / batch_size)      │
│                                                                 │
│ Exemple : 60,000 images, batch=32, 10 epochs                   │
│ = 10 × 1875 = 18,750 mises à jour des poids                    │
└─────────────────────────────────────────────────────────────────┘

---

<details>
<summary>🤔 Question Socratique : Après un entraînement de 50 epochs, est-ce que la loss baisse toujours ? À quoi faut-il faire attention ?</summary>

### 🔑 Réponse

Pas nécessairement ! Plusieurs scénarios sont possibles :

1. **Convergence saine** : La loss (train ET validation) baisse puis se stabilise. Le modèle a appris ce qu'il peut apprendre.

2. **Overfitting** : La loss d'entraînement continue de baisser, mais la loss de validation remonte. Le modèle mémorise au lieu de généraliser.

3. **Underfitting** : Les deux loss restent élevées. Le modèle n'est pas assez complexe ou le learning rate est mal réglé.

4. **Instabilité** : La loss oscille. Learning rate trop grand.

**À surveiller :**
- Toujours observer la loss de VALIDATION, pas seulement de train
- Utiliser "early stopping" : arrêter quand validation loss remonte
- Garder le modèle qui avait la meilleure validation loss

Rappelez-vous le compromis biais-variance du Chapitre 3 — il s'applique aussi aux réseaux de neurones !

</details>

---

## 🧭 Récapitulatif et Transition

### Le cycle complet en un schéma

```
┌─────────────────────────────────────────────────────────────────┐
│            RÉCAPITULATIF : CYCLE D'APPRENTISSAGE                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Pour chaque EPOCH (passage sur toutes les données) :         │
│   │                                                             │
│   └── Pour chaque BATCH (sous-ensemble) :                      │
│       │                                                         │
│       ├── 1. FORWARD PASS                                      │
│       │      Données → Prédiction (ŷ)                          │
│       │                                                         │
│       ├── 2. LOSS                                              │
│       │      Comparer ŷ à y (réalité)                          │
│       │                                                         │
│       ├── 3. BACKWARD PASS (Backpropagation)                   │
│       │      Calculer ∂Loss/∂w pour chaque poids              │
│       │                                                         │
│       └── 4. UPDATE (Gradient Descent)                         │
│              w = w - learning_rate × gradient                  │
│                                                                 │
│   Répéter 10-100 epochs jusqu'à convergence                    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Termes clés introduits

| Terme | Définition |
|-------|------------|
| **Forward Pass** | Calcul de la prédiction à partir des entrées |
| **Loss Function** | Mesure l'écart entre prédiction et réalité |
| **MSE** | Mean Squared Error (régression) |
| **Cross-Entropy** | Pour classification |
| **Gradient Descent** | Optimisation par petits pas |
| **Learning Rate** | Taille des pas (hyperparamètre crucial) |
| **Backpropagation** | Calcul des gradients couche par couche |
| **Epoch** | Un passage complet sur les données |
| **Batch** | Sous-ensemble pour une mise à jour |
| **Iteration** | Une mise à jour des poids |

---

### 🔮 Ce qui vient ensuite

*Nous avons découvert comment un réseau apprend : Forward → Loss → Backward → Update, répété des milliers de fois.*

*Mais ce processus n'est pas sans difficultés. Quand on empile beaucoup de couches (Deep Learning), de nouveaux problèmes apparaissent : vanishing gradients, instabilité, overfitting.*

*Dans la partie suivante, nous découvrirons ces défis et les solutions modernes qui ont permis l'essor du Deep Learning : ReLU (que nous connaissons déjà), Batch Normalization, et Dropout.*

---

## 📝 Réflexion Métacognitive

1. **L'analogie de la montagne** vous a-t-elle aidé à comprendre le Gradient Descent ? Pouvez-vous expliquer à quelqu'un d'autre ce qu'est le learning rate ?

2. **Le backpropagation** reste-t-il un peu "magique" ? C'est normal — l'essentiel est de comprendre l'IDÉE (propager l'erreur en arrière) plutôt que les formules.

3. **Connexion avec le Chapitre 3** : Comment le concept d'overfitting que nous avons vu s'applique-t-il ici ? Que se passe-t-il si on fait trop d'epochs ?

---

*Dans la partie suivante, nous verrons pourquoi le "Deep" dans Deep Learning pose des défis spécifiques, et comment les chercheurs les ont résolus.*
