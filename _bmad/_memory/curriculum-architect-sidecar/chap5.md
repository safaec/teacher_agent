# Chapitre 5 : Réseaux de Neurones (Conceptuel)

**Durée : 12h** | **Activité principale :** Playground Exploration

**Objectif :** Comprendre le passage du Machine Learning classique au Deep Learning, maîtriser l'architecture universelle (MLP) et comprendre intimement la mécanique d'apprentissage (Backpropagation) avant de passer aux architectures spécialisées (Chapitre 6).

**Narration logique :** Le Problème (Limites du ML) → La Brique (Neurone) → La Structure (MLP/Deep) → La Vie (Apprentissage) → Les Défis → La Pratique

---

## Leçon 5.1 : De l'Ingénierie à l'Apprentissage (1h30) — Le "Pourquoi"

Pourquoi avons-nous besoin du Deep Learning ? Comprendre le changement de paradigme.

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 5.1.1 | Le mur du ML Classique | 30min | • Rappel : Le ML excelle sur les données structurées (Excel). • Le problème des données "brutes" (Images, Son, Texte) : pour l'ordi, ce sont des matrices de chiffres sans sens. • Le goulot d'étranglement : Le Feature Engineering manuel (long, coûteux, expert-dépendant). |
| 5.1.2 | L'Apprentissage de Représentation | 30min | • La promesse du DL : Le modèle apprend ses propres filtres (features). • Comparaison des flux : ML = Input → Humain → Modèle → Output. DL = Input → Réseau → Output. |
| 5.1.3 | Introduction au "Deep" | 30min | • Concept de hiérarchie : Apprendre des concepts simples (bords) pour construire des concepts complexes (visages). • C'est la fin de l'intervention humaine dans l'extraction des variables. |

**Ressources pour Content Creator :**
- Schéma comparatif ML vs DL workflows
- Exemples visuels : features manuelles vs features apprises

---

## Leçon 5.2 : Le Neurone Artificiel (2h) — La Brique

Démystifier le neurone en le reliant à ce qu'ils connaissent déjà : la Régression Logistique.

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 5.2.1 | Analogie biologique | 30min | Neurone biologique → neurone artificiel. Dendrites (Inputs) → Corps (Somme) → Axone (Sortie). |
| 5.2.2 | Mathématiques du Neurone | 30min | • Le "Moment Eurêka" : Un neurone = Une Régression Logistique (z = wx + b). • Anatomie : Input (x) = donnée brute, Poids (w) = importance de l'entrée, Somme pondérée (z) = combinaison linéaire, Activation = fonction qui "écrase" le résultat. |
| 5.2.3 | Fonction d'activation | 30min | Sans activation = régression linéaire. Activation = non-linéarité. Pourquoi c'est crucial. |
| 5.2.4 | Activations courantes | 30min | ReLU (max(0,x)) - la plus utilisée. Sigmoid (0-1) - probabilités. Tanh (-1 à 1). Softmax (sortie multi-classe). |

**Ressources pour Content Creator :**
- Schéma du neurone artificiel annoté
- Graphiques des fonctions d'activation
- Lien explicite avec Régression Logistique (Track 1, Ch3)

---

## Leçon 5.3 : L'Architecture Standard — Le MLP (2h) — La Structure

Construire le réseau : de la largeur (Shallow) à la profondeur (Deep).

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 5.3.1 | La couche d'entrée | 15min | Input layer = vos features. Dimension = nombre de features. |
| 5.3.2 | La couche cachée (Hidden Layer) | 30min | • Passer de 1 neurone à N neurones en parallèle. • Un neurone ne peut tracer qu'une seule ligne droite → si problème complexe (non-linéaire), il échoue. • Notion de "Largeur" : Plusieurs neurones pour capter plusieurs caractéristiques (Couleur, Forme...). |
| 5.3.3 | Du Shallow au Deep (Profondeur) | 30min | • Empiler les couches : La sortie de la couche 1 devient l'entrée de la couche 2. • Définition : 1 couche cachée = Shallow Network. 2+ couches cachées = Deep Learning. • Pourquoi "Deep" ? Couche 1 : Traits simples (bâtons, courbes). Couche 2 : Combinaison (yeux, oreilles). Couche 3 : Objets complets (Visages). |
| 5.3.4 | La couche de sortie | 15min | Output layer = prédiction. 1 neurone (régression), N neurones (classification N classes). |
| 5.3.5 | Le MLP (Multi-Layer Perceptron) | 30min | • Définition officielle : Réseau de neurones "Fully Connected" (Dense). • Vocabulaire technique : Dense Layers (chaque neurone connecté à tous les précédents). • Son rôle : Le "Couteau Suisse" pour les données tabulaires et la prise de décision finale. |

**Ressources pour Content Creator :**
- Schéma d'un réseau feedforward avec annotations
- Animation de construction couche par couche
- Démo optionnelle : construction sur Keras

---

## Leçon 5.4 : La Mécanique d'Apprentissage (2h30) — Le Cerveau

Comment le MLP trouve-t-il les bons poids (w) ? Explication du cycle d'entraînement.

*Rappel pédagogique : Le cycle d'entraînement sert à trouver les valeurs des poids et coefficients pour minimiser l'écart entre prédictions et valeurs réelles (loss function).*

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 5.4.1 | Forward pass | 30min | Input → couches → prédiction. Calcul couche par couche. L'information circule de l'entrée vers la sortie. Calcul matriciel (intuition) → Le réseau fait une "devinette". |
| 5.4.2 | Fonction de perte (Loss) | 30min | "À quel point on se trompe ?" Comparer la devinette (y_pred) à la réalité (y_true). MSE pour régression, Cross-entropy pour classification. Objectif du jeu : Minimiser cette Loss. |
| 5.4.3 | Gradient descent (intuition) | 45min | Métaphore : descendre la montagne les yeux bandés. Ajuster les poids pour réduire la loss. Le Learning Rate : Taille des pas (trop gros = on rate, trop petit = c'est lent). |
| 5.4.4 | Backpropagation (concept) | 30min | "Qui est responsable de l'erreur ?" Propager l'erreur en arrière. Mise à jour des poids (w) pour faire mieux la prochaine fois. **Pas de formules complexes.** |
| 5.4.5 | Epochs et batch | 15min | Epoch = passage complet des données. Batch = sous-ensemble pour efficacité. |

**Ressources pour Content Creator :**
- Animation du gradient descent (descente de montagne)
- Analogie de la montagne expliquée visuellement
- Schéma du cycle Forward → Loss → Backward → Update

---

## Leçon 5.5 : Les Défis de la Profondeur (1h30) — Les Solutions

Ce qui change quand on est "Deep" — et comment le résoudre.

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 5.5.1 | Pourquoi "deep" fonctionne | 30min | Plus de couches = abstractions hiérarchiques. Features de bas niveau → haut niveau. |
| 5.5.2 | Défis du deep learning | 20min | Vanishing gradients, coût computationnel, besoin de données massives. |
| 5.5.3 | Les solutions modernes | 40min | • Solution 1 (Activation) : Pourquoi ReLU est indispensable (vs Sigmoid) pour laisser passer le signal dans les couches profondes. • Solution 2 (Stabilisation) : Batch Normalization — stabiliser les données à chaque étage. • Solution 3 (Régularisation) : Dropout — empêcher le réseau d'apprendre par cœur (Overfitting). |

**Ressources pour Content Creator :**
- Visualisation du vanishing gradient
- Comparaison ReLU vs Sigmoid dans réseaux profonds

---

## Leçon 5.6 : Atelier Pratique & Synthèse (2h30) — La Pratique

Visualiser pour comprendre. Consolider les apprentissages.

| # | Sous-partie | Durée | Contenu |
|---|-------------|-------|---------|
| 5.6.1 | TensorFlow Playground (Guidé) | 60min | Objectif : Manipuler un MLP visuel sans code. Exercice 1 : Résoudre un problème linéaire (1 neurone suffit). Exercice 2 : Le problème de la spirale (Besoin de Deep Learning + Non-linéarité). Observation : Regarder la "Loss curve" descendre. |
| 5.6.2 | Démo Keras (optionnel) | 45min | Entraîner MNIST en 10 lignes de code. Voir le code correspondant à ce qu'on a manipulé visuellement. |
| 5.6.3 | Synthèse & Vocabulaire | 45min | Récapitulatif du vocabulaire : Epoch, Batch, Weights, Bias, Dense, MLP, Forward, Backward, Loss, Learning Rate. Quiz de validation rapide. |

**🎯 Activité principale : Playground Exploration** — TensorFlow Playground
**🎯 Activité secondaire : Démo Keras** — MNIST minimal

**Ressources pour Content Creator :**
- Lien TensorFlow Playground avec exercices guidés
- Notebook MNIST minimal (10 lignes)
- Fiche vocabulaire récapitulative

---

## 💡 Points de Transition

| Transition | Script suggéré |
|------------|----------------|
| 5.1 → 5.2 | "Maintenant qu'on sait que le DL sert à apprendre des features tout seul, voyons quelle est la petite machine qui permet de faire ça : le neurone." |
| 5.2 → 5.3 | "Un neurone seul, c'est juste une régression. Pour créer de l'intelligence, il faut créer une équipe. C'est ce qu'on appelle un MLP." |
| 5.3 → 5.4 | "On a construit une belle Ferrari (le MLP), mais le réservoir est vide. Elle ne sait pas conduire. Voyons comment on démarre le moteur avec la Backpropagation." |
| 5.4 → 5.5 | "Le moteur tourne, mais la route est longue et pleine de pièges. Voyons les défis spécifiques aux réseaux profonds." |
| 5.5 → 5.6 | "Assez de théorie ! Mettons les mains dans le cambouis avec TensorFlow Playground." |
| Ch5 → Ch6 | "Nous avons maîtrisé le MLP, l'architecture universelle. Mais pour les images et le texte, il existe des architectures spécialisées. Direction : CNN, RNN, et les révolutionnaires Transformers." |

---

## 📊 Résumé Chapitre 5

| Leçon | Titre | Durée | Focus |
|-------|-------|-------|-------|
| 5.1 | De l'Ingénierie à l'Apprentissage | 1h30 | Le "Pourquoi" |
| 5.2 | Le Neurone Artificiel | 2h | La Brique |
| 5.3 | L'Architecture Standard — Le MLP | 2h | La Structure |
| 5.4 | La Mécanique d'Apprentissage | 2h30 | Le Cerveau |
| 5.5 | Les Défis de la Profondeur | 1h30 | Les Solutions |
| 5.6 | Atelier Pratique & Synthèse | 2h30 | La Pratique |
| **Total** | | **12h** | |

---

## Note pour Chapitre 6

Le Chapitre 6 devra maintenant couvrir :
- **Types de réseaux spécialisés** : CNN (images), RNN (séquences)
- **L'évolution vers les Transformers** : Limites RNN → Attention → Transformer
- **Fine-tuning** (comme prévu)

Cette réorganisation crée une progression logique :
- Ch5 : Architecture universelle (MLP) + mécanique d'apprentissage
- Ch6 : Architectures spécialisées (CNN, RNN) → Transformers → Fine-tuning
