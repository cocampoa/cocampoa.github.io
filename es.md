---
title: Carlos Ocampo
permalink: /es/
---

# Carlos Ocampo

**Analista de Datos · Desarrollador de Sistemas de IA**

Construyo herramientas que convierten conversaciones y notas en conocimiento estructurado y recuperable.  
Combino análisis de datos con sistemas modernos de IA para resolver problemas reales.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-cocampoa-0077B5?logo=linkedin&logoColor=white)](https://linkedin.com/in/cocampoa)
[![GitHub](https://img.shields.io/badge/GitHub-cocampoa-181717?logo=github&logoColor=white)](https://github.com/cocampoa)
[![English](https://img.shields.io/badge/🌐_Version-English-lightgrey)](../)

---

## Second Brain — Sistema de Memoria Personal con IA

> *Un sistema que lee tus conversaciones, entiende de qué tratan, y las guarda de forma que puedas recuperarlas cuando las necesites.*

**Estado actual:** 1,264 chunks indexados · 3 campos activos · 348 compromisos personales rastreados

### Stack tecnológico

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white)
![LangGraph](https://img.shields.io/badge/LangGraph-grafo%20de%20nodos-FF6B35)
![Qdrant](https://img.shields.io/badge/Qdrant-vector%20DB-DC244C)
![Anthropic](https://img.shields.io/badge/Claude-Anthropic-191919)
![Voyage--3](https://img.shields.io/badge/Voyage--3-embeddings-5C6BC0)
![Redis](https://img.shields.io/badge/Redis-cache-DC382D?logo=redis&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)

### El problema

Tengo cientos de conversaciones con IAs, notas dispersas y documentos técnicos. La información existe, pero no es recuperable: buscar en texto plano pierde el contexto, y agregar metadatos a mano no escala.

### La solución

Un pipeline de ingestión que:

1. **Detecta** si el input es contenido nuevo o una consulta
2. **Clasifica** la conversación completa a qué proyecto(s) pertenece
3. **Trocea** el texto según su tipo (chat, documentación técnica, texto libre)
4. **Enriquece** cada fragmento con intent, compromisos, entidades y nivel de madurez
5. **Indexa** en Qdrant con metadatos ricos y membresías por proyecto
6. **Recupera** con búsqueda semántica + reranking cuando se hace una consulta

### Arquitectura del grafo

```
                   ┌─→ confirm ──────────────────────────────→ END
                   │
input → router ────┼─→ retrieve → writer → [critic] → END    (query)
                   │              writer → habit     → END
                   │              writer → backlog   → END
                   │
                   └─→ ingest → reconcile → retrieve → (mismo ramal)
```

Nueve nodos: `router`, `confirm`, `ingest`, `reconcile`, `retrieve`, `writer`, `habit`, `backlog`, `critic`.

### Decisiones de diseño notables

**El bibliotecario clasifica conversaciones completas, no chunks**  
Antes de ingestar, el sistema hace una sola llamada a Claude con la conversación completa y la lista de proyectos disponibles (con sus descripciones semánticas). Claude decide a qué proyecto(s) pertenece y con qué confianza. Cada chunk hereda esa clasificación directamente. Esto es distinto a los fingerprints: el `fingerprint_service` usa similitud coseno entre embeddings para definir el vocabulario de cada proyecto; el bibliotecario usa ese vocabulario como contexto, pero la decisión final la toma el LLM.

**Eliminación del ajuste coseno local por chunk**  
La versión anterior re-puntuaba cada chunk contra los fingerprints de forma individual y usaba esa similitud coseno para ajustar las membresías heredadas de la conversación. Con embeddings Voyage-3 en corpus monoautor, el gap entre categorías era ~0.006 — ajustar sobre esa diferencia podía flipar el campo primario. Se eliminó el ajuste local; el score del bibliotecario es la señal definitiva.

**Chunking semántico por tipo de contenido**  
El sistema detecta si el texto es una conversación (turnos de chat), documentación técnica (headers / bloques de código) o texto libre, y aplica una estrategia de chunking diferente en cada caso. Límite de 600 tokens para chat, 400 para técnico y general.

**Sistema de fingerprints + emergence**  
Cada proyecto tiene un fingerprint: señales positivas y anti-señales que definen su vocabulario semántico. El `fingerprint_service` calcula `cosine_sim(text_vec, signal_vec) - ANTI_WEIGHT * cosine_sim(text_vec, anti_vec)`. Los chunks sin proyecto asignado se reclusteran periódicamente: cuando aparece una masa crítica de chunks similares sin clasificar, el sistema detecta un nuevo proyecto emergente.

---

## Otros proyectos

[![Streamlit Projects](https://img.shields.io/badge/streamlit__test-análisis%20de%20datos-FF4B4B?logo=streamlit&logoColor=white)](https://github.com/cocampoa/streamlit_test)
[![Pronósticos R](https://img.shields.io/badge/pronosticos__de__negocios-econometría%20en%20R-276DC3?logo=r&logoColor=white)](https://github.com/cocampoa/pronosticos_de_negocios)

---

## Skills

**Análisis de datos**  
![SQL](https://img.shields.io/badge/SQL-4479A1?logo=postgresql&logoColor=white)
![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![R](https://img.shields.io/badge/R-276DC3?logo=r&logoColor=white)
![Excel](https://img.shields.io/badge/Excel-217346?logo=microsoft-excel&logoColor=white)

**IA & sistemas**  
![LangGraph](https://img.shields.io/badge/LangGraph-FF6B35)
![Qdrant](https://img.shields.io/badge/Qdrant-DC244C)
![Anthropic API](https://img.shields.io/badge/Anthropic_API-191919)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)

**Visualización**  
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?logo=streamlit&logoColor=white)
![Tableau](https://img.shields.io/badge/Tableau-E97627?logo=tableau&logoColor=white)
![Matplotlib](https://img.shields.io/badge/Matplotlib-11557C)
![Plotly](https://img.shields.io/badge/Plotly-3F4F75?logo=plotly&logoColor=white)
