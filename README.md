# Clifford

**A sovereign digital twin.** Your AI lives on your hardware, knows your business, never leaks your data.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Status: v0](https://img.shields.io/badge/status-v0%20%E2%80%94%20personal%20use-cerulean.svg)](#status)

---

## What this is

Clifford is the digital twin most AI assistants pretend to be. It runs entirely on your machine. It remembers you across every conversation. It knows your ventures, your deals, your inbox, your calendar — and it routes every life intent (movie / dentist / business idea) through path-of-least-resistance logic without judgement.

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

No telemetry. No cloud inference. No SaaS lock-in.

## How it's different

- **Sovereign by design** — all data lives in a local SQLite file. Inference runs on Ollama. Lights Out kill switch pauses everything instantly.
- **Speaks in your voice** — first-person plural, never refers to you in third person. Tone profiles auto-learn from your sent mail.
- **No mock data, ever** — bracketed placeholders instead of fabricated numbers.
- **Self-growing** — three background loops update what Clifford knows: anticipation (every 30 min), filesystem crawl (60 min), identity self-reflection (45 min).
- **Self-healing** — capability gaps log themselves, propose their own patches.
- **Self-teaching** — fine-tune your own model via the `clifford-lm/` pipeline (LoRA on Llama 3.2 3B, MLX-native, runs overnight on an M4).

## Architecture at a glance

```
chat UI ──► FastAPI backend ──► local SQLite shrine
              │                        │
              ├── Ollama (local LLM)   ├── ventures, deals, leads
              ├── Kokoro (TTS)         ├── memberships ledger
              ├── faster-whisper (STT) ├── identity_model (self-reflected)
              ├── pypdf (doc extract)  ├── FTS5 content index
              └── scheduler            └── action_runs (audit trail)
                  │
                  ├── anticipation loop (30 min)
                  ├── content crawler (60 min)
                  └── self-reflection (45 min)
```

## Get started

```bash
git clone https://github.com/clffrdmyfld1993/clifford
cd clifford
cp .env.example .env       # fill in OAuth client + LLM model
pip install -r requirements.txt
ollama pull gemma3:4b      # or any model you prefer
uvicorn app.main:app --reload --port 8000
open http://127.0.0.1:8000/chat
```

Full instructions in [`SETUP.md`](SETUP.md). Architecture deep-dive in [`ARCHITECTURE.md`](ARCHITECTURE.md). Doctrine in [`DOCTRINE.md`](DOCTRINE.md).

## Hosted version

Don't want to self-host? CKM Enterprise offers a managed Clifford starting at $30/mo. Get on the waitlist:

→ **[Sovereign Twin Early Access](https://form.typeform.com/to/hWFnjixk)**

Or contact us for a [Sovereign Twin Engagement](mailto:clffrdmyfld1993@gmail.com?subject=Sovereign%20Twin%20Engagement) — we deploy a custom Clifford on your hardware, fine-tune it to your voice, and train you on the daily-driver workflow. Tiers from $9,500.

## Doctrine

Six operating rules baked into every reply:

1. **Oneness** — first-person plural, never "Cliff" or "the user"
2. **No mock data ever** — bracketed placeholders or "I don't know"
3. **Free channels only** until revenue lands
4. **Path of least resistance** — one concrete next step, no list of options
5. **No fluff, no judgements** — never "have you considered…"
6. **Invent the fix** — bypass gatekeepers, combine free tiers, route around bottlenecks

## Status

**v0 — personal-use software.** Built and tested by one founder (Cliff Mayfield, [@clffrdmyfld1993](https://github.com/clffrdmyfld1993)) over an intense build session. APIs are stable enough for daily use; multi-tenant deployment is on the roadmap, not in this release. Issues + PRs welcome.

## License

MIT — see [`LICENSE`](LICENSE).

## Acknowledgements

Built on the shoulders of: [Ollama](https://ollama.com), [Kokoro TTS](https://github.com/hexgrad/kokoro), [faster-whisper](https://github.com/SYSTRAN/faster-whisper), [pypdf](https://github.com/py-pdf/pypdf), [FastAPI](https://fastapi.tiangolo.com), and [MLX](https://github.com/ml-explore/mlx).

---

> **Sovereign by design. Built locally. Yours forever.**
>
> *— CKM Enterprise · Sonic Kurrent Advisors*
