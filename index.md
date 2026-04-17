# Second Brain By Carlos Ocampo
**AI Systems Builder**


> A system that doesn't just retrieve what you've thought — it detects structure in your thinking that you haven't named yet.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-cocampoa-0077B5?logo=linkedin&logoColor=white)](https://linkedin.com/in/cocampoa)
[![GitHub](https://img.shields.io/badge/GitHub-cocampoa-181717?logo=github&logoColor=white)](https://github.com/cocampoa)
[![Español](https://img.shields.io/badge/🌐_Versión-Español-lightgrey)](/es/)
---

## The real problem

Standard note-taking and RAG systems solve retrieval. This project solves something different: **how to maintain coherence of thought over time when thinking itself is non-linear**.

I had hundreds of AI conversations, technical and philosophical reflections, commitments, embryonic ideas. The information existed. The problem was that it had no architecture: there was no way to see which ideas were in tension, which semantic fields had matured, which positions I had developed without explicitly articulating them.

A standard semantic search returns relevant chunks. What I built returns **structured context**: which project, which maturity level, whether there are associated commitments, and — critically — whether the response is backed by real mass or is inference without anchor.

---

## Current state

**1,264 indexed chunks · 3 active fields · 348 tracked commitments**

Active fields: `second_brain` · `regulacion_vinculos_autenticidad` · `deseo`

---

## What makes this system different

### 1. The system knows itself

The pipeline includes `self_ingest`: the system processes its own source code as corpus. `ingest_node.py`, `retrieve_node.py`, `emergence_service.py` live indexed alongside the rest of the knowledge. When asked about its own architecture, it retrieves the actual state of its files — not a stale description.

This has a non-trivial consequence: the system can propose improvements to its own design based on what it knows about itself. It's not simulated introspection — it's RAG applied to the code of the system doing the RAG.

### 2. Emergent semantic fields, not manual categories

The system doesn't ask the user to label their notes. It detects clusters of chunks with high vector proximity (DBSCAN over cosine embeddings) and asks the LLM to name the field that emerged.

A field like `desire` — diffuse vocabulary, cross-cutting, no unique technical terms — appears because the chunks are geometrically close in vector space, not because someone classified them. The field existed geometrically before it existed nominally.

### 3. Centrifugal specialization cycle

When a field accumulates enough mass (≥30 chunks), the system calculates its `core_topics` and expels peripheral chunks to `unassigned`. Those floating chunks feed the emergence scanner. Fields self-purify and new fields are born from the residue of old ones.

### 4. Calibrated confidence: the system knows when it doesn't know

The writer doesn't generate with the same tone of certainty always. If the `avg_score` of retrieved chunks falls below `RETRIEVAL_CONFIDENCE_FLOOR` (configurable in `.env`), the prompt includes an explicit instruction: flag the inference before responding.

---

## A moment that reveals the potential — and the risk

During development, I ingested a text about ethics as optimization of desires. Without explicitly requesting it, the system generated a sophisticated critique of that very idea: it argued that treating desire as an optimizable function is an epistemological trap because observation collapses the function, desires are not aggregable, and whoever defines the metric defines reality.

The retrieve_log revealed something unsettling: **the highest-scoring chunks didn't come from the "desire" field — they came from the system's own source code and a contract negotiation document**. The cosine separation between them was ~0.004. The system had generated a coherent philosophical position by mixing personal reflections with its own source code.

That's not just a classification bug. It's the most interesting and dangerous property of the system: it can articulate latent positions the user hasn't consciously formulated, but it can also fabricate coherence in your voice without real backing. The difference between the two cases feels identical from the inside.

That diagnosis led directly to three architectural changes: confidence floor in the writer, bootstrapping the `desire` field from real corpus, and an emergence post-hook so new fields are born with a fingerprint, not empty.

---

## Where this is going: Telos

Second Brain solves the memory problem. The next project — **Telos** — solves the direction problem.

Telos emerged autonomously as a field within Second Brain itself: a mass of chunks about purpose, behavioral patterns, and value systems that the scanner detected as a coherent cluster before I named it.

The hypothesis: if Second Brain models *what you've thought*, Telos models *where you're moving* — inferred not from declarations but from patterns of sustained action over time. A system that distinguishes conscious desire from operative desire, and detects when you act in the opposite direction of what you say you want.

The probabilistic desire field is the first Telos module under construction.

---

## Architecture

```
input → router ────┬─→ confirm ──────────────────────────────→ END
                   │
                   ├─→ retrieve → writer → [critic] → END    (query)
                   │              writer → habit     → END
                   │              writer → backlog   → END
                   │
                   └─→ ingest → reconcile → retrieve → (same branch)
```

Nine nodes: `router` · `confirm` · `ingest` · `reconcile` · `retrieve` · `writer` · `habit` · `backlog` · `critic`

**Notable design decisions**

The librarian classifies entire conversations, not chunks — a single Claude call with the full conversation and available fields list. Each chunk inherits that classification. Removing local cosine adjustment per chunk was deliberate: with Voyage-3 on a single-author corpus, the gap between categories was ~0.006; adjusting on that difference amplified noise, not signal.

Chunking is semantic by content type: conversation (600 tokens), technical documentation (400 tokens), free text (400 tokens).

---

## Stack

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white)
![LangGraph](https://img.shields.io/badge/LangGraph-FF6B35)
![Qdrant](https://img.shields.io/badge/Qdrant-DC244C)
![Anthropic](https://img.shields.io/badge/Claude-Anthropic-191919)
![Voyage-3](https://img.shields.io/badge/Voyage--3-5C6BC0)
![Redis](https://img.shields.io/badge/Redis-DC382D?logo=redis&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)

---

## What this reveals about the broader problem

Memory systems for AI assume the user knows what they want to remember. This project starts from the opposite hypothesis: **the value is in what you didn't know you had thought**.

That has direct implications for any organization that accumulates knowledge in conversations, tickets, informal decisions, and dispersed documentation. The problem isn't retrieval. It's that the structure of knowledge isn't visible until something makes it emerge.

---

## Other projects

[![Streamlit](https://img.shields.io/badge/streamlit__test-data%20analysis-FF4B4B?logo=streamlit&logoColor=white)](https://github.com/cocampoa/streamlit_test)
[![R](https://img.shields.io/badge/pronosticos__de__negocios-econometrics%20in%20R-276DC3?logo=r&logoColor=white)](https://github.com/cocampoa/pronosticos_de_negocios)

---

## Skills

**Data analysis** · SQL · Python · R · Excel  
**AI & systems** · LangGraph · Qdrant · Anthropic API · Docker  
**Visualization** · Streamlit · Tableau · Matplotlib · Plotly
