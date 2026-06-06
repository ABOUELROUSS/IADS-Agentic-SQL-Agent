# IADS SQL Agent

> First-place winning agentic text-to-SQL system built on Oracle Cloud Infrastructure.
> Team 4 - IADS Agentic AI Hackathon 2026.

## Hackathon Achievement

This project won **1st place** at the **IADS Agentic AI Hackathon 2026**, held at the
**University of Essex** from **2-4 June 2026**.

The hackathon was organized and supported by:

- **University of Essex**
- **Institute for Analytics and Data Science (IADS)**
- **Enigen**
- **Oracle Cloud Infrastructure**

This was built as a **team project** under the supervision of **Dr Haider Raza**
from the University of Essex. The winning certificate was issued by the Institute
for Analytics and Data Science (IADS), University of Essex, with representatives
from the University of Essex and Enigen.

## What It Does

Ask a business question in plain English. The system retrieves relevant schema and
business context, generates safe Oracle SQL, validates it, executes it, and returns
an answer with the table and explanation behind the result.

```text
You: "Which product category generated the most revenue last quarter in the UK?"

Agent: Electronics generated GBP 4.2M in Q1 2026, up 18% year-on-year.
       - based on 12,847 rows of sales data

       SQL: SELECT product_category, SUM(revenue) AS total
            FROM sales
            WHERE region = 'UK' AND quarter = '2026-Q1'
            GROUP BY product_category
            ORDER BY total DESC
            FETCH FIRST 5 ROWS ONLY;
```

## Architecture

A multi-stage agentic pipeline. See [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md)
for the full design.

```text
User question
   |
[Planner] -> [Schema Retriever / RAG] -> [SQL Generator]
                                           |
                                      [Validator]
                                           |
                                      [Safe Executor]
                                           |
                                      [Critic / Reflector]
                                           |
                                      [Summariser]
                                           |
                                      Response
```

The system combines agentic planning, retrieval-augmented generation, Oracle Select
AI, SQL safety checks, cached conversation memory, explainability, and a Streamlit
chat interface.

## Key Capabilities

- Natural-language business questions over structured Oracle data
- Oracle-based RAG using schema knowledge, KPI definitions, business rules, SQL
  patterns, dataset documentation, previous analyses, and glossary context
- Select AI-backed SQL generation and agentic planning
- SQL validation and read-only guardrails before execution
- Cached-result memory for follow-up prompts such as sorting, filtering, explaining,
  or comparing previous outputs
- Reflector/retry logic for failed SQL, empty results, or missing SQL
- Explainability panel showing how the answer was calculated
- Streamlit frontend with tables, charts, summaries, and suggested follow-up prompts

## Quick Start

```bash
# Install
make dev

# Configure
cp .env.example .env  # then edit with your OCI and Oracle credentials

# Run
make run-api    # FastAPI at http://localhost:8000
make run-ui     # Streamlit at http://localhost:8501
```

## Repository Layout

```text
src/sql_agent/      Core package
  config/           Typed settings
  core/             Domain models, exceptions, logging
  llm/              OCI Generative AI client
  retrieval/        Schema embeddings + vector store (RAG)
  database/         Autonomous DB connection + safe executor
  agents/           Multi-stage pipeline + orchestrator
  safety/           SQL guard rails
  api/              FastAPI app

app/                Hackathon application pipeline and agents
frontend/           Streamlit UI
evaluation/         Benchmark harness
tests/              Unit + integration tests
prompts/            Versioned prompt templates
docs/               Architecture + decision records
scripts/            Seed DB, embed schema, run benchmark
```

## Team

This was a collaborative hackathon build delivered by **Team 4**.

| Member | Background | Slice |
|---|---|---|
| Omar | AI | Orchestration, critic, model routing |
| Hasan | AI | Planner, SQL generator, LLM client + prompts |
| Zayad | AI | Retrieval / RAG, memory, follow-up handling |
| Asad | Data Science (MSc) | Summariser, evaluation harness, golden datasets |
| Mehdi | CS (undergrad) | FastAPI service, Streamlit UI, deployment |
| Abdul Qayyum | CS (undergrad) | Database, vector store, safety/obfuscation |

Full file-level ownership and Day 1/2/3 plan: [`docs/OWNERSHIP.md`](docs/OWNERSHIP.md).

## Documents

- [`docs/ARCHITECTURE.md`](docs/ARCHITECTURE.md) - system design and path to production
- [`docs/decisions/`](docs/decisions/) - architecture decision records (ADRs)

## Licence

MIT
