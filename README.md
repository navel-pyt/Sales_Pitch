# SalesPitch — Agentic AI Campaign Intelligence

> Upload a campaign brief. Get audience recommendations, geo-confirmed signals, and a ready-to-send sales asset — in ~30 seconds.  
> Built solo as a PM-led prototype to compress the brief-to-pitch cycle for commercial teams.

<img width="531" height="891" alt="SalesPitch interface" src="https://github.com/user-attachments/assets/e8791bde-dd4f-47cc-83d5-2acc4eeeb99e" />

---

## The problem

Sales teams spent hours manually building pitch decks — pulling data from different sources, joining files locally, writing narratives from scratch. Each pitch was disconnected from actual audience data.

**This tool closes that loop.**

---
<img width="664" height="461" alt="Capture d&#39;écran 2026-03-05 212719" src="https://github.com/user-attachments/assets/03284feb-6f09-4725-a7db-1d0d496f291a" />

<img width="637" height="464" alt="Capture d&#39;écran 2026-03-05 212854" src="https://github.com/user-attachments/assets/3b23f907-1234-412a-ae88-e03979c49ab3" />

<img width="664" height="461" alt="Audience recommendation view" src="https://github.com/user-attachments/assets/55bda4dc-3814-40db-8244-bec1de112386" />
<img width="637" height="464" alt="Geo signal view" src="https://github.com/user-attachments/assets/1ef89d64-3725-4146-b4f1-cea29570840a" />

## How it works

```
1. Upload brief (PDF · Word · TXT · URL)
        ↓
2. LLM decodes brief → extracts brand, industry, keywords, themes
        ↓
3. Audience personas scored via TF-IDF + AI → top segments recommended
        ↓
4. ZIP-level geo signals pulled from local cache
        ↓
5. Clickstream data enriches brand intelligence (co-visitation, funnel, search)
        ↓
6. Sales asset generated → Hook Email · Pitch Script · 10-slide deck · 3-act narrative
```

---

## Architecture

```
Browser (pitch_app.html)
        │  HTTP / SSE (streaming)
        ▼
pitch_server.py  (Flask :5050)
   ├── brief_processor.py   ──►  LLM (AI analysis + narrative)
   ├── data_fetch.py         ──►  Local cache layer (persona + geo data)
   ├── signal_intel.py       ──►  Clickstream enrichment
   ├── brand_mapper.py       ──►  Domain → industry classifier + AI
   ├── pitch_generator.py    ──►  Output generation (email / deck / story)
   └── trends_api.py         ──►  Google Trends (24h cache)
```

---

## Data signals

| Signal | What it adds |
|---|---|
| Persona affinity scores | Which audiences match the brief |
| ZIP-level geo confirmation | Where the audience lives |
| Brand clickstream enrichment | Co-visitation, funnel, search intent |
| Real-time trends | Current category momentum |

---

## Stack

```
Backend      Flask · Python 3.10+
AI           LLM API (generative analysis + narrative)
Local DB     DuckDB (local cache layer)
Scoring      TF-IDF · cosine similarity · z-score normalization
Frontend     Single-page HTML (no framework)
```

---

## Output formats

| Format | Description |
|---|---|
| **Hook Email** | Copy-ready, personalized to brand + campaign goal |
| **Pitch Script** | 5-min talk track with data proof points |
| **Presentation** | 10-slide deck outline with geo + audience insights |
| **Storytelling** | 3-act narrative for agency presentations |

---

## Status & intent

This is a **PM-led prototype** — designed to validate the concept and workflow before full engineering investment. The architecture is documented and modular for handoff.

**What it demonstrates:**
- A solo PM can compress the 0→validated-concept cycle significantly
- The agentic AI loop (brief → data → asset) works end-to-end
- Clear separation of concerns makes engineering scale-up straightforward

---

## Quick start

```bash
# Install
pip install -r requirements.txt

# Configure
cp .env.example .env  # add your API key

# Launch
python src/pitch_server.py
# → http://localhost:5050
```

---

*Part of a Data & Insights product suite.*

---

> **Note**: This repository is a portfolio prototype. Proprietary data sources, internal tooling references, and business-specific configuration have been removed. Screenshots are included for illustrative purposes only.
