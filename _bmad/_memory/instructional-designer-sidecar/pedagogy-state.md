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
| 1 - Prompt Engineering | Not designed | — | — |
| 2 - Pipeline Data | ✅ Designed | Progressive PBL with scaffolded milestones | "Mon Pipeline Data de A à Z" |
| 3 - ML & LLMs | Not designed | — | — |
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

## Next Action

Design teaching approach for remaining modules using command **[DM]**:
- Module 1: Prompt Engineering
- Module 3: ML & LLMs (needs Domain Expert first)
- Module 4: Power BI (needs Domain Expert first)
- Final Capstone

---

## Session History

- **2026-01-14:** Session 1 completed — Overall methodology selected (PBL + Direct Instruction scaffolding, Project-focused assessment, Separate capstone)
- **2026-01-14:** Module 2 teaching design completed — Progressive PBL with scaffolded milestones, project "Mon Pipeline Data de A à Z" (student choice dataset, instructor approval, class demo day)
