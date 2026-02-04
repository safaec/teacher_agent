# Writing State

## Current Course

**Module 3 — Intelligence Artificielle : du ML classique aux LLMs**
- Target audience: Profils en reconversion avec bases Python, notebooks, pipelines (Module 2), prompt engineering (Module 1)
- Pedagogy: Direct Instruction (scaffolding + systematic progression)
- Document language: French
- Pacing: Chapter by chapter
- Depth: In-depth (step-by-step breakdowns)
- **Status: 🔄 IN PROGRESS**

---

## Module 3 Progress

### ✅ Chapitre 1 : Introduction à l'IA et au Machine Learning
- **Status:** Complete
- **Duration:** 8h
- **Folder:** `001_Chapitre_1_Introduction_IA_ML/`
- **Files written:**
  - Part_1_Quest_ce_que_IA.md (Histoire + Spectre IA→ML→NN→DL→GenAI)
  - Part_2_Types_Apprentissage.md (Supervisé, Non-supervisé, Renforcement)
  - Part_3_Paysage_IA.md (Pyramide, Acteurs majeurs, Écosystème technique)
  - Part_4_Cas_Usage_Reels.md (Recommandation, Fraude, NLP, Vision, Prédictif)
  - Part_5_Ethique_Limites.md (Biais, Hallucinations, Responsabilité)
- **Research sources:** TIME, CNBC, ACLU, Marketing AI Institute, Head of AI
- **Key content:** Full AI chronology (1950-2025), complete spectrum diagram, Netflix/Spotify/Amazon case studies, Amazon recruiting bias case study, EU AI Act overview


### ✅ Chapitre 2 : Le Workflow ML
- **Status:** Complete
- **Duration:** 10h
- **Folder:** `002_Chapitre_2_Workflow_ML/`
- **Files written:**
  - Part_1_Pipeline_ML.md (Vue d'ensemble, lien Module 2, itération/expérimentation)
  - Part_2_Train_Test_Validation.md (Généralisation, split 80/20, stratification, data leakage)
  - Part_3_Pattern_Fit_Predict.ipynb (API universelle sklearn, fit/predict/score, paramètres vs hyperparamètres)
  - Part_4_Pipelines_Sklearn.ipynb (Pipeline, ColumnTransformer, preprocessing production-ready)
  - Part_5_Premier_Modele_Complet.ipynb (Workflow end-to-end, baseline DummyClassifier, confusion matrix, joblib)
- **Research sources:** IBM ML Pipeline, V7 Labs Train/Test Split, Princeton Reproducibility Crisis, scikit-learn docs
- **Key content:**
  - Pipeline ML 5 étapes avec diagrammes ASCII
  - Data leakage prévention (rule: fit on train, transform on all)
  - Baseline comparison avec DummyClassifier (votre suggestion intégrée!)
  - Métriques au-delà de .score() : confusion matrix, precision/recall/F1
  - Model persistence avec joblib.dump()

### ✅ Chapitre 3 : Algorithmes et Évaluation ML
- **Status:** Complete
- **Duration:** 20h (merged from original Chapters 3+4)
- **Folder:** `003_Chapitre_3_Algorithmes_et_Evaluation_ML/`
- **Structure:** Each algorithm follows **Hook → Intuition → Construction → Hyperparameters → Evaluation**
- **Files written:**
  - Part_1_Regression_Lineaire.ipynb (Zillow hook, moindres carrés, fit_intercept/positive hyperparams, MAE/RMSE/R² evaluation)
  - Part_2_Regression_Logistique.ipynb (Gmail spam hook, sigmoid, C/penalty/solver hyperparams, Precision/Recall/F1/ROC-AUC)
  - Part_3_Arbres_Random_Forest.ipynb (Bank loan hook, Gini, max_depth/min_samples hyperparams, Biais-Variance diagnostic, learning curves)
  - Part_4_KMeans_Clustering.ipynb (Spotify hook, n_clusters/init/n_init hyperparams, silhouette/inertia/elbow evaluation)
  - Part_5_Selection_Validation_Optimisation.ipynb (Algorithm selection flowchart, K-Fold CV, GridSearchCV/RandomizedSearchCV, Pipeline integration)
- **Research sources:** Zillow Zestimate, IEEE spam detection 2024, Wiley loan prediction 2024, MDPI customer segmentation 2024, Arize AI F1-score, Emerald housing 2024
- **Key content:**
  - Consistent pedagogical structure across all algorithms
  - Real-world hooks for engagement (Zillow, Gmail, Bank, Spotify)
  - Hyperparameters section with practical comparisons for each algo
  - Integrated evaluation (metrics applied, not re-explained from Ch2)
  - Biais-Variance tradeoff with visual learning curves
  - Complete optimization workflow: Pipeline + CV + GridSearch

### ✅ Chapitre 4 : Réseaux de Neurones (Conceptuel)
- **Status:** Complete
- **Duration:** 12h
- **Folder:** `004_Chapitre_4_Reseaux_Neurones/`
- **Structure:** Narrative flow: Problème → Brique → Structure → Vie → Défis → Pratique
- **Files written:**
  - Part_1_De_Ingenierie_a_Apprentissage.md (Le "Pourquoi" — Feature Engineering vs Representation Learning)
  - Part_2_Neurone_Artificiel.md (La Brique — Lien avec Régression Logistique, activations ReLU/Sigmoid/Tanh/Softmax)
  - Part_3_Architecture_MLP.md (La Structure — Input/Hidden/Output layers, Largeur vs Profondeur, Fully Connected)
  - Part_4_Mecanique_Apprentissage.md (Le Cerveau — Forward Pass, Loss, Gradient Descent, Backpropagation, Epochs/Batches)
  - Part_5_Defis_Profondeur.md (Les Solutions — Vanishing Gradients, ReLU, BatchNorm, Dropout)
  - Part_6_Atelier_Pratique.ipynb (La Pratique — TensorFlow Playground exercices, Keras MNIST, Vocabulaire quiz)
- **Key content:**
  - Complete "Eureka moment": Neuron = Logistic Regression
  - Detailed definitions for ALL new terms (boxed format)
  - Mountain analogy for Gradient Descent
  - ASCII diagrams throughout (hierarchical features, backpropagation cycle)
  - TensorFlow Playground guided exercises (linear, circle, spiral problems)
  - Full Keras MNIST example with ~97% accuracy
  - Vocabulary synthesis table

### ⏳ Chapitre 5 : Des Réseaux aux Transformers + Fine-tuning
- **Status:** Pending
- **Duration:** 12h

### ⏳ Chapitre 6 : Embeddings et Recherche Vectorielle
- **Status:** Pending
- **Duration:** 10h

### ⏳ Chapitre 7 : RAG — Connecter LLM et Données
- **Status:** Pending
- **Duration:** 14h

### ⏳ Chapitre 8 : Agents et Automatisation IA
- **Status:** Pending
- **Duration:** 14h

### ⏳ Chapitre 9 : Stratégie IA en Entreprise
- **Status:** Pending
- **Duration:** 6h

### ⏳ Chapitre 10 : Projet Intégrateur
- **Status:** Pending
- **Duration:** 14h

---

## Previous Course (Completed)

**Module 2 — Pipeline Data : de la collecte à l'analyse en environnement cloud**
- Target audience: Profils en reconversion avec bases Python, Excel et SQL
- Pedagogy: Direct Instruction (scaffolding + systematic progression)
- Document language: French
- **Status: ✅ COMPLETE**

## Lessons Written

1. ✅ **Chapitre 1 : Pipeline data — vue d'ensemble** (module2-chapitre1-pipeline-data-vue-ensemble.md)
   - Status: Complete
   - Sections covered: Position du module, ETL vs ELT, Étapes de la pipeline, CRISP-DM, Sources de données
   - Research sources: IBM, Gartner, Integrate.io, DataScience-PM

2. ✅ **Chapitre 2 : Extraction des données** (module2-chapitre2-extraction-donnees.md)
   - Status: Complete
   - Sections covered: pandas (CSV/Excel/JSON), chunking, SQL connections, REST APIs, web scraping ethics, initial inspection
   - Research sources: pandas docs, KDnuggets, Speakeasy, CNIL 2025

3. ✅ **Chapitre 3 : Le Cloud pour la data** (module2-chapitre3-cloud-pour-data.md)
   - Status: Complete
   - Sections covered: Cloud fundamentals, AWS S3, boto3, GCP/Azure comparison

4. ✅ **Chapitre 4 : EDA diagnostique et qualité des données** (module2-chapitre4-eda-diagnostique-qualite.md)
   - Status: Complete
   - Sections covered: Quality dimensions, missing values, duplicates, outliers detection

5. ✅ **Chapitre 5 : Nettoyage des données** (module2-chapitre5-nettoyage-donnees.md)
   - Status: Complete
   - Sections covered: Missing values treatment, duplicates removal, outliers handling, type conversion, text cleaning

6. ✅ **Chapitre 6 : Structuration et transformation** (module2-chapitre6-structuration-transformation.md)
   - Status: Complete
   - Sections covered: Pivot/melt, merge/concat, groupby, feature engineering

7. ✅ **Chapitre 7 : EDA analytique** (module2-chapitre7-eda-analytique.md)
   - Status: Complete
   - Sections covered: Univariate/bivariate analysis, segmentation, hypothesis formulation, critical thinking

8. ✅ **Chapitre 8 : Visualisation exploratoire** (module2-chapitre8-visualisation-exploratoire.md)
   - Status: Complete
   - Sections covered: Matplotlib, Seaborn, Plotly, visualization best practices

9. ✅ **Chapitre 9 : Chargement vers le cloud** (module2-chapitre9-chargement-vers-cloud.md)
   - Status: Complete
   - Sections covered: boto3/pandas S3 upload, CSV vs Parquet, data lake organization (raw/processed/curated), data dictionary, post-upload validation, professional best practices
   - Research sources: AWS SDK pandas docs, AWS Data Lake Foundation, Microsoft Data Lake Zones, Parquet vs CSV benchmarks

10. ✅ **Chapitre 10 : Cas pratique fil rouge** (module2-chapitre10-cas-pratique-fil-rouge.md)
    - Status: Complete
    - Sections covered: TechShop e-commerce case study, multi-source extraction (CSV/Excel/JSON/Parquet), EDA diagnostique with quality checklist, data cleaning workflow, joins and feature engineering, analytical EDA answering 5 business questions, professional dashboard visualization, S3 upload with validation, presentation template
    - Research sources: Data Ladder retail quality issues, Integrate.io e-commerce pipelines, Gartner data quality statistics
    - Business scenario: TechShop online electronics retailer with realistic data quality problems

11. ✅ **Chapitre 11 : Conclusion et aide-mémoire** (module2-chapitre11-conclusion-aide-memoire.md)
    - Status: Complete
    - Sections covered: Competencies synthesis with checklist, Top 10 common mistakes to avoid, Connection to Module 3 (ML) and Module 4 (Power BI), Comprehensive aide-mémoire cheat sheet (extraction, diagnostic, cleaning, transformation, analysis, visualization, cloud), Final reflection and celebration

## Module Statistics

- **Total Chapters**: 11/11 complete
- **Estimated Duration**: 85-105 hours
- **Total Exercises**: 30+ hands-on exercises across all chapters
- **Socratic Questions**: 15+ throughout the module
- **Code Examples**: 100+ copy-paste ready snippets
- **Research Citations**: 20+ verified sources

## Terminology Consistency

| Term (FR) | Term (EN) | Usage |
|-----------|-----------|-------|
| Pipeline de données | Data pipeline | Main concept |
| Entrepôt de données | Data warehouse | ETL target |
| Lac de données | Data lake | ELT target |
| Valeurs manquantes | Missing values | Quality issue |
| Doublons | Duplicates | Quality issue |
| Moissonnage | Web scraping | Data extraction |
| Pagination | Pagination | API extraction |
| Limite de requêtes | Rate limiting | API protection |
| Chargement | Upload/Load | Cloud operations |
| Zone brute | Raw zone | Data lake layer |
| Zone traitée | Processed zone | Data lake layer |
| Zone organisée | Curated zone | Data lake layer |
| Fil rouge | Capstone project | Hands-on project |
| Taux de désabonnement | Churn rate | Customer analytics |
| Aide-mémoire | Cheat sheet | Quick reference |
| Neurone artificiel | Artificial neuron | Deep Learning unit |
| Poids | Weights | Learned parameters |
| Biais | Bias | Learned threshold |
| Couche cachée | Hidden layer | Intermediate layer |
| Couche dense | Dense layer | Fully connected |
| Fonction d'activation | Activation function | Non-linearity |
| Propagation avant | Forward pass | Prediction step |
| Rétropropagation | Backpropagation | Gradient calculation |
| Descente de gradient | Gradient descent | Optimization |
| Taux d'apprentissage | Learning rate | Step size |
| Époque | Epoch | Full data pass |
| Lot | Batch | Data subset |
| Abandon | Dropout | Regularization |
| Normalisation par lot | Batch normalization | Stabilization |
| Gradient évanescent | Vanishing gradient | Deep learning problem |

## Next Action

**Chapitre 4 : Réseaux de Neurones (Conceptuel) is COMPLETE!** 🎉
(Full 12h chapter with 6 parts following narrative structure: Problème → Brique → Structure → Vie → Défis → Pratique)

Next chapter to write: **Chapitre 5 : Architectures Spécialisées, Transformers & Fine-tuning** (12h)
- CNN et RNN (architectures spécialisées pour images/séquences)
- Limites des RNN → Introduction de l'Attention
- Architecture Transformer (Encoder-Decoder, Self-Attention)
- Du Transformer aux LLMs (BERT, GPT)
- Pre-training vs Fine-tuning (LoRA, QLoRA)
- Fine-tuning pratique avec Hugging Face
