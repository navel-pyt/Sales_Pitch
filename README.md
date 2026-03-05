# SalesPitch — Agentic AI Campaign Intelligence

> Upload a campaign brief. Get audience recommendations, geo-confirmed signals, and a ready-to-send sales asset — in ~30 seconds.  
> Built solo as a PM-led prototype to compress the brief-to-pitch cycle for commercial teams.

<img width="531" height="891" alt="Capture d&#39;écran 2026-03-02 235749" src="https://github.com/user-attachments/assets/e8791bde-dd4f-47cc-83d5-2acc4eeeb99e" />

---

## The problem

Sales teams spent hours manually building pitch decks — pulling data from different sources, joining Excel files locally, writing narratives from scratch. Each pitch was disconnected from actual audience data.

**This tool closes that loop.**

---
<img width="664" height="461" alt="Capture d&#39;écran 2026-03-05 212719" src="https://github.com/user-attachments/assets/55bda4dc-3814-40db-8244-bec1de112386" />
<img width="637" height="464" alt="Capture d&#39;écran 2026-03-05 212854" src="https://github.com/user-attachments/assets/1ef89d64-3725-4146-b4f1-cea29570840a" />

## How it works

```
1. Upload brief (PDF · Word · TXT · URL)
        ↓
2. Gemini 2.0 decodes brief → extracts brand, industry, keywords, themes
        ↓
3. Audience personas scored via TF-IDF + AI → top segments recommended
        ↓
4. ZIP-level geo signals pulled from local DuckDB cache (ClickHouse-sourced)
        ↓
5. DATOS clickstream enriches brand intelligence (co-visitation, funnel, search)
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
   ├── brief_processor.py   ──►  Gemini 2.0 Flash (AI analysis + narrative)
   ├── ch_fetch.py           ──►  DuckDB local cache (ClickHouse-sourced personas + geo)
   ├── datos_intel.py        ──►  DATOS clickstream (DAF · RIF · SEF feeds)
   ├── brand_mapper.py       ──►  Domain → industry classifier + Gemini
   ├── pitch_generator.py    ──►  Output generation (email / deck / story)
   └── trends_api.py         ──►  Google Trends (24h cache)
```

---

## Data signals

| Signal | Source | What it adds |
|---|---|---|
| Persona affinity scores | ClickHouse (local DuckDB cache) | Which audiences match the brief |
| ZIP-level geo confirmation | Paiement + Census (via DuckDB) | Where the audience lives |
| Brand clickstream | Search/Browse/Purchase (S3 samples) | Co-visitation, funnel, search intent |
| Real-time trends | Google Trends API | Current category momentum |

---

## Stack

```
Backend      Flask · Python 3.10+
AI           Google Gemini 2.0 Flash API
Local DB     DuckDB (cache layer over ClickHouse + S3)
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

This is a **PM-led prototype built on real data samples** — designed to validate the concept and workflow before full engineering investment. The architecture is documented and modular for handoff.

**What it demonstrates:**
- A solo PM can compress the 0→validated-concept cycle significantly
- The agentic AI loop (brief → data → asset) works end-to-end on real data
- Clear separation of concerns makes engineering scale-up straightforward

**Next step**: production deployment with full ClickHouse live queries (replacing local DuckDB cache) — pending engineering resourcing.

---

## Quick start

```bash
# Install
pip install -r requirements.txt

# Configure (copy .env.example → .env, add Gemini API key)
cp .env.example .env

# Build local DB from ClickHouse (one-time, ~5 min)
python src/build_unified_db.py --countries US

# Launch
python src/pitch_server.py
# → http://localhost:5050
```

---

*Part of the Data & Insights product suite.*
