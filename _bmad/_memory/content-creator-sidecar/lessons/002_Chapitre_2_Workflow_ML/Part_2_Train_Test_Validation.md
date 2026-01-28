# Chapitre 2 — Leçon 2 : Train / Test / Validation

## Objectifs d'apprentissage

À la fin de cette leçon, vous serez capable de :
- **Expliquer** pourquoi la séparation des données est essentielle pour évaluer un modèle
- **Implémenter** correctement un train/test split avec scikit-learn
- **Identifier** et **éviter** le data leakage, l'erreur #1 des débutants

---

## 🎯 Accroche : Le piège de l'étudiant tricheur

Imaginez un étudiant qui prépare un examen. Il obtient les réponses à l'avance et les mémorise parfaitement. Le jour J, il a 100% de bonnes réponses. Impressionnant, non ?

Mais donnez-lui un **nouvel examen** avec des questions différentes... et il échoue lamentablement.

**Question :** Cet étudiant a-t-il vraiment *compris* le cours, ou a-t-il simplement *mémorisé* les réponses ?

*(Réponse attendue : Il a mémorisé les réponses — il ne peut pas généraliser à de nouvelles questions)*

C'est exactement le problème d'un modèle ML entraîné et évalué sur les **mêmes données**. Il peut avoir l'air parfait, mais il ne sait pas généraliser.

---

## 2.1 Pourquoi séparer les données ?

### Le problème fondamental : généralisation vs mémorisation

Un modèle de Machine Learning a deux objectifs possibles :

| Objectif | Description | Bon ou mauvais ? |
|----------|-------------|------------------|
| **Mémoriser** | Retenir par cœur les exemples vus | ❌ Mauvais (overfitting) |
| **Généraliser** | Apprendre des patterns applicables à de nouveaux exemples | ✅ Bon |

**Question :** Si vous entraînez un modèle sur 1000 exemples et le testez sur ces mêmes 1000 exemples, que mesurez-vous réellement ?

*(Réponse attendue : Sa capacité à mémoriser, pas à généraliser)*

---

### La solution : des données qu'il n'a jamais vues

Pour mesurer la vraie performance d'un modèle, il faut le tester sur des données **qu'il n'a jamais vues pendant l'entraînement**.

```
┌─────────────────────────────────────────────────────────────────┐
│                    L'ANALOGIE DE L'EXAMEN                       │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   ÉTUDIANT                          MODÈLE ML                   │
│   ────────                          ─────────                   │
│                                                                 │
│   Cours + Exercices     ←→     Données d'entraînement (TRAIN)   │
│   (pour apprendre)              (pour apprendre les patterns)   │
│                                                                 │
│   Examen final          ←→     Données de test (TEST)           │
│   (questions nouvelles)         (exemples jamais vus)           │
│                                                                 │
│   Note à l'examen       ←→     Performance réelle               │
│   (mesure la compréhension)    (mesure la généralisation)       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

<details>
<summary>🤔 Question Socratique : Pourquoi serait-ce problématique d'évaluer un modèle de détection de spam uniquement sur les emails qu'il a déjà vus ?</summary>

### 🔑 Réponse

Si le modèle est évalué sur les emails d'entraînement, il pourrait simplement les reconnaître par cœur — comme reconnaître un email par son expéditeur ou sa date, plutôt que par son contenu.

En production, le modèle recevra de **nouveaux emails** qu'il n'a jamais vus. S'il ne peut que mémoriser, il sera incapable de détecter de nouveaux spams avec des formulations différentes.

Un modèle avec 99% de précision sur les données d'entraînement mais 60% sur de nouvelles données est **inutile** en production.

</details>

---

┌─────────────────────────────────────────────────────────────────┐
│ 📖 DÉFINITION : Généralisation                                  │
│                                                                 │
│ La généralisation est la capacité d'un modèle à faire des      │
│ prédictions correctes sur des données qu'il n'a jamais vues    │
│ pendant l'entraînement. C'est l'objectif ultime du Machine     │
│ Learning : apprendre des patterns généraux qui s'appliquent    │
│ au-delà des exemples spécifiques d'entraînement. Un modèle     │
│ qui généralise bien est utile en production ; un modèle qui    │
│ ne fait que mémoriser est inutile.                             │
└─────────────────────────────────────────────────────────────────┘

---

## 2.2 Train / Test Split

### Le ratio classique : 80/20

La pratique standard consiste à diviser vos données en deux ensembles :

```
┌─────────────────────────────────────────────────────────────────┐
│                     TRAIN / TEST SPLIT                          │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   DATASET COMPLET (100%)                                        │
│   ┌─────────────────────────────────────────────────────────┐   │
│   │░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░│▓▓▓▓▓▓▓▓▓▓▓▓▓▓▓│   │
│   │                                       │               │   │
│   │          TRAIN (80%)                  │  TEST (20%)   │   │
│   │                                       │               │   │
│   │   • Le modèle APPREND sur ces données │  • JAMAIS vu  │   │
│   │   • Utilisé pour .fit()               │  • Évaluation │   │
│   │                                       │    finale     │   │
│   └───────────────────────────────────────┴───────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

> *Source : [V7 Labs - Train Test Validation Split Best Practices](https://www.v7labs.com/blog/train-validation-test-set)*

**Question :** Pourquoi 80/20 et pas 50/50 ou 95/5 ?

*(Réponse attendue : 80/20 est un compromis — assez de données pour bien entraîner (80%), assez pour une évaluation fiable (20%))*

---

### Implémentation avec scikit-learn

```python
from sklearn.model_selection import train_test_split

# Supposons que X = features, y = target
X_train, X_test, y_train, y_test = train_test_split(
    X,
    y,
    test_size=0.2,      # 20% pour le test
    random_state=42     # Pour la reproductibilité
)
```

Décodons ce code ligne par ligne :

| Paramètre | Signification |
|-----------|---------------|
| `X` | Vos features (variables explicatives) |
| `y` | Votre target (variable à prédire) |
| `test_size=0.2` | 20% des données vont dans le test set |
| `random_state=42` | Fixe le hasard pour obtenir les mêmes résultats à chaque exécution |

**Résultat :**
- `X_train`, `y_train` → Pour entraîner le modèle (80%)
- `X_test`, `y_test` → Pour évaluer le modèle (20%)

<details>
<summary>🤔 Question Socratique : Que se passe-t-il si vous omettez `random_state` ?</summary>

### 🔑 Réponse

Sans `random_state`, scikit-learn utilise un générateur de nombres aléatoires qui change à chaque exécution. Conséquences :

1. **Résultats différents à chaque exécution** — Vous relancez le même code et obtenez une accuracy de 0.85 puis 0.82 puis 0.87...

2. **Impossible de debugger** — Si un collègue n'obtient pas les mêmes résultats que vous, est-ce un bug ou juste le hasard ?

3. **Impossible de comparer** — Comment savoir si votre nouveau modèle est meilleur si les données de test changent à chaque fois ?

**Règle d'or :** Toujours fixer `random_state` dans vos expériences. La valeur (42, 0, 123...) n'a pas d'importance, tant qu'elle est constante.

```python
# ✅ Reproductible
train_test_split(X, y, test_size=0.2, random_state=42)

# ❌ Non reproductible
train_test_split(X, y, test_size=0.2)
```

</details>

---

### La stratification : préserver les proportions

**Problème :** Imaginez un dataset avec 90% de clients fidèles et 10% de churners. Si le split est purement aléatoire, vous pourriez avoir un test set avec 15% de churners et un train set avec 8% — les proportions ne sont plus les mêmes !

**Solution :** La **stratification** garantit que les proportions de classes sont préservées dans chaque ensemble.

```python
# Sans stratification (risqué pour classes déséquilibrées)
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2)

# Avec stratification (recommandé pour classification)
X_train, X_test, y_train, y_test = train_test_split(
    X, y,
    test_size=0.2,
    stratify=y,         # ← Préserve les proportions de y
    random_state=42
)
```

```
┌─────────────────────────────────────────────────────────────────┐
│                    STRATIFICATION VISUALISÉE                    │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   DATASET ORIGINAL : 90% Fidèles (🟢), 10% Churners (🔴)        │
│                                                                 │
│   SANS STRATIFICATION (aléatoire) :                             │
│   ┌─────────────────────────────────┐  ┌───────────────────┐    │
│   │ Train: 🟢🟢🟢🟢🟢🟢🟢🟢🔴       │  │ Test: 🟢🟢🟢🔴🔴  │    │
│   │        (92% / 8%)              │  │       (60% / 40%) │    │
│   └─────────────────────────────────┘  └───────────────────┘    │
│   ❌ Proportions différentes !                                  │
│                                                                 │
│   AVEC STRATIFICATION :                                         │
│   ┌─────────────────────────────────┐  ┌───────────────────┐    │
│   │ Train: 🟢🟢🟢🟢🟢🟢🟢🟢🟢🔴    │  │ Test: 🟢🟢🟢🟢🔴  │    │
│   │        (90% / 10%)             │  │       (90% / 10%) │    │
│   └─────────────────────────────────┘  └───────────────────┘    │
│   ✅ Mêmes proportions partout !                                │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

> *Source : [Scikit-learn - train_test_split documentation](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.train_test_split.html)*

**Question :** Pour un problème de régression (prédire un prix), avez-vous besoin de stratifier ?

*(Réponse attendue : Non, la stratification s'applique aux classes discrètes — pour la régression, le split aléatoire suffit généralement)*

---

## 2.3 Le Validation Set : un troisième ensemble

### Quand 2 ensembles ne suffisent pas

Avec train/test, vous avez un problème : comment choisir les meilleurs hyperparamètres ?

**Scénario problématique :**
1. Vous entraînez avec `max_depth=5` → Test accuracy = 0.82
2. Vous entraînez avec `max_depth=10` → Test accuracy = 0.85
3. Vous entraînez avec `max_depth=15` → Test accuracy = 0.84
4. Vous choisissez `max_depth=10` car c'est le meilleur sur le test set

**Le problème ?** Vous avez utilisé le test set pour **prendre des décisions**. Il n'est plus "invisible" — vous avez optimisé pour lui !

---

### La solution : Train / Validation / Test

```
┌─────────────────────────────────────────────────────────────────┐
│                  TRAIN / VALIDATION / TEST                      │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│   DATASET COMPLET (100%)                                        │
│   ┌───────────────────────────────┬───────────┬─────────────┐   │
│   │░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░░│▒▒▒▒▒▒▒▒▒▒▒│▓▓▓▓▓▓▓▓▓▓▓▓▓│   │
│   │                               │           │             │   │
│   │       TRAIN (60%)             │VAL (20%)  │ TEST (20%)  │   │
│   │                               │           │             │   │
│   │  • Entraîner le modèle        │• Choisir  │• Évaluation │   │
│   │  • .fit()                     │  hyper-   │  FINALE     │   │
│   │                               │  params   │• UNE SEULE  │   │
│   │                               │• Comparer │  FOIS       │   │
│   │                               │  modèles  │             │   │
│   └───────────────────────────────┴───────────┴─────────────┘   │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

| Ensemble | Rôle | Quand l'utiliser |
|----------|------|------------------|
| **Train** | Apprendre les patterns | À chaque entraînement |
| **Validation** | Choisir hyperparamètres, comparer modèles | Pendant l'expérimentation |
| **Test** | Évaluation finale, non biaisée | **Une seule fois**, à la fin |

<details>
<summary>🤔 Question Socratique : Pourquoi le test set ne doit être utilisé qu'une seule fois, à la toute fin ?</summary>

### 🔑 Réponse

Chaque fois que vous regardez la performance sur le test set et que vous **ajustez quelque chose** en conséquence (hyperparamètres, features, algorithme), vous "trichez" un peu. Le test set n'est plus vraiment invisible.

Si vous faites 50 expériences en regardant le test set à chaque fois, vous finissez par optimiser indirectement pour ce test set spécifique. Votre modèle semblera excellent sur ce test, mais pourrait mal généraliser sur de vraies nouvelles données.

**Le test set est comme l'examen final** — vous n'êtes pas censé le voir avant le jour J. Le validation set, lui, est comme les examens blancs — vous pouvez en faire autant que vous voulez pour vous préparer.

</details>

---

### Implémentation : deux splits successifs

```python
from sklearn.model_selection import train_test_split

# Premier split : séparer le test set (20%)
X_temp, X_test, y_temp, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

# Deuxième split : séparer train et validation (80% restant → 75/25 = 60/20 du total)
X_train, X_val, y_train, y_val = train_test_split(
    X_temp, y_temp, test_size=0.25, random_state=42  # 25% de 80% = 20% du total
)

print(f"Train: {len(X_train)}, Val: {len(X_val)}, Test: {len(X_test)}")
```

**Note :** En pratique, on utilise souvent la **validation croisée** (cross-validation) plutôt qu'un validation set fixe. Nous verrons cela au Chapitre 4.

---

## 2.4 Data Leakage : L'erreur #1 des débutants

### Qu'est-ce que le data leakage ?

Le **data leakage** (fuite de données) se produit quand des informations du test set "fuient" dans le processus d'entraînement.

**Question :** Si vous calculez la moyenne d'une colonne sur TOUT le dataset, puis utilisez cette moyenne pour normaliser le train set, quel est le problème ?

*(Réponse attendue : La moyenne inclut des informations du test set — le modèle a indirectement "vu" les données de test)*

---

### L'impact catastrophique du leakage

Une étude de Princeton a montré que le data leakage a affecté **au moins 294 publications académiques** dans 17 disciplines, causant une crise de reproductibilité en ML.

> *Source : [Princeton - Leakage and the Reproducibility Crisis in ML-based Science](https://reproducible.cs.princeton.edu/)*

**Exemple réel — Prédiction de suicidalité :**
- Une étude a annoncé 91% de précision pour prédire la suicidalité chez les jeunes
- L'article a été **rétracté** car le modèle utilisait une sélection de features sur tout le dataset
- La vraie performance sur de nouvelles données était bien inférieure

> *Source : [IBM - Data Leakage in Machine Learning](https://www.ibm.com/think/topics/data-leakage-machine-learning)*

---

### Les formes courantes de leakage

```
┌─────────────────────────────────────────────────────────────────┐
│                    TYPES DE DATA LEAKAGE                        │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  1️⃣ PREPROCESSING AVANT SPLIT                                   │
│     ❌ scaler.fit(X)           # Sur TOUT le dataset            │
│        X_train, X_test = split(X)                               │
│                                                                 │
│     ✅ X_train, X_test = split(X)                               │
│        scaler.fit(X_train)     # SEULEMENT sur train            │
│                                                                 │
│  ─────────────────────────────────────────────────────────────  │
│                                                                 │
│  2️⃣ FEATURE SELECTION AVANT SPLIT                               │
│     ❌ best_features = select_features(X, y)   # Tout le dataset│
│        X_train, X_test = split(X[best_features])                │
│                                                                 │
│     ✅ X_train, X_test = split(X)                               │
│        best_features = select_features(X_train, y_train)        │
│                                                                 │
│  ─────────────────────────────────────────────────────────────  │
│                                                                 │
│  3️⃣ TARGET LEAKAGE (feature qui "contient" la réponse)          │
│     ❌ Feature "date_annulation" pour prédire le churn          │
│        (si date existe → client a déjà churné !)                │
│                                                                 │
│     ✅ Utiliser uniquement des features disponibles AVANT       │
│        la prédiction en production                              │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

<details>
<summary>🤔 Question Socratique : Vous entraînez un modèle de détection de fraude. Une feature "is_fraud_reported" donne 99% d'accuracy. Est-ce du leakage ?</summary>

### 🔑 Réponse

**Oui, c'est du target leakage !**

La feature "is_fraud_reported" encode directement la target que vous essayez de prédire. En production, au moment où vous devez prédire si une transaction est frauduleuse, cette information n'existe pas encore — le signalement viendra *après* (ou pas du tout).

C'est un exemple classique : utiliser une information qui n'est disponible **qu'après** l'événement que vous essayez de prédire.

**Règle :** Posez-vous toujours la question : "Cette feature serait-elle disponible au moment où je dois faire la prédiction en production ?"

</details>

---

### La règle d'or : FIT sur train, TRANSFORM sur tout

Le preprocessing doit suivre cette règle stricte :

```python
from sklearn.preprocessing import StandardScaler
from sklearn.model_selection import train_test_split

# 1. D'ABORD séparer les données
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# 2. FIT uniquement sur train
scaler = StandardScaler()
scaler.fit(X_train)              # Apprend mean/std du TRAIN seulement

# 3. TRANSFORM sur train ET test
X_train_scaled = scaler.transform(X_train)
X_test_scaled = scaler.transform(X_test)   # Utilise mean/std du train !
```

```
┌─────────────────────────────────────────────────────────────────┐
│              LA RÈGLE : FIT SUR TRAIN UNIQUEMENT                │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│                    ┌─────────────┐                              │
│                    │   SCALER    │                              │
│                    │             │                              │
│   X_train ────────►│   .fit()    │ Apprend μ, σ du train        │
│                    │             │                              │
│                    └──────┬──────┘                              │
│                           │                                     │
│                           ▼                                     │
│                    ┌─────────────┐                              │
│   X_train ────────►│.transform() │────► X_train_scaled          │
│                    └─────────────┘                              │
│                                                                 │
│                    ┌─────────────┐                              │
│   X_test  ────────►│.transform() │────► X_test_scaled           │
│                    └─────────────┘                              │
│                    (utilise μ, σ du train !)                    │
│                                                                 │
│   ⚠️ JAMAIS : scaler.fit(X_test) ou scaler.fit(X_complet)       │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

**Question :** Pourquoi applique-t-on `transform()` sur le test set avec les statistiques du train set ?

*(Réponse attendue : Parce qu'en production, on n'aura que de nouvelles données — on ne peut pas recalculer la moyenne sur des données qu'on ne connaît pas encore)*

---

┌─────────────────────────────────────────────────────────────────┐
│ 📖 DÉFINITION : Data Leakage                                    │
│                                                                 │
│ Le data leakage (fuite de données) se produit lorsque des      │
│ informations provenant de l'ensemble de test ou de données     │
│ futures sont utilisées pendant l'entraînement du modèle.       │
│ Cela crée une performance artificiellement élevée qui ne se    │
│ reproduit pas en production. Les formes courantes incluent :   │
│ preprocessing avant split, feature selection globale, et       │
│ utilisation de features non disponibles au moment de la        │
│ prédiction (target leakage).                                   │
└─────────────────────────────────────────────────────────────────┘

---

### Comment détecter le leakage ?

**Signaux d'alarme :**

| Signal | Pourquoi c'est suspect |
|--------|------------------------|
| Accuracy > 95% sur un problème difficile | Trop beau pour être vrai |
| Performance test ≈ performance train | Le modèle devrait être légèrement moins bon sur test |
| Une feature domine totalement | Vérifier si elle "encode" la réponse |
| Performance s'effondre en production | Leakage probable |

```python
# 🚨 Si vous voyez ce pattern, méfiez-vous !
train_accuracy = model.score(X_train, y_train)  # 0.99
test_accuracy = model.score(X_test, y_test)      # 0.98

# Écart trop faible + scores trop hauts = probable leakage
```

---

## 🧠 Réflexion métacognitive

1. **Avez-vous compris pourquoi le split DOIT se faire AVANT tout preprocessing ?** Si ce n'est pas clair, relisez la section sur le leakage.

2. **Pouvez-vous expliquer à quelqu'un d'autre** la différence entre mémoriser et généraliser ?

3. **Dans vos futurs projets**, comment allez-vous vous assurer de ne pas introduire de leakage ?

---

## 📝 Résumé

| Concept | Point clé |
|---------|-----------|
| Généralisation | L'objectif du ML — performer sur des données jamais vues |
| Train/Test split | 80/20 typique, `random_state` pour reproductibilité |
| Stratification | `stratify=y` pour préserver les proportions de classes |
| Validation set | 60/20/20 pour tuner les hyperparamètres |
| Data leakage | Information du test qui "fuit" vers le train — erreur fatale |
| Règle d'or | FIT sur train, TRANSFORM sur tout |

---

## 📊 Checklist anti-leakage

Avant de soumettre un modèle, vérifiez :

- [ ] Le split train/test est fait EN PREMIER
- [ ] Tous les scalers sont FIT sur train uniquement
- [ ] La sélection de features est faite sur train uniquement
- [ ] Aucune feature n'encode directement la target
- [ ] Toutes les features seraient disponibles en production au moment de la prédiction

---

## ➡️ Prochaine leçon

Dans la **Leçon 2.3 : Le pattern fit/predict**, nous allons découvrir l'API universelle de scikit-learn — le pattern que TOUS les modèles partagent : `.fit()`, `.predict()`, `.score()`.

**Question de transition :** Une fois vos données séparées proprement, comment "apprenez-vous" au modèle à faire des prédictions ?
