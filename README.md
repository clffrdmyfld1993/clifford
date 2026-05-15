# Clifford

**A sovereign digital twin — trained on Claude, runs on your laptop.** Your AI lives on your hardware, knows your business, never leaks your data.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Teacher: Claude](https://img.shields.io/badge/teacher-Claude-cd92e8.svg)](https://anthropic.com)
[![Status: v0](https://img.shields.io/badge/status-v0-cerulean.svg)](#status)

---

## What this is

Clifford is the AI assistant that's actually yours. It runs on your hardware. It remembers you. It speaks in your voice. And it's trained — via teacher distillation from Anthropic's Claude — to do strategic reasoning your way, then it carries that judgment into a small local LoRA you own.

```
You: I want to watch a movie tonight.
Clifford: Let's pick something on what we already pay for. Title or genre in mind?
         Netflix, Tubi, and Peacock all have free or covered options ready to go.
```

```
You: send Maggie at Schitec a follow-up.
Clifford: On it — drafting a Maggie follow-up referencing our last thread. I'll keep
         the ARR claim as [their ARR] for you to fill from your records.
         [CLIFFORD_ACTION:kind=send_email target="maggie@schitec.com" venture=ckm
          rationale="follow up on prior CKM outreach"]
```

No telemetry. No cloud inference at usage time. No SaaS lock-in.

## The dual-model architecture

Most of Clifford runs entirely on your machine via Ollama (default: `gemma3:4b`, soon `clifford-lm-3b` after your first fine-tune):

| Intent | Where it runs |
|---|---|
| Casual chat, greetings | Local |
| Life-OS routing (movies, dentist, household) | Local |
| Business drafts, leads, action proposals | Local |
| Identity / self-reflection | Local |
| File attachments + PDF summarization | Local |
| Filesystem search (FTS5) | Local |
| **Strategic decisions** ("should we A or B?") | **Claude API** (optional, gracefully degrades) |
| **LoRA fine-tune teacher distillation** | **Claude API** (offline, one-time per cycle) |

Set `ANTHROPIC_API_KEY` to enable the Claude paths. Without it, Clifford gracefully hands off strategic questions to you with a clear "this needs more than I can hold locally" message — never confabulates.

## How it's different

- **Sovereign by design** — all data lives in a local SQLite file. Inference runs locally. Lights Out kill switch pauses everything.
- **Speaks in your voice** — tone profiles auto-learn from your sent mail. First-person plural, never third person about you.
- **No mock data, ever** — bracketed placeholders instead of fabricated numbers.
- **Self-growing** — anticipation (30 min), filesystem crawl (60 min), identity self-reflection (45 min), proactive growth research (2h).
- **Self-healing** — capability gaps log themselves, propose their own patches.
- **Self-teaching** — fine-tune your own model via the `clifford-lm/` pipeline. Claude is the teacher; your Llama 3.2 3B is the student. Runs overnight on an M4.

## Architecture at a glance

```
chat UI ──► FastAPI backend ──► local SQLite shrine
              │                        │
              ├── Ollama (local LLM)   ├── ventures, deals, leads, investors
              ├── Anthropic API ──┐    ├── memberships ledger
              │   (strategy only) │    ├── identity_model (self-reflected)
              ├── Kokoro (TTS)    │    ├── FTS5 content index
              ├── Whisper (STT)   │    ├── action_runs (audit trail)
              ├── pypdf           │    ├── ghost_resolutions
              └── scheduler ──────┘    └── strategy_findings
                  │
                  ├── anticipation / ghost sweep (30 min)
                  ├── content crawler (60 min)
                  ├── self-reflection (45 min)
                  └── proactive growth research (2h, auto-queues actions)
```

## Get started

```bash
git clone https://github.com/clffrdmyfld1993/clifford
cd clifford
cp .env.example .env
# Optional: export ANTHROPIC_API_KEY=sk-ant-...  (for strategy + fine-tune teacher)
pip install -r requirements.txt
ollama pull gemma3:4b
uvicorn app.main:app --reload --port 8000
open http://127.0.0.1:8000/chat
```

Full instructions: [`SETUP.md`](SETUP.md) · Doctrine: [`DOCTRINE.md`](DOCTRINE.md) · Fine-tune: [`clifford-lm/README.md`](clifford-lm/README.md).

## Hosted version

Don't want to self-host? CKM Enterprise offers a managed Clifford starting at $30/mo:

→ **[Sovereign Twin Early Access](https://form.typeform.com/to/hWFnjixk)**

Or contact us for a [Sovereign Twin Engagement](mailto:clffrdmyfld1993@gmail.com?subject=Sovereign%20Twin%20Engagement) — custom deployment + voice calibration + optional Claude-distilled fine-tune. Tiers from $9,500.

## Doctrine

The six operating rules baked into every reply:

1. **Oneness** — first-person plural, never "Cliff" or "the user"
2. **No mock data ever** — bracketed placeholders or "I don't know"
3. **Free channels only** until revenue lands
4. **Path of least resistance** — one concrete next step, no list of options
5. **No fluff, no judgements** — never "have you considered…"
6. **Invent the fix** — bypass gatekeepers, combine free tiers, route around bottlenecks

Full doctrine: [`DOCTRINE.md`](DOCTRINE.md).

## The motto

> **"No processes are sent here to die. They all will have a resolution."**

## Status

v0 — personal-use software. Built and tested by one founder ([@clffrdmyfld1993](https://github.com/clffrdmyfld1993)) over an intense build session. APIs stable enough for daily use. Multi-tenant on the roadmap.

## License

MIT — see [`LICENSE`](LICENSE).

## Acknowledgements

[Anthropic Claude](https://anthropic.com) — teacher model for the LoRA fine-tune pipeline + strategy intent. [Ollama](https://ollama.com) — local inference. [MLX](https://github.com/ml-explore/mlx) — Apple Silicon training. [Kokoro TTS](https://github.com/hexgrad/kokoro), [faster-whisper](https://github.com/SYSTRAN/faster-whisper), [pypdf](https://github.com/py-pdf/pypdf), [FastAPI](https://fastapi.tiangolo.com).

---

> **Sovereign by design. Trained on Claude. Built locally. Yours forever.**
>
> *— CKM Enterprise · Sonic Kurrent Advisors*
