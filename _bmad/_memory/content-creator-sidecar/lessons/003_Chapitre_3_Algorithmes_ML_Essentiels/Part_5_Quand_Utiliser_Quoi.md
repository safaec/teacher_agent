# Part 5 : Quand Utiliser Quoi ?

**Durée estimée : 1h30**

## 🎯 Objectifs d'apprentissage

À la fin de cette partie, vous serez capable de :

1. **Choisir** le bon algorithme selon le type de problème
2. **Évaluer** les compromis entre interprétabilité et performance
3. **Appliquer** la règle "commencer simple"
4. **Comparer** les algorithmes sur un même dataset (Algorithm Tournament)

---

## 🧭 Le Flowchart de Sélection d'Algorithme

Vous faites face à un nouveau problème de Machine Learning. Par où commencer ?

Voici un arbre de décision pour vous guider :

```
┌───────────────────────────────────────────────────────────────────────────────┐
│                    QUEL ALGORITHME CHOISIR ?                                  │
├───────────────────────────────────────────────────────────────────────────────┤
│                                                                               │
│                        ┌─────────────────────┐                                │
│                        │ Avez-vous des LABELS│                                │
│                        │ (réponses connues)? │                                │
│                        └──────────┬──────────┘                                │
│                           OUI /        \ NON                                  │
│                              /          \                                     │
│              ┌───────────────┐          ┌───────────────┐                     │
│              │ SUPERVISÉ     │          │ NON-SUPERVISÉ │                     │
│              └───────┬───────┘          └───────┬───────┘                     │
│                      │                          │                             │
│                      ▼                          ▼                             │
│    ┌─────────────────────────────┐      ┌─────────────────┐                   │
│    │ La cible est-elle...        │      │ → K-Means       │                   │
│    │ • Un NOMBRE ? → Régression  │      │ → DBSCAN        │                   │
│    │ • Une CATÉGORIE ? → Classif.│      │ → Hierarchical  │                   │
│    └─────────────┬───────────────┘      └─────────────────┘                   │
│                  │                                                            │
│          ┌──────┴──────┐                                                      │
│          │             │                                                      │
│          ▼             ▼                                                      │
│    ┌──────────┐  ┌───────────────────┐                                        │
│    │RÉGRESSION│  │ CLASSIFICATION    │                                        │
│    └────┬─────┘  └────────┬──────────┘                                        │
│         │                 │                                                   │
│         ▼                 ▼                                                   │
│  • Linéaire ?        • 2 classes ?                                           │
│    → Linear Reg.       → Log. Reg. / RF                                      │
│  • Non-linéaire ?    • Plusieurs classes ?                                   │
│    → RF / XGBoost      → RF / Log. Reg.                                      │
│                                                                               │
└───────────────────────────────────────────────────────────────────────────────┘
```

**Question :** Vous voulez prédire le taux de churn (désabonnement) de vos clients. C'est quel type de problème ?

*(Réponse attendue : Classification binaire supervisée — on a des labels (churné/pas churné) et on prédit une catégorie)*

---

## 5.1 Guide de Sélection par Type de Problème

### Régression (prédire un nombre)

| Algorithme | Quand l'utiliser | Avantages | Limites |
|------------|------------------|-----------|---------|
| **Régression Linéaire** | Relation linéaire, besoin d'interpréter | Simple, rapide, coefficients interprétables | Échoue si relation non-linéaire |
| **Random Forest Regressor** | Relation complexe, beaucoup de features | Robuste, gère non-linéarité | Moins interprétable |
| **XGBoost** | Performance maximale, compétitions | Excellent sur données tabulaires | Complexe à tuner |

**Recommandation :** Commencer par **Régression Linéaire** pour établir une baseline, puis essayer **Random Forest** si la performance n'est pas suffisante.

### Classification (prédire une catégorie)

| Algorithme | Quand l'utiliser | Avantages | Limites |
|------------|------------------|-----------|---------|
| **Régression Logistique** | Relation linéaire, besoin de probabilités | Rapide, probabilités calibrées, interprétable | Limite si frontière non-linéaire |
| **Arbre de Décision** | Besoin d'explicabilité totale | Très interprétable, visualisable | Overfitting si trop profond |
| **Random Forest** | Performance robuste | Excellente performance, gère overfitting | Boîte noire |

**Recommandation :** Commencer par **Régression Logistique** (baseline), puis **Random Forest** pour améliorer la performance.

### Clustering (trouver des groupes)

| Algorithme | Quand l'utiliser | Avantages | Limites |
|------------|------------------|-----------|---------|
| **K-Means** | Clusters sphériques, K connu approximativement | Simple, rapide, scalable | Nécessite de choisir K |
| **DBSCAN** | Clusters de forme arbitraire, outliers | Trouve K automatiquement, détecte outliers | Paramètres sensibles |
| **Hierarchical** | Besoin de visualiser la hiérarchie | Dendrogramme informatif | Lent sur gros datasets |

**Recommandation :** Commencer par **K-Means** avec la méthode du coude.

---

## 5.2 Les Compromis à Considérer

Choisir un algorithme, c'est faire des compromis. Voici les principaux axes :

```
┌───────────────────────────────────────────────────────────────────────┐
│                     LES COMPROMIS DU ML                               │
├───────────────────────────────────────────────────────────────────────┤
│                                                                       │
│   INTERPRÉTABILITÉ ◄────────────────────────────► PERFORMANCE        │
│                                                                       │
│   │                                                              │    │
│   │  Régression     Arbre        Random      Gradient           │    │
│   │  Linéaire/      de           Forest      Boosting           │    │
│   │  Logistique     Décision                 (XGBoost)          │    │
│   │                                                              │    │
│   │  ████████       ██████       ████        ██                 │    │
│   │  Très           Assez        Peu         Boîte              │    │
│   │  interprétable  interprétable interprét. noire              │    │
│   │                                                              │    │
│   └──────────────────────────────────────────────────────────────┘    │
│                                                                       │
│   VITESSE ◄─────────────────────────────────────────► PRÉCISION      │
│                                                                       │
│   │  Régression < Decision Tree < Random Forest < Neural Net     │    │
│   │  (rapide)                                        (lent)      │    │
│   └──────────────────────────────────────────────────────────────┘    │
│                                                                       │
│   SIMPLICITÉ ◄──────────────────────────────────────► COMPLEXITÉ     │
│                                                                       │
│   │  Peu de paramètres ─────────────────► Beaucoup à tuner       │    │
│   │  LinReg (0)        RF (~5)           XGBoost (~20)           │    │
│   └──────────────────────────────────────────────────────────────┘    │
│                                                                       │
└───────────────────────────────────────────────────────────────────────┘
```

### Quand choisir l'interprétabilité ?

- **Domaine régulé** (banque, santé) : vous devez expliquer vos décisions
- **Debugging** : comprendre pourquoi le modèle se trompe
- **Confiance des stakeholders** : le métier veut comprendre
- **Extraction de connaissance** : apprendre des insights sur les données

**Exemple :** Une banque refuse un prêt. Le client demande pourquoi. Avec la régression logistique, vous pouvez dire : "Votre ratio d'endettement (coefficient = -2.3) est le facteur principal."

### Quand choisir la performance ?

- **Compétitions** (Kaggle) : seul le score compte
- **Systèmes automatisés** : pas d'humain dans la boucle
- **Grand volume** : quelques % de précision = millions d'euros

<details>
<summary>🤔 Question Socratique : Une startup veut détecter les fraudes bancaires. Interprétabilité ou performance ?</summary>

### 🔑 Réponse

**Les deux sont importants !**

- **Performance** : Chaque fraude non détectée coûte de l'argent
- **Interprétabilité** : Les régulateurs peuvent exiger des explications

**Solution hybride :** Utiliser un modèle performant (Random Forest) + techniques d'explicabilité (SHAP, LIME) pour expliquer les décisions a posteriori.

</details>

---

## 5.3 Recommandations Pratiques

### La règle d'or : "Commencer Simple"

```
┌─────────────────────────────────────────────────────────────────────────┐
│ 📖 DÉFINITION : Baseline (Modèle de référence)                          │
│                                                                         │
│ Une **baseline** est un modèle simple qui sert de point de comparaison. │
│                                                                         │
│ Tout nouveau modèle doit **battre la baseline** pour être utile.        │
│ Si un modèle complexe fait à peine mieux qu'une régression linéaire,    │
│ gardez la régression linéaire !                                         │
│                                                                         │
│ Baselines typiques :                                                    │
│ • Classification : DummyClassifier (prédit la classe majoritaire)      │
│ • Régression : DummyRegressor (prédit la moyenne)                      │
│ • Puis : Régression Logistique / Linéaire                              │
└─────────────────────────────────────────────────────────────────────────┘
```

**Pourquoi commencer simple ?**

1. **Référence** : Savoir si votre modèle complexe apporte vraiment quelque chose
2. **Debugging** : Si la baseline est déjà bonne, le problème est peut-être facile
3. **Temps** : Un modèle simple en production vaut mieux qu'un modèle parfait jamais livré
4. **Maintenabilité** : Plus c'est simple, plus c'est facile à maintenir

### Le workflow recommandé

| Étape | Régression Linéaire (Prédiction continue) | Régression Logistique (Classification binaire/multi) | Arbres & Random Forest (Classification) | ⚠️ K-Means (Non-Supervisé / Clustering) |
| :--- | :--- | :--- | :--- | :--- |
| **1. Imports** | `from sklearn.linear_model import LinearRegression` | `from sklearn.linear_model import LogisticRegression` | `from sklearn.tree import DecisionTreeClassifier`<br>`from sklearn.ensemble import RandomForestClassifier` | `from sklearn.cluster import KMeans` |
| **2. Scaling (Prétraitement)** | **OBLIGATOIRE**<br>`scaler = StandardScaler()`<br>`X_scaled = scaler.fit_transform(X)` | **OBLIGATOIRE**<br>`scaler = StandardScaler()`<br>`X_scaled = scaler.fit_transform(X)` | **INUTILE**<br>(Les arbres gèrent les échelles différentes)<br>`X_scaled = X` | **CRITIQUE**<br>`scaler = StandardScaler()`<br>`X_scaled = scaler.fit_transform(X)` |
| **3. Split (Jeu de données)** | `X_train, X_test, y_train, y_test = train_test_split(X_scaled, y)` | `X_train, X_test, y_train, y_test = train_test_split(X_scaled, y)` | `X_train, X_test, y_train, y_test = train_test_split(X, y)` | **Pas de split standard**<br>(On utilise tout X, car pas de y à prédire pour valider)<br>`X_train = X_scaled` |
| **4. Instanciation (Création)** | `model = LinearRegression()` | `model = LogisticRegression()`<br>(Gère binaire et multi auto) | `model = DecisionTreeClassifier()`<br>**OU**<br>`model = RandomForestClassifier()` | `model = KMeans(n_clusters=3)`<br>(Il faut choisir K) |
| **5. Hyperparamètres (Réglages clés)** | Souvent aucun pour la version simple. | `C=1.0` (Inverse de la régularisation)<br>`solver='liblinear'` | `max_depth=5`<br>`min_samples_leaf=2`<br>`n_estimators=100` (Forest) <br> `class_weight='balanced` (Forest) <br>| `n_clusters=3`<br>`init='k-means++'` |
| **6. Entraînement (fit)** | `model.fit(X_train, y_train)`<br>(Apprend la ligne) | `model.fit(X_train, y_train)`<br>(Apprend la frontière) | `model.fit(X_train, y_train)`<br>(Construit l'arbre) | `model.fit(X_train)` 🚨 **Jamais de y ici !**<br>(Trouve les centres) |
| **7. Prédiction (predict)** | `y_pred = model.predict(X_test)`<br>(Sortie : nombre continu) | `y_pred = model.predict(X_test)`<br>(Sortie : classe 0 ou 1) | `y_pred = model.predict(X_test)`<br>(Sortie : classe) | `clusters = model.predict(X)`<br>(Sortie : ID du groupe 0, 1, 2...) |
| **8. Évaluation (Métriques)** | `mean_squared_error(y_test, y_pred)`<br>`r2_score(y_test, y_pred)` | `accuracy_score(y_test, y_pred)`<br>`confusion_matrix(y_test, y_pred)` | `accuracy_score(y_test, y_pred)`<br>`confusion_matrix(y_test, y_pred)` | `model.inertia_` (Méthode Elbow)<br>`silhouette_score(X, clusters)` |

Différence évaluation et prédiction

```
PHASE 1 : PRÉDICTION
                  (L'étudiant passe l'examen)

      [ X_test ]  ---------->  [  MODÈLE  ]  ---------->  [ y_pred ]
    (Les questions)          (Le cerveau entraîné)      (La copie de l'élève)
                                                                |
                                                                |
                                                                |
                  PHASE 2 : ÉVALUATION                          |
                  (Le prof corrige la copie)                    |
                                                                v
      [ y_test ]  ----------------------------------->  {  COMPARER  }
 (Le corrigé officiel)                                          |
   (La Vraie Vie)                                               |
                                                                v
                                                          [   SCORE   ]
                                                     (Note / Erreur / Accuracy)
````

Essayer plusieurs modèles

```
1. DummyClassifier/Regressor
   │
   ▼ (battre cette baseline triviale)
2. Régression Logistique / Linéaire
   │
   ▼ (si pas assez performant)
3. Random Forest
   │
   ▼ (si besoin de +2-3% de performance)
4. Gradient Boosting (XGBoost, LightGBM)
   │
   ▼ (si vraiment nécessaire)
5. Ensemble / Stacking
```

### Ne pas sur-ingéniérer

> "Un bon data scientist n'est pas celui qui utilise les algorithmes les plus complexes, mais celui qui résout le problème business avec la solution la plus simple possible."

**Signes de sur-ingénierie :**

- Vous passez des jours à tuner des hyperparamètres pour gagner 0.1%
- Votre pipeline a 15 étapes de preprocessing
- Personne ne comprend votre code
- Le modèle met 10 minutes à faire une prédiction

---

## 5.4 Tableau Comparatif Final

| Critère | Rég. Linéaire | Rég. Logistique | Arbre | Random Forest | K-Means |
|---------|---------------|-----------------|-------|---------------|---------|
| **Type de problème** | Régression | Classification | Les deux | Les deux | Clustering |
| **Interprétabilité** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐ |
| **Performance** | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐ |
| **Vitesse (train)** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ |
| **Gère non-linéarité** | ❌ | ❌ | ✅ | ✅ | ✅ |
| **Robuste au bruit** | ⭐⭐ | ⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ |
| **Besoin de normaliser** | ⚠️ Parfois | ⚠️ Recommandé | ❌ Non | ❌ Non | ✅ Obligatoire |

---

## 🏆 Activité : Algorithm Tournament

**Objectif :** Comparer 4 algorithmes sur le même dataset et couronner le champion !

### Le Dataset : Prédire la qualité du vin

```python
from sklearn.datasets import load_wine
from sklearn.model_selection import train_test_split, cross_val_score
from sklearn.linear_model import LogisticRegression
from sklearn.tree import DecisionTreeClassifier
from sklearn.ensemble import RandomForestClassifier
from sklearn.preprocessing import StandardScaler
from sklearn.pipeline import Pipeline
import pandas as pd
import numpy as np

# Charger les données
wine = load_wine()
X, y = wine.data, wine.target

# Split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.2, random_state=42)

# Les concurrents
competitors = {
    'Régression Logistique': Pipeline([
        ('scaler', StandardScaler()),
        ('clf', LogisticRegression(max_iter=1000))
    ]),
    'Arbre de Décision': DecisionTreeClassifier(max_depth=5, random_state=42),
    'Random Forest': RandomForestClassifier(n_estimators=100, random_state=42)
}

# Le tournoi !
print("═" * 60)
print("🏆 ALGORITHM TOURNAMENT - Classification de Vins")
print("═" * 60)

results = []
for name, model in competitors.items():
    # Cross-validation (plus robuste)
    cv_scores = cross_val_score(model, X_train, y_train, cv=5)

    # Score sur test
    model.fit(X_train, y_train)
    test_score = model.score(X_test, y_test)

    results.append({
        'Modèle': name,
        'CV Score (mean)': cv_scores.mean(),
        'CV Score (std)': cv_scores.std(),
        'Test Score': test_score
    })

    print(f"\n{name}:")
    print(f"  CV: {cv_scores.mean():.1%} (+/- {cv_scores.std()*2:.1%})")
    print(f"  Test: {test_score:.1%}")

# Classement
print("\n" + "═" * 60)
print("🥇🥈🥉 CLASSEMENT FINAL")
print("═" * 60)

df_results = pd.DataFrame(results).sort_values('Test Score', ascending=False)
for i, (_, row) in enumerate(df_results.iterrows()):
    medal = ['🥇', '🥈', '🥉'][i]
    print(f"{medal} {row['Modèle']:25s} : {row['Test Score']:.1%}")
```

### Questions pour l'analyse

1. **Quel algorithme a gagné ?** Est-ce surprenant ?
2. **L'écart est-il significatif ?** (regarder l'écart-type du CV)
3. **Si vous deviez choisir pour la production**, prendriez-vous forcément le gagnant ? Pourquoi ?

<details>
<summary>🔑 Réponse aux questions</summary>

1. **Random Forest gagne souvent** sur ce type de dataset tabulaire. C'est attendu car il gère bien les interactions entre features et est robuste au bruit.

2. **L'écart n'est pas toujours significatif.** Si Random Forest fait 98% et Logistic Regression 95%, mais avec un écart-type de 3%, la différence n'est pas si importante.

3. **En production, on pourrait choisir Logistic Regression** si :
   - La différence de performance est faible
   - On a besoin d'expliquer les prédictions
   - La vitesse de prédiction compte
   - On veut un modèle simple à maintenir

**Le meilleur algorithme dépend du contexte, pas juste du score !**

</details>

---

## 📚 Récapitulatif du Chapitre 3

### Les 4 algorithmes que vous maîtrisez maintenant

| Algorithme | Type | Quand l'utiliser | Code essentiel |
|------------|------|------------------|----------------|
| **Régression Linéaire** | Régression | Prédire un nombre, relation linéaire | `LinearRegression()` |
| **Régression Logistique** | Classification | Prédire une catégorie, obtenir des probabilités | `LogisticRegression()` |
| **Random Forest** | Les deux | Performance robuste, features importance | `RandomForestClassifier/Regressor()` |
| **K-Means** | Clustering | Trouver des groupes naturels | `KMeans(n_clusters=k)` |

### Checklist avant de choisir un algorithme

- [ ] J'ai des labels ? (Supervisé vs Non-supervisé)
- [ ] Je prédis un nombre ou une catégorie ? (Régression vs Classification)
- [ ] J'ai besoin d'interpréter le modèle ?
- [ ] Quelle est ma contrainte de temps/ressources ?
- [ ] J'ai établi une baseline simple ?

### La prochaine étape

Maintenant que vous savez construire des modèles, comment savoir s'ils sont **vraiment bons** ? Le **Chapitre 4 : Évaluation et Optimisation** vous apprendra à :

- Choisir les bonnes métriques (pas juste l'accuracy !)
- Diagnostiquer overfitting/underfitting
- Optimiser les hyperparamètres avec GridSearchCV

---

## 🤔 Réflexion Métacognitive

Avant de passer au chapitre suivant :

1. Si quelqu'un vous dit "J'utilise toujours XGBoost parce que c'est le meilleur", que lui répondriez-vous ?
2. Pouvez-vous expliquer à un non-technicien la différence entre régression et classification ?
3. Dans votre futur travail, quel algorithme pensez-vous utiliser le plus souvent ?

---

## 🎉 Félicitations

Vous avez terminé le **Chapitre 3 : Algorithmes ML Essentiels** !

Vous savez maintenant :

- ✅ Prédire des nombres avec la régression linéaire
- ✅ Classifier avec la régression logistique
- ✅ Utiliser les arbres et Random Forest pour des prédictions robustes
- ✅ Segmenter des données avec K-Means
- ✅ Choisir le bon algorithme selon le contexte

**Prochain chapitre :** Évaluation et Optimisation — apprendre à mesurer et améliorer vos modèles !
