# Chapitre 6 — Structuration et transformation

**Durée estimée : 8-10 heures**

---

## Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

1. **Restructurer** des DataFrames avec pivot et melt selon les besoins d'analyse
2. **Combiner** des sources de données avec merge et concat en choisissant la bonne méthode
3. **Agréger** des données avec groupby et créer des statistiques par groupe
4. **Créer** de nouvelles variables pertinentes (feature engineering) pour l'analyse et le ML

---

## 🧠 Réflexion métacognitive

### Auto-évaluation

| Compétence | 1 | 2 | 3 | 4 | 5 |
|------------|---|---|---|---|---|
| Je sais utiliser pivot et melt | ○ | ○ | ○ | ○ | ○ |
| Je maîtrise les différents types de merge | ○ | ○ | ○ | ○ | ○ |
| Je peux créer des agrégations avec groupby | ○ | ○ | ○ | ○ | ○ |
| Je sais créer des features temporelles | ○ | ○ | ○ | ○ | ○ |
| Je peux encoder des variables catégorielles | ○ | ○ | ○ | ○ | ○ |

### Questions de réflexion

1. **Quel type de merge** utilisez-vous le plus souvent dans votre pratique ?

2. **Quelles features** créeriez-vous à partir d'une date de naissance ?

3. **Comment décideriez-vous** entre one-hot encoding et label encoding ?

---

## 📚 Résumé du chapitre

### Points clés à retenir

1. **Restructuration** :
   - `pivot` : Long → Wide (pour reporting)
   - `melt` : Wide → Long (pour visualisation)

2. **Combinaison** :
   - `merge` : Jointure sur clé (inner, left, right, outer)
   - `concat` : Empilement (vertical ou horizontal)

3. **Agrégation** :
   - `groupby` + `agg` pour statistiques par groupe
   - Fonctions : sum, mean, count, nunique...

4. **Feature Engineering** :
   - Variables calculées (ratios, différences)
   - Variables temporelles (année, mois, jour_semaine)
   - Binning (cut, qcut)
   - Encoding (get_dummies, cat.codes)

---

## ➡️ Prochain chapitre

**Chapitre 7 : EDA analytique** — Vous apprendrez à comprendre vos données propres, identifier des patterns et formuler des hypothèses.

---

*Module 2 — Pipeline Data | Chapitre 6 sur 11*
