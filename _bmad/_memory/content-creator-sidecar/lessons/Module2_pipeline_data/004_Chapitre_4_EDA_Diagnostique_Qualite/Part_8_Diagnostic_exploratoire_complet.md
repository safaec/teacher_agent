# Chapitre 4 — EDA diagnostique et qualité des données

**Durée estimée : 8-10 heures**

---

## Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

1. **Distinguer** l'EDA diagnostique (trouver les problèmes) de l'EDA analytique (comprendre les patterns)
2. **Évaluer** la qualité des données selon les 6 dimensions standardisées
3. **Détecter** les valeurs manquantes, doublons et outliers avec les outils pandas appropriés
4. **Documenter** les problèmes identifiés dans une checklist de qualité priorisée

---

## 4.8 Diagnostic exploratoire complet

⸻

**1️⃣ Étape 0 : préparation**

- Charger le dataset, importer les bibliothèques nécessaires :
- Objectif : avoir le dataset prêt à l’inspection.
- Vérifier le type de chaque colonne : df.info()

⸻

**2️⃣ Étape 1 : statistiques descriptives (code)**

Objectif : comprendre les distributions, valeurs min/max, médiane, moyenne, etc.

- Pour les colonnes numériques : df.describe()  
 moyenne, mediane, min, max, std, quartiles
- Pour les colonnes catégorielles : df['categorie'].value_counts()  
 fréquence de chaque catégorie

💡 Pourquoi en code avant graphique ?

- Tu repères les anomalies extrêmes et décides quelles visualisations seront pertinentes.
- Par exemple, si un âge = 150, tu sais que tu veux un scatterplot ou boxplot pour voir sa position.

⸻

**3️⃣ Étape 2 : exactitude / validité (code + parfois visuel)**

Objectif : vérifier que les valeurs sont réalistes et valides.

- Exemples : âge > 0 et < 120, salaire positif, date de naissance correcte

Visualisation utile : histogrammes ou scatterplots pour détecter valeurs aberrantes

⸻

**4️⃣ Étape 3 : unicité / doublons (code)**

- Vérifier si certaines lignes sont dupliquées
- Pas besoin de graphique ici : l’info est exacte et claire en tableau.

⸻

**5️⃣ Étape 4 : valeurs manquantes (code + visualisation)**

- Détecter les NaN : df.isna().sum()  
 nombre de valeurs manquantes par colonne
- Visualisation recommandée : heatmap pour voir la distribution des valeurs manquantes

⸻

**6️⃣ Étape 5 : cohérence (code + visuel)**

- Vérifier si les relations logiques entre colonnes sont respectées :
  - Exemple : age > 18 si categorie = 'adulte'
  - salaire > 0 pour toutes les catégories

- Visualisation utile : scatterplot ou boxplot par catégorie pour détecter incohérences ou outliers dans les sous-groupes

⸻

**7️⃣ Étape 6 : outliers (code + visuel)**

- Détecter valeurs extrêmes pour les numériques : IQR method ou zscore
- Visualisation : boxplot ou scatterplot pour voir leur position

⸻

**8️⃣ Étape 7 : fraîcheur (si applicable, code)**

- Vérifier si les dates sont récentes ou cohérentes :
  - df['date'].max()
  - df['date'].min()

- Visualisation possible : histogramme ou lineplot pour voir la répartition dans le temps : sns.histplot(df['date'], bins=20)

⸻

**✅ Ordre logique résumé**

1. Statistiques descriptives → code (describe, value_counts)
2. Exactitude / validité → code + histogrammes /
3. Unicité / doublons → code
4. Valeurs manquantes → code + heatmap
5. Cohérence entre colonnes → code + scatterplots / boxplots par catégorie
6. Outliers numériques → code + boxplots / scatterplots
7. Fraîcheur / temporalité → code + histogrammes

⸻

**Règles pratiques pour le diagnostic**

- Toujours commencer par code pour identifier les problèmes précis
- Puis utiliser visualisations pour :
- Voir l’étendue / la dispersion
- Identifier les patterns ou anomalies difficiles à voir en tableau
- Ne jamais nettoyer ou corriger avant d’avoir terminé le diagnostic, sinon tu risques de masquer des anomalies importantes
