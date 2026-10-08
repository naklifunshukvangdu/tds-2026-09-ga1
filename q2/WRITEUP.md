# Q2. Response headers (0.5)

**Exam id:** `q-http-headers-server`  
**Status:** Accepted. The header value rotates about once an hour; refetch before Check.

## Requirements

GET the `questionData` URL printed on the exam card (`questionId=q-http-headers-server`). Read response header `X-Exam-Code`. Paste that 8-character code. The body is not the answer.

## Files

- `q2/fetch-code.ps1` — prints `X-Exam-Code` from a URL you pass in
- `q2/WRITEUP.md`

Do not store `quizSign` in this folder.

## Packages

PowerShell `Invoke-WebRequest`, or a browser `fetch`.

## Setup

Copy the `curl -i "..."` URL from the Q2 card, then:

```powershell
cd C:\Users\berwa\Desktop\kaggle-assignment\q2
.\fetch-code.ps1 "<paste questionData URL from the card>"
```

Or in the exam page console:

```javascript
const url = document.querySelector('[data-question="q-http-headers-server"] pre code').textContent.match(/https:\S+/)[0];
const resp = await fetch(url);
resp.headers.get("x-exam-code");
```

## Code

`q2/fetch-code.ps1`:

```powershell
$resp = Invoke-WebRequest -Uri $url -UseBasicParsing
$resp.Headers["X-Exam-Code"]
```

## Accepted answer

Earlier accepted code: `8K28T7F7`. After Save-all on 7 Oct 2026 the live code was `N375EVXM`. Refetch; do not reuse a stale code.
