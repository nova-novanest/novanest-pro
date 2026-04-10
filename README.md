# Novanest Pro

**An AI operating loop running my US home services business solo from Barcelona.**

[![Live](https://img.shields.io/badge/live-novanestpro.com-0f62fe)](https://novanestpro.com)
[![Founder](https://img.shields.io/badge/founder-hovhannes.dev-1d7a50)](https://hovhannes.dev)
[![License](https://img.shields.io/badge/license-MIT-lightgrey)](LICENSE)

<p align="center">
  <img src="https://hovhannes.dev/hovo-ladder.jpg" alt="Hovhannes installing security cameras in Los Angeles" width="640">
  <br>
  <em>Field research before the system existed: I flew to Los Angeles and worked the jobs myself for two months to understand what was actually breaking solo service businesses.</em>
</p>

---

## What it does

A real Thumbtack lead arrives. Within 60 seconds, the system has researched the customer, priced the job using task-based logic, written a personalized reply, sent it through the Thumbtack API, and dispatched a trained local technician through Telegram. If the customer goes silent, an AI caller follows up by voice. If the customer responds, the loop continues until the job is booked.

I built this alone from Barcelona. It runs a real business in 15 California cities for real customers. I don't touch the keyboard during execution.

---

## Architecture

```
                    ┌──────────────────────────┐
                    │ Customer on Thumbtack    │
                    │ (clicks "request a quote")│
                    └────────────┬─────────────┘
                                 │
                                 ▼
               ┌──────────────────────────────────┐
               │  Thumbtack v4 webhook            │  ← solo-operator API access
               │  POST /webhook                   │
               └────────────┬─────────────────────┘
                            │
                            ▼
        ┌──────────────────────────────────────────┐
        │  FastAPI backend (Python)                │
        │  /opt/novanest-api/app/                  │
        │  Single-server deployment, SQLite        │
        └──┬─────────────┬────────────┬───────────┘
           │             │            │
           ▼             ▼            ▼
       thumbtack.py  processor.py  database.py
       (parse +      (orchestrate  (SQLite
        persist)     pipeline)      schema)
                         │
        ┌────────────────┼──────────────────────┐
        │                │                      │
        ▼                ▼                      ▼
  intelligence.py  playbook.py        openclaw_notifier.py
  ─────────────    ───────────        ─────────────────
  zip lookup       task-based pricing  push draft to Nova
  drive time       reply generation    orchestrator in Telegram
  neighborhood     "winning formula"   for human approval
                         │
                         ▼
        ┌──────────────────────────────────┐
        │  PATCH /internal/approval/{id}/  │  ← I tap ✅ in Telegram
        │  status=approved                 │      from my phone
        └────────────┬─────────────────────┘
                     │
                     ▼
            ┌────────────────────┐
            │ Reply sent to      │
            │ Thumbtack customer │
            └─────┬──────────────┘
                  │
                  ▼
       ┌──────────────────────┐         ┌─────────────────────┐
       │ Customer replies?    │ ──Yes──▶│ Loop continues until│
       └──────┬───────────────┘         │ job is booked       │
              │                          └──────────┬──────────┘
              No                                    │
              │                                     ▼
              ▼                          Telegram pros chat
       retell_caller.py                  → dispatch to local technician
       (AI voice fallback)
```

---

## Stack

| Layer | Tech |
|---|---|
| Backend | Python, FastAPI, SQLite |
| AI orchestration | Anthropic Claude API on a custom multi-agent runtime (openclaw) |
| External APIs | Thumbtack v4 (solo-operator access), Telegram Bot API, Retell AI |
| Deployment | Single systemd service, single Linux server |
| Codebase size | ~3,000 lines of Python across 10 modules |

---

## Traction (April 2026)

| Metric | Value |
|---|---|
| Inbound leads handled | **125** |
| Jobs won | **27** |
| California cities served | **15** |
| Founders | **1** |
| Employees | **0** |
| Time zones from customer | **9** |
| Founder location | Barcelona, Spain |
| Customer location | Los Angeles area, California |

---

## What's notable

- **Solo-operator Thumbtack API access.** Normally reserved for registered businesses with significant volume. I got it as a one-person operation, alone, from another continent.
- **Multi-agent architecture.** The orchestrator delegates research, pricing, and reply drafting to specialized sub-agents that share workspace and memory. Not one prompt — a system.
- **Self-correcting reply pipeline.** A post-send QA pass detects rule violations (e.g. non-sequiturs to literal price questions) and ships corrective replies within minutes, without human intervention.
- **Live in production.** Handles real customers, real jobs, real revenue. Not a demo.
- **Built after physical fieldwork.** I installed cameras and smart-home systems in Los Angeles myself for two months before writing the code. The product is downstream of the operating insight, not the other way around.

---

## What's next

The next version of Novanest is a **personal AI agent** that connects directly to one solo service pro — handyman, plumber, electrician, locksmith, HVAC tech — and runs the business side of their work for them.

**The AI runs the business. The craftsman does the craft.**

Currently testing on other pros' Thumbtack accounts. If you're a solo service pro and want in, [email me](mailto:contact@novanestpro.com).

---

## Code

Production code is private. The system is live and handles real customer data; the source isn't open.

**This repo intentionally contains only this README** and a license. If you're a partner, investor, or service pro who wants to discuss the system in depth, contact [contact@novanestpro.com](mailto:contact@novanestpro.com).

---

## License

[MIT](LICENSE) — for the README and any future template/documentation files in this repo. The production system is not part of this repo and is not licensed for distribution.

---

Built by **[Hovhannes Hovhannisyan](https://hovhannes.dev)** (Hovo) from Barcelona.

[novanestpro.com](https://novanestpro.com) · [hovhannes.dev](https://hovhannes.dev) · [github.com/nova-novanest](https://github.com/nova-novanest)
