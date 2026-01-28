# Leçon 1.2 : Les Types d'Apprentissage en Machine Learning

## Objectifs pédagogiques

À la fin de cette leçon, vous serez capable de :

- **Distinguer** les trois paradigmes d'apprentissage : supervisé, non-supervisé, et par renforcement
- **Identifier** quel type d'apprentissage utiliser selon le problème posé
- **Reconnaître** des exemples concrets de chaque type dans le monde réel

---

## 🎯 Accroche : Comment Netflix sait ce que vous voulez regarder

Netflix possède plus de **260 millions d'abonnés** dans le monde. Chaque utilisateur voit une page d'accueil différente, personnalisée selon ses goûts. Magie ? Non, **Machine Learning**.

Les chiffres sont impressionnants :

- **80%** du contenu regardé sur Netflix provient des recommandations algorithmiques
- Le système de recommandation fait économiser **1 milliard de dollars par an** à Netflix en réduisant les désabonnements
- Netflix traite **plusieurs téraoctets** de données d'interaction par jour

> *Source : [Head of AI — Netflix Case Study](https://headofai.ai/ai-industry-case-studies/netflixs-ai-personalization-strategy-saves-1-billion-yearly-in-customer-retention/)*

**Question :** Comment pensez-vous que Netflix apprend vos préférences ? A-t-il accès à vos pensées, ou utilise-t-il autre chose ?

*(Réponse attendue : Netflix apprend à partir de vos comportements passés — ce que vous avez regardé, pendant combien de temps, ce que vous avez abandonné, ce que vous avez noté...)*

Ce système de recommandation utilise plusieurs types d'apprentissage machine. Explorons-les ensemble.

---

## Les trois grands paradigmes d'apprentissage

Avant de plonger dans les détails, voici une vue d'ensemble :

```
┌─────────────────────────────────────────────────────────────────────────────┐
│              LES TROIS TYPES D'APPRENTISSAGE EN ML                          │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  ┌─────────────────────┐  ┌─────────────────────┐  ┌─────────────────────┐ │
│  │     SUPERVISÉ       │  │   NON-SUPERVISÉ     │  │   RENFORCEMENT      │ │
│  ├─────────────────────┤  ├─────────────────────┤  ├─────────────────────┤ │
│  │                     │  │                     │  │                     │ │
│  │  "Apprendre avec    │  │  "Découvrir des     │  │  "Apprendre par     │ │
│  │   un professeur"    │  │   patterns cachés"  │  │   essai-erreur"     │ │
│  │                     │  │                     │  │                     │ │
│  │  Données étiquetées │  │  Pas d'étiquettes   │  │  Récompenses/       │ │
│  │  (input → output)   │  │  (juste des inputs) │  │  Pénalités          │ │
│  │                     │  │                     │  │                     │ │
│  │  Ex: Spam/Non-spam  │  │  Ex: Segmentation   │  │  Ex: Jeux vidéo     │ │
│  │      Prix maison    │  │      clients        │  │      Robotique      │ │
│  │                     │  │                     │  │                     │ │
│  └─────────────────────┘  └─────────────────────┘  └─────────────────────┘ │
│                                                                             │
│         ████████████████      ████████████████       ████                  │
│         80% des cas           15% des cas            5% des cas            │
│         industriels           industriels            industriels           │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Question :** Selon vous, pourquoi l'apprentissage supervisé représente-t-il 80% des cas industriels ?

*(Réponse attendue : Parce que dans la plupart des cas business, on a des données historiques avec les résultats connus — clients qui ont acheté ou non, emails qui étaient spam ou non, etc.)*

---

## 2.1 Apprentissage Supervisé : Apprendre avec un professeur

### L'intuition

Imaginez que vous apprenez à reconnaître des champignons comestibles. Vous avez deux options :

**Option A — Sans superviseur :**
> "Voici 1000 champignons. Bonne chance pour trouver lesquels sont comestibles !"

**Option B — Avec superviseur :**
> "Voici 1000 champignons. Pour chacun, je te dis s'il est comestible ou toxique. Apprends les différences."

L'option B est l'**apprentissage supervisé**. Le "superviseur", c'est l'ensemble des **étiquettes** (labels) — les bonnes réponses qu'on connaît déjà.

### Définition formelle

**Apprentissage supervisé** : Type de ML où le modèle apprend à partir de paires, **où la réponse correcte est déjà connue**. **(entrée, sortie attendue)**.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                      APPRENTISSAGE SUPERVISÉ                                │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   PHASE D'ENTRAÎNEMENT :                                                    │
│                                                                             │
│   ┌─────────────────┐                           ┌─────────────────┐        │
│   │ Caractéristiques│     ┌───────────┐        │    Étiquette    │        │
│   │    (Features)   │ ──► │  MODÈLE   │ ──►    │    (Label)      │        │
│   │                 │     │           │   compare avec           │        │
│   │ - Couleur       │     └───────────┘        │  "Comestible"   │        │
│   │ - Forme         │           │              │       ou        │        │
│   │ - Odeur         │           │              │   "Toxique"     │        │
│   └─────────────────┘           ▼              └─────────────────┘        │
│                          Ajuste ses                                        │
│                          paramètres                                        │
│                                                                             │
│   PHASE DE PRÉDICTION :                                                     │
│                                                                             │
│   ┌─────────────────┐     ┌───────────┐        ┌─────────────────┐        │
│   │   NOUVEAU       │ ──► │  MODÈLE   │ ──►    │   Prédiction    │        │
│   │   champignon    │     │  (entraîné)│        │  "Comestible"   │        │
│   └─────────────────┘     └───────────┘        └─────────────────┘        │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

<details>
<summary>🤔 Question Socratique : Pourquoi appelle-t-on ces données "étiquetées" ?</summary>

### 🔑 Réponse

On utilise le terme "étiquette" (label en anglais) par analogie avec des objets physiques.

Imaginez une bibliothèque où chaque livre a une étiquette indiquant son genre (roman, science-fiction, histoire...). Quand vous voulez classer un nouveau livre, vous regardez les livres déjà étiquetés pour apprendre les caractéristiques de chaque genre.

En ML :

- **Feature** = caractéristique du livre (nombre de pages, mots-clés, auteur)
- **Label** = l'étiquette "genre" collée sur le livre
- **Modèle** = vos règles mentales pour classer un nouveau livre

Les données "étiquetées" ont cette information de classification déjà disponible. Les données "non-étiquetées" n'ont que les caractéristiques, sans la réponse.

</details>

---

### Les deux tâches de l'apprentissage supervisé

L'apprentissage supervisé se divise en deux grandes catégories selon ce qu'on prédit :

| Tâche | Ce qu'on prédit | Output | Exemples |
|-------|-----------------|--------|----------|
| **Régression** | Un nombre continu | 42.5, 156.0, -3.2 | Prix d'une maison, température demain |
| **Classification** | Une catégorie | "A", "B", "C" ou 0/1 | Spam/Non-spam, type de tumeur |

**Question :** Si Netflix prédit la note (1-5 étoiles) que vous donneriez à un film, est-ce de la régression ou de la classification ?

*(Réponse attendue : Les deux réponses sont acceptables ! Si on prédit une note exacte (4.3), c'est de la régression. Si on prédit une catégorie (1, 2, 3, 4 ou 5 étoiles), c'est de la classification.)*

---

### Régression : Prédire un nombre

**Scénario :** Vous êtes agent immobilier. Un client veut vendre sa maison et vous demande : "Quel prix puis-je en tirer ?"

Vous n'allez pas inventer un chiffre. Vous allez regarder des maisons similaires qui ont été vendues récemment :

| Surface | Chambres | Jardin | Quartier | **Prix vendu** |
|---------|----------|--------|----------|----------------|
| 80 m² | 2 | Non | Centre | 250 000 € |
| 120 m² | 4 | Oui | Banlieue | 320 000 € |
| 95 m² | 3 | Oui | Centre | 310 000 € |
| 150 m² | 5 | Oui | Banlieue | 380 000 € |

La maison de votre client : 100 m², 3 chambres, jardin, banlieue. Prix estimé ?

C'est exactement ce que fait la **régression linéaire** — mais avec des milliers d'exemples et une formule mathématique.

```
Prix = w₁ × Surface + w₂ × Chambres + w₃ × Jardin + w₄ × Quartier + b

Où w₁, w₂, w₃, w₄ sont les "poids" appris par le modèle
et b est le biais (prix de base)
```

**Question :** Dans cette formule, que représente concrètement w₁ (le poids de la surface) ?

*(Réponse attendue : C'est le prix par mètre carré. Si w₁ = 2500, alors chaque m² supplémentaire ajoute 2500€ au prix estimé.)*

---

### Classification : Prédire une catégorie

**Scénario :** Vous travaillez pour une banque. Votre mission : détecter les transactions frauduleuses parmi des millions de transactions quotidiennes.

Vous avez des données historiques :

| Montant | Heure | Pays | Historique client | **Fraude ?** |
|---------|-------|------|-------------------|--------------|
| 50 € | 14h | France | Normal | Non |
| 3000 € | 3h | Nigeria | Inhabituel | Oui |
| 150 € | 19h | France | Normal | Non |
| 2500 € | 4h | Russie | Inhabituel | Oui |

Le modèle apprend les patterns : montants élevés + heures tardives + pays inhabituel = probablement une fraude.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    CLASSIFICATION BINAIRE                                   │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│                    Montant ↑                                                │
│                         │                                                   │
│                   3000€ │  ⚫    ⚫        ⚫ = Fraude                       │
│                         │    ⚫     ⚫      ⚪ = Légitime                    │
│                   2000€ │  ────────────────                                 │
│                         │     ⚪  │  ⚫       Frontière de décision         │
│                   1000€ │  ⚪    ⚪│                                         │
│                         │⚪  ⚪   │                                          │
│                     50€ │⚪ ⚪ ⚪ ⚪│                                          │
│                         └────────┼────────────► Heure                      │
│                           12h   18h    3h                                   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

Le modèle trace une "frontière de décision" qui sépare les deux classes.

<details>
<summary>🤔 Question Socratique : Et si la banque veut détecter plusieurs types de fraude (vol de carte, usurpation d'identité, blanchiment) ?</summary>

### 🔑 Réponse

On passe de la **classification binaire** (2 classes) à la **classification multiclasse** (N classes).

Les étiquettes deviennent :

- 0 = Légitime
- 1 = Vol de carte
- 2 = Usurpation d'identité
- 3 = Blanchiment d'argent

Le modèle doit maintenant tracer plusieurs frontières pour séparer toutes les classes. C'est plus complexe, mais le principe reste le même : apprendre des exemples étiquetés pour prédire la catégorie de nouveaux cas.

**Algorithmes courants :**

- Régression logistique (malgré son nom, c'est pour la classification !)
- Arbres de décision / Random Forest
- SVM (Support Vector Machines)
- Réseaux de neurones

Vous apprendrez ces algorithmes en détail au **Chapitre 3**.

</details>

---

### Exemples concrets d'apprentissage supervisé

| Domaine | Problème | Type | Input | Output |
|---------|----------|------|-------|--------|
| **Email** | Filtrer le spam | Classification | Texte, expéditeur | Spam / Non-spam |
| **Immobilier** | Estimer un prix | Régression | Surface, localisation | Prix en € |
| **Médical** | Diagnostic | Classification | Symptômes, analyses | Maladie A, B, C |
| **E-commerce** | Prédire achat | Classification | Historique, profil | Achète / N'achète pas |
| **Finance** | Prédire le churn | Classification | Comportement | Reste / Part |
| **Météo** | Température demain | Régression | Données actuelles | 23.5°C |

---

## 2.2 Apprentissage Non-Supervisé : Découvrir des patterns cachés

### L'intuition

Revenons à notre bibliothèque, mais cette fois sans étiquettes :

> "Voici 10 000 livres. Je ne sais pas quels genres existent. Peux-tu les organiser en groupes cohérents ?"

Il n'y a pas de "bonne réponse" prédéfinie. Vous devez **découvrir** la structure dans les données.

C'est l'**apprentissage non-supervisé**.

### Définition formelle

**Apprentissage non-supervisé** : Type de ML où le modèle cherche des patterns dans des données **sans étiquettes**.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    APPRENTISSAGE NON-SUPERVISÉ                              │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   DONNÉES SANS ÉTIQUETTES :                                                 │
│                                                                             │
│        ●  ●    ●        ▲  ▲        ■   ■                                  │
│     ●  ●    ●  ●     ▲    ▲  ▲    ■  ■   ■                                │
│        ●    ●           ▲  ▲         ■  ■                                  │
│                                                                             │
│   Le modèle DÉCOUVRE que ces points forment 3 groupes naturels              │
│                                                                             │
│                              │                                              │
│                              ▼                                              │
│                                                                             │
│   RÉSULTAT : 3 clusters identifiés                                          │
│                                                                             │
│   ┌──────────┐    ┌──────────┐    ┌──────────┐                             │
│   │ Groupe 1 │    │ Groupe 2 │    │ Groupe 3 │                             │
│   │ ●●●●●●● │    │ ▲▲▲▲▲▲▲ │    │ ■■■■■■■ │                             │
│   └──────────┘    └──────────┘    └──────────┘                             │
│                                                                             │
│   → C'est à l'humain d'interpréter ce que représentent ces groupes !       │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Question :** Quelle est la grande différence avec l'apprentissage supervisé ?

*(Réponse attendue : En supervisé, on sait ce qu'on cherche (les étiquettes existent). En non-supervisé, on ne sait pas — on laisse l'algorithme découvrir des structures.)*

---

### Les deux tâches principales du non-supervisé

| Tâche | Objectif | Résultat | Exemples |
|-------|----------|----------|----------|
| **Clustering** | Regrouper les données similaires | K groupes | Segmentation clients |
| **Réduction de dimension** | Simplifier des données complexes | Moins de variables | Visualisation, compression |

---

### Clustering : Segmentation des clients

**Scénario :** Vous êtes responsable marketing. Vous avez 100 000 clients mais vous ne pouvez pas créer 100 000 campagnes différentes. Comment personnaliser intelligemment ?

Vous avez des données sur chaque client :

- Âge
- Revenu annuel
- Fréquence d'achat
- Montant moyen par commande
- Catégories de produits achetés

Mais vous n'avez **pas d'étiquettes** — personne ne vous a dit "ce client est de type A".

L'algorithme de clustering (ex: **K-Means**) analyse les données et découvre des groupes naturels :

```
┌─────────────────────────────────────────────────────────────────────────────┐
│           SEGMENTATION CLIENTS PAR CLUSTERING                               │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   Dépenses mensuelles ↑                                                     │
│                        │                                                    │
│                  500€  │      🔵🔵🔵           CLUSTER 3                    │
│                        │    🔵 🔵 🔵 🔵        "VIP / Gros acheteurs"       │
│                        │       🔵🔵                                         │
│                  200€  │──────────────────────────────────                  │
│                        │  🟢🟢🟢     🔴🔴                                   │
│                  100€  │ 🟢 🟢 🟢   🔴🔴🔴                                  │
│                        │🟢  🟢 🟢  🔴 🔴 🔴    CLUSTER 2                    │
│                   50€  │ 🟢🟢      🔴🔴🔴     "Occasionnels sensibles      │
│                        │                        au prix"                    │
│                        └─────────────────────────────────► Fréquence       │
│                          1x/mois    1x/sem    quotidien    d'achat         │
│                                                                             │
│   CLUSTER 1 : "Fidèles petits budgets" (🟢)                                │
│   - Achètent souvent mais petits montants                                  │
│   - Stratégie : Programme fidélité, petites récompenses                    │
│                                                                             │
│   CLUSTER 2 : "Occasionnels sensibles au prix" (🔴)                        │
│   - Achètent peu et peu souvent                                            │
│   - Stratégie : Promotions, réengagement                                   │
│                                                                             │
│   CLUSTER 3 : "VIP" (🔵)                                                   │
│   - Achètent beaucoup et souvent                                           │
│   - Stratégie : Service premium, avant-premières                           │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

<details>
<summary>🤔 Question Socratique : Comment l'algorithme "sait-il" qu'il faut 3 groupes et pas 5 ou 10 ?</summary>

### 🔑 Réponse

Il ne le "sait" pas ! C'est une décision que **vous** devez prendre.

Il existe des méthodes pour aider :

1. **Méthode du coude (Elbow method)** : On teste K=2, 3, 4, 5... et on regarde quand la qualité des clusters arrête de s'améliorer significativement.

2. **Score de silhouette** : Mesure à quel point les points sont bien assignés à leur cluster (proche des voisins du même cluster, loin des autres).

3. **Connaissance métier** : Parfois, le business dicte le nombre. "On veut 3 tiers de clients : bronze, argent, or."

C'est un exemple où le ML nécessite toujours un **jugement humain**.

</details>

---

### Réduction de dimension : Simplifier pour comprendre

**Problème :** Vous avez des données avec 100 variables (features). Comment visualiser des patterns dans un espace à 100 dimensions ?

La **réduction de dimension** projette les données dans un espace plus petit (souvent 2D ou 3D) tout en préservant les relations importantes.

**Analogie :** Imaginez que vous photographiez une sculpture 3D. La photo 2D perd de l'information, mais capture l'essentiel de la forme.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    RÉDUCTION DE DIMENSION                                   │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│   DONNÉES ORIGINALES (100 dimensions)                                       │
│   Impossible à visualiser !                                                 │
│                                                                             │
│        [x₁, x₂, x₃, ... , x₁₀₀]  →  ???                                    │
│                                                                             │
│                              │                                              │
│                              ▼  PCA, t-SNE, UMAP                            │
│                                                                             │
│   DONNÉES RÉDUITES (2 dimensions)                                           │
│   Visualisable !                                                            │
│                                                                             │
│         y ↑                                                                 │
│           │   ○ ○                                                           │
│           │  ○ ○ ○       △                                                  │
│           │   ○ ○      △ △ △                                                │
│           │           △  △                                                  │
│           │    □ □ □                                                        │
│           │   □  □  □                                                       │
│           └──────────────────→ x                                            │
│                                                                             │
│   On peut maintenant VOIR que les données forment 3 groupes !              │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Applications :**

- **Visualisation** : Voir des clusters dans des données complexes
- **Compression** : Stocker des images avec moins de données
- **Prétraitement ML** : Réduire le bruit avant d'entraîner un modèle
- **Embeddings** : Les représentations vectorielles (Chapitre 7) sont une forme de réduction de dimension

---

## 2.3 Apprentissage par Renforcement : Apprendre par essai-erreur

### L'intuition

Comment un bébé apprend-il à marcher ? Pas en regardant des milliers d'exemples de gens qui marchent (supervisé), ni en analysant des patterns dans des données (non-supervisé).

Il essaie, tombe, se relève, réessaie, et progressivement trouve ce qui fonctionne.

C'est l'**apprentissage par renforcement** (Reinforcement Learning, RL).

### Définition formelle

**Apprentissage par renforcement** : Type de ML où un **agent** apprend à prendre des **actions** dans un **environnement** pour maximiser une **récompense** cumulée.

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    APPRENTISSAGE PAR RENFORCEMENT                           │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│                      ┌─────────────────────┐                                │
│                      │    ENVIRONNEMENT    │                                │
│                      │    (le monde/jeu)   │                                │
│                      └──────────┬──────────┘                                │
│                                 │                                           │
│            ┌────────────────────┼────────────────────┐                      │
│            │                    │                    │                      │
│            ▼                    ▼                    ▼                      │
│     ┌──────────┐         ┌──────────┐         ┌──────────┐                 │
│     │  État    │         │Récompense│         │  Nouvel  │                 │
│     │ actuel   │         │   +10    │         │   état   │                 │
│     │  (s)     │         │   -5     │         │   (s')   │                 │
│     └────┬─────┘         └──────────┘         └──────────┘                 │
│          │                    ▲                                             │
│          │                    │                                             │
│          ▼                    │                                             │
│     ┌─────────────────────────┴───────┐                                    │
│     │            AGENT                │                                    │
│     │                                 │                                    │
│     │   Observe l'état → Choisit     │                                    │
│     │   une action → Reçoit          │                                    │
│     │   récompense → Apprend         │                                    │
│     │                                 │                                    │
│     └─────────────────────────────────┘                                    │
│                    │                                                        │
│                    ▼                                                        │
│              ┌──────────┐                                                   │
│              │  Action  │  →  Modifie l'environnement                      │
│              │  (a)     │                                                   │
│              └──────────┘                                                   │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

**Question :** En quoi est-ce différent de l'apprentissage supervisé ?

*(Réponse attendue : En supervisé, on a des paires (input, output correct). En RL, on n'a pas la "bonne action" — seulement un signal de récompense qui dit si le résultat était bon ou mauvais.)*

---

### L'exemple célèbre : AlphaGo

En 2016, **AlphaGo** (DeepMind/Google) a battu Lee Sedol, champion du monde de Go.

Pourquoi est-ce impressionnant ?

- Le Go a plus de positions possibles qu'il y a d'atomes dans l'univers (~10^170)
- Impossible de calculer toutes les possibilités comme aux échecs
- Les experts pensaient que l'IA ne battrait pas un champion avant 2030

**Comment AlphaGo a-t-il appris ?**

1. **Supervisé (d'abord)** : Apprentissage sur des millions de parties jouées par des humains
2. **Renforcement (ensuite)** : L'IA joue contre elle-même des millions de fois
   - Gagne = récompense positive
   - Perd = récompense négative
   - Apprend des stratégies que les humains n'avaient jamais découvertes

<details>
<summary>🤔 Question Socratique : Pourquoi AlphaGo a-t-il découvert des coups que les humains n'avaient jamais joués en 3000 ans de Go ?</summary>

### 🔑 Réponse

Parce que le RL n'est pas limité par les conventions humaines.

Les joueurs humains apprennent en imitant des maîtres, en étudiant des ouvertures classiques. Ils sont contraints par la tradition et les "règles non-écrites" de ce qui est considéré comme un bon coup.

AlphaGo, lui, n'a qu'un objectif : **maximiser les victoires**. Il n'a pas de préjugés sur ce qui "devrait" être joué.

Le fameux "Move 37" de la partie 2 contre Lee Sedol a été décrit par les commentateurs comme "bizarre", "une erreur de débutant". Mais c'était un coup génial que personne n'avait imaginé.

C'est à la fois la force et le mystère du RL : on ne comprend pas toujours POURQUOI l'agent fait ce qu'il fait.

</details>

---

### Applications du renforcement

| Domaine | Agent | Environnement | Récompense |
|---------|-------|---------------|------------|
| **Jeux vidéo** | Personnage IA | Le jeu | Score, victoire |
| **Robotique** | Robot | Monde physique | Tâche accomplie |
| **Trading** | Algorithme | Marché financier | Profit |
| **Contrôle industriel** | Système | Usine/processus | Efficacité, économies |
| **Conduite autonome** | Véhicule | La route | Sécurité, arrivée à destination |
| **Recommandation** | Système de reco | Utilisateur | Engagement, clics |

---

### Survol conceptuel (hors scope pratique)

**Note importante :** Dans ce bootcamp, nous ne coderons pas d'agents de renforcement. C'est un domaine spécialisé qui nécessite :

- Des environnements de simulation complexes
- Beaucoup de puissance de calcul
- Des connaissances mathématiques avancées

Cependant, comprendre le concept est essentiel car :

- Les LLMs modernes utilisent du RL (RLHF — Reinforcement Learning from Human Feedback)
- Les agents IA autonomes (Chapitre 9) s'inspirent des principes du RL
- C'est une question d'entretien fréquente !

---

## Tableau comparatif : Quel type d'apprentissage choisir ?

```
┌─────────────────────────────────────────────────────────────────────────────┐
│              COMMENT CHOISIR LE BON TYPE D'APPRENTISSAGE ?                  │
├─────────────────────────────────────────────────────────────────────────────┤
│                                                                             │
│  Vous avez des données étiquetées ?                                         │
│                    │                                                        │
│          ┌────────┴────────┐                                               │
│          │                 │                                               │
│         OUI               NON                                              │
│          │                 │                                               │
│          ▼                 ▼                                               │
│  ┌──────────────┐  Vous voulez des groupes ?                               │
│  │  SUPERVISÉ   │         │                                                │
│  │              │  ┌──────┴──────┐                                         │
│  │ • Régression │  │             │                                         │
│  │ • Classif.   │ OUI          NON                                         │
│  └──────────────┘  │             │                                         │
│                    ▼             ▼                                         │
│            ┌──────────┐  ┌──────────────┐                                  │
│            │CLUSTERING│  │ RÉDUCTION    │                                  │
│            │          │  │ DE DIMENSION │                                  │
│            │• K-Means │  │              │                                  │
│            │• DBSCAN  │  │ • PCA        │                                  │
│            └──────────┘  │ • t-SNE      │                                  │
│                          └──────────────┘                                  │
│                                                                             │
│  Cas spécial : Apprentissage par RENFORCEMENT                              │
│  → Quand l'agent doit apprendre par interaction avec un environnement      │
│  → Pas de "dataset" classique, mais des simulations ou le monde réel       │
│                                                                             │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## Résumé de la leçon

### Les trois types d'apprentissage

| Type | Données | Objectif | Analogie |
|------|---------|----------|----------|
| **Supervisé** | Étiquetées (input → output) | Prédire l'output pour de nouveaux inputs | Apprendre avec un professeur |
| **Non-supervisé** | Non étiquetées | Découvrir des structures cachées | Explorer seul |
| **Renforcement** | Interactions + récompenses | Maximiser les récompenses | Apprendre par essai-erreur |

### Points clés à retenir

- **80% des cas industriels** = apprentissage supervisé
- **Régression** = prédire un nombre, **Classification** = prédire une catégorie
- **Clustering** = regrouper des données similaires sans étiquettes
- **Renforcement** = agent + environnement + récompenses

---

## Réflexion métacognitive

Avant de passer à la suite, réfléchissez :

1. **Pour le système de recommandation de Netflix**, quels types d'apprentissage pensez-vous qu'il utilise ?
   *(Indice : probablement plusieurs !)*

2. **Quel type d'apprentissage vous semble le plus intuitif ? Le plus mystérieux ?**

3. **Pouvez-vous identifier un problème dans votre domaine professionnel qui pourrait bénéficier de chaque type d'apprentissage ?**

---

## Pour aller plus loin (optionnel)

- 📺 [StatQuest — Machine Learning Fundamentals](https://www.youtube.com/watch?v=Gv9_4yMHFhI) — Explications visuelles excellentes
- 🎮 [OpenAI Gym](https://gymnasium.farama.org/) — Environnements pour expérimenter le RL
- 📄 [Netflix Tech Blog — Recommendations](https://netflixtechblog.com/tagged/recommendations) — Articles techniques de Netflix
