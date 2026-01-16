# Chapitre 7 — EDA analytique

**Durée estimée : 10-12 heures**

---

## Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

1. **Analyser** la distribution d'une variable et interpréter ses caractéristiques statistiques
2. **Identifier** les relations entre variables (corrélation, associations)
3. **Comparer** des sous-populations et détecter des patterns significatifs
4. **Formuler** des hypothèses à partir des observations et éviter les pièges d'interprétation

---

## 🎯 Le Hook : Comment Target a su qu'une adolescente était enceinte avant son père

En 2012, un père furieux s'est rendu dans un magasin Target aux États-Unis pour se plaindre : sa fille de 16 ans recevait des coupons pour des produits de bébé. "Vous encouragez ma fille à tomber enceinte ?"

Quelques jours plus tard, il a rappelé pour s'excuser. Sa fille était effectivement enceinte. Il ne le savait pas encore.

Comment Target a-t-il deviné ? Par **l'analyse exploratoire des données** (EDA).

Les data scientists de Target avaient identifié un pattern : les femmes enceintes achètent des produits spécifiques (vitamines, coton, savon non parfumé) à des moments précis de leur grossesse. En analysant les achats, ils pouvaient prédire une grossesse avec une précision troublante.

C'est la puissance de l'EDA analytique : **découvrir des patterns cachés** dans les données.

> 💭 **Question Socratique #1** : Cette histoire soulève des questions éthiques. À partir de quand l'analyse de données devient-elle intrusive ? Où tracer la ligne entre "utile" et "dérangeant" ?

---

## 7.1 Différence EDA diagnostique vs analytique

### Rappel : vous êtes passé de l'autre côté

| Aspect | EDA Diagnostique (Chapitre 4) | EDA Analytique (Ce chapitre) |
|--------|------------------------------|------------------------------|
| **Données** | Brutes, avec problèmes | Nettoyées, structurées |
| **Question** | "Qu'est-ce qui ne va pas ?" | "Que disent les données ?" |
| **Objectif** | Identifier les erreurs | Comprendre les phénomènes |
| **Mindset** | Auditeur / Détective | Scientifique / Explorateur |
| **Output** | Checklist de problèmes | Hypothèses, insights |

```
Données brutes → [EDA DIAGNOSTIQUE] → [NETTOYAGE] → [EDA ANALYTIQUE] → Insights
                                                     (Vous êtes ici)
```

---

### Le mindset de l'exploration

En EDA analytique, vous cherchez à répondre à des questions comme :
- Quelle est la distribution de mes clients par âge ?
- Y a-t-il une relation entre le montant d'achat et l'ancienneté ?
- Quels segments de clients sont les plus rentables ?
- Existe-t-il des patterns saisonniers dans les ventes ?

**L'EDA est un dialogue avec vos données.** Vous posez des questions, les données répondent, et leurs réponses génèrent de nouvelles questions.

---

## 7.2 Analyse univariée

### Qu'est-ce que l'analyse univariée ?

L'analyse **univariée** examine une seule variable à la fois pour comprendre sa distribution.

### Statistiques de tendance centrale

```python
import pandas as pd
import numpy as np

# Mesures de centralité
moyenne = df['age'].mean()       # Sensible aux outliers
mediane = df['age'].median()     # Robuste aux outliers
mode = df['age'].mode()[0]       # Valeur la plus fréquente

print(f"Moyenne : {moyenne:.2f}")
print(f"Médiane : {mediane:.2f}")
print(f"Mode : {mode}")
```

**Quand utiliser quoi ?**

| Mesure | Quand l'utiliser | Exemple |
|--------|------------------|---------|
| **Moyenne** | Distribution symétrique | Taille des personnes |
| **Médiane** | Distribution asymétrique ou outliers | Salaires (PDG vs employés) |
| **Mode** | Variables catégorielles ou multimodales | Taille de vêtements |

---

### Statistiques de dispersion

```python
# Mesures de dispersion
etendue = df['age'].max() - df['age'].min()
variance = df['age'].var()
ecart_type = df['age'].std()
iqr = df['age'].quantile(0.75) - df['age'].quantile(0.25)
cv = df['age'].std() / df['age'].mean()  # Coefficient de variation

print(f"Étendue : {etendue}")
print(f"Écart-type : {ecart_type:.2f}")
print(f"IQR : {iqr}")
print(f"CV : {cv:.2%}")  # En pourcentage
```

**Interprétation du coefficient de variation (CV) :**
- CV < 15% : Faible dispersion
- 15% < CV < 30% : Dispersion modérée
- CV > 30% : Forte dispersion

---

### Forme de la distribution

```python
from scipy import stats

# Asymétrie (Skewness)
skewness = df['revenus'].skew()
# > 0 : Queue à droite (revenus typiques)
# < 0 : Queue à gauche
# ≈ 0 : Symétrique

# Aplatissement (Kurtosis)
kurtosis = df['revenus'].kurtosis()
# > 0 : Pic aigu, queues lourdes
# < 0 : Distribution plate
# ≈ 0 : Similaire à la normale

print(f"Skewness : {skewness:.2f}")
print(f"Kurtosis : {kurtosis:.2f}")
```

---

### ✍️ Exercice 7.1 : Analyse univariée complète (15 min)

```python
import pandas as pd
import numpy as np

# Données de salaires
np.random.seed(42)
salaires = np.concatenate([
    np.random.normal(45000, 8000, 900),   # Employés
    np.random.normal(80000, 15000, 90),   # Managers
    np.random.normal(200000, 50000, 10)   # Dirigeants
])
df = pd.DataFrame({'salaire': salaires})

# 1. Calculez les mesures de centralité
moyenne = df['salaire']._____
mediane = df['salaire']._____
print(f"Moyenne : {moyenne:,.0f} €")
print(f"Médiane : {mediane:,.0f} €")

# 2. Pourquoi la moyenne et la médiane sont-elles différentes ?
# Réponse : _____

# 3. Laquelle utiliseriez-vous pour représenter le "salaire typique" ?
# Réponse : _____

# 4. Calculez le coefficient de variation
cv = df['salaire'].std() / df['salaire'].mean()
print(f"CV : {cv:.1%}")
# Interprétation : _____

# 5. Calculez le skewness et interprétez
skewness = df['salaire'].skew()
print(f"Skewness : {skewness:.2f}")
# Interprétation : _____
```

---

### Distribution des variables catégorielles

```python
# Fréquences absolues
df['categorie'].value_counts()

# Fréquences relatives (pourcentages)
df['categorie'].value_counts(normalize=True) * 100

# Tableau croisé
pd.crosstab(df['region'], df['categorie'])

# Avec marges
pd.crosstab(df['region'], df['categorie'], margins=True)
```

---

> 💭 **Question Socratique #2** : Si une variable a une moyenne de 50, une médiane de 30, et un mode de 25, que pouvez-vous déduire sur la forme de sa distribution ?

---

## 7.3 Analyse bivariée

### Qu'est-ce que l'analyse bivariée ?

L'analyse **bivariée** examine la relation entre **deux** variables.

### Relation entre deux variables numériques : Corrélation

```python
# Corrélation de Pearson (linéaire)
correlation = df['age'].corr(df['revenus'])
print(f"Corrélation : {correlation:.2f}")

# Matrice de corrélation
corr_matrix = df[['age', 'revenus', 'anciennete', 'satisfaction']].corr()
print(corr_matrix)
```

**Interprétation de la corrélation :**

| Valeur | Interprétation |
|--------|----------------|
| 0.8 à 1.0 | Forte corrélation positive |
| 0.5 à 0.8 | Corrélation modérée positive |
| 0.2 à 0.5 | Faible corrélation positive |
| -0.2 à 0.2 | Pas de corrélation |
| -0.5 à -0.2 | Faible corrélation négative |
| -0.8 à -0.5 | Corrélation modérée négative |
| -1.0 à -0.8 | Forte corrélation négative |

---

### ⚠️ ATTENTION : Corrélation ≠ Causalité

**Exemple célèbre :** Il existe une forte corrélation entre les ventes de glaces et les noyades. Les glaces causent-elles les noyades ?

Non ! Les deux sont causés par un **facteur confondant** : la chaleur estivale.

```
        Chaleur (cause commune)
           ↙          ↘
    Ventes de       Noyades
    glaces
```

**Règle d'or :** La corrélation suggère une piste à explorer, pas une causalité prouvée.

---

### ✍️ Exercice 7.2 : Analyse de corrélation (15 min)

```python
import pandas as pd
import numpy as np

# Données simulées
np.random.seed(42)
n = 500
df = pd.DataFrame({
    'heures_etude': np.random.uniform(1, 10, n),
    'note_examen': None,  # À calculer
    'heures_sommeil': np.random.uniform(4, 10, n),
    'stress': np.random.uniform(1, 10, n)
})

# La note dépend des heures d'étude (corrélation positive)
df['note_examen'] = 5 + 1.5 * df['heures_etude'] + np.random.normal(0, 2, n)
df['note_examen'] = df['note_examen'].clip(0, 20)

# 1. Calculez la matrice de corrélation
corr_matrix = df.corr()
print("Matrice de corrélation :")
print(corr_matrix.round(2))

# 2. Quelle est la corrélation entre heures_etude et note_examen ?
corr_etude_note = df['heures_etude'].corr(df['note_examen'])
print(f"\nCorrélation étude-note : {corr_etude_note:.2f}")
# Interprétation : _____

# 3. Y a-t-il une corrélation entre stress et note_examen ?
corr_stress_note = df['_____'].corr(df['_____'])
print(f"Corrélation stress-note : {corr_stress_note:.2f}")
# Interprétation : _____

# 4. Peut-on conclure que "étudier plus CAUSE de meilleures notes" ?
# Réponse : _____
```

---

### Relation entre variables catégorielles : Tables de contingence

```python
# Tableau croisé simple
contingence = pd.crosstab(df['region'], df['segment'])
print(contingence)

# Avec pourcentages par ligne
contingence_pct = pd.crosstab(df['region'], df['segment'], normalize='index') * 100
print(contingence_pct.round(1))

# Test du Chi² pour l'indépendance
from scipy.stats import chi2_contingency
chi2, p_value, dof, expected = chi2_contingency(contingence)
print(f"\nTest Chi² : p-value = {p_value:.4f}")
if p_value < 0.05:
    print("→ Les variables sont liées (rejet de l'indépendance)")
else:
    print("→ Pas de lien significatif détecté")
```

---

### Relation numérique-catégorielle : Comparaison de groupes

```python
# Statistiques par groupe
df.groupby('region')['revenus'].describe()

# Moyenne par segment
df.groupby('segment')['montant_achat'].mean()

# Boxplot comparatif (code, la visualisation au Chapitre 8)
import matplotlib.pyplot as plt
df.boxplot(column='revenus', by='region')
```

---

### 🤖 IA : Interpréter des corrélations

**Prompt efficace :**

```
J'ai une matrice de corrélation avec ces valeurs significatives :
- age / revenus : 0.65
- ancienneté / satisfaction : 0.42
- heures_travail / satisfaction : -0.38
- age / ancienneté : 0.78

Contexte : Données RH d'une entreprise de 500 employés

1. Interprète chaque corrélation en termes business
2. Identifie les corrélations qui pourraient être dues à des facteurs confondants
3. Suggère des analyses complémentaires pour mieux comprendre ces relations
```

---

### ✍️ Exercice 7.3 : Analyse bivariée complète (20 min)

```python
import pandas as pd
import numpy as np

# Données clients
np.random.seed(42)
n = 1000
df = pd.DataFrame({
    'client_id': range(n),
    'age': np.random.randint(18, 70, n),
    'segment': np.random.choice(['Bronze', 'Silver', 'Gold'], n, p=[0.6, 0.3, 0.1]),
    'anciennete_mois': np.random.randint(1, 60, n),
    'nb_achats': np.random.randint(1, 50, n),
    'montant_total': None  # À calculer
})

# Le montant dépend du segment et de l'ancienneté
df['montant_total'] = (
    df['nb_achats'] * 50 +
    df['anciennete_mois'] * 10 +
    df['segment'].map({'Bronze': 0, 'Silver': 500, 'Gold': 2000}) +
    np.random.normal(0, 200, n)
).clip(0)

# ANALYSE BIVARIÉE

# 1. Corrélation entre ancienneté et montant total
corr = df['anciennete_mois'].corr(df['montant_total'])
print(f"Corrélation ancienneté-montant : {corr:.2f}")

# 2. Montant moyen par segment
print("\nMontant moyen par segment :")
print(df.groupby('segment')['montant_total'].mean().round(0))

# 3. Y a-t-il un lien entre l'âge et le segment ?
contingence = pd.crosstab(
    pd.cut(df['age'], bins=[0, 30, 50, 100], labels=['Jeune', 'Adulte', 'Senior']),
    df['segment']
)
print("\nÂge vs Segment :")
print(contingence)

# 4. Interprétation
# a) Les clients Gold dépensent-ils plus ? _____
# b) L'ancienneté est-elle corrélée aux dépenses ? _____
# c) Les seniors sont-ils plus représentés chez les Gold ? _____
```

---

## 7.4 Segmentation et analyse par groupes

### Identifier des sous-populations

```python
# Segmentation simple par quantiles
df['segment_valeur'] = pd.qcut(
    df['montant_total'],
    q=4,
    labels=['Low', 'Medium', 'High', 'VIP']
)

# Analyse par segment
df.groupby('segment_valeur').agg({
    'client_id': 'count',
    'age': 'mean',
    'anciennete_mois': 'mean',
    'nb_achats': 'mean',
    'montant_total': 'sum'
}).round(1)
```

---

### Questions métier typiques

```python
# QUI : Profil des meilleurs clients
top_clients = df.nlargest(100, 'montant_total')
print("Profil des 100 meilleurs clients :")
print(f"- Âge moyen : {top_clients['age'].mean():.0f}")
print(f"- Ancienneté moyenne : {top_clients['anciennete_mois'].mean():.0f} mois")
print(f"- Segment dominant : {top_clients['segment'].mode()[0]}")

# QUOI : Produits les plus achetés par segment
df.groupby('segment')['categorie_produit'].value_counts()

# QUAND : Patterns temporels
df.groupby(df['date'].dt.month)['montant'].sum()

# OÙ : Performance par région
df.groupby('region')['montant'].agg(['sum', 'mean', 'count'])

# POURQUOI : Facteurs de churn (clients qui partent)
df.groupby('a_churne').agg({
    'satisfaction': 'mean',
    'nb_reclamations': 'mean',
    'anciennete_mois': 'mean'
})
```

---

### ✍️ Exercice 7.4 : Segmentation clients (20 min)

```python
import pandas as pd
import numpy as np

# Données e-commerce
np.random.seed(42)
n = 2000
df = pd.DataFrame({
    'client_id': range(n),
    'date_inscription': pd.date_range('2020-01-01', periods=n, freq='12H'),
    'derniere_visite': pd.date_range('2024-06-01', periods=n, freq='6H'),
    'nb_visites': np.random.randint(1, 100, n),
    'nb_achats': np.random.randint(0, 30, n),
    'montant_total': np.random.exponential(200, n),
    'canal_acquisition': np.random.choice(['Organique', 'Payant', 'Social', 'Email'], n)
})

# 1. Créer un score de valeur client (RFM simplifié)
# R = Récence (jours depuis dernière visite)
df['recence'] = (pd.Timestamp.now() - df['derniere_visite']).dt.days

# F = Fréquence (nb_achats)
# M = Montant (montant_total)

# 2. Segmenter les clients en 4 groupes de valeur
df['segment_valeur'] = pd.qcut(df['montant_total'], q=4, labels=['Bronze', 'Silver', 'Gold', 'Platinum'])

# 3. Profil de chaque segment
profil_segments = df.groupby('segment_valeur').agg({
    'client_id': 'count',
    'recence': 'mean',
    'nb_visites': 'mean',
    'nb_achats': 'mean',
    'montant_total': ['mean', 'sum']
}).round(1)

print("Profil par segment :")
print(profil_segments)

# 4. Canal d'acquisition le plus efficace par segment
print("\nCanal dominant par segment :")
for segment in ['Bronze', 'Silver', 'Gold', 'Platinum']:
    canal = df[df['segment_valeur'] == segment]['canal_acquisition'].mode()[0]
    print(f"  {segment}: {canal}")

# 5. Insights
# a) Quel segment représente le plus de valeur totale ? _____
# b) Les clients Platinum visitent-ils plus souvent ? _____
# c) Y a-t-il un canal clairement meilleur pour les hautes valeurs ? _____
```

---

> 💭 **Question Socratique #3** : Une entreprise découvre que ses clients les plus rentables sont les 35-45 ans. Devrait-elle ignorer les autres tranches d'âge dans sa stratégie marketing ? Quels seraient les risques ?

---

## 7.5 Formulation d'hypothèses

### De l'observation à l'hypothèse

L'EDA génère des **observations** qui deviennent des **hypothèses** à tester.

```
OBSERVATION                          HYPOTHÈSE
┌────────────────────┐              ┌────────────────────────────────┐
│ "Les clients Gold  │              │ "L'ancienneté augmente la      │
│ ont une ancienneté │      →       │ probabilité de devenir Gold"   │
│ 2x plus élevée"    │              │                                │
└────────────────────┘              │ → À tester avec régression     │
                                    │   ou analyse de cohortes       │
                                    └────────────────────────────────┘
```

### Variables candidates pour le ML

```python
# Identifier les variables les plus corrélées à la cible
target = 'montant_total'
correlations = df.corr()[target].drop(target).abs().sort_values(ascending=False)
print("Variables les plus corrélées au montant :")
print(correlations.head(10))

# Variation inter-groupes
for col in ['segment', 'region', 'canal']:
    print(f"\n{col} vs {target}:")
    print(df.groupby(col)[target].mean().sort_values(ascending=False))
```

---

### Questions à approfondir

```python
# Template de documentation d'hypothèses
hypotheses = [
    {
        'observation': "Les clients Gold ont 2x plus d'ancienneté",
        'hypothese': "L'ancienneté cause la montée en gamme",
        'alternative': "Les Gold s'inscrivent à des moments différents",
        'test_suggere': "Analyse de cohortes par date d'inscription",
        'priorite': 'Haute'
    },
    {
        'observation': "Corrélation négative stress/satisfaction (-0.4)",
        'hypothese': "Le stress réduit la satisfaction",
        'alternative': "Un troisième facteur cause les deux",
        'test_suggere': "Analyse par département et charge de travail",
        'priorite': 'Moyenne'
    }
]

# Sauvegarder
import json
with open('hypotheses_eda.json', 'w') as f:
    json.dump(hypotheses, f, indent=2)
```

---

### ✍️ Exercice 7.5 : Formuler des hypothèses (15 min)

À partir de ces observations fictives, formulez des hypothèses et proposez des analyses complémentaires :

| Observation | Hypothèse | Analyse complémentaire |
|-------------|-----------|------------------------|
| Les ventes sont 30% plus élevées le week-end | _____ | _____ |
| Les clients qui appellent le support dépensent moins | _____ | _____ |
| La région Sud a le meilleur taux de conversion | _____ | _____ |
| Les utilisateurs mobiles abandonnent plus leur panier | _____ | _____ |

---

## 7.6 Pensée critique et pièges à éviter

### Piège 1 : Corrélation ≠ Causalité (rappel)

Toujours chercher :
- Les **facteurs confondants** (variable cachée qui cause les deux)
- La **causalité inverse** (B cause A, pas A cause B)
- La **simple coïncidence** (surtout avec beaucoup de variables testées)

---

### Piège 2 : Biais de sélection

```
┌─────────────────────────────────────────────────────────┐
│                    BIAIS DE SÉLECTION                   │
├─────────────────────────────────────────────────────────┤
│ Vous analysez UNIQUEMENT les clients qui ont acheté.    │
│ Mais que se passe-t-il avec ceux qui n'ont PAS acheté ? │
│                                                         │
│ Exemple : "Nos clients sont très satisfaits !"          │
│ Problème : Les insatisfaits ont déjà quitté.            │
└─────────────────────────────────────────────────────────┘
```

---

### Piège 3 : Survivorship Bias

**Exemple classique :** "Les avions qui reviennent de mission ont des impacts de balles sur les ailes. Renforçons les ailes !"

**Erreur :** Les avions touchés au moteur ou au cockpit ne reviennent PAS. On ne les analyse pas car ils n'existent plus dans les données.

**En data science :** Analyser uniquement les entreprises qui réussissent pour trouver les "facteurs de succès" ignore toutes les entreprises qui ont fait pareil et ont échoué.

---

### Piège 4 : Ce que les données ne disent PAS

```python
# Ce que vous pouvez voir
print("Données disponibles :")
print(df.columns.tolist())

# Ce que vous ne pouvez PAS voir
print("\nQuestions auxquelles ces données ne répondent pas :")
print("- Pourquoi les clients achètent-ils ?")
print("- Quelle est leur satisfaction réelle ?")
print("- Que font-ils chez les concurrents ?")
print("- Quels facteurs externes influencent les ventes ?")
```

**Règle d'or :** Les données montrent le "quoi", rarement le "pourquoi".

---

### ✍️ Exercice 7.6 : Identifier les biais (15 min)

Pour chaque situation, identifiez le biais ou piège potentiel :

**Situation 1 :** Une analyse montre que les employés qui prennent des formations ont de meilleurs résultats. Conclusion : "Les formations améliorent la performance."

- Biais potentiel : _____
- Explication alternative : _____

**Situation 2 :** Une étude sur les "secrets du succès" interview 100 entrepreneurs millionnaires.

- Biais potentiel : _____
- Ce qui manque : _____

**Situation 3 :** Les données de satisfaction client montrent 4.5/5. L'équipe conclut que les clients sont très satisfaits.

- Biais potentiel : _____
- Question à se poser : _____

**Situation 4 :** Une corrélation de 0.7 est trouvée entre la taille des bureaux et la productivité.

- Piège potentiel : _____
- Facteur confondant possible : _____

---

## 🧠 Réflexion métacognitive

### Auto-évaluation

| Compétence | 1 | 2 | 3 | 4 | 5 |
|------------|---|---|---|---|---|
| Je sais interpréter les statistiques descriptives | ○ | ○ | ○ | ○ | ○ |
| Je comprends la différence corrélation/causalité | ○ | ○ | ○ | ○ | ○ |
| Je peux comparer des groupes et identifier des patterns | ○ | ○ | ○ | ○ | ○ |
| Je sais formuler des hypothèses à partir d'observations | ○ | ○ | ○ | ○ | ○ |
| Je reconnais les biais courants dans l'analyse | ○ | ○ | ○ | ○ | ○ |

### Questions de réflexion

1. **Quelle observation** de ce chapitre vous a le plus surpris ?

2. **Comment éviteriez-vous** de tomber dans le piège de la corrélation/causalité dans votre travail ?

3. **Quelles questions** poseriez-vous avant de tirer des conclusions d'une analyse ?

---

## 📚 Résumé du chapitre

### Points clés à retenir

1. **Analyse univariée** :
   - Tendance centrale : moyenne (sensible), médiane (robuste), mode
   - Dispersion : écart-type, IQR, coefficient de variation
   - Forme : skewness (asymétrie), kurtosis (aplatissement)

2. **Analyse bivariée** :
   - Numérique-Numérique : corrélation de Pearson
   - Catégorielle-Catégorielle : tables de contingence, Chi²
   - Numérique-Catégorielle : comparaison de moyennes

3. **Corrélation ≠ Causalité** :
   - Chercher les facteurs confondants
   - Envisager la causalité inverse
   - Ne pas conclure hâtivement

4. **Biais à connaître** :
   - Biais de sélection
   - Survivorship bias
   - Données manquantes (ce qu'on ne voit pas)

5. **De l'observation à l'hypothèse** :
   - Documenter les observations
   - Formuler des hypothèses testables
   - Identifier les analyses complémentaires

---

## ➡️ Prochain chapitre

**Chapitre 8 : Visualisation exploratoire** — Vous apprendrez à créer des visualisations efficaces avec Matplotlib, Seaborn et Plotly.

---

*Module 2 — Pipeline Data | Chapitre 7 sur 11*
