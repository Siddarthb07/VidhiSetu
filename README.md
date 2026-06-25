# Lexprobe

**Indian-legal research assistant** — retrieval-augmented generation with a **citation-audit** layer so answers stay grounded in sources.

> **This repository is public architecture documentation only.** The application codebase is **closed source** while the product is in **closed beta**. Nothing here is a runnable deployment of the product.

## What Lexprobe does

Natural-language queries over Indian legal material. Outputs pass through an audit pipeline before they reach the user — hallucination-prone claims are filtered or flagged when citations do not support them.

## Architecture (high level)

```
┌─────────────┐     ┌──────────────┐     ┌─────────────┐
│  Next.js 14 │────▶│   FastAPI    │────▶│  PostgreSQL │
│  frontend   │     │   API layer  │     │  (metadata) │
└─────────────┘     └──────┬───────┘     └─────────────┘
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
        ┌──────────┐ ┌──────────┐ ┌──────────┐
        │  Qdrant  │ │  Redis   │ │  Ollama  │
        │  (RAG)   │ │ (cache)  │ │ Llama 3  │
        └──────────┘ └──────────┘ └──────────┘
                           │
                           ▼
                  ┌─────────────────┐
                  │ Citation audit  │
                  │ + hallucination │
                  │    filter       │
                  └─────────────────┘
```

| Layer | Role |
|-------|------|
| **Frontend** | Next.js 14 — query UI, citation display, audit status |
| **API** | FastAPI — orchestration, auth, retrieval, audit gating |
| **Vector store** | Qdrant — chunked Indian legal corpus (~1,300+ instruments indexed in production) |
| **Cache** | Redis — session + hot retrieval paths |
| **LLM** | Ollama (Llama 3 8B family) — local inference; Groq used in some deployments |
| **Audit** | Post-generation citation check — unsupported claims removed before response ships |

## Repository contents

| File | Purpose |
|------|---------|
| `README.md` | This document — public architecture overview |
| `LexProbe_System_Design.docx` | Detailed design export for reviewers (admissions / mentors). **Not runnable code.** |

## Links

- **Portfolio case file:** [siddarthb07.github.io/siddarthb](https://siddarthb07.github.io/siddarthb/) (Issue #001 — Lexprobe panel)
- **Builder:** [@Siddarthb07](https://github.com/Siddarthb07)

## Collaboration

For demo requests or architecture discussions, use the email on the [personal site](https://siddarthb07.github.io/siddarthb/).

---

Documentation in this repository is shared for transparency. The closed-source application and production deployment are **not** licensed here.
