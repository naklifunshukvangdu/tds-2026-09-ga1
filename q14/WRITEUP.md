# Q14. MCP server (3)

**Exam id:** `q-mcp-server-live-server`  
**Status:** Accepted. Live URL; keep Flask and the tunnel up through the deadline.

## Requirements

HTTP MCP endpoint at `/mcp`. One tool `solve_challenge`. `inputSchema` has no required properties. On `tools/call`, ignore JSON arguments and read header `X-Exam-Challenge`. Return the first 16 lowercase hex chars of:

```text
SHA-256("<challenge>:<course-email>")
```

Also handle `initialize`, `notifications/initialized`, `tools/list`, and `ping`.

Grader: “Live MCP checks passed.”

## Files

- `q14/server.py` — Flask JSON-RPC
- `q14/requirements.txt`
- `q14/WRITEUP.md`

## Packages

```powershell
cd C:\Users\berwa\Desktop\kaggle-assignment\q14
pip install -r requirements.txt
```

(`flask`)

## Setup

```powershell
cd C:\Users\berwa\Desktop\kaggle-assignment\q14
python server.py
```

```powershell
cloudflared tunnel --url http://localhost:8014
```

Submit `https://<host>.trycloudflare.com/mcp`.

Health check (Python, not PowerShell quoting):

```python
import json, urllib.request
url = "https://<host>.trycloudflare.com/mcp"
body = json.dumps({"jsonrpc":"2.0","id":1,"method":"tools/list"}).encode()
req = urllib.request.Request(url, data=body, headers={"Content-Type":"application/json"})
print(urllib.request.urlopen(req).read())
```

## Code

`q14/server.py` (port 8014). Tool call:

```python
NORMALIZED_EMAIL = "<course-email>".strip().lower()
challenge = request.headers.get("X-Exam-Challenge", "").strip()
digest_input = f"{challenge}:{NORMALIZED_EMAIL}"
answer = hashlib.sha256(digest_input.encode("utf-8")).hexdigest()[:16].lower()
return _jsonrpc_result(req_id, {"content": [{"type": "text", "text": answer}]})
```

Routes `/` and `/mcp` both accept POST.

## Accepted answer

Working public URL on 7 Oct 2026:

```text
https://parenting-breaks-pix-sewing.trycloudflare.com/mcp
```
