# Q5. Fix broken JSON (1)

**Exam id:** `q-broken-json-server`  
**Status:** Accepted. Saved 22/22.

## Requirements

Repair `broken.json` so `json.loads` succeeds, then paste the **entire** fixed document. About nine syntax errors (missing commas, comments, single quotes, unquoted keys, trailing commas). The accepted file is a JSON array of 300 objects (`record_00000` …).

## Files

- `q5/broken.json` — from the exam ZIP
- `q5/fix_json.py` — repair script
- `q5/fixed.json` — paste this into the exam
- `q5/split_chunks.py` — optional paste helper, does not change JSON
- `q5/WRITEUP.md`

## Packages

Python stdlib (`json`, `re`).

## Setup

```powershell
python C:\Users\berwa\Desktop\kaggle-assignment\q5\fix_json.py
python -c "import json; d=json.load(open(r'C:\Users\berwa\Desktop\kaggle-assignment\q5\fixed.json',encoding='utf-8')); print(type(d).__name__, len(d))"
```

Expected: `list 300`.

## Code

`q5/fix_json.py` applies regex repairs, then walks `JSONDecodeError` positions until parse succeeds:

```python
fixed = re.sub(r'("[^"]*"\s*:\s*"[^"]*")(\s*\n\s*")', r'\1,\2', fixed)
fixed = re.sub(r'("[^"]*"\s*:\s*\d+\.?\d*)(\s*\n\s*")', r'\1,\2', fixed)
fixed = fixed.replace("'", '"')
fixed = re.sub(r',(\s*[}\]])', r'\1', fixed)
fixed = re.sub(r'//[^\n]*', '', fixed)
fixed = re.sub(r'([,{]\s*)([a-zA-Z_][a-zA-Z0-9_]*)\s*:', r'\1"\2":', fixed)
```

Then it inserts/deletes at `e.pos` for remaining delimiter errors and writes `fixed.json`.

## Accepted answer

The contents of `q5/fixed.json`. Do not paste `broken.json`.
