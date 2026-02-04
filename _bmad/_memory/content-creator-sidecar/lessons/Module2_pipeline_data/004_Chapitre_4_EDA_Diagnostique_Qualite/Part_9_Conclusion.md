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

## QCM – Diagnostic exploratoire et visualisation

1️⃣ Quand commence-t-on le diagnostic exploratoire sur un dataset ?

A. Après avoir nettoyé toutes les valeurs manquantes

B. Avant toute modification ou nettoyage des données ✅

C. Une fois les outliers supprimés

D. Après avoir créé tous les graphiques

Réponse : B

⸻

2️⃣ Quelle fonction permet de détecter les valeurs manquantes dans un dataframe pandas ?

A. df.isna() ✅
B. df.head()
C. df.duplicated()
D. df.describe()

Réponse : A

⸻

3️⃣ Pourquoi préfère-t-on utiliser fig, ax = plt.subplots() plutôt que plt.plot() directement ?

A. Pour écrire moins de code

B. Pour pouvoir stocker et modifier les graphiques plus facilement ✅

C. Parce que plt.plot() ne fonctionne pas sur les datasets pandas

D. Pour colorer automatiquement les points

Réponse : B

⸻

4️⃣ Quelle est la différence entre histogramme (hist) et bar chart (bar) ?

A. Histogramme pour valeurs continues, bar chart pour valeurs catégorielles ✅

B. Histogramme pour valeurs catégorielles, bar chart pour valeurs continues

C. Histogramme affiche des lignes, bar chart des points

D. Aucune différence, c’est interchangeable

Réponse : A

⸻

5️⃣ Dans une heatmap de valeurs manquantes, pourquoi l’axe va de 0 à 1 ?

A. Parce que matplotlib normalise automatiquement toutes les colonnes

B. Parce que les booléens True/False sont convertis en 1/0 ✅

C. Parce que l’échelle est proportionnelle à la moyenne des colonnes

D. Parce que Seaborn ne supporte pas d’autres échelles

Réponse : B

⸻

6️⃣ Comment repérer les outliers dans un diagnostic exploratoire ?

A. Seulement avec des bar charts

B. Avec boxplots, scatterplots ou méthodes IQR/Z-score ✅

C. En regardant les valeurs manquantes

D. En calculant uniquement la moyenne

Réponse : B

⸻

7️⃣ Pour visualiser la relation entre deux variables numériques et une variable catégorielle, quel graphique est le plus adapté ?

A. Histogramme

B. Boxplot

C. Scatterplot avec hue pour la catégorie ✅

D. Bar chart

Réponse : C

⸻

8️⃣ Quelle est la bonne façon de tracer un scatterplot par tranches d’âge ?

A. Créer une colonne catégorielle avec pd.cut puis utiliser hue ✅

B. Modifier les valeurs d’âge pour les rendre toutes continues

C. Utiliser plt.scatter() directement sans modifier le dataset

D. Utiliser df.describe()

Réponse : A

⸻

9️⃣ Quelle étape fait partie du diagnostic exploratoire mais pas du nettoyage ?

A. Supprimer les valeurs manquantes
B. Remplacer les outliers

C. Identifier les doublons et les anomalies ✅

D. Normaliser les colonnes

Réponse : C

⸻

🔟 Quel est l’ordre logique pour un diagnostic complet d’un dataset ?

A. Nettoyage → visualisation → code d’inspection → analyse

B. Code d’inspection → plan des visualisations → graphiques → analyse ✅

C. Visualisation → suppression des outliers → statistiques descriptives

D. Scatterplots → histogrammes → heatmap → boxplots

Réponse : B

⸻

💡 Astuce pour le QCM :
 • Tout ce qu’on a appris tourne autour de diagnostic avant nettoyage, fig/ax pour le contrôle des graphiques, choix du type de graphique selon le type de variable, et repérage des anomalies / NaN / outliers.

---

## 🧠 Réflexion métacognitive

### Auto-évaluation

| Compétence | 1 | 2 | 3 | 4 | 5 |
|------------|---|---|---|---|---|
| Je distingue EDA diagnostique et analytique | ○ | ○ | ○ | ○ | ○ |
| Je connais les 6 dimensions de qualité | ○ | ○ | ○ | ○ | ○ |
| Je sais détecter les valeurs manquantes | ○ | ○ | ○ | ○ | ○ |
| Je sais identifier les doublons | ○ | ○ | ○ | ○ | ○ |
| Je sais détecter les outliers (IQR, Z-score) | ○ | ○ | ○ | ○ | ○ |
| Je peux créer un rapport de qualité priorisé | ○ | ○ | ○ | ○ | ○ |

### Questions de réflexion

1. **Quelle dimension de qualité** vous semble la plus difficile à évaluer ? Pourquoi ?

2. **Comment expliqueriez-vous** l'importance de l'EDA diagnostique à un manager non-technique ?

3. **Dans votre futur métier**, quel type de problème de qualité pensez-vous rencontrer le plus souvent ?

---

## 📚 Résumé du chapitre

### Points clés à retenir

1. **EDA Diagnostique ≠ Analytique** :
   - Diagnostique = trouver les problèmes (avant nettoyage)
   - Analytique = comprendre les patterns (après nettoyage)

2. **6 dimensions de qualité** :
   - Complétude, Unicité, Cohérence
   - Exactitude, Validité, Fraîcheur

3. **Outils de détection** :
   - Missing : `isnull()`, `missingno`
   - Doublons : `duplicated()`
   - Outliers : IQR (robuste), Z-score (distributions normales)

4. **Documentation** :
   - Toujours créer une checklist de qualité
   - Prioriser : Haute > Moyenne > Basse

---

## 🔗 Sources et références

- [IBM - Data Quality Dimensions](https://www.ibm.com/think/topics/data-quality-dimensions)
- [Atlan - Data Quality Dimensions 2025](https://atlan.com/data-quality-dimensions/)
- [Monte Carlo - 6 Data Quality Dimensions](https://www.montecarlodata.com/blog-6-data-quality-dimensions-examples/)
- [dbt Labs - Data Quality Dimensions](https://www.getdbt.com/blog/data-quality-dimensions)

---

## ➡️ Prochain chapitre

**Chapitre 5 : Nettoyage des données** — Vous apprendrez à corriger les problèmes identifiés : traiter les valeurs manquantes, supprimer les doublons, et gérer les outliers.

---

*Module 2 — Pipeline Data | Chapitre 4 sur 11*
