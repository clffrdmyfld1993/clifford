# Setup

Run Clifford on your own hardware. ~10 minutes from clone to chat.

## Prerequisites

- **macOS, Linux, or WSL** — tested on Apple Silicon (M-series)
- **Python 3.11+** — `brew install python@3.13` if missing
- **Ollama** — `brew install ollama` then `ollama serve`
- **~5 GB disk** for the base LLM weights

## Install

```bash
# 1. Clone
git clone https://github.com/clffrdmyfld1993/clifford
cd clifford

# 2. Python deps
python3 -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt

# 3. Pull a base LLM
ollama pull gemma3:4b
# (alternative: llama3.2:3b, qwen2.5:1.5b — see ARCHITECTURE.md for tradeoffs)

# 4. Optional: install pypdf for PDF attachments
pip install pypdf

# 5. Configure
cp .env.example .env
# Open .env in your editor and fill in:
#   - GOOGLE_CLIENT_ID + GOOGLE_CLIENT_SECRET (for OAuth — optional in dev mode)
#   - CLIFFORD_LLM_MODEL (default: gemma3:4b)
#   - SESSION_SECRET (any random string)

# 6. Boot
uvicorn app.main:app --reload --port 8000

# 7. Open the chat
open http://127.0.0.1:8000/chat
```

## First-run checklist

When you open `/chat`, Clifford will be empty — no ventures, no memberships, no identity model. The first 10 minutes:

1. **Set up your twin's identity** — open the chat, say "hi, my name is [your name]". Tell it about yourself.
2. **Add your ventures** — click 🏢 Enterprise → ventures pre-seed with examples; edit or delete via PATCH/DELETE.
3. **Add your memberships** — tell Clifford "I have Netflix, Spotify, Adobe CC" — or use POST `/v1/life/memberships` directly.
4. **Index your filesystem** — POST `/v1/files/index/rebuild` (the scheduler will do this hourly after).
5. **Wait 45 min** — the self-reflection loop will write your first `identity_model` based on what you've fed it.

## OAuth setup (for connected accounts)

Clifford can ingest data from Google services (Gmail, Calendar, Drive). To enable:

1. Go to [Google Cloud Console](https://console.cloud.google.com) → Credentials
2. Create an OAuth 2.0 Client ID — type: Web application
3. Authorized redirect URI: `http://127.0.0.1:8000/v1/auth/callback`
4. Copy Client ID + Secret into `.env`
5. Restart the server, visit `http://127.0.0.1:8000/v1/auth/google` to authorize

(LinkedIn, Airtable, GitHub, etc. connectors require similar setup — see `app/routes/auth.py` for the pattern.)

## Optional: train your own LLM

The `clifford-lm/` directory has a complete LoRA fine-tune pipeline. See [`clifford-lm/README.md`](clifford-lm/README.md) for the overnight training procedure that produces your own `clifford-lm-3b` model.

## Lights Out

To pause every background loop (anticipation, crawl, reflection, mic, autonomous actions) — say "lights out" in the chat or POST `/v1/sovereignty/lights_out`. Direct chat keeps working. Say "lights on" to resume.

## Troubleshooting

- **Server won't start** — check Ollama is running: `curl http://127.0.0.1:11434/api/tags`
- **Slow first reply** — gemma3:4b on a cold M4 can take 60–90s. Subsequent replies are faster. Try `llama3.2:3b` if quality drift is acceptable.
- **Mic permission denied** (native Mac app) — Settings → Privacy & Security → Microphone → enable for the Clifford app.
- **PDFs not summarizing** — `pip install pypdf`.

## Update

```bash
cd clifford
git pull
pip install -r requirements.txt  # in case deps changed
# restart uvicorn
```

## Contributing

Issues + PRs welcome at [github.com/clffrdmyfld1993/clifford/issues](https://github.com/clffrdmyfld1993/clifford/issues).

The doctrine (see [DOCTRINE.md](DOCTRINE.md)) is non-negotiable for accepted PRs. Everything else is open for discussion.
