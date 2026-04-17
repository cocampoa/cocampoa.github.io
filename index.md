# Second Brain

> Un sistema que no solo recupera lo que pensaste — detecta estructura en tu pensamiento que tú mismo no has nombrado todavía.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-cocampoa-0077B5?logo=linkedin&logoColor=white)](https://linkedin.com/in/cocampoa)
[![GitHub](https://img.shields.io/badge/GitHub-cocampoa-181717?logo=github&logoColor=white)](https://github.com/cocampoa)

---

## El problema real

Los sistemas de notas y RAG estándar resuelven recuperación. Este proyecto resuelve algo distinto: **cómo mantener coherencia de pensamiento a través del tiempo cuando el pensamiento mismo es no-lineal**.

Tenía cientos de conversaciones con IAs, reflexiones técnicas y filosóficas, compromisos, ideas embrionarias. La información existía. El problema era que no tenía arquitectura — no había forma de ver qué ideas estaban en tensión, qué campos semánticos habían madurado, qué posiciones había desarrollado sin haberlas articulado explícitamente.

Un buscador semántico estándar devuelve chunks relevantes. Lo que construí devuelve **contexto estructurado**: qué proyecto, qué nivel de madurez, si hay compromisos asociados, y — crucialmente — si la respuesta está respaldada por masa real o es inferencia sin ancla.

---

## Qué hace diferente a este sistema

### Campos semánticos emergentes, no categorías manuales

El sistema no pide al usuario que etiquete sus notas. En cambio, detecta clusters de chunks con alta proximidad vectorial (DBSCAN sobre embeddings coseno) y le pide al LLM que nombre el campo que emergió. Un campo como "deseo" — vocabulario difuso, transversal, sin términos técnicos únicos — aparece porque los chunks están cerca en el espacio vectorial, no porque alguien lo clasificó.

### Ciclo de especialización centrífuga

Cuando un campo acumula masa suficiente (≥30 chunks), el sistema calcula sus `core_topics` y expulsa los chunks periféricos hacia `unassigned`. Esos chunks flotantes alimentan el scanner de emergencia. El resultado: los campos se purifican solos y los campos nuevos nacen de los residuos de los viejos.

### Confianza calibrada en el writer

El writer no genera con el mismo tono de certeza siempre. Si el `avg_score` de los chunks recuperados cae bajo `RETRIEVAL_CONFIDENCE_FLOOR`, el prompt incluye una instrucción explícita: marcar la inferencia antes de responder. El sistema sabe cuándo no sabe.

### El bibliotecario vs. el fingerprint

Dos mecanismos distintos para clasificar:
- **Fingerprint service**: similitud coseno entre embeddings para definir el vocabulario semántico de cada campo
- **Bibliotecario (LLM)**: recibe la conversación completa + lista de campos con sus descripciones y decide la asignación con confianza explícita

La eliminación del ajuste coseno local por chunk — con Voyage-3 monoautor el gap entre categorías era ~0.006 — fue una decisión deliberada: ajustar sobre esa diferencia amplificaba ruido, no señal.

---

## Arquitectura

```
input → router ────┬─→ confirm ──────────────────────────────→ END
                   │
                   ├─→ retrieve → writer → [critic] → END    (query)
                   │              writer → habit     → END
                   │              writer → backlog   → END
                   │
                   └─→ ingest → reconcile → retrieve → (mismo ramal)
```

Nueve nodos: `router`, `confirm`, `ingest`, `reconcile`, `retrieve`, `writer`, `habit`, `backlog`, `critic`.

---

## Stack

![Python](https://img.shields.io/badge/Python-3.10+-3776AB?logo=python&logoColor=white)
![FastAPI](https://img.shields.io/badge/FastAPI-009688?logo=fastapi&logoColor=white)
![LangGraph](https://img.shields.io/badge/LangGraph-grafo%20de%20nodos-FF6B35)
![Qdrant](https://img.shields.io/badge/Qdrant-vector%20DB-DC244C)
![Anthropic](https://img.shields.io/badge/Claude-Anthropic-191919)
![Voyage-3](https://img.shields.io/badge/Voyage--3-embeddings-5C6BC0)
![Redis](https://img.shields.io/badge/Redis-cache-DC382D?logo=redis&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)

---

## Estado actual

**1,264 chunks indexados · 3 campos activos · 348 compromisos rastreados**

Campos activos: `second_brain`, `regulacion_vinculos_autenticidad`, `deseo`

---

## Lo que esto revela sobre el problema más amplio

Los sistemas de memoria para IA asumen que el usuario sabe qué quiere recordar. Este proyecto parte de la hipótesis opuesta: **el valor está en lo que no sabías que habías pensado**.

Eso tiene implicaciones directas para cualquier organización que acumule conocimiento en conversaciones, tickets, decisiones informales y documentación dispersa — que es básicamente todas. El problema no es recuperación. Es que la estructura del conocimiento no es visible hasta que algo la hace emerger.

---

## Otros proyectos

[![Streamlit](https://img.shields.io/badge/streamlit__test-análisis%20de%20datos-FF4B4B?logo=streamlit&logoColor=white)](https://github.com/cocampoa/streamlit_test)
[![R](https://img.shields.io/badge/pronosticos__de__negocios-econometría%20en%20R-276DC3?logo=r&logoColor=white)](https://github.com/cocampoa/pronosticos_de_negocios)

---

## Skills

**Análisis de datos** · SQL · Python · R · Excel

**IA & sistemas** · LangGraph · Qdrant · Anthropic API · Docker

**Visualización** · Streamlit · Tableau · Matplotlib · Plotly
