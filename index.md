---
title: Carlos Ocampo
---

# Carlos Ocampo

**Data Analyst · AI Systems Builder**

I build tools that turn conversations and notes into structured, retrievable knowledge.  
I combine classical data analysis with modern AI systems to solve real problems.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-cocampoa-0077B5?logo=linkedin&logoColor=white)](https://linkedin.com/in/cocampoa)
[![GitHub](https://img.shields.io/badge/GitHub-cocampoa-181717?logo=github&logoColor=white)](https://github.com/cocampoa)
[![Español](https://img.shields.io/badge/🌐_Versión-Español-lightgrey)](./es)

---

## Second Brain — Personal AI Memory System

> *A system that reads your conversations, understands what they're about, and stores them so you can retrieve them when you need them.*

**Current state:** 1,264 chunks indexed · 3 active projects · 348 personal commitments tracked

### Tech stack

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white)
![LangGraph](https://img.shields.io/badge/LangGraph-node%20graph-FF6B35)
![Qdrant](https://img.shields.io/badge/Qdrant-vector%20DB-DC244C)
![Anthropic](https://img.shields.io/badge/Claude-Anthropic-191919)
![Voyage--3](https://img.shields.io/badge/Voyage--3-embeddings-5C6BC0)
![Redis](https://img.shields.io/badge/Redis-cache-DC382D?logo=redis&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)

### The problem

I have hundreds of AI conversations, scattered notes, and technical documents. The information exists but isn't retrievable: plain text search loses context, and manually adding metadata doesn't scale.

### The solution

An ingestion pipeline that:

1. **Detects** whether the input is new content or a query
2. **Classifies** the full conversation into the project(s) it belongs to
3. **Chunks** the text by type (chat turns, technical docs, free text)
4. **Enriches** each fragment with intent, commitments, entities, and maturity level
5. **Indexes** in Qdrant with rich metadata and per-project memberships
6. **Retrieves** via semantic search + reranking when a query comes in

### Graph architecture

```
                   ┌─→ confirm ──────────────────────────────→ END
                   │
input → router ────┼─→ retrieve → writer → [critic] → END    (query)
                   │              writer → habit     → END
                   │              writer → backlog   → END
                   │
                   └─→ ingest → reconcile → retrieve → (same branch)
```

Nine nodes: `router`, `confirm`, `ingest`, `reconcile`, `retrieve`, `writer`, `habit`, `backlog`, `critic`.

### Notable design decisions

**The librarian classifies full conversations, not chunks**  
Before ingesting, the system makes a single Claude API call with the full conversation and the list of available projects (with their semantic descriptions). Claude decides which project(s) it belongs to and with what confidence. Each chunk inherits that classification directly. This is separate from fingerprints: `fingerprint_service` uses cosine similarity between embeddings to define each project's vocabulary; the librarian uses that vocabulary as context, but the final decision is made by the LLM.

**Removing the per-chunk local cosine adjustment**  
The previous version re-scored each chunk against fingerprints individually, using that cosine similarity to adjust the inherited conversation memberships. With Voyage-3 embeddings on a single-author corpus, the gap between categories was ~0.006 — adjusting on that difference could flip the primary project. The local adjustment was removed; the librarian's score is the definitive signal.

**Semantic chunking by content type**  
The system detects whether the text is a conversation (chat turns), technical documentation (headers / code blocks), or free text, and applies a different chunking strategy in each case. 600-token limit for chat, 400 for technical and general.

**Fingerprints + emergence**  
Each project has a fingerprint: positive signals and anti-signals that define its semantic vocabulary. The `fingerprint_service` computes `cosine_sim(text_vec, signal_vec) - ANTI_WEIGHT * cosine_sim(text_vec, anti_vec)`. Unassigned chunks are re-clustered periodically: when a critical mass of similar unclassified chunks appears, the system detects an emerging new project.

---

## Other projects

[![Streamlit Projects](https://img.shields.io/badge/streamlit__test-data%20analysis-FF4B4B?logo=streamlit&logoColor=white)](https://github.com/cocampoa/streamlit_test)
[![Business Forecasting](https://img.shields.io/badge/pronosticos__de__negocios-econometrics%20in%20R-276DC3?logo=r&logoColor=white)](https://github.com/cocampoa/pronosticos_de_negocios)

---

## Skills

**Data analysis**  
![SQL](https://img.shields.io/badge/SQL-4479A1?logo=postgresql&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![R](https://img.shields.io/badge/R-276DC3?logo=r&logoColor=white)
![Excel](https://img.shields.io/badge/Excel-217346?logo=microsoft-excel&logoColor=white)

**AI & systems**  
![LangGraph](https://img.shields.io/badge/LangGraph-FF6B35)
![Qdrant](https://img.shields.io/badge/Qdrant-DC244C)
![Anthropic API](https://img.shields.io/badge/Anthropic_API-191919)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)

**Visualization**  
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?logo=streamlit&logoColor=white)
![Tableau](https://img.shields.io/badge/Tableau-E97627?logo=tableau&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C)
![Plotly](https://img.shields.io/badge/Plotly-3F4F75?logo=plotly&logoColor=white)
