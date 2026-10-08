# Q11. Cosine similarity (1.5)

**Exam id:** `q-cosine-similarity-server`  
**Status:** Accepted. Saved 22/22.

## Requirements

250 documents, 10 queries, 64-d unit vectors in `embeddings.json`. For each query, return the top 5 `doc_id`s by cosine similarity. On a tie, the smaller `doc_id` wins. Submit a JSON object mapping query ids to arrays of five doc ids.

Grader: “Cosine similarity search verified: 10/10 queries matched exactly.”

## Files

- `q11/embeddings.json`
- `q11/recompute.py` — writes `answers.json`
- `q11/answers.json` — paste this
- `q11/debug3.py` — explicit `(-similarity, doc_id)` sort check
- `q11/requirements.txt`
- `q11/WRITEUP.md`

## Packages

```powershell
pip install -r C:\Users\berwa\Desktop\kaggle-assignment\q11\requirements.txt
```

(`numpy`)

## Setup

```powershell
python C:\Users\berwa\Desktop\kaggle-assignment\q11\recompute.py
```

Paste `q11/answers.json`. Pasting through some browser tools rewrote ids (`D000212` → `D0000212`). Check those ids before Check.

## Code

`q11/recompute.py`:

```python
doc_norm = doc_emb / np.linalg.norm(doc_emb, axis=1, keepdims=True)
sims = doc_norm @ qnorm
top5_idx = np.argsort(-sims)[:5]
answers[qid] = [doc_ids[i] for i in top5_idx]
```

Tie-break check in `debug3.py`:

```python
triples.sort(key=lambda t: (-t[0], t[1]))
```

## Accepted answer

```json
{
  "Q001": ["D000001", "D000021", "D000006", "D000101", "D000141"],
  "Q002": ["D000107", "D000087", "D000022", "D000212", "D000177"],
  "Q003": ["D000023", "D000093", "D000033", "D000073", "D000228"],
  "Q004": ["D000014", "D000064", "D000219", "D000159", "D000059"],
  "Q005": ["D000190", "D000040", "D000200", "D000250", "D000025"],
  "Q006": ["D000226", "D000136", "D000146", "D000116", "D000101"],
  "Q007": ["D000037", "D000087", "D000232", "D000212", "D000172"],
  "Q008": ["D000248", "D000093", "D000218", "D000208", "D000148"],
  "Q009": ["D000054", "D000219", "D000119", "D000144", "D000104"],
  "Q010": ["D000220", "D000190", "D000245", "D000155", "D000215"]
}
```
