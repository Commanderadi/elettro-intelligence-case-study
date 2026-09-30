# ELETTRO Intelligence Platform (Case Study)

> Source is private. This repo documents what the platform does, how it's built, and the decisions behind it.

A **multi-tenant sales analytics platform** I **conceived and built solo** at ELETTRO (KN Elettro Industries). It replaces Excel-based sales reporting with a central, self-serve portal: upload sales data, and get segmentation, maps, product analytics, PDF reports, and an AI assistant that answers questions about the data.

---

## At a glance

| | |
|---|---|
| **Role** | Sole engineer: idea, data model, API, frontend, AI features |
| **Frontend** | Next.js · Recharts · Leaflet |
| **Backend** | FastAPI · SQLAlchemy · PostgreSQL (Neon) |
| **AI** | Hosted LLM APIs · structured outputs · retrieval-augmented generation (RAG) |
| **Size** | 40+ REST endpoints · ~11,000 lines of code |

---

## The problem

Sales data lived in Excel exports. Every report was built by hand, answers to simple questions ("which regions are declining?", "who are our best customers?") took days, and nobody had one shared view of the numbers.

## What I built

- **Excel / CSV ingestion** with validation and **automated geo-enrichment**, so raw sales rows are mapped to locations automatically
- **RFM customer segmentation** (Recency, Frequency, Monetary) to identify champions, at-risk and lapsed customers
- **Interactive map of India** (Leaflet) showing sales by region
- **Product analytics:** trends, top and bottom performers, mix
- **Automated PDF reports** for management
- **AI chatbot over sales data:** users ask questions in plain English; answers are grounded in the company's data using structured outputs and RAG-based retrieval
- **Multi-tenant design** with row-level security, so each tenant only sees its own data

## Architecture

```
 Excel / CSV ─▶ FastAPI ingestion ─▶ geo-enrichment ─▶ PostgreSQL (Neon)
                        │                                   │
 Next.js UI ◀── REST (40+ endpoints) ◀── SQLAlchemy ◀───────┘
    │  charts (Recharts), map (Leaflet), PDF reports
    └─ AI chat ─▶ retrieval over sales data ─▶ LLM (structured output) ─▶ answer
```


## Engineering decisions

- **FastAPI + SQLAlchemy:** Python keeps the analytics code (pandas-style transforms, RFM) next to the API.
- **Serverless Postgres (Neon):** a managed database with branching, and low cost at this scale.
- **Structured LLM outputs:** answers come back in a typed schema, so the UI can render tables and figures reliably instead of parsing free text.
- **Multi-tenancy from day one:** row-level security patterns rather than separate databases per tenant.

---

*Want a walkthrough? Reach me at [adityasingh3499@gmail.com](mailto:adityasingh3499@gmail.com) or on [LinkedIn](https://www.linkedin.com/in/aditya-singh-89b884189).*
