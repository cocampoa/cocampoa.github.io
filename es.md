---
title: Carlos Ocampo
permalink: /es/
---
# Second Brain

> Un sistema que no solo recupera lo que pensaste — detecta estructura en tu pensamiento que tú mismo no has nombrado todavía.

[![LinkedIn](https://img.shields.io/badge/LinkedIn-cocampoa-0077B5?logo=linkedin&logoColor=white)](https://linkedin.com/in/cocampoa)
[![GitHub](https://img.shields.io/badge/GitHub-cocampoa-181717?logo=github&logoColor=white)](https://github.com/cocampoa)
[![English](https://img.shields.io/badge/🌐_Version-English-lightgrey)](../)

---

## El problema real

Los sistemas de notas y RAG estándar resuelven recuperación. Este proyecto resuelve algo distinto: **cómo mantener coherencia de pensamiento a través del tiempo cuando el pensamiento mismo es no-lineal**.

Tenía cientos de conversaciones con IAs, reflexiones técnicas y filosóficas, compromisos, ideas embrionarias. La información existía. El problema era que no tenía arquitectura: no había forma de ver qué ideas estaban en tensión, qué campos semánticos habían madurado, qué posiciones había desarrollado sin haberlas articulado explícitamente.

Un buscador semántico estándar devuelve chunks relevantes. Lo que construí devuelve **contexto estructurado**: qué proyecto, qué nivel de madurez, si hay compromisos asociados, y — crucialmente — si la respuesta está respaldada por masa real o es inferencia sin ancla.

---

## Estado actual

**1,264 chunks indexados · 3 campos activos · 348 compromisos rastreados**

Campos activos: `second_brain` · `regulacion_vinculos_autenticidad` · `deseo`

---

## Lo que hace diferente a este sistema

### 1. El sistema se conoce a sí mismo

El pipeline incluye `self_ingest`: el sistema procesa su propio código fuente como corpus. `ingest_node.py`, `retrieve_node.py`, `emergence_service.py` viven indexados junto al resto del conocimiento. Cuando se le pregunta sobre su arquitectura, recupera el estado real de los archivos — no una descripción desactualizada.

Esto tiene una consecuencia no trivial: el sistema puede proponer mejoras a su propio diseño basándose en lo que sabe de sí mismo. No es introspección simulada — es RAG aplicado al código del sistema que hace el RAG.

### 2. Campos semánticos emergentes, no categorías manuales

El sistema no pide al usuario que etiquete sus notas. Detecta clusters de chunks con alta proximidad vectorial (DBSCAN sobre embeddings coseno) y le pide al LLM que nombre el campo que emergió.

Un campo como `deseo` — vocabulario difuso, transversal, sin términos técnicos únicos — aparece porque los chunks están cerca en el espacio vectorial, no porque alguien lo clasificó. El campo existía geométricamente antes de existir nominalmente.

### 3. Ciclo de especialización centrífuga

Cuando un campo acumula masa suficiente (≥30 chunks), el sistema calcula sus `core_topics` y expulsa los chunks periféricos hacia `unassigned`. Esos chunks flotantes alimentan el scanner de emergencia. Los campos se purifican solos y los campos nuevos nacen de los residuos de los viejos.

### 4. Confianza calibrada: el sistema sabe cuándo no sabe

El writer no genera con el mismo tono de certeza siempre. Si el `avg_score` de los chunks recuperados cae bajo `RETRIEVAL_CONFIDENCE_FLOOR` (configurable en `.env`), el prompt incluye una instrucción explícita: marcar la inferencia antes de responder.

---

## Un momento que revela el potencial — y el riesgo

Durante el desarrollo, ingresté un texto sobre ética como optimización de deseos. Sin pedirlo explícitamente, el sistema generó una crítica sofisticada de esa misma idea: argumentó que tratar el deseo como función optimizable es una trampa epistemológica porque la observación colapsa la función, los deseos no son agregables y quien define la métrica define la realidad.

El retrieve_log reveló algo perturbador: **los chunks con mayor score no venían del campo "deseo" — venían de código del propio sistema y de un contrato de negociación**. La separación coseno entre ellos era ~0.004. El sistema había generado una posición filosófica coherente mezclando reflexiones personales con su propio código fuente.

Eso no es solo un bug de clasificación. Es la propiedad más interesante y peligrosa del sistema: puede articular posiciones latentes que el usuario no ha formulado conscientemente, pero también puede fabricar coherencia en tu voz sin respaldo real. La diferencia entre los dos casos se siente igual desde adentro.

Ese diagnóstico llevó directamente a tres cambios de arquitectura: confidence floor en el writer, bootstrap del campo `deseo` desde corpus real, y post-hook de emergence para que los campos nuevos nazcan con fingerprint, no vacíos.

---

## Hacia dónde va: Telos

Second Brain resuelve el problema de la memoria. El siguiente proyecto — **Telos** — resuelve el problema de la dirección.

Telos emergió autónomamente como campo dentro del propio Second Brain: una masa de chunks sobre propósito, patrones de comportamiento y sistemas de valores que el scanner detectó como cluster coherente antes de que yo lo nombrara así.

La hipótesis: si Second Brain modela *qué has pensado*, Telos modela *hacia qué te mueves* — inferido no de declaraciones sino de patrones de acción sostenida en el tiempo. Un sistema que distingue deseo consciente de deseo operante, y que detecta cuando actúas en dirección opuesta a lo que dices querer.

El campo probabilístico del deseo es el primer módulo de Telos en construcción.

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

Nueve nodos: `router` · `confirm` · `ingest` · `reconcile` · `retrieve` · `writer` · `habit` · `backlog` · `critic`

**Decisiones de diseño notables**

El bibliotecario clasifica conversaciones completas, no chunks — una sola llamada a Claude con la conversación completa y la lista de campos disponibles. Cada chunk hereda esa clasificación. La eliminación del ajuste coseno local por chunk fue deliberada: con Voyage-3 monoautor el gap entre categorías era ~0.006, ajustar sobre esa diferencia amplificaba ruido, no señal.

El chunking es semántico por tipo de contenido: conversación (600 tokens), documentación técnica (400 tokens), texto libre (400 tokens).

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

## Lo que esto revela sobre el problema más amplio

Los sistemas de memoria para IA asumen que el usuario sabe qué quiere recordar. Este proyecto parte de la hipótesis opuesta: **el valor está en lo que no sabías que habías pensado**.

Eso tiene implicaciones directas para cualquier organización que acumule conocimiento en conversaciones, tickets, decisiones informales y documentación dispersa. El problema no es recuperación. Es que la estructura del conocimiento no es visible hasta que algo la hace emerger.

---

## Otros proyectos

[![Streamlit](https://img.shields.io/badge/streamlit__test-análisis%20de%20datos-FF4B4B?logo=streamlit&logoColor=white)](https://github.com/cocampoa/streamlit_test)
[![R](https://img.shields.io/badge/pronosticos__de__negocios-econometría%20en%20R-276DC3?logo=r&logoColor=white)](https://github.com/cocampoa/pronosticos_de_negocios)

---

## Skills

**Análisis de datos** · SQL · Python · R · Excel  
**IA & sistemas** · LangGraph · Qdrant · Anthropic API · Docker  
**Visualización** · Streamlit · Tableau · Matplotlib · Plotly

