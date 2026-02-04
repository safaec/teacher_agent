
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

## 4.2 Les 6 dimensions de la qualité des données

### Framework standardisé

La qualité des données s'évalue selon **6 dimensions principales** reconnues par l'industrie :

```
┌─────────────────────────────────────────────────────────────────────┐
│                    DIMENSIONS DE QUALITÉ                            │
├─────────────────┬─────────────────┬─────────────────────────────────┤
│  COMPLÉTUDE     │   UNICITÉ       │         COHÉRENCE               │
│  (Completeness) │   (Uniqueness)  │       (Consistency)             │
│                 │                 │                                 │
│  Toutes les     │  Pas de         │  Mêmes valeurs dans             │
│  données sont   │  doublons       │  différents systèmes            │
│  présentes ?    │                 │                                 │
├─────────────────┼─────────────────┼─────────────────────────────────┤
│  EXACTITUDE     │   VALIDITÉ      │         FRAÎCHEUR               │
│  (Accuracy)     │   (Validity)    │       (Timeliness)              │
│                 │                 │                                 │
│  Les données    │  Format et      │  Données à jour,                │
│  reflètent la   │  règles         │  pas obsolètes                  │
│  réalité ?      │  respectés ?    │                                 │
└─────────────────┴─────────────────┴─────────────────────────────────┘
```

*(Source : [IBM - Data Quality Dimensions](https://www.ibm.com/think/topics/data-quality-dimensions))*

### 1. Complétude (Completeness)

**Question :** Toutes les données requises sont-elles présentes ?

**Exemples de problèmes :**

- Client sans adresse email
- Commande sans date de livraison
- Produit sans prix

**Métrique :** `% de valeurs non-nulles`

### 2. Unicité (Uniqueness)

**Question :** Y a-t-il des enregistrements en double ?

**Exemples de problèmes :**

- Même client enregistré 3 fois
- Même transaction comptée deux fois
- Doublons dus à des imports multiples

**Métrique :** `% d'enregistrements uniques`

### 3. Cohérence (Consistency)

**Question :** Les données sont-elles logiquement cohérentes ?

**Exemples de problèmes :**

- Date de naissance > date d'inscription
- Ville "Paris" avec code postal "69000" (Lyon)
- Total ≠ somme des lignes

**Métrique :** `% de règles de cohérence respectées`

### 4. Exactitude (Accuracy)

**Question :** Les données reflètent-elles la réalité ?

**Exemples de problèmes :**

- Âge de 150 ans (erreur de saisie)
- Température de -100°C à Paris
- Salaire négatif

**Métrique :** `% de valeurs dans les plages attendues`

### 5. Validité (Validity)

**Question :** Les données respectent-elles le format attendu ?

**Exemples de problèmes :**

- Email sans "@"
- Code postal avec des lettres
- Date au format "31/13/2024"

**Métrique :** `% de valeurs conformes au format`

### 6. Fraîcheur (Timeliness)

**Question :** Les données sont-elles à jour ?

**Exemples de problèmes :**

- Adresse d'un client déménagé il y a 2 ans
- Prix catalogue de 2019
- Statut de commande non mis à jour

**Métrique :** `Âge moyen des données`

### ✍️ Exercice 4.2 : Identifier les dimensions (10 min)

Pour chaque problème, identifiez la dimension de qualité concernée :

| Problème | Dimension | Justification |
|----------|-----------|---------------|
| 15% des clients n'ont pas de numéro de téléphone | Complétude | La donnée attendue est absente pour une partie des enregistrements |
| Le même produit apparaît 3 fois avec des prix différents | Unicité | Une entité censée être unique est dupliquée dans le jeu de données |
| Un client a une date de naissance en 2050 | Exactitude | La valeur ne respecte pas les règles de domaine (date future impossible) |
| Le code postal contient des caractères spéciaux | Validité | Le format ne respecte pas les contraintes définies pour ce champ |
| L'adresse d'un entrepôt fermé depuis 2 ans est encore active | Fraîcheur | La donnée est obsolète par rapport à la réalité actuelle |
| Le total de la facture ne correspond pas à la somme des lignes | Cohérence | Incohérence logique entre plusieurs champs liés |

> 💭 **Question Socratique #2** : Une entreprise peut-elle avoir des données 100% complètes mais de mauvaise qualité ? Donnez un exemple concret.

> 💭 **Réponse possible** :

Oui, absolument.
La complétude ne garantit en rien la qualité globale des données.

🔍 Pourquoi ?

La complétude répond uniquement à la question : « Est-ce que tous les champs sont remplis ? »

Elle ne dit rien sur :

 • la justesse des valeurs (exactitude),
 • le respect des règles métier (validité),
 • la cohérence entre champs,
 • l’actualité des informations,
 • l’unicité des entités.

🧪 Exemple concret (très parlant)

Base clients d’une banque

 • Tous les clients ont :
 • un numéro de téléphone ✅
 • une adresse email ✅
 • une date de naissance ✅
👉 Complétude = 100%

Mais :

 • 40% des numéros de téléphone sont "0000000000"
 • 25% des emails sont "test@test.com"
 • Certains clients ont une date de naissance en 1900 ou 2028

➡️ Les données sont :

 • complètes ❌ mais
 • inexactes, invalides et inexploitables

🎯 Message clé

“Rempli” ≠ “Correct” ≠ “Utile”

Une entreprise peut donc :

 • avoir des KPI de complétude parfaits
 • et pourtant prendre de mauvaises décisions basées sur de mauvaises données

C’est pour cette raison que beaucoup d’entreprises :

 • optimisent d’abord la complétude (facile à mesurer)
 • mais échouent sur les dimensions plus difficiles comme l’exactitude ou la cohérence
