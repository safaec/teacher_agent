# Module 3 — Chapitre 1 : Introduction à l'IA et au Machine Learning

**Durée totale : 8h** | **Track : ML Practitioner**

---

## 🎯 Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

1. **Expliquer** ce qu'est l'intelligence artificielle et la distinguer du Machine Learning et du Deep Learning
2. **Identifier** le type d'apprentissage approprié (supervisé, non-supervisé, par renforcement) pour un problème métier donné
3. **Évaluer** les implications éthiques et les limites des systèmes d'IA dans des contextes réels

---

## 🔥 Accroche : Le problème qui a tout changé

> **En mars 2016, quelque chose d'« impossible » s'est produit.**

Lee Sedol, champion du monde du jeu de Go, affronte AlphaGo, une intelligence artificielle développée par DeepMind. Le Go est considéré comme le jeu de stratégie le plus complexe au monde — plus de positions possibles qu'il n'y a d'atomes dans l'univers. Les experts prédisaient qu'aucune machine ne pourrait battre un champion humain avant au moins 10 ans.

**Résultat : AlphaGo gagne 4 parties à 1.**

Ce n'était pas de la chance. La machine avait *appris* à jouer en analysant des millions de parties, puis en jouant contre elle-même. Elle a même inventé des coups que les humains n'avaient jamais imaginés.

*Source : [Timeline of Artificial Intelligence - Wikipedia](https://en.wikipedia.org/wiki/Timeline_of_artificial_intelligence)*

**❓ Question de départ :** Comment une machine peut-elle « apprendre » quelque chose qu'aucun humain ne lui a explicitement enseigné ?

---

# Leçon 1.1 : Qu'est-ce que l'IA ?

**Durée : 1h30** | **Type : Théorie + Discussion**

---

## 🔗 Lien avec vos acquis (Module 2)

Dans le Module 2, vous avez appris à :

- Extraire des données de sources variées (CSV, API, SQL)
- Nettoyer et transformer ces données
- Les analyser avec pandas et les visualiser

**Maintenant, la question est :** que faire *après* avoir préparé ces données ? Comment passer de l'analyse descriptive (« que s'est-il passé ? ») à l'analyse prédictive (« que va-t-il se passer ? ») ?

C'est exactement là qu'intervient le **Machine Learning**.

---

## 1.1.1 Histoire de l'IA : De Turing à ChatGPT

### La question fondamentale (1950)

Tout commence par une question philosophique posée par **Alan Turing** en 1950 dans son article *"Computing Machinery and Intelligence"* :

> **« Les machines peuvent-elles penser ? »**

Pour y répondre, Turing propose un test : si une machine peut converser avec un humain sans que celui-ci puisse déterminer s'il parle à une machine ou à un humain, alors on peut considérer que la machine « pense ».

C'est le célèbre **Test de Turing**.

*Source : [The History of AI - Coursera](https://www.coursera.org/articles/history-of-ai)*

---

<details>
<summary>🤔 Question Socratique : Pourquoi le Test de Turing est-il encore débattu aujourd'hui ?</summary>

### 🔑 Réponse

Le Test de Turing mesure la capacité d'une machine à **imiter** le comportement humain, pas nécessairement à **comprendre**. Un système comme ChatGPT peut passer le test en générant des réponses convaincantes, mais cela ne prouve pas qu'il « comprend » réellement ce qu'il dit.

C'est la différence entre :

- **Intelligence simulée** : imiter les apparences de l'intelligence
- **Intelligence réelle** : comprendre véritablement le sens et le contexte

Ce débat reste ouvert et constitue l'un des grands mystères de la philosophie de l'IA.

</details>

---

### Les grandes étapes de l'IA

| Année | Événement | Signification |
|-------|-----------|---------------|
| **1950** | Test de Turing | La question fondamentale est posée |
| **1956** | Conférence de Dartmouth | Naissance officielle de l'IA comme discipline |
| **1958** | Perceptron | Premier réseau de neurones artificiel |
| **1997** | Deep Blue bat Kasparov | L'IA maîtrise les échecs |
| **2016** | AlphaGo bat Lee Sedol | L'IA maîtrise l'intuition au Go |
| **2017** | Article "Attention Is All You Need" | Invention des Transformers |
| **2022** | Lancement de ChatGPT | L'IA générative devient accessible au grand public |

*Source : [AI History Timeline - EdrawMind](https://edrawmind.wondershare.com/history/ai-history-timeline.html)*

---

### L'« Hiver de l'IA » : Pourquoi l'IA a failli disparaître

Entre les années 1970 et 2000, l'IA a connu plusieurs périodes de déception appelées **« hivers de l'IA »**. Les promesses excessives des chercheurs n'ont pas été tenues, les financements se sont taris, et le domaine a stagné.

**Pourquoi l'IA a-t-elle finalement décollé dans les années 2010 ?**

Trois facteurs convergents :

1. **Les données** : Internet et les smartphones génèrent des quantités massives de données d'entraînement
2. **La puissance de calcul** : Les GPU (cartes graphiques) permettent des calculs parallèles massifs
3. **Les algorithmes** : Nouvelles architectures comme les réseaux de neurones profonds

> 💡 **Insight clé :** L'IA d'aujourd'hui n'est pas « plus intelligente » que celle des années 1960 — elle a simplement accès à beaucoup plus de données et de puissance de calcul.

---

## 1.1.2 Définitions : IA vs ML vs DL

### Le schéma des cercles concentriques

Imaginez trois cercles imbriqués :

```
┌─────────────────────────────────────────────┐
│           Intelligence Artificielle          │
│  ┌─────────────────────────────────────┐    │
│  │         Machine Learning            │    │
│  │  ┌─────────────────────────────┐    │    │
│  │  │       Deep Learning         │    │    │
│  │  └─────────────────────────────┘    │    │
│  └─────────────────────────────────────┘    │
└─────────────────────────────────────────────┘
```

---

### Définitions précises

**🤖 Intelligence Artificielle (IA)**

> Tout système informatique capable d'effectuer des tâches qui nécessiteraient normalement l'intelligence humaine.

**Exemples :**

- Un thermostat « intelligent » qui ajuste la température
- Un GPS qui calcule le meilleur itinéraire
- Un assistant vocal qui répond à vos questions

**Caractéristique :** Peut être basée sur des règles programmées (« si température > 25°C, alors activer climatisation »).

---

**📊 Machine Learning (ML)**

> Sous-ensemble de l'IA où les systèmes **apprennent à partir des données** sans être explicitement programmés pour chaque situation.

**Exemples :**

- Un filtre anti-spam qui apprend à distinguer les emails légitimes des spams
- Un système de recommandation Netflix qui apprend vos goûts
- Un modèle de prédiction du prix immobilier

**Caractéristique :** Le système découvre les patterns par lui-même à partir d'exemples.

---

**🧠 Deep Learning (DL)**

> Sous-ensemble du ML utilisant des **réseaux de neurones artificiels à plusieurs couches** pour apprendre des représentations complexes.

**Exemples :**

- Reconnaissance faciale sur votre téléphone
- Traduction automatique (Google Translate)
- Génération de texte (ChatGPT)

**Caractéristique :** Nécessite beaucoup de données et de puissance de calcul, mais peut capturer des patterns très complexes.

---

<details>
<summary>🤔 Question Socratique : Un GPS qui calcule le meilleur itinéraire utilise-t-il du Machine Learning ?</summary>

### 🔑 Réponse

**Pas nécessairement.** Un GPS classique utilise des algorithmes déterministes (comme Dijkstra ou A*) pour trouver le chemin le plus court. C'est de l'IA « classique » basée sur des règles.

**En revanche**, Google Maps ou Waze utilisent du Machine Learning pour :

- Prédire le trafic en temps réel
- Estimer les temps de trajet basés sur les données historiques
- Apprendre les préférences de l'utilisateur

C'est un bon exemple de la différence entre **IA basée sur des règles** et **IA basée sur l'apprentissage**.

</details>

---

### Tableau comparatif

| Aspect | IA Classique | Machine Learning | Deep Learning |
|--------|-------------|------------------|---------------|
| **Programmation** | Règles explicites | Apprend des patterns | Apprend des représentations |
| **Données nécessaires** | Peu | Modéré | Beaucoup |
| **Puissance de calcul** | Faible | Modérée | Très élevée |
| **Interprétabilité** | Élevée | Moyenne | Faible |
| **Exemple** | Thermostat programmé | Filtre anti-spam | ChatGPT |

---

## 1.1.3 L'état actuel de l'IA (2024-2025)

### Ce que l'IA sait faire aujourd'hui

L'adoption de l'IA a explosé ces dernières années. Selon le rapport **Stanford AI Index 2025** :

- **78% des organisations** utilisaient l'IA en 2024 (contre 55% l'année précédente)
- Les investissements mondiaux ont atteint **252,3 milliards de dollars** en 2024
- Les entreprises ayant adopté l'IA voient leur productivité croître **4,8 fois plus vite** que la moyenne

*Source : [Stanford HAI - 2025 AI Index Report](https://hai.stanford.edu/ai-index/2025-ai-index-report)*

---

### Exemples concrets d'impact

| Domaine | Exemple | Impact mesuré |
|---------|---------|---------------|
| **Juridique** | Rédaction de réponses aux plaintes | 16h → 3-4 min (x100 plus rapide) |
| **Santé** | Détection du mélanome par IA | 100% de détection |
| **Développement** | Outils de codage IA | 50% des développeurs les utilisent quotidiennement |
| **Retail** | Recommandations personnalisées | 400-660 milliards $/an de potentiel |

*Source : [McKinsey - The State of AI in 2025](https://www.mckinsey.com/capabilities/quantumblack/our-insights/the-state-of-ai)*

---

### Ce que l'IA ne sait PAS (encore) faire

Malgré ces avancées impressionnantes, l'IA a des limites importantes :

❌ **Comprendre véritablement** le sens et le contexte (elle simule la compréhension)
❌ **Raisonner** de manière générale sur des problèmes nouveaux
❌ **Avoir du bon sens** dans des situations inhabituelles
❌ **Être créative** au sens humain (elle recombine des patterns existants)
❌ **Ressentir** des émotions ou avoir une conscience

> 💡 **Démystification :** L'IA n'est pas magique — c'est de la **reconnaissance de patterns à grande échelle**. Elle excelle à trouver des régularités dans des données massives, mais elle ne « comprend » pas ce qu'elle fait.

---

<details>
<summary>🤔 Question Socratique : Pourquoi dit-on que l'IA « ne comprend pas » si elle peut répondre correctement à des questions complexes ?</summary>

### 🔑 Réponse

**La nuance est cruciale :**

Un LLM comme ChatGPT prédit le prochain mot le plus probable basé sur des milliards d'exemples de texte. Il n'a pas de « modèle mental » du monde.

**Analogie :** Imaginez quelqu'un qui a mémorisé toutes les réponses possibles à un examen sans comprendre le sujet. Il obtiendrait 100%, mais ne pourrait pas appliquer ses connaissances à une situation nouvelle non prévue.

**Preuve :** Les LLMs peuvent échouer spectaculairement sur des problèmes simples de logique ou de mathématiques qui nécessitent un vrai raisonnement, même s'ils réussissent des tâches apparemment plus complexes.

C'est pourquoi on parle de **« perroquet stochastique »** — un système qui répète des patterns sans comprendre leur signification profonde.

</details>

---

## 📚 Encadré : Perspective alternative

### « L'IA est-elle vraiment intelligente ? »

**Point de vue 1 — Les optimistes :**
> « La différence entre imiter l'intelligence et être intelligent est peut-être une fausse distinction. Si un système se comporte de manière indiscernable d'un être intelligent, il EST intelligent. » — Position fonctionnaliste

**Point de vue 2 — Les sceptiques :**
> « Un perroquet peut répéter des phrases en français sans comprendre le français. De même, les LLMs manipulent des symboles sans comprendre leur signification. » — Position du « Chinese Room » (John Searle)

**Votre position ?** Il n'y a pas de bonne réponse définitive — ce débat anime la philosophie de l'IA depuis 70 ans.

---

## 🧠 Réflexion métacognitive

Avant de passer à la suite, prenez un moment pour réfléchir :

1. **Qu'est-ce qui vous a surpris** dans cette leçon ?
2. **Quel concept vous semble le plus flou ?** (IA vs ML vs DL ? L'histoire ? Les limites ?)
3. **Comment expliquerez-vous** la différence entre ML et DL à un collègue non-technique ?

> 📝 Notez vos réponses — nous y reviendrons à la fin du chapitre.

---

# Leçon 1.2 : Types d'apprentissage

**Durée : 2h** | **Type : Théorie + Exemples**

---

## 🔥 Accroche : Comment Netflix sait-il ce que vous voulez regarder ?

Netflix possède plus de **260 millions d'abonnés** dans le monde. Chaque utilisateur voit une page d'accueil différente, personnalisée selon ses goûts. Cette personnalisation génère **75-80% de tout ce qui est regardé** sur la plateforme.

Comment font-ils ? Ils n'ont pas 260 millions d'employés qui regardent vos habitudes. La réponse : **le Machine Learning**.

*Source : [How ML Powers Recommendation Systems - Boston Institute of Analytics](https://bostoninstituteofanalytics.org/blog/how-machine-learning-powers-recommendation-systems-netflix-amazon-spotify/)*

---

**❓ Question de départ :** Mais concrètement, comment une machine peut-elle « apprendre » vos préférences ?

---

## 1.2.1 Apprentissage supervisé

### Le concept

> **Définition :** L'apprentissage supervisé utilise des données **étiquetées** — c'est-à-dire des exemples où l'on connaît déjà la bonne réponse.

**Analogie :** C'est comme apprendre avec un professeur qui vous corrige à chaque exercice.

- Vous voyez des exemples : « Cette image est un chat », « Cette image est un chien »
- Vous apprenez les caractéristiques qui distinguent les deux
- Ensuite, vous pouvez classifier de nouvelles images

---

### Les deux grandes familles

**📈 Régression : prédire un nombre**

| Entrée (X) | Sortie (y) |
|------------|------------|
| Surface, localisation, nombre de pièces | **Prix de la maison** (ex: 350 000€) |
| Âge, revenus, historique | **Montant du crédit** accordé |
| Données météo | **Température** de demain |

**Question type :** « Combien ? » ou « Quelle valeur ? »

---

**🏷️ Classification : prédire une catégorie**

| Entrée (X) | Sortie (y) |
|------------|------------|
| Texte d'un email | **Spam** ou **Non-spam** |
| Caractéristiques d'une transaction | **Fraude** ou **Légitime** |
| Image médicale | **Tumeur maligne** ou **Bénigne** |

**Question type :** « Quelle catégorie ? » ou « Oui ou non ? »

---

### Exemple concret : Prédiction du churn (désabonnement)

Une entreprise de télécommunications veut prédire quels clients risquent de partir.

**Données historiques (étiquetées) :**

| Client | Ancienneté | Appels SAV | Facture moyenne | **A quitté ?** |
|--------|------------|------------|-----------------|----------------|
| A | 24 mois | 0 | 45€ | Non |
| B | 6 mois | 5 | 80€ | **Oui** |
| C | 36 mois | 2 | 55€ | Non |
| D | 3 mois | 8 | 90€ | **Oui** |

Le modèle apprend : « Les clients récents qui appellent souvent le SAV avec des factures élevées ont tendance à partir. »

**Nouveau client :**

| Client | Ancienneté | Appels SAV | Facture moyenne | Prédiction |
|--------|------------|------------|-----------------|------------|
| E | 4 mois | 6 | 85€ | **Risque élevé ⚠️** |

---

<details>
<summary>🤔 Question Socratique : Pourquoi appelle-t-on cet apprentissage « supervisé » ?</summary>

### 🔑 Réponse

On l'appelle « supervisé » car les données d'entraînement contiennent la **bonne réponse** (l'étiquette ou label). Le modèle peut comparer ses prédictions à cette vérité et s'ajuster.

C'est comme avoir un superviseur qui dit : « Non, cette image n'est pas un chat, c'est un chien. Recommence. »

**Sans supervision** (voir section suivante), le modèle devrait découvrir les patterns tout seul, sans savoir ce qui est « correct ».

</details>

---
