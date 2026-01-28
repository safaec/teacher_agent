# Pedagogy State

## Current Course

**Boot Camp Data & Intelligence Artificielle**

| Module | Title | Duration |
|--------|-------|----------|
| 1 | Prompt Engineering : utiliser efficacement l'IA | TBD |
| 2 | Pipeline Data : de la collecte à l'analyse en cloud | ~85-105h |
| 3 | Intelligence Artificielle : ML classique aux LLMs | TBD |
| 4 | Visualisation décisionnelle avec Power BI | TBD |
| **Final** | **Capstone Project** | TBD |

**Target Audience:** Career changers (profils en reconversion) with Python, Excel, SQL basics

---

## Overall Methodology

**Primary Approach:** Project-Based Learning (PBL)

**Scaffolding:** Direct Instruction for complex theoretical concepts (ML theory, cloud architecture, LLM mechanisms)

**Rationale:**
- Career changers need portfolio-ready work to demonstrate competencies
- Technical bootcamp requires hands-on practice
- Situated learning theory: skills transfer better in authentic contexts
- Direct instruction scaffolding ensures solid foundations for steep learning curves

---

## Assessment Philosophy

**Approach:** Project-Focused

| Component | Weight | Description |
|-----------|--------|-------------|
| Module Projects | 70-80% | One substantial project per module practicing module concepts |
| Labs & Participation | 20-30% | Hands-on exercises, engagement, formative assessment |

**No formal exams** — competency demonstrated through project deliverables.

---

## Capstone Structure

**Decision:** Separate Final Integrative Project (after Module 4)

**Rationale:** Module 4 has its own learning objectives (Power BI mastery). A separate capstone allows:
- Each module to have dedicated projects focused on module skills
- Students to demonstrate full integration of all 4 modules
- Student choice in domain/topic for personal relevance

**Course Flow:**
```
M1 (Prompt) → M2 (Data) → M3 (ML/LLM) → M4 (Power BI) → Final Capstone
     ↓            ↓            ↓             ↓              ↓
  Project 1   Project 2    Project 3     Project 4    Integrative Project
```

---

## Module Teaching Designs

| Module | Status | Teaching Approach | Project |
|--------|--------|-------------------|---------|
| 1 - Prompt Engineering | ✅ Designed | (Designed separately) | (See Module 1 docs) |
| 2 - Pipeline Data | ✅ Designed | Progressive PBL with scaffolded milestones | "Mon Pipeline Data de A à Z" |
| 3 - ML & LLMs | ✅ Designed | Two-Track PBL with Conceptual Bridge | "Mon Modèle ML" + "Mon App LLM" |
| 4 - Power BI | Not designed | — | — |
| Final Capstone | Not designed | — | — |

---

## Module 2 — Pipeline Data (Teaching Design)

### Class Context
- **Class size:** 6 students (intimate group, whole-class collaboration)
- **Audience:** Career changers with Python, Excel, SQL basics

### Teaching Approach
**Progressive Project-Based Learning with Scaffolded Milestones**

Each chapter builds a "layer" that students immediately apply. By Chapter 10, they've practiced each skill individually — the fil rouge integrates everything.

**Teaching Flow:**
```
Ch 1-2 (Intro + Extraction)    → Mini-project: Extract & profile a real dataset
Ch 3 (Cloud)                   → Lab: Set up AWS + read from S3
Ch 4-5 (EDA + Cleaning)        → Mini-project: Quality report + cleaning pipeline
Ch 6-7 (Transform + Analyze)   → Mini-project: Build analysis-ready dataset
Ch 8-9 (Visualize + Load)      → Lab: Visualizations + upload to S3
Ch 10 (Fil Rouge)              → MODULE PROJECT: Complete pipeline capstone
Ch 11 (Synthesis)              → Reflection + aide-mémoire mastery
```

### Module Project: "Mon Pipeline Data de A à Z"

**Dataset Selection:** Student choice (any domain) — instructor approval required

**Core Requirements (Mandatory):**

| # | Deliverable |
|---|-------------|
| 1 | Dataset selection (min. 1,000 rows, 10+ columns) — approved by instructor |
| 2 | Multi-source extraction (at least 2 different sources) |
| 3 | Quality audit report (missing %, duplicates, outliers, type issues) |
| 4 | Cleaning pipeline notebook with documented decisions |
| 5 | Transformation: 1 merge/join + 1 aggregation + 1 derived feature |
| 6 | EDA analytique: 5 visualizations answering business questions + interpretation |
| 7 | Cloud upload to S3 with proper structure (raw/, processed/, final/) |
| 8 | Data dictionary (columns, types, meaning, transformations) |
| 9 | 3-minute pitch at class demo day |

**Optional Extensions (for students who want to go further):**
- API integration (advanced extraction)
- Advanced EDA (correlation analysis, multi-dimensional segmentation)
- Parquet export with CSV comparison
- Automated quality check functions
- Cross-dataset joins
- AI-assisted documentation

**Project Timeline:**
- Week 1: Dataset selection + Extraction + S3 setup → Checkpoint: Dataset approved
- Week 2: Quality audit + Cleaning + Transformation → Checkpoint: Pipeline reviewed
- Week 3: EDA + Visualization + Cloud upload + Pitch → Final: Demo day

### Activities by Chapter

| Ch | Title | Primary Activity | Supporting Activities |
|----|-------|------------------|----------------------|
| 1 | Pipeline Overview | Concept Map (ETL/ELT flow) | Discussion: data pipelines in real life |
| 2 | Extraction | Guided Lab (CSV, SQL, API) | Pair work: debug extraction code |
| 3 | Cloud (AWS S3) | Hands-on Setup (bucket, credentials) | Demo + follow-along, checklist |
| 4 | EDA Diagnostique | Dataset Autopsy (dirty dataset) | Group: compare findings, prioritize |
| 5 | Nettoyage | Cleaning Challenge | Peer code review |
| 6 | Transformation | Puzzle Lab (business questions) | Live coding with group input |
| 7 | EDA Analytique | Hypothesis Hunt | Discussion: correlation ≠ causation |
| 8 | Visualisation | Viz Critique | Mini-challenge: best viz contest |
| 9 | Cloud Upload | Pipeline Completion | Checklist validation |
| 10 | Fil Rouge | PROJECT WORK | Checkpoints + office hours |
| 11 | Conclusion | Reflection + Quiz | **Class Demo Day** |

### Assessment

**Grading Breakdown:**
| Component | Weight |
|-----------|--------|
| Module Project | 70% |
| Labs & Participation | 20% |
| Demo Day Pitch | 10% |

**Project Rubric (70%):**

| Criterion | Weight | Excellent (90-100%) | Good (70-89%) | Needs Work (50-69%) | Insufficient (<50%) |
|-----------|--------|---------------------|---------------|---------------------|---------------------|
| Extraction | 10% | 2+ sources, clean code, documented | 2 sources, minor issues | 1 source only | Not functional |
| Quality Audit | 10% | Complete, all dimensions | Most dimensions | Surface-level | Missing |
| Cleaning Pipeline | 15% | All issues addressed, justified | Most issues, some justification | Basic, poor docs | Incomplete |
| Transformation | 15% | 3+ transformations, elegant | 3 transformations, functional | <3 or poor quality | Missing key ones |
| EDA & Viz | 15% | 5+ insightful viz, story arc | 5 viz, adequate interpretation | Fewer, weak interpretation | Missing |
| Cloud Upload | 10% | Proper structure, verified | Uploaded, structure issues | Uploaded with errors | Not uploaded |
| Data Dictionary | 5% | Complete, professional | Minor gaps | Incomplete | Missing |

**Demo Day Pitch Rubric (10%):**
- Clarity (explained in 3 min): /3
- Insights (meaningful findings): /3
- Decisions (justified choices): /2
- Next Steps (articulated future work): /2

---

## Module 3 — Intelligence Artificielle (Teaching Design)

### Class Context
- **Class size:** 4 students (intimate group, personalized feedback possible)
- **Prerequisites:** Module 1 (prompt engineering), Module 2 (data pipeline, cleaned datasets)
- **Primary environment:** VS Code + Jupyter, Colab for GPU demos (Ch 5-6 only)
- **API access:** OpenAI/Anthropic keys available
- **Vector DB:** ChromaDB (local, no cloud needed)

### Teaching Approach
**Two-Track Project-Based Learning with Conceptual Bridge**

Students build two distinct applications across the module, then integrate skills in a final project.

```
Track 1: ML Practitioner (Ch 1-4)
├── Input: YOUR Module 2 cleaned dataset
├── Output: ML Analysis Report + trained model
└── Duration: ~40h

Bridge: Deep Learning Concepts (Ch 5-6)
├── Focus: Understanding (no project)
├── "How we got from linear regression to ChatGPT"
└── Duration: ~22h

Track 2: LLM Builder (Ch 7-10)
├── Input: Document corpus (PDFs, text files)
├── Output: Working LLM application (RAG or Agent)
└── Duration: ~44h

Final Project (Ch 11)
├── Integrate ML + LLM (student choice from 3 options)
└── Duration: ~14h
```

### Track 1 Mini-Project: "Mon Modèle ML"

**Connection to Module 2:** Students use their OWN cleaned dataset from "Mon Pipeline Data de A à Z"

**Requirements:**

| # | Deliverable |
|---|-------------|
| 1 | Problem Definition: Formulate a prediction question (regression or classification) |
| 2 | Baseline Model: Train simple model (Linear/Logistic Regression), document performance |
| 3 | Improved Model: Train Random Forest or other algorithm, compare to baseline |
| 4 | Evaluation Report: Metrics (R²/RMSE or Precision/Recall/F1), confusion matrix if classification |
| 5 | Hyperparameter Tuning: GridSearchCV or RandomizedSearchCV with justification |
| 6 | Interpretation: What do the results MEAN? Feature importance, business insights |
| 7 | Overfitting Analysis: Train vs test scores, cross-validation results |

**Timeline:**
- Week 1 (Ch 1-2): Problem defined, data prepared, baseline model
- Week 2 (Ch 3-4): Improved model, evaluation, tuning, final report

### Track 2 Mini-Project: "Mon App LLM"

**Fresh Start:** Students choose a document corpus (not Module 2 data)

**Requirements:**

| # | Deliverable |
|---|-------------|
| 1 | Corpus Selection: 5-10 documents (PDFs, markdown, or text) on a topic of interest |
| 2 | Chunking Strategy: Document how you split documents, justify chunk size/overlap |
| 3 | Vector Store: ChromaDB setup with embedded documents |
| 4 | RAG Pipeline: Working retrieval + generation chain (LangChain LCEL) |
| 5 | Prompt Engineering: System prompt with context injection, citation requirement |
| 6 | Agent Feature (optional): Add at least one tool (web search, calculator, or custom) |
| 7 | Demo: 5 example queries with responses, showing retrieval quality |

**Timeline:**
- Week 4 (Ch 7-8): Corpus selected, chunked, embedded, basic RAG working
- Week 5 (Ch 9-10): Agent features added, demo prepared, strategy doc

### Final Project (Ch 11)

**Student Choice from 3 Options:**

| Option | Description |
|--------|-------------|
| A: Intelligent Data Analyst | Data → ML (predict) → RAG (explain) → Chat Interface |
| B: Research Agent | Query → Agent (LangGraph) → Tools (search, analyze) → Report |
| C: Custom Knowledge Assistant | Docs → Embed & Store → RAG + MCP → Chatbot |

### Activities by Chapter

| Ch | Title | Hours | Primary Activity | Supporting Activities |
|----|-------|-------|------------------|----------------------|
| **TRACK 1: ML PRACTITIONER** |||||
| 1 | Introduction à l'IA | 8h | Use Case Gallery: Analyze 5 real AI products | Discussion: "What can YOUR data solve?" |
| 2 | Le Workflow ML | 10h | Pipeline Lab: Build complete sklearn pipeline | Pair debug: fix data leakage |
| 3 | Algorithmes ML | 10h | Algorithm Tournament: Same data, 4 algorithms | Visual: draw decision tree by hand |
| 4 | Évaluation & Optimisation | 12h | Diagnostic Challenge: Diagnose & fix overfitting | GridSearch competition |
| **BRIDGE: CONCEPTS** |||||
| 5 | Réseaux de Neurones | 10h | Playground Exploration: TensorFlow Playground | Demo: train MNIST in 10 lines |
| 6 | Transformers & Fine-tuning | 12h | Attention Visualization: See BERT attention | Colab: fine-tune sentiment classifier |
| **TRACK 2: LLM BUILDER** |||||
| 7 | Embeddings & Vecteurs | 10h | Semantic Search Lab: Build search over sentences | Challenge: find "odd one out" |
| 8 | RAG | 14h | RAG Debugging: Fix broken RAG pipeline | Compare: similarity vs MMR |
| 9 | Agents & Automatisation | 14h | Tool Builder Workshop: Create 3 custom tools | LangGraph: conditional workflow |
| 10 | Stratégie IA | 6h | Case Study Debate: Argue AI approaches | Cost calculator exercise |
| **FINAL PROJECT** |||||
| 11 | Projet Intégrateur | 14h | PROJECT WORK | Checkpoints + peer feedback |

### Assessment

**Grading Breakdown:**

| Component | Weight |
|-----------|--------|
| Track 1: Mon Modèle ML | 30% |
| Track 2: Mon App LLM | 35% |
| Labs & Participation | 15% |
| Final Project | 20% |

**Track 1 Rubric (30%):**

| Criterion | Weight | Excellent (90-100%) | Good (70-89%) | Needs Work (50-69%) |
|-----------|--------|---------------------|---------------|---------------------|
| Problem Definition | 10% | Clear, business-relevant | Clear but generic | Vague |
| Model Training | 20% | Baseline + improved, clean code | Both models, minor issues | Only one model |
| Evaluation | 25% | All metrics, cross-validation | Most metrics | Few metrics |
| Hyperparameter Tuning | 15% | GridSearch with justification | Done, weak justification | Minimal |
| Interpretation | 20% | Insightful, features explained | Adequate | Surface-level |
| Code Quality | 10% | Clean, documented | Minor issues | Messy |

**Track 2 Rubric (35%):**

| Criterion | Weight | Excellent (90-100%) | Good (70-89%) | Needs Work (50-69%) |
|-----------|--------|---------------------|---------------|---------------------|
| Corpus & Chunking | 15% | 5+ docs, justified strategy | Adequate | Few docs |
| Vector Store | 15% | ChromaDB working properly | Working, minor issues | Partially working |
| RAG Pipeline | 25% | LCEL chain, citations | Working, adequate | Basic, poor retrieval |
| Prompt Engineering | 15% | Effective system prompt | Adequate | Basic |
| Agent/Tools | 10% | 1+ custom tools | Attempted | — |
| Demo Quality | 20% | 5+ queries, shows limits | Adequate | Minimal |

**Final Project Rubric (20%):**

| Criterion | Weight |
|-----------|--------|
| Integration (ML + LLM work together) | 30% |
| Functionality | 25% |
| Code Quality | 15% |
| Presentation | 20% |
| Reflection | 10% |

### Checkpoints

| Week | Checkpoint | Reviewed |
|------|------------|----------|
| Week 1 | Track 1 Kickoff | Problem definition approved |
| Week 2 | Track 1 Complete | ML model delivered |
| Week 3 | Bridge Complete | Conceptual understanding |
| Week 4 | Track 2 Kickoff | Corpus selected |
| Week 5 | Track 2 Complete | LLM app delivered |
| Week 6 | Final Project | Demo day |

### Demo Day (End of Module)

| Time | Activity |
|------|----------|
| 5 min | Student presents final project |
| 3 min | Live demo |
| 5 min | Q&A from peers + instructor |
| 2 min | Feedback |

**Total:** ~60 min for all 4 students

---

## Next Action

Design teaching approach for remaining modules using command **[DM]**:
- Module 4: Power BI (needs Domain Expert first)
- Final Capstone

---

## Session History

- **2026-01-14:** Session 1 completed — Overall methodology selected (PBL + Direct Instruction scaffolding, Project-focused assessment, Separate capstone)
- **2026-01-14:** Module 2 teaching design completed — Progressive PBL with scaffolded milestones, project "Mon Pipeline Data de A à Z" (student choice dataset, instructor approval, class demo day)
- **2026-01-26:** Module 1 noted as designed (completed separately by teacher)
- **2026-01-26:** Module 3 teaching design completed — Two-Track PBL with Conceptual Bridge: Track 1 "Mon Modèle ML" (uses Module 2 data, 30%), Track 2 "Mon App LLM" (fresh corpus, RAG/agents, 35%), Final Project (student choice, 20%). 4 students, VS Code primary, Colab for GPU only.
