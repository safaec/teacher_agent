# Course State

## Current Course

**Boot Camp Data & Intelligence Artificielle**
- Duration: 3 months
- Modules: 4

## Modules Overview

| Module | Title | Status |
|--------|-------|--------|
| 1 | Prompt Engineering | ✅ Completed |
| 2 | Pipeline Data | ✅ Detailed (Phase 2 done) |
| 3 | Intelligence Artificielle (ML & LLM) | ✅ Detailed (Phase 2 done) |
| 4 | Visualisation décisionnelle (Power BI) | ⏳ Pending |

---

## Module 2 — Pipeline Data (Summary)

**Duration:** 3 weeks (~85-105h)
**Status:** ✅ Completed
**Output:** `_bmad-output/Module 2 — Pipeline Data (version finale).md`

---

## Module 3 — Intelligence Artificielle (ML & LLM) — DETAILED

**Duration:** 4 weeks (~120h) + 2 weeks final project
**Audience:** Profils en reconversion avec bases Python, notebooks, data pipelines (Module 2), prompt engineering (Module 1)
**Environment:** Python, VS Code, Jupyter notebooks, Google Colab (for GPU)

### Learning Goal
> Students become **AI practitioners** who understand core ML/AI principles, can build predictive models, and create LLM-powered applications (RAG, agents).

### Prerequisites from Previous Modules
- **Module 1:** Prompt engineering, agent prompts in .md format, orchestrator patterns
- **Module 2:** Python, pandas, data cleaning, APIs, SQL, cloud basics (AWS S3)

### Time Allocation (Rebalanced)

| Block | Chapters | Hours | % |
|-------|----------|-------|---|
| Classical ML | Ch 1-4 | 40h | 33% |
| Deep Learning / Transformers | Ch 5-6 | 22h | 18% |
| LLM Application | Ch 7-10 | 44h | 37% |
| Project | Ch 11 | 14h | 12% |

---

## Chapter 1: Introduction à l'IA et au Machine Learning
**Duration: 8h**

### Learning Objectives
> Students can explain what AI/ML is, distinguish between types of learning, and identify appropriate use cases.

### Content Breakdown

| Section | Topics | Hours |
|---------|--------|-------|
| 1.1 Qu'est-ce que l'IA? | History (Turing → Deep Blue → GPT), AI vs ML vs DL definitions, current state | 1.5h |
| 1.2 Types d'apprentissage | Supervised, unsupervised, reinforcement learning with examples | 2h |
| 1.3 Le paysage de l'IA | The AI pyramid (Big Tech → Enterprise → Startups), who does what | 1.5h |
| 1.4 Cas d'usage | Real-world applications: recommendation, fraud detection, NLP, vision | 2h |
| 1.5 Éthique et limites | Bias, hallucinations, environmental cost, responsible AI | 1h |

### Key Concepts
- Artificial Intelligence vs Machine Learning vs Deep Learning
- Supervised learning = labeled data (regression, classification)
- Unsupervised learning = no labels (clustering, dimensionality reduction)
- Reinforcement learning = rewards (conceptual only)
- The AI adoption pyramid: Big Tech (train from scratch) → Enterprise (fine-tune + RAG) → Startups (APIs)
- AI is not magic - it's pattern recognition at scale

### Hands-on
- **Demo:** Show pre-trained models in action (image classifier, chatbot, recommendation)
- **Exercise:** Given 5 business problems, identify which type of ML applies

### Tools
- None (conceptual chapter)

---

## Chapter 2: Le Workflow ML
**Duration: 10h**

### Learning Objectives
> Students can execute the complete ML workflow from data to prediction using scikit-learn.

### Content Breakdown

| Section | Topics | Hours |
|---------|--------|-------|
| 2.1 Le pipeline ML | Data → Preprocess → Train → Evaluate → Deploy (overview) | 1h |
| 2.2 Train / Test / Validation | Why split data, data leakage dangers, stratification | 2h |
| 2.3 Le pattern fit/predict | Universal API: `.fit()`, `.predict()`, `.score()` | 2h |
| 2.4 Pipelines scikit-learn | `Pipeline`, `ColumnTransformer`, preprocessing steps | 3h |
| 2.5 Premier modèle complet | End-to-end example: load → clean → train → evaluate | 2h |

### Key Concepts
- Train/test split - WHY it prevents cheating
- Data leakage - the #1 beginner mistake
- The fit/predict pattern (universal across all sklearn)
- Preprocessing must be FIT on train, TRANSFORM on test
- Pipeline = reproducible, no leakage
- Random state for reproducibility

### Hands-on
- **Exercise 1:** Split a dataset, train a simple model, evaluate
- **Exercise 2:** Build a complete Pipeline with preprocessing
- **Mini-project:** Predict house prices (regression) end-to-end

### Tools
```python
from sklearn.model_selection import train_test_split
from sklearn.pipeline import Pipeline
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import StandardScaler, OneHotEncoder
```

---

## Chapter 3: Algorithmes ML Essentiels
**Duration: 10h** (condensed from 14h - focus on intuition)

### Learning Objectives
> Students can choose, train, and interpret regression, classification, and clustering models.

### Content Breakdown

| Section | Topics | Hours |
|---------|--------|-------|
| 3.1 Régression linéaire | Concept, coefficients interpretation, when to use | 2h |
| 3.2 Régression logistique | Binary classification, probability output, threshold | 2h |
| 3.3 Arbres de décision & Random Forest | Visual intuition, ensemble concept, feature importance | 2.5h |
| 3.4 K-Means clustering | Unsupervised, choosing K, elbow method | 2h |
| 3.5 Quand utiliser quoi? | Algorithm selection guide, decision tree | 1.5h |

### Key Concepts

**Regression (predicting numbers):**
- Linear regression: y = wx + b
- Coefficients = feature importance
- Assumptions: linearity, no multicollinearity

**Classification (predicting categories):**
- Logistic regression = linear regression + sigmoid
- Outputs probability (0-1), threshold decides class
- Decision trees: visual, interpretable, prone to overfit
- Random Forest: many trees → vote → robust

**Clustering (finding groups):**
- K-Means: pick K, iterate until stable
- Elbow method for choosing K
- Use cases: customer segmentation, anomaly detection

### Hands-on
- **Exercise 1:** Linear regression on housing data, interpret coefficients
- **Exercise 2:** Logistic regression for churn prediction
- **Exercise 3:** Decision tree + Random Forest comparison
- **Exercise 4:** Customer segmentation with K-Means

### Tools
```python
from sklearn.linear_model import LinearRegression, LogisticRegression
from sklearn.tree import DecisionTreeClassifier, plot_tree
from sklearn.ensemble import RandomForestClassifier
from sklearn.cluster import KMeans
```

---

## Chapter 4: Évaluation et Optimisation
**Duration: 12h**

### Learning Objectives
> Students can evaluate models correctly, diagnose problems, and tune hyperparameters.

### Content Breakdown

| Section | Topics | Hours |
|---------|--------|-------|
| 4.1 Métriques de régression | MSE, RMSE, MAE, R² — when to use which | 2h |
| 4.2 Métriques de classification | Accuracy, precision, recall, F1, confusion matrix, ROC-AUC | 3h |
| 4.3 Overfitting / Underfitting | Bias-variance tradeoff, visual diagnosis, solutions | 2h |
| 4.4 Validation croisée | K-Fold, stratified K-Fold, why it's more robust | 2h |
| 4.5 Optimisation des hyperparamètres | GridSearchCV, RandomizedSearchCV, key hyperparameters | 3h |

### Key Concepts

**Regression Metrics:**
- MSE = penalizes large errors more
- R² = % of variance explained (0-1)
- MAE = average absolute error (interpretable)

**Classification Metrics:**
- Accuracy misleads with imbalanced data!
- Precision = "of predicted positives, how many correct?"
- Recall = "of actual positives, how many found?"
- F1 = harmonic mean (balance precision/recall)
- Confusion matrix visualization
- ROC-AUC for ranking quality

**Overfitting:**
- High train score + low test score = overfitting
- Solutions: more data, regularization, simpler model, cross-validation

**Hyperparameter Tuning:**
- Hyperparameters vs parameters (set before vs learned during training)
- GridSearchCV: exhaustive but slow
- RandomizedSearchCV: faster, often good enough
- Key hyperparameters:
  - RandomForest: n_estimators, max_depth, min_samples_split
  - LogisticRegression: C (regularization)
  - KMeans: n_clusters

### Hands-on
- **Exercise 1:** Calculate and compare all metrics on a classification problem
- **Exercise 2:** Diagnose overfitting, apply fixes
- **Exercise 3:** Cross-validation vs simple train/test comparison
- **Exercise 4:** GridSearchCV to optimize a Random Forest
- **Mini-project:** Build the best model you can, justify hyperparameter choices

### Tools
```python
from sklearn.metrics import (mean_squared_error, r2_score, accuracy_score,
                             precision_score, recall_score, f1_score,
                             confusion_matrix, classification_report, roc_auc_score)
from sklearn.model_selection import cross_val_score, GridSearchCV, RandomizedSearchCV
```

---

## Chapter 5: Réseaux de Neurones (Conceptuel)
**Duration: 10h**

### Learning Objectives
> Students can explain how neural networks learn, without needing advanced math.

### Content Breakdown

| Section | Topics | Hours |
|---------|--------|-------|
| 5.1 Le neurone artificiel | Weighted sum + activation = decision, visual analogy | 2h |
| 5.2 Couches et architectures | Input → Hidden → Output, depth = complexity | 2h |
| 5.3 Comment ça apprend | Forward pass, loss, gradient descent (intuition), backprop (concept) | 2.5h |
| 5.4 Types de réseaux | CNN (images), RNN (sequences), when to use each | 2h |
| 5.5 Du neurone au deep learning | Why deep works, vanishing gradients, modern architectures | 1.5h |

### Key Concepts

**The Neuron:**
- Neuron = weighted sum of inputs + bias + activation function
- Activation functions: ReLU (most common), sigmoid, tanh
- Visualize as a "decision gate"

**Layers:**
- Input layer = your features
- Hidden layers = learned representations
- Output layer = prediction
- More layers = more complex patterns (but harder to train)

**Learning (intuition, NO math):**
- Forward pass: input → prediction
- Loss function: "how wrong are we?"
- Gradient descent: "adjust weights to reduce loss"
- Backpropagation: "which weights caused the error?" (intuition only)
- Epochs: repeat until good enough

**Network Types:**
- Feedforward (FNN): tabular data, basic
- CNN: images, spatial patterns, convolution = "sliding window"
- RNN: sequences, memory of previous inputs
- LSTM/GRU: better memory for long sequences

### Hands-on
- **Demo 1:** Interactive neuron visualization (TensorFlow Playground)
- **Demo 2:** Watch a network train in real-time
- **Exercise:** Train a simple neural network on MNIST (Keras, 10 lines)

### Tools
```python
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense
# TensorFlow Playground: https://playground.tensorflow.org/
```

---

## Chapter 6: Des Réseaux aux Transformers + Fine-tuning
**Duration: 12h**

### Learning Objectives
> Students can explain how Transformers and LLMs work, and understand when/how to fine-tune.

### Content Breakdown

| Section | Topics | Hours |
|---------|--------|-------|
| 6.1 Les limites des RNN | Sequential processing, vanishing gradients, slow training | 1h |
| 6.2 L'attention | "Attention is all you need", self-attention intuition, key-query-value | 3h |
| 6.3 Architecture Transformer | Encoder-decoder, positional encoding, multi-head attention | 2.5h |
| 6.4 Du Transformer aux LLMs | BERT (encoder), GPT (decoder), how scale creates capability | 2h |
| 6.5 Pre-training vs Fine-tuning | What each means, when to fine-tune, LoRA/QLoRA intro | 2.5h |
| 6.6 Fine-tuning pratique | Light hands-on: fine-tune BERT for classification | 1h |

### Key Concepts

**Why Transformers:**
- RNNs process sequentially (slow, forgets long sequences)
- Transformers process in parallel (fast, sees everything at once)
- Attention = "which words matter for understanding this word?"

**Attention Mechanism (intuition):**
- Query: "what am I looking for?"
- Key: "what do I contain?"
- Value: "what do I provide if matched?"
- Self-attention: every word attends to every other word

**Transformer Architecture:**
- Encoder: understands input (BERT)
- Decoder: generates output (GPT)
- Positional encoding: order matters!
- Multi-head: multiple attention patterns in parallel

**LLM Evolution:**
- BERT (2018): encoder, good for understanding/classification
- GPT (2018→): decoder, good for generation
- Scale matters: GPT-3 = 175B parameters, emergent abilities

**Pre-training vs Fine-tuning:**
- Pre-training: train on massive internet text (done by Big Tech)
- Fine-tuning: adapt to YOUR specific task/data
- Full fine-tuning: update all weights (expensive)
- LoRA/QLoRA: update small adapters (efficient, modern approach)
- When to fine-tune: specific style, domain vocabulary, consistent behavior

### Hands-on
- **Demo 1:** Visualize attention patterns in a sentence
- **Demo 2:** Show how GPT generates token by token
- **Exercise:** Fine-tune BERT for sentiment classification (Hugging Face, Colab)

### Tools
```python
from transformers import AutoTokenizer, AutoModelForSequenceClassification, Trainer
# Hugging Face ecosystem
# Google Colab for GPU access
```

---

## Chapter 7: Embeddings et Recherche Vectorielle
**Duration: 10h**

### Learning Objectives
> Students can create embeddings, compute similarity, and build a vector search system.

### Content Breakdown

| Section | Topics | Hours |
|---------|--------|-------|
| 7.1 Qu'est-ce qu'un embedding? | Words as vectors, semantic meaning in numbers | 2h |
| 7.2 Créer des embeddings | OpenAI embeddings, Sentence Transformers, choosing a model | 2h |
| 7.3 Similarité et distance | Cosine similarity, Euclidean distance, when to use which | 1.5h |
| 7.4 Bases vectorielles | ChromaDB, Pinecone, FAISS — why we need them | 2.5h |
| 7.5 Chunking de documents | How to split text, overlap, chunk size tradeoffs | 2h |

### Key Concepts

**Embeddings:**
- Embedding = vector representation of meaning
- Similar meanings → similar vectors
- Example: "king" - "man" + "woman" ≈ "queen"
- Dimension: typically 384, 768, or 1536

**Creating Embeddings:**
- OpenAI ada-002: 1536 dimensions, good quality, API cost
- Sentence Transformers: free, local, various sizes
- Voyage AI, Cohere: alternatives

**Similarity:**
- Cosine similarity: angle between vectors (most common)
- Range: -1 (opposite) to 1 (identical)
- Euclidean: absolute distance

**Vector Databases:**
- Why: millions of vectors, need fast search
- ChromaDB: simple, local, perfect for learning
- Pinecone: managed, scalable, production-ready
- FAISS: Facebook's library, very fast, no persistence by default

**Chunking:**
- LLMs have context limits → must chunk long documents
- Chunk size: 256-1024 tokens typically
- Overlap: 10-20% to avoid cutting mid-sentence
- Strategies: by paragraph, by sentence, fixed size

### Hands-on
- **Exercise 1:** Create embeddings for sentences, compute similarity
- **Exercise 2:** Build semantic search over a small document set
- **Exercise 3:** Set up ChromaDB, add documents, query
- **Mini-project:** Semantic search engine for a folder of PDFs

### Tools
```python
from sentence_transformers import SentenceTransformer
import chromadb
from openai import OpenAI  # for ada-002
from langchain.document_loaders import PyPDFLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
```

---

## Chapter 8: RAG — Connecter LLM et Données
**Duration: 14h** (increased from 12h)

### Learning Objectives
> Students can build a complete RAG pipeline that answers questions from custom documents.

### Content Breakdown

| Section | Topics | Hours |
|---------|--------|-------|
| 8.1 Pourquoi RAG? | LLM limitations (cutoff, hallucinations), RAG as solution | 1.5h |
| 8.2 Architecture RAG | Ingest → Embed → Store → Retrieve → Generate | 2h |
| 8.3 LangChain & LCEL | Chains, runnables, the new expression language | 3h |
| 8.4 Retrieval strategies | Similarity, MMR, hybrid search, reranking | 2.5h |
| 8.5 Prompts pour RAG | System prompts, context injection, citation | 1.5h |
| 8.6 Évaluation du RAG | How to measure retrieval quality, answer quality | 1.5h |
| 8.7 RAG complet | End-to-end: PDF → ChromaDB → ChatBot | 2h |

### Key Concepts

**Why RAG:**
- LLMs have knowledge cutoff (don't know recent info)
- LLMs hallucinate (make up facts)
- RAG = "give the LLM your documents as context"
- No training required, just retrieval

**RAG Architecture:**
```
Documents → Chunk & Embed → Vector Store
                                ↓
Answer ← LLM Generate ← Retrieve Top K ← Query
```

**LangChain (LCEL):**
- LCEL = LangChain Expression Language (the new way)
- | pipe operator chains components
- RunnablePassthrough, RunnableLambda
- Clean, composable pipelines

**Retrieval:**
- Similarity search: nearest neighbors
- MMR (Maximal Marginal Relevance): diversity + relevance
- Hybrid: combine keyword + semantic search
- Reranking: use a second model to reorder results

**Prompts:**
- System prompt: "You are a helpful assistant. Use ONLY the context below."
- Context injection: "{context}" placeholder
- Require citations: "Quote the source for each fact"

### Hands-on
- **Exercise 1:** Simple RAG with LangChain (5 documents)
- **Exercise 2:** Compare retrieval strategies (similarity vs MMR)
- **Exercise 3:** RAG with different document types (PDF, HTML, structured)
- **Exercise 4:** Evaluate RAG quality
- **Mini-project:** "Chat with your PDF" app

### Tools
```python
from langchain_openai import ChatOpenAI, OpenAIEmbeddings
from langchain_community.vectorstores import Chroma
from langchain.chains import RetrievalQA
from langchain_core.runnables import RunnablePassthrough
from langchain_core.output_parsers import StrOutputParser
from langchain.prompts import ChatPromptTemplate
```

---

## Chapter 9: Agents et Automatisation IA
**Duration: 14h** (increased from 12h)

### Learning Objectives
> Students can build AI agents that use tools, make decisions, and automate workflows.

### Prerequisites from Module 1
- ✅ Agent prompts in .md file format
- ✅ Instruction patterns for LLMs
- ✅ Orchestrator agent concept

### What This Chapter Adds (vs Module 1)
- Module 1: Prompt-based agents (conceptual, LLM pretends to act)
- Module 3: Code-based agents (LangGraph, MCP, actual tool execution)

### Content Breakdown

| Section | Topics | Hours |
|---------|--------|-------|
| 9.1 Du prompt à l'exécution | Bridge from Module 1: "Now we make agents REAL" | 1h |
| 9.2 Function calling | How LLMs output structured calls (OpenAI/Claude tool use) | 2h |
| 9.3 LangGraph: état et flux | State management, nodes, edges, conditional routing | 3.5h |
| 9.4 Créer des tools | Build custom tools (Python functions → LLM-callable) | 2.5h |
| 9.5 MCP en pratique | Turn Module 1 .md patterns into MCP servers | 3h |
| 9.6 Agent complet | Combine everything: orchestrator + tools + state + error handling | 2h |

### Key Concepts

**Bridge from Module 1:**
```
Module 1 (Prompt):              Module 3 (Code):
orchestrator.md          →      LangGraph StateGraph
"Route to search agent"  →      def route(state): return "search"
LLM pretends to search   →      Agent ACTUALLY calls search API
```

**Function Calling:**
- LLM outputs structured JSON for tool calls
- Your code parses and executes
- OpenAI format vs Claude format

**LangGraph:**
- Graph-based agent orchestration
- Nodes = steps (LLM calls, tool execution)
- Edges = transitions (conditional or fixed)
- State = shared context across nodes
- Better than chains for complex workflows

**MCP (Model Context Protocol):**
- Anthropic's standard for LLM ↔ tools communication
- MCP Server: exposes tools and resources
- MCP Client: the LLM/application that uses them
- Resources: files, data the LLM can access
- Tools: actions the LLM can take
- Why it matters: standardization, interoperability

**Agent Patterns:**
- Sequential: A → B → C (simple chain)
- Branching: if condition then A else B
- Loop: repeat until satisfied
- Human-in-the-loop: pause for approval

### Hands-on
- **Exercise 1:** Create a simple agent with tools (calculator, search)
- **Exercise 2:** Build a LangGraph workflow with conditional routing
- **Exercise 3:** Set up a basic MCP server, connect to Claude
- **Exercise 4:** Convert a Module 1 .md agent to MCP server
- **Mini-project:** Research agent that searches web, summarizes, saves report

### Tools
```python
from langgraph.graph import StateGraph, END
from langchain_core.tools import tool
from langchain.agents import AgentExecutor
# MCP package for Python
# Claude Desktop for testing MCP
```

---

## Chapter 10: Stratégie IA en Entreprise
**Duration: 6h**

### Learning Objectives
> Students can recommend the right AI approach for business problems, justify costs, and compare options.

### Content Breakdown

| Section | Topics | Hours |
|---------|--------|-------|
| 10.1 Framework de décision | Flowchart: when API vs RAG vs fine-tune | 1.5h |
| 10.2 Analyse des coûts | Token pricing, infrastructure costs, TCO calculation | 1.5h |
| 10.3 Comparaison des fournisseurs | OpenAI vs Claude vs Mistral vs open-source | 1h |
| 10.4 Études de cas | Real scenarios: startup, SMB, enterprise, regulated | 1.5h |
| 10.5 Rédiger une recommandation | Structure a 1-page AI strategy document | 0.5h |

### Key Concepts

**Decision Framework:**
- Need custom knowledge? → RAG
- Need specific behavior/tone? → Fine-tune
- Data highly sensitive? → Self-hosted / on-premise
- Low volume? → API is cheapest
- High volume? → Fine-tuning becomes cost-effective

**Cost Analysis:**
| Approach | Upfront | Per-use |
|----------|---------|---------|
| API only | $0 | $$$/token |
| RAG + API | $ (infra) | $$$/token |
| Fine-tuned API | $$ (train) | $$/token |
| Self-hosted | $$$ (GPUs) | $/token |

**Vendor Comparison:**
- OpenAI: best ecosystem, expensive, closed
- Anthropic (Claude): safety, long context, enterprise
- Mistral: EU-based, open models available, competitive pricing
- Open-source (LLaMA, Mixtral): full control, need infrastructure

**Company Size Patterns:**
- Startup (< 50 people): API only
- Scale-up (50-500): RAG + APIs
- Enterprise (500+): Hybrid (RAG + fine-tune + APIs)
- Big Tech: Train from scratch

### Hands-on
- **Exercise 1:** Calculate cost for 3 different scenarios
- **Exercise 2:** Given a business case, choose and justify an approach
- **Exercise 3:** Write a 1-page AI strategy recommendation

### Tools
- Pricing calculators (OpenAI, Anthropic)
- Spreadsheet for TCO analysis

---

## Chapter 11: Projet Intégrateur
**Duration: 14h**

### Learning Objectives
> Students can build an end-to-end AI application combining ML, RAG, and agents.

### Content Breakdown

| Section | Topics | Hours |
|---------|--------|-------|
| 11.1 Cahier des charges | Define project scope, requirements, success criteria | 2h |
| 11.2 Pipeline de données | Extract, clean, prepare data (connect to Module 2) | 3h |
| 11.3 Modèle ML | Train, evaluate, tune a predictive model | 3h |
| 11.4 Intégration LLM | Add RAG or agent layer on top | 4h |
| 11.5 Présentation | Document, demo, defend your choices | 2h |

### Project Options

**Option A: Intelligent Data Analyst**
```
Data (CSV) → ML (predict trends) → RAG (explain results) → Chat Interface
```
User asks: "Why did sales drop in Q3?"
→ ML model identifies patterns → RAG retrieves context → LLM explains

**Option B: Research Agent**
```
Query → Agent (LangGraph) → Tools (search, analyze) → Report
```
User asks: "Research competitors in market X"
→ Agent searches → analyzes → produces structured report

**Option C: Custom Knowledge Assistant**
```
Docs (PDF) → Embed & Store → RAG + MCP → Chatbot
```
User uploads documents → System creates knowledge base → Chat with data + agent actions

### Deliverables
- Working prototype
- Code repository (clean, documented)
- 1-page architecture diagram
- 5-minute demo video or live presentation
- Reflection: what worked, what didn't, what you'd do differently

---

## What Students Can BUILD After Module 3

1. **Predictive Models:** Regression, classification with scikit-learn
2. **RAG Chatbot:** Chat with custom documents
3. **AI Agent:** LLM that uses tools and takes actions
4. **End-to-end Pipeline:** Data → ML → LLM → Insights

## Skills Acquired

| Skill | Job Relevance |
|-------|---------------|
| Train regression & classification models | High |
| Evaluate models with proper metrics | High |
| Use scikit-learn fluently | High |
| Understand neural networks conceptually | Medium |
| Explain how LLMs work | High |
| Build RAG applications | Very High |
| Use LangChain/LangGraph | High |
| Use MCP for tool integration | High |
| Integrate AI with data pipelines | Very High |
| Recommend AI strategy for business | High |

---

## Key Decisions Made

1. **Rebalanced time:** ML reduced (Ch 3: 14h→10h), LLM increased (Ch 8-9: +4h total)
2. **Neural networks:** Conceptual only, no heavy math
3. **Fine-tuning:** Light coverage integrated in Ch 6 (not separate chapter)
4. **Tools included:** scikit-learn, LangChain (LCEL), LangGraph, MCP, ChromaDB
5. **Chapter 9 bridges Module 1:** From prompt-based agents to code-based agents
6. **Added Chapter 10:** AI Strategy (vendor comparison, costs, decision framework)
7. **Hyperparameter optimization:** Added to Chapter 4

## Explicit OUT of Scope

- Deep learning math (backpropagation derivation)
- PyTorch/TensorFlow from scratch
- Full LLM fine-tuning (only LoRA intro)
- Multi-agent orchestration (CrewAI, AutoGen)
- MLOps / model deployment
- Time series forecasting
- Advanced ensemble methods (XGBoost, LightGBM)

---

## Workflow State

- Phase 1 (Big Picture): ✅ Completed
- Phase 2 (Detailed Concepts): ✅ Completed for Module 2 and Module 3
- Next: Define detailed concepts for Module 4 (Power BI)

## Next Action

Options:
- [DC] Define detailed concepts for Module 4 (Power BI)
- [RC] Review and refine Module 3
