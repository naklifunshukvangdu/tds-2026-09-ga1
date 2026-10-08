# Q15. The Ledger Agent (4)

**Exam id:** `q-ledger-agent-server`  
**Status:** Accepted. Live URL; keep the server and tunnel up through the deadline.

## Requirements

Public HTTPS `POST` that takes `{"question": "..."}` and returns `{"answer": ...}` in under 15 seconds per question. Data is this student’s ledger API (orders, rates, export). API notes:

- Business dates: Asia/Kolkata
- Duplicate order ids: latest `updated_at` is current
- Only `status` `paid` counts as revenue

The grader asks several unseen questions and stops at the first wrong or late answer.

Grader: **All 6 questions answered correctly.**

## Files

- `q15/cache.py` — download orders + rates (`quizSign` is in this script; do not copy it here)
- `q15/orders_raw.json`, `q15/rates.json`
- `q15/data.py` — Kolkata dates, USD conversion, current / paid / refunded snapshots
- `q15/queries.py` — metrics
- `q15/llm.py` — regex mapper (`_fast_answer`); Groq is not used on the passing path
- `q15/server.py` — FastAPI `POST /ask`
- `q15/requirements.txt`
- `q15/WRITEUP.md`

Do not copy API keys from `llm.py` into notes.

## Packages

```powershell
cd C:\Users\berwa\Desktop\kaggle-assignment\q15
pip install -r requirements.txt
```

(`fastapi`, `uvicorn`, `requests`; `groq` is imported by `llm.py` even though the passing path does not call it)

## Setup

```powershell
cd C:\Users\berwa\Desktop\kaggle-assignment\q15
python cache.py
python -u server.py
```

```powershell
cloudflared tunnel --url http://localhost:8015
```

Submit `https://<host>.trycloudflare.com/ask`.

Known-good check:

```python
{"question": "What was the total revenue in USD from the South region in June 2026?"}
# -> {"answer": "23679.13"}
```

## Code

Passing metric rules in `queries.py` / `llm.py`:

- Revenue / “earn” / AOV: **current paid** rows only (latest row still `paid`), `created_at` month in Asia/Kolkata
- Distinct customers: current paid rows for that product (word-boundary product match so “Air Fryer” does not match East)
- Refunded counts “placed in \<month\>”: latest refunded snapshot, `created_at` month Kolkata
- “Which product…”: product name only
- Money: plain two-decimal string (`"23679.13"`), not `Total revenue: $…`
- Counts: digit strings (`"199"`)

`llm.py` `answer_question` returns `_fast_answer` immediately. Groq was too slow for the 15s limit.

```python
if re.search(r"\b(revenue|earn|earned|earning|sales|income)\b", ql) and "refund" not in ql:
    return _format({"op": "total_revenue", "usd": queries.current_paid_revenue(metric_filters)}, question)
```

`server.py` listens on port 8015, CORS `*`.

## Accepted answer

Working public URL on 7 Oct 2026:

```text
https://editors-fig-marshall-layers.trycloudflare.com/ask
```

Sample grader batch that passed: South June `23679.13`, East AOV `274.63`, Mixer, South worth `269.22`, Air Fryer `199`, West January refunds `26`.
