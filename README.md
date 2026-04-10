# Novanest Pro

**An AI operating loop running my US home services business solo from Barcelona.**

🔗 **Live:** [novanestpro.com](https://novanestpro.com)
🔗 **Founder:** [hovhannes.dev](https://hovhannes.dev)

---

## What it does

A real Thumbtack lead arrives. Within 60 seconds, the system has researched the customer, priced the job using task-based logic, written a personalized reply, sent it through the Thumbtack API, and dispatched a trained local technician through Telegram. If the customer goes silent, an AI caller follows up by voice. If the customer responds, the loop continues until the job is booked.

I built this alone from Barcelona. It runs a real business in 15 California cities for real customers. I don't touch the keyboard during execution.

## Architecture

```
Customer sends a lead on Thumbtack
        │
        ▼
Thumbtack v4 webhook  ──▶  FastAPI backend (Python)
        │
        ▼
thumbtack.py  ──▶  parse the lead, persist in SQLite
        │
        ▼
processor.py  ──▶  orchestrate the response pipeline
        │
        ├──▶ intelligence.py        (zip / drive-time / neighborhood research)
        │
        ├──▶ playbook.py            (task-based pricing + reply generation)
        │
        ├──▶ openclaw_notifier.py   (Nova orchestrator approves draft)
        │
        ├──▶ Telegram pros chat     (dispatch to local technician)
        │
        └──▶ retell_caller.py       (AI voice fallback if customer silent)
```

## Stack

- **Backend:** Python, FastAPI, SQLite
- **AI:** Anthropic Claude API, on a custom multi-agent orchestrator (openclaw)
- **Integrations:** Thumbtack v4 API (solo-operator access), Telegram Bot API, Retell AI
- **Deployment:** Single systemd service on a single Linux server
- **Code size:** ~3,000 lines of Python

## Traction (April 2026)

| Metric | Value |
|---|---|
| Inbound leads handled | 125 |
| Jobs won | 27 |
| California cities served | 15 |
| Founders | 1 |
| Employees | 0 |
| Time zones from customer | 9 |

## What's notable

- **Solo-operator Thumbtack API access** — normally reserved for registered businesses with significant volume. I got it alone.
- **Multi-agent architecture** — the orchestrator delegates research, pricing, and reply drafting to specialized sub-agents that share workspace and memory.
- **Self-correcting reply pipeline** — a post-send QA pass detects rule violations (e.g. non-sequiturs to literal price questions) and ships corrective replies within minutes.
- **Live in production** — handles real customers, real jobs, real revenue.

## What's next

The next version is a **personal AI agent** that connects directly to one solo service pro — handyman, plumber, electrician, locksmith, HVAC tech — and runs the business side of their work for them.

**The AI runs the business. The craftsman does the craft.**

Currently testing on other pros' Thumbtack accounts.

## Code

Production code is private. The system is live and handles real customer data; the source isn't open. If you're a partner, investor, or service pro who wants to discuss it in depth, contact [contact@novanestpro.com](mailto:contact@novanestpro.com).

---

Built by [Hovhannes Hovhannisyan](https://hovhannes.dev) (Hovo) from Barcelona.
