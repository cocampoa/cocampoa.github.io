# Carlos Ocampo

**Data Analyst · AI Systems Builder**

Construyo herramientas que convierten conversaciones y notas en conocimiento estructurado y recuperable.  
Combino análisis de datos clásico con sistemas modernos de IA para resolver problemas reales.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-cocampoa-0077B5?logo=linkedin&logoColor=white)](https://linkedin.com/in/cocampoa)
[![GitHub](https://img.shields.io/badge/GitHub-cocampoa-181717?logo=github&logoColor=white)](https://github.com/cocampoa)

---

## Second Brain — Sistema de Memoria Personal con IA

> *Un sistema que lee tus conversaciones, entiende de qué tratan, y las guarda de forma que puedas recuperarlas cuando las necesites.*

### Stack tecnológico

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white)
![LangGraph](https://img.shields.io/badge/LangGraph-grafo%20de%20nodos-FF6B35)
![Qdrant](https://img.shields.io/badge/Qdrant-vector%20DB-DC244C?logo=qdrant&logoColor=white)
![Anthropic](https://img.shields.io/badge/Claude-Anthropic-191919?logo=anthropic&logoColor=white)
![Redis](https://img.shields.io/badge/Redis-cache-DC382D?logo=redis&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)

### El problema

Tengo cientos de conversaciones con IAs, notas dispersas y documentos técnicos.  
La información existe, pero no es recuperable. Buscar en texto plano no es suficiente — el contexto se pierde.

### La solución

Un pipeline de ingestión que:

1. **Lee** conversaciones y documentos (chat, técnico, texto libre)
2. **Entiende** de qué tratan — clasifica por intent (idea, tarea, emoción, arquitectura, etc.)
3. **Asigna** cada fragmento al proyecto correcto usando un sistema de fingerprints semánticos
4. **Indexa** en una base de datos vectorial (Qdrant) con metadatos ricos
5. **Recupera** con búsqueda semántica + reranking cuando se hace una consulta

### Arquitectura del grafo

```
input → router → ingest → reconcile → retrieve → writer → respuesta
           ↑
     bibliotecario: clasifica la conversación completa
     y asigna membresías a proyectos antes de ingestar
```

### Decisiones de diseño notables

**Bibliotecario como señal única de clasificación**  
El sistema usa un clasificador LLM ("bibliotecario") que lee la conversación completa y decide a qué proyecto(s) pertenece. Los chunks individuales heredan esa clasificación. Se descartó el ajuste local por similitud coseno porque con embeddings Voyage-3 en corpus monoautor el gap entre categorías es ~0.006 — ajustar sobre esa diferencia era ruido puro.

**Chunking semántico por tipo de contenido**  
El chunker detecta si el texto es una conversación (turnos), documentación técnica (headers/código), o texto libre, y aplica una estrategia diferente en cada caso.

**Sistema de fingerprints por proyecto**  
Cada proyecto tiene un fingerprint (señales positivas + anti-señales) que define su vocabulario semántico. El bibliotecario usa estos fingerprints para clasificar sin necesidad de re-entrenar ningún modelo.

**Emergence: nuevos proyectos que nacen solos**  
Chunks sin proyecto asignado se reclusteran periódicamente. Cuando aparece una masa crítica de chunks similares sin clasificar, el sistema detecta un nuevo proyecto emergente.

---

## Data Analysis & Visualización

### Streamlit Projects

Proyectos de análisis de datos con interfaces interactivas construidas en Streamlit.

![Python](https://img.shields.io/badge/Python-3776AB?logo=python&logoColor=white)
![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?logo=streamlit&logoColor=white)
![Jupyter](https://img.shields.io/badge/Jupyter-F37626?logo=jupyter&logoColor=white)

[Ver repositorio →](https://github.com/cocampoa/streamlit_test)

---

### Pronósticos de Negocios

Guía de referencia rápida para análisis de datos, econometría y series de tiempo en R.

![R](https://img.shields.io/badge/R-276DC3?logo=r&logoColor=white)

[Ver repositorio →](https://github.com/cocampoa/pronosticos_de_negocios)

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
