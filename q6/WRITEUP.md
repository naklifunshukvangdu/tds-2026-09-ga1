# Q6. Config precedence (1)

**Exam id:** `q-config-precedence-server`  
**Status:** Accepted. Live URL; keep the server and tunnel up through the deadline.

## Requirements

Public `GET /effective-config`. Merge layers, low to high:

1. Defaults: `port` 8000, `workers` 1, `debug` false, `log_level` `info`, `api_key` `default-secret-000`
2. `config.development.yaml`: `workers` 3, `log_level` `error`
3. `.env` with `APP_*` keys, prefix stripped and lowercased (`APP_PORT=8278`, `APP_LOG_LEVEL=warning`, `APP_API_KEY=…`)
4. OS environment variables named `APP_*`
5. Query `?set=key=value` (highest)

Coerce ints and bools. Mask `api_key` as `****`. Browser Check needs CORS `allow_origins=["*"]`.

Grader: “Config precedence, type coercion, alias, and secret masking all verified.”

## Files

- `q6/server.py` — FastAPI app
- `q6/config.development.yaml`
- `q6/.env` — local secrets (not copied here)
- `q6/.env.example` — keys without values
- `q6/requirements.txt`
- `q6/WRITEUP.md`

## Packages

```powershell
cd C:\Users\berwa\Desktop\kaggle-assignment\q6
pip install -r requirements.txt
```

(`fastapi`, `uvicorn`, `pyyaml`, `python-dotenv`)

## Setup

Two terminals:

```powershell
cd C:\Users\berwa\Desktop\kaggle-assignment\q6
python server.py
```

```powershell
cloudflared tunnel --url http://localhost:8278
```

Submit `https://<host>.trycloudflare.com/effective-config`. Quick tunnels expire; mint a new host if Check fails.

Healthy response:

```json
{"port":8278,"workers":3,"debug":false,"log_level":"warning","api_key":"****"}
```

## Code

`q6/server.py` merge order:

```python
merged = dict(DEFAULTS)
merged.update(load_yaml_layer())
merged.update(load_env_file_layer())
merged.update(load_os_env_layer())
merged.update(parse_cli_overrides(sets))
```

CORS and mask:

```python
app.add_middleware(CORSMiddleware, allow_origins=["*"], allow_methods=["*"], allow_headers=["*"])

@app.get("/effective-config")
def get_effective_config(set: Optional[List[str]] = Query(default=None)):
    response = dict(effective_config(set))
    response["api_key"] = "****"
    return response
```

## Accepted answer

Working public URL on 7 Oct 2026:

```text
https://physiology-audit-vids-medal.trycloudflare.com/effective-config
```
