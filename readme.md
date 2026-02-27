# 🩺 Health Tracker — Personal Health Platform

A full-stack, AI-powered health tracking platform supporting both **patient** and **provider** roles. Built as a non-commercial personal project to explore applied LLM integration in a clinical context.

> **Stack:** FastAPI · Next.js · Clerk · PostgreSQL · Railway · Docker · OpenAI · Anthropic

---

## Overview

Health Tracker is a dual-role web application that combines traditional health data management with LLM-powered conversational AI. Users can log vitals, upload lab reports and DNA data, and interact with an AI health assistant that maintains full medical context across sessions.

The system enforces strict **data isolation**, **PHI scrubbing**, and **audit logging** — reflecting real-world compliance considerations in healthcare software.

---

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                        Frontend                              │
│         Next.js + Clerk Auth + Tailwind CSS                  │
│                                                              │
│   ┌─────────────────┐       ┌──────────────────────┐        │
│   │   Patient UI     │       │     Provider UI       │       │
│   │  - Health log    │       │  - Clinical prompts   │       │
│   │  - Chat (basic)  │       │  - Model selector     │       │
│   │  - DNA toggle    │       │  - Higher token limits│       │
│   │  - Lab reports   │       │  - Technical context  │       │
│   └────────┬────────┘       └──────────┬───────────┘        │
└────────────┼───────────────────────────┼────────────────────┘
             │         HTTPS / REST       │
┌────────────▼───────────────────────────▼────────────────────┐
│                      Backend (FastAPI)                        │
│                                                              │
│  ┌──────────────┐  ┌────────────────┐  ┌─────────────────┐  │
│  │   Registros   │  │  LLM Chat      │  │  File Services  │  │
│  │  (vitals API) │  │  GPT / Claude  │  │  DNA + Labs     │  │
│  └──────────────┘  └───────┬────────┘  └────────┬────────┘  │
│                            │                     │           │
│  ┌──────────────┐  ┌───────▼────────┐  ┌────────▼────────┐  │
│  │  Topic       │  │  Conversation  │  │  PDF / Email    │  │
│  │  Classifier  │  │  History       │  │  News / Research│  │
│  └──────────────┘  └───────┬────────┘  └─────────────────┘  │
│                            │                                  │
│  ┌──────────────┐  ┌───────▼────────┐  ┌─────────────────┐  │
│  │  Stripe      │  │  Medical       │  │  Audit Logging  │  │
│  │  Credits     │  │  Profile       │  │  PHI Scrubbing  │  │
│  └──────────────┘  └───────┬────────┘  └─────────────────┘  │
└────────────────────────────┼────────────────────────────────┘
                             │
              ┌──────────────▼──────────────┐
              │        PostgreSQL DB          │
              │  Auto-migrations on startup   │
              └─────────────────────────────┘

              Deployed on Railway via Docker Compose
```

---

## Key Features

### AI Chat Engine
- Dual model support: **GPT** and **Claude Opus** — selectable per session
- **Topic classifier** keeps conversations scoped to health topics
- **Medical context toggle** injects patient profile into chat context
- DNA file uploads parsed and injected into LLM context
- Lab reports selectable from dropdown and passed as context
- Full conversation save / load / delete with persistent history

### Health Data Management
- **Registro** endpoints for logging vitals and health entries
- CSV import/export with full round-trip compatibility
- Medical profile creation and management
- Lab report storage and retrieval

### Roles & Access
| Feature | Patient | Provider |
|---|---|---|
| Log vitals | ✅ | ✅ |
| AI chat | ✅ (consumer prompts) | ✅ (clinical prompts) |
| Model selection | ❌ | ✅ |
| Token limits | Standard | Elevated |
| DNA context | ✅ | ✅ |

### Security & Compliance Design
- Per-user data isolation enforced at API layer
- PHI scrubbing before any external logging
- Full audit log of data access and mutations
- Clerk-based auth with role claims

### Infrastructure
- Docker Compose for local development
- Railway deployment with `Procfile` + `railway.json`
- Auto-migrations on app startup — no manual DB management
- Build pipeline optimized for Railway (`.next/cache` handling)

---

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Next.js, Tailwind CSS, Clerk |
| Backend | FastAPI, Python |
| Database | PostgreSQL |
| AI | OpenAI API, Anthropic API |
| Payments | Stripe |
| Deployment | Railway, Docker Compose |
| File Handling | CSV, PDF generation, DNA file parsing |

---

## Development Highlights

- **33 commits** across Jan–Feb 2026
- Resolved race conditions in `get_or_create` DB patterns
- Tuned topic classifier to balance health topic strictness vs. usability
- Mobile-optimized layouts across all core views
- Iterative prompt engineering for both patient and provider personas

---

## Notes

This is a **personal, non-commercial project** built for health self-tracking and to explore LLM integration patterns in a regulated-domain context. Not intended for clinical use. No PHI is stored in logs or external services.

---

*Built by Joel Guerra — [linkedin.com/in/joelguerra-ms](https://linkedin.com/in/joelguerra-ms)*
