# Chapitre 8 — Visualisation exploratoire

**Durée estimée : 8-10 heures**

---

## Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

1. **Choisir** le type de graphique approprié selon la question et les données
2. **Créer** des visualisations avec Matplotlib et Seaborn
3. **Personnaliser** les graphiques pour une communication efficace
4. **Appliquer** les bonnes pratiques de visualisation (lisibilité, accessibilité)

---

## 🎯 Le Hook : Le graphique qui a sauvé des milliers de vies

En 1854, le choléra ravageait Londres. Les médecins pensaient que la maladie se transmettait par l'air. Mais le Dr John Snow avait une autre théorie.

Il a créé une **carte** pointant chaque cas de choléra dans le quartier de Soho. Le résultat était saisissant : les cas se concentraient autour d'une pompe à eau sur Broad Street.

Cette simple visualisation a prouvé que le choléra se transmettait par l'eau contaminée, pas par l'air. La pompe a été fermée, l'épidémie a cessé.

**Une bonne visualisation ne décore pas les données. Elle révèle la vérité.**

> 💭 **Question Socratique #1** : Pourquoi pensez-vous qu'une carte était plus convaincante qu'un tableau de chiffres pour démontrer l'origine de l'épidémie ?

---

## 8.1 Principes de visualisation

### Choisir le bon graphique

```
┌──────────────────────────────────────────────────────────────────────┐
│                    QUEL GRAPHIQUE CHOISIR ?                          │
├──────────────────────────────────────────────────────────────────────┤
│                                                                      │
│  COMPARAISON                    DISTRIBUTION                         │
│  ┌─────────────────┐           ┌─────────────────┐                  │
│  │ Bar chart       │           │ Histogram       │                  │
│  │ (catégories)    │           │ (1 variable)    │                  │
│  │                 │           │                 │                  │
│  │ Grouped bar     │           │ Boxplot         │                  │
│  │ (comparaison)   │           │ (outliers)      │                  │
│  └─────────────────┘           └─────────────────┘                  │
│                                                                      │
│  RELATION                       ÉVOLUTION                            │
│  ┌─────────────────┐           ┌─────────────────┐                  │
│  │ Scatter plot    │           │ Line chart      │                  │
│  │ (2 numériques)  │           │ (temps)         │                  │
│  │                 │           │                 │                  │
│  │ Heatmap         │           │ Area chart      │                  │
│  │ (corrélations)  │           │ (volumes)       │                  │
│  └─────────────────┘           └─────────────────┘                  │
│                                                                      │
│  COMPOSITION                    PARTIE D'UN TOUT                     │
│  ┌─────────────────┐           ┌─────────────────┐                  │
│  │ Stacked bar     │           │ Pie chart       │                  │
│  │ (composition    │           │ (< 5 catégories)│                  │
│  │ dans le temps)  │           │                 │                  │
│  └─────────────────┘           └─────────────────┘                  │
└──────────────────────────────────────────────────────────────────────┘
```

---

### Guide de sélection rapide

| Question | Graphique recommandé |
|----------|---------------------|
| Comment se distribue une variable ? | Histogramme, KDE |
| Y a-t-il des outliers ? | Boxplot |
| Quelle est la tendance dans le temps ? | Line chart |
| Comment comparer des catégories ? | Bar chart |
| Y a-t-il une relation entre deux variables ? | Scatter plot |
| Quelles variables sont corrélées ? | Heatmap |
| Quelle est la composition d'un tout ? | Pie chart (< 5 parts) ou bar chart |

---

### ✍️ Exercice 8.1 : Choix du graphique (10 min)

Pour chaque question, identifiez le type de graphique le plus approprié :

| Question | Graphique |
|----------|-----------|
| Comment les ventes ont-elles évolué sur 12 mois ? | _____ |
| Quelle est la répartition des âges de nos clients ? | _____ |
| Y a-t-il un lien entre le prix et les ventes ? | _____ |
| Comment se comparent les ventes par région ? | _____ |
| Quelle part de marché a chaque concurrent ? | _____ |
| Y a-t-il des valeurs extrêmes dans les salaires ? | _____ |

---

## 8.2 Matplotlib : fondamentaux

### Installation et import

```python
import matplotlib.pyplot as plt
import numpy as np
import pandas as pd

# Pour des graphiques inline dans Jupyter
%matplotlib inline
```

---

### Anatomie d'un graphique Matplotlib

```
┌─────────────────────────────────────────────────────────┐
│  Figure (conteneur global)                              │
│  ┌───────────────────────────────────────────────────┐ │
│  │  Axes (zone de tracé)                  ┌───────┐  │ │
│  │                                        │Legend │  │ │
│  │       Title                            └───────┘  │ │
│  │  ┌────────────────────────────────────────────┐  │ │
│  │  │                    .                       │  │ │
│  │ Y│                 .     .                    │  │ │
│  │ l│              .           .                 │  │ │
│  │ a│           .                 .              │  │ │
│  │ b│        .                       .           │  │ │
│  │ e│     .                             .        │  │ │
│  │ l│  .                                   .     │  │ │
│  │  └────────────────────────────────────────────┘  │ │
│  │              X label                             │ │
│  └───────────────────────────────────────────────────┘ │
└─────────────────────────────────────────────────────────┘
```

---

### Graphiques de base

```python
# Données exemple
np.random.seed(42)
x = np.arange(1, 13)
y = np.random.randint(100, 500, 12)

# LINE CHART
plt.figure(figsize=(10, 6))
plt.plot(x, y, marker='o', linestyle='-', color='blue', linewidth=2)
plt.title('Ventes mensuelles 2024', fontsize=14)
plt.xlabel('Mois')
plt.ylabel('Ventes (k€)')
plt.grid(True, alpha=0.3)
plt.show()
```

```python
# BAR CHART
categories = ['Nord', 'Sud', 'Est', 'Ouest']
valeurs = [450, 380, 290, 420]

plt.figure(figsize=(8, 5))
plt.bar(categories, valeurs, color=['#3498db', '#e74c3c', '#2ecc71', '#f39c12'])
plt.title('Ventes par région')
plt.ylabel('Ventes (k€)')
plt.show()
```

```python
# SCATTER PLOT
x = np.random.normal(50, 10, 100)
y = 2 * x + np.random.normal(0, 10, 100)

plt.figure(figsize=(8, 6))
plt.scatter(x, y, alpha=0.6, c='blue', edgecolors='black')
plt.title('Relation prix / ventes')
plt.xlabel('Prix (€)')
plt.ylabel('Ventes')
plt.show()
```

```python
# HISTOGRAM
ages = np.random.normal(40, 12, 1000)

plt.figure(figsize=(8, 5))
plt.hist(ages, bins=30, color='steelblue', edgecolor='black', alpha=0.7)
plt.title('Distribution des âges')
plt.xlabel('Âge')
plt.ylabel('Fréquence')
plt.axvline(np.median(ages), color='red', linestyle='--', label=f'Médiane: {np.median(ages):.0f}')
plt.legend()
plt.show()
```

---

### Subplots : plusieurs graphiques

```python
fig, axes = plt.subplots(2, 2, figsize=(12, 10))

# Graphique 1
axes[0, 0].plot(x, y)
axes[0, 0].set_title('Line chart')

# Graphique 2
axes[0, 1].bar(categories, valeurs)
axes[0, 1].set_title('Bar chart')

# Graphique 3
axes[1, 0].scatter(x, y)
axes[1, 0].set_title('Scatter plot')

# Graphique 4
axes[1, 1].hist(ages, bins=20)
axes[1, 1].set_title('Histogram')

plt.tight_layout()
plt.show()
```

---

### 🤖 IA : Générer du code Matplotlib

**Prompt efficace :**

```
Génère du code Matplotlib pour créer :
- Un graphique à barres horizontales
- Données : {'Python': 85, 'SQL': 72, 'R': 45, 'Excel': 90}
- Triées par valeur décroissante
- Couleurs dégradées du vert (max) au rouge (min)
- Titre : "Compétences de l'équipe Data"
- Labels sur les barres
- Figure de 10x6 pouces
```

---

### ✍️ Exercice 8.2 : Créer un dashboard Matplotlib (20 min)

```python
import matplotlib.pyplot as plt
import numpy as np

# Données
np.random.seed(42)
mois = ['Jan', 'Fév', 'Mar', 'Avr', 'Mai', 'Jun', 'Jul', 'Aoû', 'Sep', 'Oct', 'Nov', 'Déc']
ventes = [120, 135, 140, 155, 180, 210, 195, 185, 170, 165, 190, 250]
regions = ['Nord', 'Sud', 'Est', 'Ouest']
ventes_region = [450, 380, 290, 420]
ages = np.random.normal(35, 10, 500)

# Créer un dashboard 2x2
fig, axes = plt.subplots(2, 2, figsize=(14, 10))

# 1. Line chart : évolution mensuelle
axes[0, 0].plot(mois, ventes, marker='o', color='#3498db', linewidth=2)
axes[0, 0].set_title('Évolution des ventes 2024', fontsize=12, fontweight='bold')
axes[0, 0].set_ylabel('Ventes (k€)')
axes[0, 0].grid(True, alpha=0.3)
# Ajoutez une ligne horizontale pour la moyenne
axes[0, 0].axhline(y=np.mean(ventes), color='red', linestyle='--', alpha=0.7)

# 2. Bar chart : ventes par région
axes[0, 1].bar(regions, ventes_region, color=['#3498db', '#e74c3c', '#2ecc71', '#f39c12'])
axes[0, 1].set_title('Ventes par région', fontsize=12, fontweight='bold')
axes[0, 1].set_ylabel('Ventes (k€)')

# 3. Histogram : distribution des âges
axes[1, 0].hist(ages, bins=25, color='steelblue', edgecolor='black', alpha=0.7)
axes[1, 0].set_title('Distribution des âges clients', fontsize=12, fontweight='bold')
axes[1, 0].set_xlabel('Âge')
axes[1, 0].set_ylabel('Fréquence')

# 4. Pie chart : répartition par région (en %)
axes[1, 1].pie(ventes_region, labels=regions, autopct='%1.1f%%', startangle=90,
               colors=['#3498db', '#e74c3c', '#2ecc71', '#f39c12'])
axes[1, 1].set_title('Répartition par région', fontsize=12, fontweight='bold')

plt.tight_layout()
plt.savefig('dashboard.png', dpi=150)  # Sauvegarder
plt.show()
```

---

## 8.3 Seaborn : visualisations statistiques

### Pourquoi Seaborn ?

Seaborn est construit sur Matplotlib mais offre :
- Des graphiques statistiques prêts à l'emploi
- Une meilleure esthétique par défaut
- Une intégration native avec pandas

```python
import seaborn as sns

# Configurer le style
sns.set_theme(style="whitegrid")
```

---

### Distributions

```python
import pandas as pd
import numpy as np

# Données
df = pd.DataFrame({
    'age': np.random.normal(40, 12, 500),
    'revenus': np.random.exponential(50000, 500),
    'segment': np.random.choice(['A', 'B', 'C'], 500)
})

# HISTPLOT : histogramme amélioré
plt.figure(figsize=(10, 5))
sns.histplot(df['age'], kde=True, bins=30)
plt.title('Distribution des âges avec KDE')
plt.show()

# KDEPLOT : courbe de densité
plt.figure(figsize=(10, 5))
sns.kdeplot(data=df, x='age', hue='segment', fill=True, alpha=0.5)
plt.title('Densité par segment')
plt.show()

# BOXPLOT : distribution + outliers
plt.figure(figsize=(10, 5))
sns.boxplot(data=df, x='segment', y='revenus')
plt.title('Revenus par segment')
plt.show()

# VIOLINPLOT : boxplot + densité
plt.figure(figsize=(10, 5))
sns.violinplot(data=df, x='segment', y='age')
plt.title('Distribution des âges par segment')
plt.show()
```

---

### Relations

```python
# SCATTERPLOT avec couleur
plt.figure(figsize=(10, 6))
sns.scatterplot(data=df, x='age', y='revenus', hue='segment', alpha=0.6)
plt.title('Âge vs Revenus par segment')
plt.show()

# REGPLOT : scatter + régression linéaire
plt.figure(figsize=(10, 6))
sns.regplot(data=df, x='age', y='revenus', scatter_kws={'alpha':0.3})
plt.title('Régression âge / revenus')
plt.show()

# HEATMAP : matrice de corrélation
plt.figure(figsize=(8, 6))
corr_matrix = df[['age', 'revenus']].corr()
sns.heatmap(corr_matrix, annot=True, cmap='coolwarm', center=0, vmin=-1, vmax=1)
plt.title('Corrélations')
plt.show()

# PAIRPLOT : toutes les relations d'un coup
sns.pairplot(df, hue='segment', diag_kind='kde')
plt.show()
```

---

### Catégorielles

```python
# COUNTPLOT : fréquences
plt.figure(figsize=(8, 5))
sns.countplot(data=df, x='segment', palette='Set2')
plt.title('Nombre de clients par segment')
plt.show()

# BARPLOT : moyenne par catégorie
plt.figure(figsize=(8, 5))
sns.barplot(data=df, x='segment', y='revenus', estimator=np.mean, palette='Blues')
plt.title('Revenus moyens par segment')
plt.show()

# CATPLOT : graphiques par catégorie (flexible)
g = sns.catplot(data=df, x='segment', y='revenus', kind='box', height=5, aspect=1.5)
g.fig.suptitle('Distribution des revenus par segment', y=1.02)
plt.show()
```

---

### ✍️ Exercice 8.3 : Visualisation Seaborn (20 min)

```python
import pandas as pd
import numpy as np
import seaborn as sns
import matplotlib.pyplot as plt

# Données clients
np.random.seed(42)
n = 800
df = pd.DataFrame({
    'client_id': range(n),
    'age': np.random.randint(18, 70, n),
    'genre': np.random.choice(['Homme', 'Femme'], n),
    'region': np.random.choice(['Paris', 'Lyon', 'Marseille', 'Bordeaux'], n),
    'montant_annuel': np.random.exponential(1000, n),
    'nb_achats': np.random.randint(1, 50, n),
    'satisfaction': np.random.randint(1, 6, n)
})

sns.set_theme(style="whitegrid")

# 1. Distribution des montants avec KDE
plt.figure(figsize=(10, 5))
sns.histplot(data=df, x='_____', kde=True, bins=30)
plt.title('Distribution des montants annuels')
plt.show()

# 2. Boxplot montant par région
plt.figure(figsize=(10, 5))
sns.boxplot(data=df, x='_____', y='_____')
plt.title('Montants par région')
plt.show()

# 3. Scatter age vs montant avec couleur par genre
plt.figure(figsize=(10, 6))
sns.scatterplot(data=df, x='_____', y='_____', hue='_____', alpha=0.6)
plt.title('Âge vs Montant par genre')
plt.show()

# 4. Heatmap des corrélations numériques
plt.figure(figsize=(8, 6))
cols_num = ['age', 'montant_annuel', 'nb_achats', 'satisfaction']
sns.heatmap(df[cols_num].corr(), annot=True, cmap='coolwarm', center=0)
plt.title('Corrélations')
plt.show()

# 5. Satisfaction moyenne par région et genre
plt.figure(figsize=(10, 5))
sns.barplot(data=df, x='region', y='satisfaction', hue='genre')
plt.title('Satisfaction par région et genre')
plt.show()
```

---

> 💭 **Question Socratique #2** : Un boxplot montre que le segment "Premium" a une boîte plus haute que les autres segments. Cela signifie-t-il que les clients Premium dépensent plus EN MOYENNE ? Pourquoi ou pourquoi pas ?

---

## 8.4 Plotly : interactivité

### Pourquoi l'interactivité ?

- **Exploration** : Zoom, pan, hover pour les détails
- **Présentations** : Impressionner les stakeholders
- **Dashboards** : Utilisateurs peuvent explorer eux-mêmes

### Installation

```bash
pip install plotly
```

### Graphiques interactifs basiques

```python
import plotly.express as px

# LINE CHART interactif
fig = px.line(df, x='mois', y='ventes', title='Évolution des ventes')
fig.show()

# SCATTER interactif avec hover
fig = px.scatter(df, x='age', y='revenus', color='segment',
                 hover_data=['client_id', 'region'],
                 title='Âge vs Revenus')
fig.show()

# BAR CHART interactif
fig = px.bar(df, x='region', y='ventes', color='categorie',
             title='Ventes par région et catégorie')
fig.show()

# HISTOGRAM interactif
fig = px.histogram(df, x='age', nbins=30, title='Distribution des âges')
fig.show()
```

---

### Quand utiliser Plotly vs Matplotlib/Seaborn ?

| Critère | Matplotlib/Seaborn | Plotly |
|---------|-------------------|--------|
| Exploration personnelle | ✅ | ✅ |
| Publications/rapports statiques | ✅ | ○ |
| Présentations interactives | ○ | ✅ |
| Dashboards web | ○ | ✅ |
| Notebooks Jupyter | ✅ | ✅ |
| Apprentissage | ✅ (plus documenté) | ○ |

---

### ✍️ Exercice 8.4 : Graphique Plotly interactif (10 min)

```python
import plotly.express as px
import pandas as pd
import numpy as np

# Données
np.random.seed(42)
df = pd.DataFrame({
    'pays': np.random.choice(['France', 'Allemagne', 'Espagne', 'Italie', 'UK'], 200),
    'annee': np.random.choice([2022, 2023, 2024], 200),
    'ventes': np.random.randint(100, 1000, 200),
    'clients': np.random.randint(10, 100, 200)
})

# Créer un scatter interactif
fig = px.scatter(
    df,
    x='_____',           # Axe X : ventes
    y='_____',           # Axe Y : clients
    color='_____',       # Couleur : pays
    size='_____',        # Taille : ventes
    hover_data=['annee'],
    title='Ventes vs Clients par pays'
)
fig.show()

# Quels avantages voyez-vous par rapport à un scatter Matplotlib ?
# 1. _____
# 2. _____
```

---

## 8.5 Bonnes pratiques

### Lisibilité

```python
# Mauvais exemple
plt.plot(x, y)
plt.show()

# Bon exemple
plt.figure(figsize=(10, 6))
plt.plot(x, y, linewidth=2, color='#2c3e50', marker='o', markersize=6)
plt.title('Évolution des ventes mensuelles', fontsize=14, fontweight='bold')
plt.xlabel('Mois', fontsize=12)
plt.ylabel('Ventes (k€)', fontsize=12)
plt.xticks(fontsize=10)
plt.yticks(fontsize=10)
plt.grid(True, alpha=0.3)
plt.tight_layout()
plt.show()
```

---

### Accessibilité : les couleurs

```python
# Palette accessible (daltonisme)
# Éviter : rouge-vert ensemble
# Préférer : bleu-orange, ou utiliser des patterns

# Palette ColorBrewer (testé pour daltonisme)
colors_safe = ['#377eb8', '#ff7f00', '#4daf4a', '#984ea3', '#e41a1c']

# Palette Seaborn accessible
sns.set_palette("colorblind")
```

---

### Éviter les graphiques trompeurs

```python
# ❌ MAUVAIS : Axe Y tronqué
plt.figure(figsize=(8, 5))
plt.bar(['A', 'B', 'C'], [98, 99, 100])
plt.ylim(97, 101)  # Exagère les différences !
plt.title('Graphique TROMPEUR')
plt.show()

# ✅ BON : Axe Y complet
plt.figure(figsize=(8, 5))
plt.bar(['A', 'B', 'C'], [98, 99, 100])
plt.ylim(0, 110)  # Perspective honnête
plt.title('Graphique HONNÊTE')
plt.show()
```

**Règles d'or :**
- Commencer l'axe Y à 0 pour les bar charts
- Éviter les effets 3D inutiles
- Ne pas utiliser de double axe Y sans justification
- Toujours légender et titrer

---

### Storytelling visuel

```python
# Mettre en évidence un point clé
fig, ax = plt.subplots(figsize=(10, 6))
ax.plot(mois, ventes, 'o-', color='gray', alpha=0.5)
ax.plot(mois[-1], ventes[-1], 'o', color='red', markersize=15, label='Record Décembre')
ax.annotate('Record !', xy=(mois[-1], ventes[-1]), xytext=(10, 230),
            arrowprops=dict(arrowstyle='->', color='red'),
            fontsize=12, color='red')
ax.set_title('Les ventes atteignent un record en décembre', fontsize=14, fontweight='bold')
ax.legend()
plt.show()
```

---

### ✍️ Exercice 8.5 : Améliorer un graphique (15 min)

Améliorez ce graphique selon les bonnes pratiques :

```python
import matplotlib.pyplot as plt
import numpy as np

# Données
categories = ['Électronique', 'Vêtements', 'Alimentation', 'Maison', 'Sport']
ventes_2023 = [450, 380, 290, 420, 310]
ventes_2024 = [520, 410, 320, 480, 350]

# Graphique AVANT (à améliorer)
plt.bar(categories, ventes_2023)
plt.bar(categories, ventes_2024)
plt.show()

# Graphique APRÈS (votre version améliorée)
x = np.arange(len(categories))
width = 0.35

fig, ax = plt.subplots(figsize=(12, 6))

# Barres côte à côte
bars1 = ax.bar(x - width/2, ventes_2023, width, label='2023', color='#3498db')
bars2 = ax.bar(x + width/2, ventes_2024, width, label='2024', color='#e74c3c')

# Personnalisation
ax.set_title('_____', fontsize=14, fontweight='bold')
ax.set_xlabel('_____', fontsize=12)
ax.set_ylabel('_____', fontsize=12)
ax.set_xticks(x)
ax.set_xticklabels(categories, rotation=0)
ax.legend()
ax.grid(axis='y', alpha=0.3)

# Labels sur les barres
for bar in bars1:
    ax.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 10,
            f'{bar.get_height()}', ha='center', va='bottom', fontsize=9)
for bar in bars2:
    ax.text(bar.get_x() + bar.get_width()/2, bar.get_height() + 10,
            f'{bar.get_height()}', ha='center', va='bottom', fontsize=9)

plt.tight_layout()
plt.show()
```

---

## 8.6 Préparation pour Power BI

### Format des données pour Power BI

Power BI préfère les données en format **long** (tidy) :

```python
# Format WIDE (difficile pour Power BI)
#   Client  Jan  Fev  Mar
#   Alice   100  150  120

# Format LONG (idéal pour Power BI)
#   Client  Mois  Ventes
#   Alice   Jan   100
#   Alice   Fev   150
#   Alice   Mar   120

# Conversion avec melt
df_long = pd.melt(df_wide, id_vars=['Client'], var_name='Mois', value_name='Ventes')
```

---

### Export pour Power BI

```python
# CSV (universel)
df.to_csv('data_pour_powerbi.csv', index=False, encoding='utf-8-sig')

# Excel (avec onglets multiples)
with pd.ExcelWriter('data_powerbi.xlsx') as writer:
    df_ventes.to_excel(writer, sheet_name='Ventes', index=False)
    df_clients.to_excel(writer, sheet_name='Clients', index=False)
    df_produits.to_excel(writer, sheet_name='Produits', index=False)
```

---

## 🧠 Réflexion métacognitive

### Auto-évaluation

| Compétence | 1 | 2 | 3 | 4 | 5 |
|------------|---|---|---|---|---|
| Je sais choisir le bon type de graphique | ○ | ○ | ○ | ○ | ○ |
| Je maîtrise les graphiques de base Matplotlib | ○ | ○ | ○ | ○ | ○ |
| Je peux créer des visualisations statistiques Seaborn | ○ | ○ | ○ | ○ | ○ |
| Je connais les bonnes pratiques de visualisation | ○ | ○ | ○ | ○ | ○ |
| Je peux préparer des données pour Power BI | ○ | ○ | ○ | ○ | ○ |

### Questions de réflexion

1. **Quel type de graphique** vous semble le plus utile dans votre futur métier ?

2. **Comment éviteriez-vous** de créer des graphiques trompeurs ?

3. **Quelle est la différence** entre "explorer" et "communiquer" avec une visualisation ?

---

## 📚 Résumé du chapitre

### Points clés à retenir

1. **Choisir le bon graphique** :
   - Distribution → Histogramme, Boxplot
   - Relation → Scatter, Heatmap
   - Évolution → Line chart
   - Comparaison → Bar chart

2. **Matplotlib** :
   - `plt.plot()`, `plt.bar()`, `plt.scatter()`, `plt.hist()`
   - `plt.subplots()` pour les dashboards
   - Personnalisation : titres, labels, couleurs, grille

3. **Seaborn** :
   - Plus esthétique par défaut
   - `histplot`, `boxplot`, `violinplot` pour distributions
   - `scatterplot`, `heatmap` pour relations
   - `barplot`, `countplot` pour catégories

4. **Bonnes pratiques** :
   - Toujours titrer et légender
   - Commencer l'axe Y à 0 (bar charts)
   - Utiliser des couleurs accessibles
   - Éviter les graphiques 3D inutiles

---

## ➡️ Prochain chapitre

**Chapitre 9 : Chargement vers le cloud** — Vous apprendrez à écrire vos données transformées vers S3.

---

*Module 2 — Pipeline Data | Chapitre 8 sur 11*
