# QCM - Chapitre 6 : Structuration et Transformation des Données

---

## Questions à Choix Multiples

### Question 1 : Format Wide vs Long

Quel format de données est généralement préféré par les bibliothèques de visualisation comme Seaborn ?

- [ ] A) Le format Wide car il est plus compact
- [ ] B) Le format Long car il permet de mapper facilement les variables aux axes visuels
- [ ] C) Les deux formats sont équivalents pour la visualisation
- [ ] D) Le format Wide car les colonnes correspondent aux catégories

<details>
<summary>Voir la réponse</summary>

**Réponse : B**

Le format Long est préféré car il permet de mapper une variable → axe X, une variable → axe Y, une variable → couleur (hue), etc. Cette logique est beaucoup plus difficile à implémenter avec le format Wide.
</details>

---

### Question 2 : Pivot vs Melt

Quelle opération permet de transformer un DataFrame du format Wide vers le format Long ?

- [ ] A) `pivot()`
- [ ] B) `melt()`
- [ ] C) `pivot_table()`
- [ ] D) `unstack()`

<details>
<summary>Voir la réponse</summary>

**Réponse : B**

`melt()` transforme Wide → Long en "fondant" plusieurs colonnes en une seule colonne de variables et une colonne de valeurs. `pivot()` fait l'inverse : Long → Wide.
</details>

---

### Question 3 : Merge vs Concat

Quelle est la principale différence entre `merge()` et `concat()` ?

- [ ] A) `merge()` est plus rapide que `concat()`
- [ ] B) `merge()` joint sur une clé commune, `concat()` empile sans condition
- [ ] C) `concat()` ne fonctionne qu'avec deux DataFrames
- [ ] D) `merge()` ne peut joindre que sur une seule colonne

<details>
<summary>Voir la réponse</summary>

**Réponse : B**

`merge()` effectue une jointure basée sur une ou plusieurs colonnes clés (comme un JOIN SQL), tandis que `concat()` empile simplement les DataFrames (comme un UNION SQL).
</details>

---

### Question 4 : GroupBy - Agrégations

Quel code permet de calculer la somme ET la moyenne des ventes par région ?

- [ ] A) `df.groupby('region')['ventes'].sum().mean()`
- [ ] B) `df.groupby('region')['ventes'].agg(['sum', 'mean'])`
- [ ] C) `df.groupby('region').agg('sum', 'mean')`
- [ ] D) `df.groupby('region')['ventes'].apply(['sum', 'mean'])`

<details>
<summary>Voir la réponse</summary>

**Réponse : B**

La méthode `.agg()` avec une liste de fonctions permet d'appliquer plusieurs agrégations simultanément sur une colonne.
</details>

---

### Question 5 : Binning

Quelle est la différence entre `pd.cut()` et `pd.qcut()` ?

- [ ] A) `cut()` crée des intervalles de taille égale, `qcut()` crée des groupes avec le même nombre d'éléments
- [ ] B) `cut()` est pour les données numériques, `qcut()` pour les catégorielles
- [ ] C) `qcut()` est plus rapide que `cut()`
- [ ] D) Il n'y a pas de différence significative

<details>
<summary>Voir la réponse</summary>

**Réponse : A**

`pd.cut()` découpe en intervalles de taille égale (ex: 0-25, 25-50, 50-75, 75-100), tandis que `pd.qcut()` découpe en quantiles pour avoir approximativement le même nombre d'observations dans chaque groupe.
</details>

---

### Question 6 : One-Hot Encoding

Quand ne PAS utiliser le One-Hot Encoding ?

- [ ] A) Pour les variables nominales (sans ordre)
- [ ] B) Pour les variables ordinales (avec un ordre logique)
- [ ] C) Quand on a peu de catégories
- [ ] D) Pour les algorithmes de Machine Learning

<details>
<summary>Voir la réponse</summary>

**Réponse : B**

Pour les variables ordinales (comme Basic < Standard < Premium), le Label Encoding est plus approprié car il préserve l'ordre naturel. Le One-Hot Encoding perdrait cette information d'ordre.
</details>

---

## Questions à Réponse Libre

### Question 7 : Cas pratique - Jointures multiples

Vous avez trois tables : `clients`, `commandes`, et `produits`.

**Décrivez les étapes pour créer un DataFrame final contenant : nom du client, date de commande, nom du produit, et montant total (quantité × prix).**

<details>
<summary>Voir une solution</summary>

```python
# Étape 1 : Joindre commandes et clients
df = pd.merge(commandes, clients, on='client_id', how='left')

# Étape 2 : Joindre avec produits
df = pd.merge(df, produits, on='produit_id', how='left')

# Étape 3 : Calculer le montant total
df['montant_total'] = df['quantite'] * df['prix']

# Sélectionner les colonnes finales
resultat = df[['nom', 'date_commande', 'nom_produit', 'montant_total']]
```

Ou en chaînant les opérations :

```python
df = (
    commandes
    .merge(clients, on='client_id', how='left')
    .merge(produits, on='produit_id', how='left')
    .assign(montant_total=lambda x: x['quantite'] * x['prix'])
)
```

</details>

---

### Question 8 : Feature Engineering

À partir d'une colonne `date_naissance`, listez au moins 5 features que vous pourriez créer pour une analyse client.

<details>
<summary>Voir une réponse attendue</summary>

Features possibles :

1. **Âge** : `(datetime.now() - date_naissance).dt.days // 365`
2. **Tranche d'âge** : 18-25, 26-35, 36-50, 51-65, 65+
3. **Génération** : Baby Boomer, Gen X, Millennial, Gen Z
4. **Mois de naissance** : pour analyses saisonnières
5. **Signe astrologique** : pour certaines analyses marketing
6. **Est majeur** : variable binaire (âge >= 18)
7. **Est senior** : variable binaire (âge >= 60)
8. **Décennie de naissance** : 1960s, 1970s, 1980s...
9. **Anniversaire ce mois** : pour ciblage marketing
10. **Jour de la semaine de naissance** : pour analyses comportementales

</details>

---

## Questions de Réflexion - Cas Réels

### Question 9 : Reporting mensuel

Votre manager vous demande un tableau de reporting des ventes mensuelles par région. Les données sont actuellement en format Long (une ligne par transaction).

**Données actuelles (Format Long) :**

```
┌────────┬───────┬────────┐
│ Region │ Mois  │ Ventes │
├────────┼───────┼────────┤
│ Nord   │ Jan   │ 1200   │
│ Nord   │ Fev   │ 1350   │
│ Nord   │ Mar   │ 1100   │
│ Sud    │ Jan   │ 980    │
│ Sud    │ Fev   │ 1050   │
│ Sud    │ Mar   │ 1200   │
│ Est    │ Jan   │ 850    │
│ ...    │ ...   │ ...    │
└────────┴───────┴────────┘
```

**Quel format de données allez-vous lui fournir et pourquoi ?**

<details>
<summary>Voir la réponse attendue</summary>

Le **format Wide** est préférable pour un reporting destiné au management :

**Format Wide (à fournir) :**

```
┌────────┬───────┬───────┬───────┬─────────┐
│ Region │  Jan  │  Fev  │  Mar  │  Total  │
├────────┼───────┼───────┼───────┼─────────┤
│ Nord   │ 1200  │ 1350  │ 1100  │  3650   │
│ Sud    │  980  │ 1050  │ 1200  │  3230   │
│ Est    │  850  │  920  │  880  │  2650   │
│ Ouest  │ 1100  │ 1180  │ 1250  │  3530   │
├────────┼───────┼───────┼───────┼─────────┤
│ Total  │ 4130  │ 4500  │ 4430  │ 13060   │
└────────┴───────┴───────┴───────┴─────────┘
```

**Pourquoi ce format ?**

- Plus lisible dans Excel ou PowerPoint
- Permet une lecture rapide des comparaisons entre périodes
- Les régions en lignes, les mois en colonnes
- Possibilité d'ajouter une colonne "Total" facilement
- Format habituel pour les tableaux de bord managériaux

Le format Long serait gardé en interne pour les analyses et visualisations.
</details>

---

### Question 10 : Fusion de données après acquisition

Votre entreprise vient de racheter un concurrent. Vous devez fusionner les bases clients des deux entreprises. Les deux bases ont des structures différentes et aucun identifiant commun.

**Quelles sont les étapes clés de votre approche ?**

<details>
<summary>Voir la réponse attendue</summary>

1. **Audit des deux bases** : comprendre les colonnes disponibles dans chaque système
2. **Identifier les attributs communs** : email, téléphone, nom + adresse, etc.
3. **Définir des règles de dédoublonnage** : que faire si un client existe dans les deux bases ?
4. **Choisir un identifiant maître** : créer un nouvel ID unique ou utiliser l'un des systèmes comme référence
5. **Traiter les conflits** : si les informations diffèrent (ex: deux adresses différentes), quelle source prime ?
6. **Documenter les décisions** : garder une trace des règles appliquées
7. **Valider avec le métier** : faire vérifier un échantillon par les équipes commerciales

</details>

---

### Question 11 : Choix de la granularité d'agrégation

Vous devez analyser les performances commerciales. Votre base contient les transactions individuelles. Le directeur commercial veut voir les résultats par vendeur, le directeur régional par région, et le DAF par mois.

**Comment structurez-vous votre travail ?**

<details>
<summary>Voir la réponse attendue</summary>

**Approche recommandée :**

1. **Garder les données brutes** comme source de vérité
2. **Créer plusieurs vues agrégées** selon les besoins :
   - Vue par vendeur (groupby vendeur)
   - Vue par région (groupby région)
   - Vue par mois (groupby mois)
3. **Permettre le croisement** : certains voudront région × mois
4. **Automatiser** : créer un script/notebook qui génère toutes les vues
5. **Documenter les règles** : comment sont calculés les indicateurs ?

Ne pas créer une seule agrégation "définitive" - chaque destinataire a des besoins différents.
</details>

---

### Question 12 : Feature engineering pour prédiction de churn

Vous devez créer des features pour un modèle de prédiction de désabonnement (churn). Vous avez accès à l'historique des commandes, les données clients, et les interactions support.

**Quelles catégories de features envisagez-vous ?**

<details>
<summary>Voir la réponse attendue</summary>

**Features comportementales (commandes) :**

- Fréquence d'achat (commandes/mois)
- Récence (jours depuis dernière commande)
- Montant moyen du panier
- Évolution du montant (tendance à la hausse/baisse)
- Diversité des catégories achetées

**Features temporelles :**

- Ancienneté du client
- Saisonnalité des achats
- Jours entre les commandes (régularité)

**Features d'engagement (support) :**

- Nombre de tickets support
- Temps de résolution moyen
- Nombre de réclamations
- Note de satisfaction (si disponible)

**Features démographiques :**

- Segment client
- Canal d'acquisition
- Localisation géographique

L'idée est de **capturer des signaux** de désengagement avant qu'il ne soit trop tard.
</details>

---

### Question 13 : Explosion du nombre de colonnes

Après un One-Hot Encoding de la colonne "ville" (500 villes différentes), votre DataFrame passe de 10 à 510 colonnes.

**Est-ce un problème ? Que faites-vous ?**

<details>
<summary>Voir la réponse attendue</summary>

**Oui, c'est potentiellement un problème :**

- Curse of dimensionality (malédiction de la dimension)
- Temps de calcul plus long
- Risque d'overfitting pour les modèles ML
- Beaucoup de colonnes avec très peu de 1 (sparse)

**Solutions possibles :**

1. **Regrouper** : passer de 500 villes à 10-20 régions
2. **Target encoding** : garder que la ville cible
3. **Garder le top N** : encoder uniquement les 20 villes les plus fréquentes, le reste = "Autre"
4. **Ne pas encoder** : certains algorithmes (arbres) gèrent les catégories nativement

</details>

---

### Question 14 : Données en temps réel vs batch

Votre système actuel traite les données en batch chaque nuit. Le marketing demande des tableaux de bord mis à jour en temps réel.

**Quels sont les impacts sur votre pipeline de transformation ?**

<details>
<summary>Voir la réponse attendue</summary>

**Changements à anticiper :**

1. **Architecture** : passer d'un traitement batch à du streaming (ou micro-batch)

2. **Agrégations** :
   - Les groupby complets ne sont plus possibles
   - Accepter des approximations

3. **Jointures** :
   - Les tables de référence doivent être accessibles en temps réel
   - Prévoir du cache pour éviter des requêtes constantes

4. **Feature engineering** :
   - Certaines features (ex: moyenne sur 30 jours) doivent être précalculées
   - Stockage d'état nécessaire

5. **Qualité des données** :
   - Moins de temps pour valider
   - Prévoir des alertes automatiques

**Question à poser au métier** : "temps réel" signifie quoi exactement ? (à la seconde, à l'heure, toutes les 15 min ?)
</details>

---

### Question 15 : Confidentialité et agrégation

Vous devez partager des statistiques de ventes avec un partenaire externe, mais les données individuelles sont confidentielles.

**Comment procédez-vous ?**

<details>
<summary>Voir la réponse attendue</summary>

**Règles de base :**

1. **Agréger suffisamment** : pas de groupe avec moins de 5-10 individus
2. **Supprimer les identifiants** : pas de client_id, email, etc.
3. **Attention aux quasi-identifiants** : combinaison ville + âge + métier peut être identifiante
4. **Arrondir les valeurs** : éviter les montants exacts qui permettraient de remonter à une transaction

**Bonnes pratiques :**

- Définir un seuil minimum d'agrégation (ex: min 10 transactions par cellule)
- Remplacer les petits groupes par "Autres" ou NaN
- Faire valider par le DPO/juridique
- Documenter ce qui a été partagé et avec qui

**Exemple** : Au lieu de "Ventes par ville par jour", partager "Ventes par région par mois"
</details>

---

### Question 16 : Détection d'anomalies après transformation

Après avoir créé une feature "panier_moyen = montant_total / nb_commandes", vous observez des valeurs de 50 000€.

**Comment réagissez-vous ?**

<details>
<summary>Voir la réponse attendue</summary>

**Étapes de diagnostic :**

1. **Ne pas supprimer immédiatement** : c'est peut-être une vraie valeur
2. **Investiguer la source** :
   - Client B2B avec grosses commandes ?
   - Erreur de saisie (virgule mal placée) ?
   - Bug dans le calcul (division par 1 commande avec gros montant) ?
3. **Regarder la distribution** : combien de valeurs extrêmes ?
4. **Vérifier les données brutes** : remonter à la transaction d'origine

**Actions possibles selon le diagnostic :**

- Si erreur de saisie → corriger à la source
- Si client B2B légitime → créer un segment séparé ou transformer (log)
- Si outlier isolé → décider de l'exclure ou non selon l'usage

**Leçon** : Toujours explorer les valeurs extrêmes après feature engineering
</details>

---
