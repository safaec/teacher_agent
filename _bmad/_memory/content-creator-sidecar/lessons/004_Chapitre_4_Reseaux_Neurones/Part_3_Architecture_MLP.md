# Chapitre 4 : Réseaux de Neurones (Conceptuel)

## Part 3 : L'Architecture Standard — Le MLP (Multi-Layer Perceptron)

**Durée estimée : 2h**

---

### Objectifs d'apprentissage

À la fin de cette partie, vous serez capable de :
1. **Décrire** les trois types de couches d'un réseau feedforward (entrée, cachées, sortie)
2. **Expliquer** la différence entre largeur et profondeur d'un réseau
3. **Définir** ce qu'est un MLP (Multi-Layer Perceptron) et ses caractéristiques

---

### 🎯 Accroche : Le Problème de la Spirale

*Dans la partie précédente, nous avons vu qu'un neurone unique est équivalent à une régression logistique. Mais un seul neurone a une limite fondamentale...*

Imaginons ce problème de classification :

```
┌─────────────────────────────────────────────────────────────────┐
│                   LE PROBLÈME DE LA SPIRALE                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│           ●●●                                                   │
│         ●●   ○○○                                                │
│        ●●     ○○○                                               │
│       ●●  ○○○   ○○                                              │
│      ●●  ○○  ●●  ○                                              │
│     ●●  ○○  ●●  ○○                                              │
│    ●●  ○○  ●●   ○○                                              │
│       ○○  ●●   ○○                                               │
│         ●●●  ○○                                                 │
│              ○○                                                 │
│                                                                 │
│   ● = Classe A (spirale intérieure)                            │
│   ○ = Classe B (spirale extérieure)                            │
│                                                                 │
│   DÉFI : Tracez une ligne droite qui sépare ● et ○             │
│                                                                 │
│   ...Impossible, n'est-ce pas ?                                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Question :** Un seul neurone peut-il résoudre ce problème ? Pourquoi ?

*(Réponse attendue : Non ! Un seul neurone ne peut tracer qu'une frontière linéaire (une ligne droite). La spirale nécessite une frontière courbe et complexe. Il faut plusieurs neurones travaillant ensemble.)*

---

## 3.1 La Couche d'Entrée (Input Layer)

### Le point de départ

La couche d'entrée est simplement l'endroit où vos données entrent dans le réseau.

```
┌─────────────────────────────────────────────────────────────────┐
│                    COUCHE D'ENTRÉE                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   VOS DONNÉES                      COUCHE D'ENTRÉE              │
│   ═══════════                      ════════════════             │
│                                                                 │
│   ┌────────────────────┐           ┌───┐                       │
│   │ Surface : 75 m²    │  ──────►  │ ○ │  x₁ = 75              │
│   │ Chambres : 3       │  ──────►  │ ○ │  x₂ = 3               │
│   │ Étage : 4          │  ──────►  │ ○ │  x₃ = 4               │
│   │ Ascenseur : 1      │  ──────►  │ ○ │  x₄ = 1               │
│   └────────────────────┘           └───┘                       │
│                                                                 │
│   4 features           ──────►     4 neurones d'entrée         │
│                                                                 │
│   ═══════════════════════════════════════════════════════════  │
│                                                                 │
│   RÈGLE SIMPLE :                                               │
│                                                                 │
│   Nombre de neurones d'entrée = Nombre de features             │
│                                                                 │
│   • Image 28×28 pixels → 784 neurones d'entrée                 │
│   • Appartement avec 10 caractéristiques → 10 neurones         │
│   • Texte tokenisé en 512 tokens → 512 neurones                │
│                                                                 │
│   ⚠️ Note : Ces "neurones" ne font aucun calcul !              │
│      Ils passent simplement les valeurs à la couche suivante.  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---
```
┌─────────────────────────────────────────────────────────────────┐
│ 📖 DÉFINITION : Couche d'Entrée (Input Layer)                   │
│                                                                 │
│ La couche d'entrée est la première couche d'un réseau de       │
│ neurones. Elle reçoit les données brutes et les transmet       │
│ à la première couche cachée.                                    │
│                                                                 │
│ Caractéristiques :                                              │
│ • Pas de calcul effectué (juste un passage de données)         │
│ • Pas de poids ni de biais                                     │
│ • Dimension = nombre de features des données d'entrée          │
│                                                                 │
│ Note : Techniquement, la couche d'entrée n'est pas comptée     │
│ quand on parle du nombre de couches d'un réseau.               │
│ Un réseau "à 3 couches" a 1 entrée + 2 cachées + 1 sortie.     │
└─────────────────────────────────────────────────────────────────┘
```
---

## 3.2 La Couche Cachée (Hidden Layer) — La Largeur

### Pourquoi "cachée" ?

On l'appelle "cachée" car elle n'est ni visible à l'entrée (vos données), ni à la sortie (la prédiction). Elle fait le travail "en coulisses".

### Le pouvoir de la largeur

Un seul neurone = une seule ligne de séparation. Mais si on met **plusieurs neurones en parallèle** ?

```
┌─────────────────────────────────────────────────────────────────┐
│            UN NEURONE vs PLUSIEURS NEURONES                     │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   UN SEUL NEURONE                 QUATRE NEURONES EN PARALLÈLE │
│   ═══════════════                 ════════════════════════════ │
│                                                                 │
│        ●  ●  ○  ○                      ●  ●  ○  ○              │
│       ● ● ● ○ ○ ○                     ● ● │\ ○ ○ ○             │
│      ● ● ●───○ ○ ○                   ● ●──┼─\○ ○ ○             │
│     ● ● ●/   ○ ○ ○                  ● ● ● │  \○ ○ ○            │
│    ● ● ●     ○ ○ ○                 ● ● ● ●│   ○ ○ ○            │
│                                                                 │
│   Une seule ligne                  Quatre lignes               │
│   = Frontière linéaire             = Frontière polygonale      │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   Chaque neurone apprend à détecter UN pattern :               │
│                                                                 │
│   Neurone 1 : "Y a-t-il un bord en haut ?"                     │
│   Neurone 2 : "Y a-t-il un bord à droite ?"                    │
│   Neurone 3 : "Y a-t-il une courbe ici ?"                      │
│   Neurone 4 : "Y a-t-il une diagonale là ?"                    │
│                                                                 │
│   La couche suivante COMBINE ces détections !                  │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Question :** Si chaque neurone peut détecter un pattern simple, que se passe-t-il si on a 100 neurones dans une couche cachée ?

*(Réponse attendue : On peut détecter 100 patterns différents ! Plus il y a de neurones, plus le réseau peut capturer des caractéristiques diverses des données. C'est la "largeur" du réseau.)*

---

### Visualisation d'une couche cachée

```
┌─────────────────────────────────────────────────────────────────┐
│              ANATOMIE D'UNE COUCHE CACHÉE                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   ENTRÉE            COUCHE CACHÉE              VERS LA SUITE   │
│   (3 features)      (4 neurones)                               │
│                                                                 │
│                     ┌─────────────────┐                        │
│        x₁ ─────────►│  h₁ = f(w·x+b)  │────────►               │
│           \        /└─────────────────┘\                       │
│            \      /                      \                     │
│             \    /  ┌─────────────────┐   \                    │
│        x₂ ───\──/───│  h₂ = f(w·x+b)  │────►                   │
│               \/    └─────────────────┘    /                   │
│               /\                          /                    │
│              /  \   ┌─────────────────┐  /                     │
│        x₃ ─/────\──►│  h₃ = f(w·x+b)  │─►                      │
│           /      \  └─────────────────┘                        │
│          /        \ ┌─────────────────┐                        │
│         /          ►│  h₄ = f(w·x+b)  │────────►               │
│                     └─────────────────┘                        │
│                                                                 │
│   ═══════════════════════════════════════════════════════════  │
│                                                                 │
│   CHAQUE NEURONE :                                             │
│   • Reçoit TOUTES les entrées (x₁, x₂, x₃)                    │
│   • A ses PROPRES poids (w₁, w₂, w₃) et biais (b)             │
│   • Calcule sa PROPRE somme pondérée                          │
│   • Applique sa fonction d'activation (ReLU)                  │
│   • Produit UNE sortie (h)                                    │
│                                                                 │
│   "LARGEUR" de cette couche = 4 neurones                       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---
```
┌─────────────────────────────────────────────────────────────────┐
│ 📖 DÉFINITION : Couche Cachée (Hidden Layer)                    │
│                                                                 │
│ Une couche cachée est une couche de neurones située entre      │
│ l'entrée et la sortie du réseau. Elle effectue des             │
│ transformations intermédiaires sur les données.                 │
│                                                                 │
│ Caractéristiques :                                              │
│ • Chaque neurone reçoit toutes les sorties de la couche        │
│   précédente (connexion "fully connected")                     │
│ • Chaque neurone a ses propres poids et biais (paramètres)     │
│ • Applique une fonction d'activation (généralement ReLU)       │
│                                                                 │
│ La "largeur" d'une couche = nombre de neurones dans la couche. │
│ Plus la couche est large, plus elle peut capturer de patterns  │
│ différents simultanément.                                       │
│                                                                 │
│ Nomenclature typique :                                          │
│ • Couche cachée de 64 neurones → [64]                          │
│ • Couche cachée de 256 neurones → [256]                        │
└─────────────────────────────────────────────────────────────────┘
```
---

## 3.3 Du Shallow au Deep — La Profondeur

### Le problème de la couche unique

Même avec beaucoup de neurones dans UNE couche, il y a des limites :

```
┌─────────────────────────────────────────────────────────────────┐
│         UNE SEULE COUCHE CACHÉE (SHALLOW NETWORK)               │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Entrée ────► [Couche Cachée : 100 neurones] ────► Sortie     │
│                                                                 │
│   Ce que chaque neurone peut détecter :                        │
│   • Patterns SIMPLES uniquement                                │
│   • Combinaisons LINÉAIRES des entrées (+ activation)          │
│                                                                 │
│   LIMITE : Pour reconnaître un visage, il faudrait un neurone  │
│   qui détecte DIRECTEMENT "visage" depuis les pixels.          │
│   C'est très difficile !                                       │
│                                                                 │
│   PIXELS ────────────────────────────────────────► VISAGE      │
│           (un seul saut = très complexe)                       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### La solution : empiler les couches

```
┌─────────────────────────────────────────────────────────────────┐
│         PLUSIEURS COUCHES (DEEP NETWORK)                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   Entrée ──► [C1] ──► [C2] ──► [C3] ──► [C4] ──► Sortie       │
│                │        │        │        │                     │
│                │        │        │        └─ Concepts complets │
│                │        │        └─ Parties d'objets           │
│                │        └─ Formes simples                       │
│                └─ Bords et textures                             │
│                                                                 │
│   PIXELS → BORDS → FORMES → PARTIES → OBJETS                   │
│           (plusieurs petits sauts = plus facile !)             │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Question :** Pourquoi est-il plus facile de faire plusieurs "petits sauts" plutôt qu'un seul "grand saut" ?

*(Réponse attendue : Chaque couche peut se spécialiser dans un niveau d'abstraction. La couche 1 n'a qu'à détecter des bords (facile), la couche 2 combine ces bords en formes (facile car elle a les bords), etc. Diviser le problème en étapes simples est toujours plus facile que de tout faire d'un coup.)*

---

### Shallow vs Deep : comparaison

```
┌─────────────────────────────────────────────────────────────────┐
│              SHALLOW vs DEEP NETWORKS                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   SHALLOW NETWORK                                              │
│   ═══════════════                                              │
│   • 1 couche cachée                                            │
│   • Peut approximer n'importe quelle fonction...               │
│   • ...mais peut nécessiter ÉNORMÉMENT de neurones             │
│   • Pas de hiérarchie de features                              │
│                                                                 │
│   Entrée ──► [████████████████████] ──► Sortie                 │
│              (milliers de neurones)                            │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   DEEP NETWORK                                                 │
│   ════════════                                                 │
│   • 2+ couches cachées                                         │
│   • Apprend des représentations HIÉRARCHIQUES                  │
│   • Plus efficace en paramètres                                │
│   • Chaque couche = un niveau d'abstraction                    │
│                                                                 │
│   Entrée ──► [████] ──► [████] ──► [████] ──► Sortie          │
│              (moins de neurones au total)                      │
│                                                                 │
│   ═══════════════════════════════════════════════════════════  │
│                                                                 │
│   CONVENTION :                                                 │
│   • 1 couche cachée     = "Shallow" (peu profond)             │
│   • 2-5 couches cachées = "Deep" (profond)                    │
│   • 10+ couches         = "Very Deep" (très profond)          │
│   • 100+ couches        = Architectures spéciales (ResNet)    │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

<details>
<summary>🤔 Question Socratique : Théoriquement, une seule couche cachée très large peut approximer n'importe quelle fonction (théorème d'approximation universelle). Alors pourquoi utiliser plusieurs couches ?</summary>

### 🔑 Réponse

C'est une question fondamentale ! Le **Théorème d'Approximation Universelle** dit effectivement qu'un réseau avec une seule couche cachée (assez large) peut approximer n'importe quelle fonction continue.

Mais en pratique, les réseaux profonds sont préférés pour plusieurs raisons :

1. **Efficacité des paramètres** : Un réseau profond peut représenter la même fonction avec BEAUCOUP moins de paramètres. Exemple : pour certaines fonctions, un réseau shallow aurait besoin de 2ⁿ neurones, alors qu'un réseau profond n'en a besoin que de O(n).

2. **Hiérarchie naturelle** : Les problèmes du monde réel (images, texte) ont une structure hiérarchique naturelle. Un réseau profond la capture naturellement.

3. **Généralisation** : Les réseaux profonds généralisent souvent mieux car ils apprennent des features réutilisables à chaque niveau.

4. **Optimisation** : Paradoxalement, les réseaux très larges et peu profonds sont plus difficiles à optimiser en pratique.

Le théorème dit ce qui est **possible**, pas ce qui est **pratique**.

</details>

---
```
┌─────────────────────────────────────────────────────────────────┐
│ 📖 DÉFINITION : Profondeur d'un Réseau                          │
│                                                                 │
│ La profondeur d'un réseau de neurones correspond au nombre     │
│ de couches successives entre l'entrée et la sortie.            │
│                                                                 │
│ Conventions de comptage (varient selon les sources) :          │
│ • "Réseau à 3 couches" peut signifier :                        │
│   - 3 couches de poids (entrée + 2 cachées + sortie)          │
│   - OU 3 couches cachées + entrée + sortie                    │
│                                                                 │
│ L'important est que la profondeur permet d'apprendre des       │
│ représentations hiérarchiques, où chaque couche construit      │
│ sur les abstractions de la précédente.                         │
│                                                                 │
│ Plus un réseau est profond, plus il peut capturer des          │
│ patterns complexes, mais plus il est difficile à entraîner     │
│ (problèmes de vanishing gradients, coût computationnel).       │
└─────────────────────────────────────────────────────────────────┘
```
---

### Ce que chaque couche "voit" (exemple visuel)

```
┌─────────────────────────────────────────────────────────────────┐
│          HIÉRARCHIE DES REPRÉSENTATIONS APPRISES                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   COUCHE 1 : Détecteurs de bords                               │
│   ══════════════════════════════                               │
│                                                                 │
│   │  ─  /  \  ●  ○  ╱  ╲                                       │
│                                                                 │
│   Chaque neurone s'active pour un type de pattern local        │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   COUCHE 2 : Combinaisons de bords → Textures/Formes simples   │
│   ═══════════════════════════════════════════════════════════  │
│                                                                 │
│   ┌─┐  ╱╲  ◠  ◡  ∩  ⊂  ▭  △                                   │
│   └─┘  ╲╱                                                      │
│                                                                 │
│   Combine les bords de la couche 1 en formes reconnaissables   │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   COUCHE 3 : Parties d'objets                                  │
│   ══════════════════════════                                   │
│                                                                 │
│   👁  👃  👄  🚗(roue)  🏠(fenêtre)  ✋                         │
│                                                                 │
│   Détecte des composants significatifs d'objets                │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   COUCHE 4+ : Objets complets / Concepts                       │
│   ══════════════════════════════════════                       │
│                                                                 │
│   😀 (visage)  🚗 (voiture)  🏠 (maison)  🐱 (chat)            │
│                                                                 │
│   Reconnaît les objets en combinant leurs parties              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

## 3.4 La Couche de Sortie (Output Layer)

### Adapter la sortie au problème

La couche de sortie doit correspondre au type de problème que vous résolvez :

```
┌─────────────────────────────────────────────────────────────────┐
│               COUCHE DE SORTIE SELON LE PROBLÈME                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   RÉGRESSION (prédire un nombre)                               │
│   ══════════════════════════════                               │
│                                                                 │
│   Exemple : Prédire le prix d'un appartement                   │
│                                                                 │
│   [Couches cachées] ───► [ ○ ] ───► 245,000 €                  │
│                          1 neurone                              │
│                          Activation : Aucune (linéaire)         │
│                          ou ReLU si valeur toujours positive   │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   CLASSIFICATION BINAIRE (oui/non)                             │
│   ════════════════════════════════                             │
│                                                                 │
│   Exemple : Spam ou non-spam ?                                 │
│                                                                 │
│   [Couches cachées] ───► [ ○ ] ───► 0.87 = 87% spam            │
│                          1 neurone                              │
│                          Activation : Sigmoid                   │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   CLASSIFICATION MULTI-CLASSE (1 parmi N)                      │
│   ═══════════════════════════════════════                      │
│                                                                 │
│   Exemple : Quel chiffre (0-9) ?                               │
│                                                                 │
│                          ┌─ ○ ─┐  0.02 = 2% (classe 0)         │
│                          │  ○  │  0.01 = 1% (classe 1)         │
│   [Couches cachées] ───► │  ○  │  ...                          │
│                          │  ○  │  0.85 = 85% (classe 3) ← MAX  │
│                          │  ○  │  ...                          │
│                          └─ ○ ─┘  0.02 = 2% (classe 9)         │
│                          10 neurones                            │
│                          Activation : Softmax                   │
│                          Somme = 1.00                           │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

┌─────────────────────────────────────────────────────────────────┐
│ 📖 DÉFINITION : Couche de Sortie (Output Layer)                 │
│                                                                 │
│ La couche de sortie est la dernière couche du réseau. Elle     │
│ produit la prédiction finale dans le format approprié au       │
│ problème.                                                       │
│                                                                 │
│ Configuration selon le type de tâche :                          │
│                                                                 │
│ ┌─────────────────┬────────────┬─────────────────────────┐     │
│ │ Type de tâche   │ Neurones   │ Activation              │     │
│ ├─────────────────┼────────────┼─────────────────────────┤     │
│ │ Régression      │ 1          │ Linéaire (ou ReLU)      │     │
│ │ Binaire         │ 1          │ Sigmoid                 │     │
│ │ Multi-classe    │ N classes  │ Softmax                 │     │
│ │ Multi-label     │ N labels   │ Sigmoid (par neurone)   │     │
│ └─────────────────┴────────────┴─────────────────────────┘     │
│                                                                 │
│ Multi-label = plusieurs catégories possibles simultanément     │
│ (ex: une image peut être "plage" ET "coucher de soleil")       │
└─────────────────────────────────────────────────────────────────┘

---

## 3.5 Le MLP — Multi-Layer Perceptron

### L'architecture complète

Le **MLP** (Multi-Layer Perceptron) est le type de réseau que nous venons de construire :

```
┌─────────────────────────────────────────────────────────────────┐
│            MLP : MULTI-LAYER PERCEPTRON                         │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   ENTRÉE           COUCHES CACHÉES              SORTIE         │
│   ══════           ════════════════             ══════         │
│                                                                 │
│   ┌───┐          ┌───┐      ┌───┐             ┌───┐           │
│   │ ○ │─────────►│ ● │─────►│ ● │────────────►│ ○ │           │
│   │   │\   /────►│   │\────►│   │\   /───────►│   │           │
│   │ ○ │─\─/─────►│ ● │─\───►│ ● │─\─/────────►│ ○ │           │
│   │   │\/\  /──►│   │\/\──►│   │\/\  /──────►│   │           │
│   │ ○ │/\─\/───►│ ● │/─\──►│ ● │/─\/────────►│ ○ │           │
│   │   │  \/\───►│   │  \──►│   │  \/────────►└───┘           │
│   │ ○ │──/──\──►│ ● │───\─►│ ● │                              │
│   └───┘        └───┘      └───┘                               │
│                                                                 │
│   4 features    Couche 1     Couche 2         3 classes        │
│                 (5 neurones)  (4 neurones)     (softmax)       │
│                                                                 │
│   ═══════════════════════════════════════════════════════════  │
│                                                                 │
│   CARACTÉRISTIQUES DU MLP :                                    │
│                                                                 │
│   1. FEEDFORWARD : L'information va de gauche à droite,        │
│      jamais en arrière (pas de boucles)                        │
│                                                                 │
│   2. FULLY CONNECTED (Dense) : Chaque neurone d'une couche    │
│      est connecté à TOUS les neurones de la couche suivante   │
│                                                                 │
│   3. COUCHES DENSES : Autre nom pour "fully connected"         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Question :** Pourquoi appelle-t-on ces couches "Dense" ou "Fully Connected" ?

*(Réponse attendue : Parce que chaque neurone est connecté à TOUS les neurones de la couche précédente. Il n'y a pas de connexion "sautée" ou "manquante". La matrice de poids est "pleine" — d'où "dense".)*

---

### Notation et vocabulaire

```
┌─────────────────────────────────────────────────────────────────┐
│              VOCABULAIRE TECHNIQUE DU MLP                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   TERME                  SIGNIFICATION                         │
│   ═════                  ════════════                          │
│                                                                 │
│   MLP                    Multi-Layer Perceptron                │
│                                                                 │
│   Feedforward            Information va entrée → sortie        │
│                          (pas de retour en arrière)            │
│                                                                 │
│   Fully Connected (FC)   Tous les neurones connectés entre    │
│                          couches adjacentes                    │
│                                                                 │
│   Dense                  Synonyme de Fully Connected           │
│                          (utilisé dans Keras)                  │
│                                                                 │
│   Architecture [4,64,32,3]  4 entrées, 64 neurones (couche 1), │
│                             32 neurones (couche 2), 3 sorties  │
│                                                                 │
│   Paramètres             Poids (w) + Biais (b) à apprendre    │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   EXEMPLE DE COMPTAGE DE PARAMÈTRES :                          │
│                                                                 │
│   Architecture : [4, 64, 32, 3]                                │
│                                                                 │
│   Couche 1 : 4 × 64 = 256 poids + 64 biais = 320 paramètres   │
│   Couche 2 : 64 × 32 = 2048 poids + 32 biais = 2080 paramètres │
│   Sortie   : 32 × 3 = 96 poids + 3 biais = 99 paramètres      │
│                                                                 │
│   TOTAL : 320 + 2080 + 99 = 2,499 paramètres                   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

---

┌─────────────────────────────────────────────────────────────────┐
│ 📖 DÉFINITION : MLP (Multi-Layer Perceptron)                    │
│                                                                 │
│ Le Multi-Layer Perceptron (MLP) est une architecture de        │
│ réseau de neurones composée de :                                │
│                                                                 │
│ • Une couche d'entrée (recevant les features)                  │
│ • Une ou plusieurs couches cachées (fully connected + ReLU)    │
│ • Une couche de sortie (adaptée au type de problème)           │
│                                                                 │
│ Caractéristiques :                                              │
│ • Feedforward : pas de connexions récurrentes                  │
│ • Fully connected (Dense) : toutes les connexions existent    │
│ • Universel : peut approximer n'importe quelle fonction       │
│                                                                 │
│ C'est l'architecture de base, le "couteau suisse" du Deep     │
│ Learning. Elle fonctionne bien pour les données tabulaires     │
│ et sert de "tête" de classification dans des architectures     │
│ plus complexes (CNN, Transformers).                             │
│                                                                 │
│ Synonymes : Feedforward Neural Network, Dense Network          │
└─────────────────────────────────────────────────────────────────┘

---

### Quand utiliser un MLP ?

```
┌─────────────────────────────────────────────────────────────────┐
│              QUAND UTILISER UN MLP ?                            │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   ✅ BON POUR :                                                │
│                                                                 │
│   • Données TABULAIRES (comme vos datasets du Chapitre 3)     │
│     → Prix immobilier, prédiction de churn, scoring crédit    │
│                                                                 │
│   • Quand les RELATIONS entre features ne sont pas spatiales  │
│     ou séquentielles                                           │
│                                                                 │
│   • Comme "TÊTE" de classification après un extracteur de     │
│     features (CNN pour images, BERT pour texte)                │
│                                                                 │
│   • Prototypage rapide avant d'essayer des architectures      │
│     plus complexes                                             │
│                                                                 │
│   ─────────────────────────────────────────────────────────────│
│                                                                 │
│   ❌ MOINS ADAPTÉ POUR :                                       │
│                                                                 │
│   • Images (préférer CNN — Chapitre 6)                        │
│     → Structure spatiale 2D, invariance aux translations       │
│                                                                 │
│   • Texte / Séquences (préférer RNN, Transformers — Chap. 6)  │
│     → Structure séquentielle, contexte long                    │
│                                                                 │
│   • Graphes (préférer GNN)                                    │
│     → Relations entre entités, topologie complexe              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

<details>
<summary>🤔 Question Socratique : Si le MLP est "universel" (peut approximer n'importe quoi), pourquoi a-t-on inventé CNN, RNN, Transformers ?</summary>

### 🔑 Réponse

Excellente question ! Le MLP peut théoriquement tout faire, mais :

1. **Efficacité** : Un CNN exploite la structure spatiale des images (pixels voisins sont liés). Un MLP traiterait chaque pixel indépendamment et aurait besoin de BEAUCOUP plus de paramètres pour apprendre la même chose.

2. **Inductive bias** : Les architectures spécialisées "savent" déjà certaines choses sur le problème. Un CNN "sait" que les patterns locaux sont importants (via les convolutions). Un Transformer "sait" que les mots éloignés peuvent être liés (via l'attention).

3. **Généralisation** : Les architectures spécialisées généralisent mieux car elles ont les bons "a priori" pour le type de données.

Analogie : Un couteau suisse peut tout faire (ouvrir, couper, visser), mais un tournevis dédié vissera toujours mieux.

Le MLP reste essentiel car :
- Il est la base de toutes les autres architectures
- Il sert de "tête" finale dans CNN, Transformers, etc.
- Il est optimal pour les données tabulaires

</details>

---

## 🧭 Récapitulatif et Transition

### L'architecture MLP en un schéma

```
┌─────────────────────────────────────────────────────────────────┐
│               MLP : VUE D'ENSEMBLE                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│           ┌─────────────────────────────────────────┐          │
│           │                                         │          │
│   ENTRÉE  │  COUCHES CACHÉES (1 ou plusieurs)      │  SORTIE  │
│           │                                         │          │
│   ┌───┐   │   ┌───┐     ┌───┐     ┌───┐           │  ┌───┐   │
│   │ ○ │──►│──►│ ● │────►│ ● │────►│ ● │──────────►│──►│ ○ │   │
│   │ ○ │──►│──►│ ● │────►│ ● │────►│ ● │──────────►│──►│ ○ │   │
│   │ ○ │──►│──►│ ● │────►│ ● │                     │  └───┘   │
│   │ ○ │──►│──►│ ● │────►│ ● │                     │          │
│   └───┘   │   └───┘     └───┘                     │          │
│           │     │         │                        │          │
│    Pas de │   ReLU      ReLU                      │ Sigmoid/ │
│    calcul │  (activation)(activation)             │ Softmax/ │
│           │                                        │ Linéaire │
│           └────────────────────────────────────────┘          │
│                                                                 │
│   LARGEUR = Neurones par couche (capacité)                     │
│   PROFONDEUR = Nombre de couches (abstraction)                 │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Termes clés introduits

| Terme | Définition |
|-------|------------|
| **Input Layer** | Couche d'entrée, dimension = nombre de features |
| **Hidden Layer** | Couche(s) intermédiaire(s), fait le "travail" |
| **Output Layer** | Couche de sortie, adaptée au problème |
| **Largeur** | Nombre de neurones dans une couche |
| **Profondeur** | Nombre de couches du réseau |
| **Shallow** | Réseau à 1 couche cachée |
| **Deep** | Réseau à 2+ couches cachées |
| **Fully Connected** | Chaque neurone connecté à tous les précédents |
| **Dense** | Synonyme de Fully Connected (Keras) |
| **MLP** | Multi-Layer Perceptron, l'architecture de base |
| **Feedforward** | Information va de l'entrée vers la sortie |

---

### 🔮 Ce qui vient ensuite

*Nous avons construit une belle architecture — le MLP. Mais pour l'instant, c'est une coquille vide. Les poids sont initialisés aléatoirement, le réseau ne "sait" rien.*

*Comment le réseau apprend-il ? Comment ajuste-t-il ses milliers (ou millions) de poids pour faire de bonnes prédictions ?*

*C'est le sujet de la partie suivante : Forward Pass, Loss Function, Gradient Descent, et le fameux Backpropagation.*

---

## 📝 Réflexion Métacognitive

1. **Dessinez** un MLP à 3 entrées, 1 couche cachée de 4 neurones, et 2 sorties. Combien de paramètres (poids + biais) ?

2. **Expliquez** avec vos propres mots pourquoi la profondeur aide à apprendre des concepts complexes.

3. **Prédiction** : Si vous deviez deviner comment le réseau "apprend", que proposeriez-vous ? (Nous verrons la vraie réponse dans la partie suivante !)

---

*Dans la partie suivante, nous allons donner vie à notre MLP en découvrant la mécanique d'apprentissage : comment le réseau ajuste ses poids pour minimiser l'erreur.*
