# Chapitre 3 — Le Cloud pour la data

**Durée estimée : 8-10 heures**

---

## Objectifs d'apprentissage

À la fin de ce chapitre, vous serez capable de :

1. **Expliquer** les avantages du cloud computing et identifier quand l'utiliser plutôt qu'un environnement local
2. **Configurer** un compte AWS et gérer les credentials de manière sécurisée
3. **Lire et écrire** des données depuis/vers AWS S3 en utilisant boto3 et pandas
4. **Comparer** les équivalences entre AWS, GCP et Azure pour le stockage de données

---

## 🎯 Le Hook : Comment Netflix gère 700 millions d'heures de streaming par jour

---

En 2024, Netflix a diffusé plus de **700 millions d'heures de contenu par jour** à travers le monde — soit un volume de data absolument colossal à stocker, gérer et distribuer en temps réel. ([clubic.com][1])

Imaginez les serveurs nécessaires… et la complexité d’un tel système.

Il y a encore une quinzaine d’années, une charge pareille aurait obligé une entreprise comme Netflix à **construire ses propres data centers physiques**, avec des milliers de serveurs, des coûts fixes énormes et des limites strictes de capacité pré-allouée. Cela signifiait prévoir à l’avance combien de stockage on avait besoin, acheter les disques, les serveurs, la clim’, la sécurité, et espérer ne pas se retrouver à court de ressources lors d’un pic d’utilisation.

La réponse aujourd’hui ? **Netflix n’a quasiment plus de serveurs qui lui appartiennent physiquement.** Du moins, pas dans des data centers qu’il gère. ([IT Pro][2])

Netflix utilise **AWS (Amazon Web Services)** pour presque tous ses besoins de calcul, de stockage, de bases de données, de traitement vidéo et d’analytique — une architecture comprenant **plus de 100 000 instances de serveurs virtuels** dans plusieurs régions à travers le monde. ([LinkedIn][3])

Au lieu de construire eux-mêmes des data centers coûteux et limités en taille, ils **louent de la puissance de calcul et du stockage à la demande**, ce qui leur permet de dimensionner automatiquement les ressources en fonction du trafic — pendant les pics comme les creux. C’est ça, **le cloud** : accéder à des ressources informatiques comme on utilise l’électricité — **tu ne vois ni les centrales ni les transformateurs, mais elles répondent à la demande**.

⚠️ Une clarification importante : quand on dit “stockage illimité”, ce n’est pas magique. Les fournisseurs cloud (comme AWS) ont bien **des limites physiques**, mais leur capacité est tellement mutualisée sur des millions de clients et planifiée sur des années qu’un seul client ne rencontre quasiment jamais ces limites. Le cloud offre ainsi une **illusion d’illimité** à l’utilisateur, parce que l’infrastructure est très vaste, très flexible et redimensionnée en continu par le fournisseur. ([LinkedIn][3])

Aujourd’hui, Netflix peut :

* déployer des **milliers de machines virtuelles en quelques minutes**,
* ajouter des **pétaoctets de stockage** selon les besoins,
* répartir la charge **dans plusieurs régions AWS** pour assurer la disponibilité mondiale de son service. ([InfoQ][4])

Alors oui : **ils n’ont plus (ou presque plus) de serveurs “propres”** dans des salles blanches quelque part.
Mais derrière cette abstraction se cachent **des fermes physiques de serveurs, de la connectivité et de l’énergie**, construites par AWS et mutualisées entre des milliers d’entreprises, ce qui rend la mise à l’échelle matérielle beaucoup plus simple et rapide.

---
C'est ça, **le cloud** : accéder à des ressources informatiques comme on utilise l'électricité — vous payez ce que vous consommez.

> 💭 **Question Socratique #1** : Si le cloud permet d'accéder à des ressources "illimitées", pourquoi toutes les entreprises ne l'ont-elles pas encore adopté ? Quels freins peuvent exister ?

> 💭 **🧠 Réponse courte** : 

Si le cloud était **réellement illimité, toujours moins cher, toujours plus simple et toujours plus sûr**, toutes les entreprises y seraient déjà.
Si ce n’est pas le cas, c’est parce que le cloud **déplace les contraintes**, il ne les supprime pas.

---

1️⃣ Le premier frein : **le coût réel (pas le coût affiché)**

Sur le papier :

* paiement à l’usage
* pas d’investissement initial

Dans la réalité :

* coûts variables difficiles à prévoir
* factures qui explosent avec la croissance
* egress (sortie de données) très cher
* mauvaise architecture = surcoûts permanents

👉 Pour certaines entreprises **stables et prévisibles**, un data center amorti depuis 10 ans coûte **moins cher** que le cloud.

➡️ Le cloud est optimal pour :

* l’élasticité
* l’incertitude
* la croissance rapide
  Pas forcément pour les charges fixes et constantes.

---

2️⃣ Le deuxième frein : **le verrouillage fournisseur (vendor lock-in)**

Une fois dans le cloud :

* APIs propriétaires
* services managés difficiles à migrer
* dépendance forte à AWS / Azure / GCP

👉 Quitter le cloud peut devenir :

* techniquement complexe
* financièrement dissuasif
* stratégiquement risqué

Certaines entreprises préfèrent :

* garder le contrôle
* éviter une dépendance critique à un acteur externe

---

3️⃣ Le troisième frein : **la souveraineté et la réglementation**

Pour certaines données :

* santé
* défense
* finance
* administrations

Les questions clés sont :

* où sont physiquement les données ?
* sous quelle juridiction ?
* qui peut y accéder légalement ?

👉 Le cloud public pose de vrais problèmes de :

* conformité (RGPD, lois extraterritoriales)
* contrôle juridique
* audits complexes

➡️ Résultat : cloud privé, hybride… ou pas de cloud du tout.

---

4️⃣ Le quatrième frein : **les compétences internes**

Le cloud n’est pas “plus simple” :

* c’est **différent**
* plus distribué
* plus abstrait
* plus automatisé

Sans équipes formées :

* mauvaise sécurité
* mauvaises architectures
* coûts incontrôlés
* incidents graves

👉 Certaines entreprises savent très bien gérer :

* 3 data centers on-prem
  Mais pas :
* 200 services cloud interconnectés

---

5️⃣ Le cinquième frein : **la latence et le temps réel**

Le cloud est excellent pour :

* le web
* le streaming
* l’analytique
* le batch

Mais moins adapté pour :

* systèmes industriels temps réel
* trading ultra-basse latence
* systèmes embarqués
* contrôle physique de machines

👉 Quand chaque milliseconde compte, **la distance physique** reste une contrainte incompressible.

---

6️⃣ Le point clé à comprendre (le vrai cœur de la question)

> Le cloud ne supprime pas les limites.
> Il **externalise** leur gestion.

Avant :

* limites visibles
* matérielles
* assumées par l’entreprise

Aujourd’hui :

* limites masquées
* statistiques
* assumées par le fournisseur

Certaines entreprises **préfèrent voir leurs limites** plutôt que les déléguer.

---

7️⃣ Synthèse socratique

| Idée reçue          | Réalité                  |
| ------------------- | ------------------------ |
| Cloud = illimité    | Capacité mutualisée      |
| Cloud = moins cher  | Parfois, pas toujours    |
| Cloud = simple      | Complexité déplacée      |
| Cloud = universel   | Pas pour tous les usages |
| Cloud = sans risque | Risques différents       |

---

🧩 Réponse finale (en une phrase)

Toutes les entreprises n’ont pas adopté le cloud parce que **l’“illimité” est une abstraction utile, mais pas gratuite**, et que selon le contexte (coût, contrôle, réglementation, temps réel), **posséder ses propres limites peut être un choix rationnel**.

---

## 3.1 Introduction au cloud computing

### Qu'est-ce que le cloud ?

Le **cloud computing** consiste à accéder à des ressources informatiques (serveurs, stockage, bases de données, etc.) via Internet, sans posséder l'infrastructure physique.

```
┌─────────────────────────────────────────────────────────────┐
│                    AVANT (On-Premise)                       │
├─────────────────────────────────────────────────────────────┤
│  Vous devez :                                               │
│  • Acheter les serveurs physiques                          │
│  • Les installer dans un local climatisé                   │
│  • Les maintenir et les mettre à jour                      │
│  • Prévoir la capacité pour les pics d'activité            │
│  • Gérer la sécurité physique                              │
└─────────────────────────────────────────────────────────────┘

┌─────────────────────────────────────────────────────────────┐
│                    APRÈS (Cloud)                            │
├─────────────────────────────────────────────────────────────┤
│  Vous pouvez :                                              │
│  • Démarrer des ressources en quelques clics               │
│  • Payer uniquement ce que vous utilisez                   │
│  • Augmenter/réduire la capacité à la demande              │
│  • Accéder depuis n'importe où                             │
│  • Déléguer la maintenance au fournisseur                  │
└─────────────────────────────────────────────────────────────┘
```

### Les avantages du cloud

| Avantage | Description | Exemple |
|----------|-------------|---------|
| **Scalabilité** | Ajuster les ressources selon les besoins | Black Friday : x10 serveurs temporaires |
| **Pay-as-you-go** | Payer uniquement ce qu'on consomme | 0€ la nuit si aucun traitement |
| **Disponibilité** | Accès mondial 24/7 | Équipes à Paris, New York, Tokyo |
| **Maintenance déléguée** | Le fournisseur gère l'infra | Plus de serveur en panne à 3h du matin |
| **Innovation rapide** | Nouveaux services constamment | GPU pour ML sans achat matériel |

Lors d’un pic d’utilisation, les serveurs existants atteignent rapidement leurs limites physiques (CPU, RAM, réseau, entrées/sorties disque). Même s’ils sont suffisants en temps normal, ils ne peuvent pas absorber soudainement un grand nombre de requêtes simultanées sans dégrader les performances. Ajouter des serveurs permet de répartir la charge : chaque serveur traite une partie des requêtes avec sa propre mémoire et sa propre capacité de calcul. Cela évite les saturations, les ralentissements et les erreurs, tout en garantissant la continuité du service. Dimensionner l’infrastructure uniquement pour le pic serait coûteux et inefficace ; l’ajout temporaire de serveurs permet donc de répondre à une demande exceptionnelle sans surpayer en permanence des ressources inutilisées.

Lors des pics, les serveurs ajoutés servent principalement à apporter du calcul et de la mémoire supplémentaires, pas du stockage. Les données “officielles” (bases de données, data lake, fichiers métiers) restent centralisées sur des systèmes conçus pour être fiables et persistants. Les nouveaux serveurs accèdent à ces données via le réseau, les traitent, puis disparaissent une fois le pic passé. Leur stockage local est au mieux utilisé de façon temporaire (cache, fichiers intermédiaires, logs, résultats transitoires), mais il n’est pas considéré comme un espace de stockage fiable, car ces serveurs sont éphémères et peuvent être supprimés à tout moment. Utiliser leur disque pour stocker des données critiques créerait un risque de perte, de duplication incohérente et de complexité inutile.

👉 En résumé : serveurs de pic = calcul + RAM, stockage durable = systèmes dédiés. C’est cette séparation qui rend l’infrastructure scalable, robuste et maîtrisable.

---

### Les trois grands fournisseurs cloud

Le marché est dominé par trois acteurs majeurs :

| Fournisseur | Part de marché (Q2 2025) | Croissance annuelle | Force principale |
|-------------|--------------------------|---------------------|------------------|
| **AWS** (Amazon) | **30%** | +17.5% | Leader historique, écosystème complet |
| **Azure** (Microsoft) | **20%** | +39% | Intégration Microsoft, IA (OpenAI) |
| **GCP** (Google) | **13%** | +32% | Data/Analytics, IA (Gemini) |

*(Source : [Statista - Cloud Market Share Q2 2025](https://www.statista.com/chart/18819/worldwide-market-share-of-leading-cloud-infrastructure-service-providers/))*

**Tendance 2025 :** Azure et GCP gagnent du terrain grâce à leurs offres IA (OpenAI/Gemini). AWS reste leader mais sa part diminue lentement (33% en 2021 → 30% en 2025).

---

### ✍️ Exercice 3.1 : Cloud ou local ? (10 min)

Pour chaque scénario, indiquez si vous recommanderiez le **cloud** ou une solution **locale (on-premise)**, et justifiez :

| Scénario | Cloud / Local | Justification |
|----------|---------------|---------------|
| Startup avec 3 data scientists et budget limité | _____ | _____ |
| Banque traitant des données ultra-sensibles (régulation stricte) | _____ | _____ |
| Site e-commerce avec pics saisonniers (Noël, soldes) | _____ | _____ |
| Hôpital devant conserver des données 30 ans | _____ | _____ |
| Projet ML expérimental de 3 mois | _____ | _____ |

---

> 💭 **Question Socratique #2** : AWS domine le marché avec 30% de parts, mais Azure croît 2x plus vite (+39% vs +17.5%). À votre avis, AWS risque-t-il de perdre sa position de leader ? Quels facteurs pourraient accélérer ou freiner ce changement ?


