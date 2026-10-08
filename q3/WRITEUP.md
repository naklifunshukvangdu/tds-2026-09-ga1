# Q3. POST HTTP with uv (0.5)

**Exam id:** `q-uv-http-post`  
**Status:** Accepted. Saved 22/22.

## Requirements

POST JSON to `https://httpbin.org/post` using `uv run --with httpie`. Body fields:

- `email` = `<course-email>`
- `request_id` = the 8-hex id on the exam card (this attempt used `fca895e2`)

The grader checks `headers.Host == "httpbin.org"` and `json.email` / `json.request_id`. Paste the **full** httpbin JSON response.

## Files

- `q3/run.ps1` — the command that was run
- `q3/httpbin-response.json` — the submitted body
- `q3/WRITEUP.md`

## Packages

```powershell
pip install uv
```

HTTPie is pulled in for the one command via `--with httpie`.

## Setup

```powershell
cd C:\Users\berwa\Desktop\kaggle-assignment\q3
.\run.ps1
```

If `uv` is not on PATH, `python -m uv` is the launcher used here. If the card shows a different `request_id`, edit `run.ps1`.

## Code

```powershell
python -m uv run --with httpie -- http --json POST https://httpbin.org/post email=<course-email> request_id=fca895e2
```

## Accepted answer

`q3/httpbin-response.json`. `origin` and `X-Amzn-Trace-Id` change on a new request; paste the response from the run you just made.
