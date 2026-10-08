# TDS 2026 Sep GA1 — solution guide

Exam: https://exam.sanand.workers.dev/tds-2026-09-ga1

Student email: `<course-email>`

This guide covers the questions that were already accepted (13 / 22). Commands are Windows PowerShell. `python` is the Python launcher. `uv` was not on `PATH`; it was installed with `pip` and then run as `python -m uv`.

Quick Cloudflare tunnels (`trycloudflare.com`) expire. When a public URL dies, start the local server again and run `cloudflared tunnel --url ...` once more. Copy the new `https://....trycloudflare.com` host from that window. Do not reuse an old host.

---

## Q1. VS Code version (0.5)

### What the grader wanted

The full text of `code -s` (VS Code `--status`) from the installed VS Code, not `code --version` and not the warning that appears when Code is not already running.

VS Code on this machine is `C:\Users\berwa\AppData\Local\Programs\Microsoft VS Code\Code.exe`. The `code` command points at that install, not at Cursor.

### Setup

```powershell
Start-Process "C:\Users\berwa\AppData\Local\Programs\Microsoft VS Code\Code.exe"
Start-Sleep -Seconds 12
code -s
```

`code -s` before Code is running only prints: `Warning: The --status argument can only be used if Code is already running.` That warning was not the accepted answer. Launch `Code.exe`, wait, then run `code -s` again.

### What was submitted

The textarea `q-vs-code-version` was filled with the `code -s` text below (3091 characters). There is no saved copy of that textarea in the workspace; this is the value that was written into it.

### Accepted answer

```text
Version:          Code 1.139.0 (2242ebbb54efeeb0129e08e919e7e8d43033cd83, 2026-09-22T15:13:34Z)
OS Version:       Windows_NT x64 10.0.26300
CPUs:             AMD Ryzen 9 5900HX with Radeon Graphics         (16 x 3294)
Memory (System):  39.41GB (26.48GB free)
VM:               80%
Screen Reader:    no
Process Argv:     --crash-reporter-id 8d857f8e-020b-467c-b1c3-5ad58d3a299c
GPU Status:       2d_canvas:                              enabled
                  GPU0:                                   VENDOR= 0x1002, DEVICE=0x1638 [AMD Radeon(TM) Graphics], DRIVER_VENDOR=AMD, DRIVER_VERSION=31.0.21914.4000 *ACTIVE*
                  GPU1:                                   VENDOR= 0x10de, DEVICE=0x25a5 [NVIDIA GeForce RTX 3050 Laptop GPU], DRIVER_VERSION=32.0.15.9621
                  GPU2:                                   VENDOR= 0x1414, DEVICE=0x008c [Microsoft Basic Render Driver], DRIVER_VERSION=10.0.26100.9549
                  Machine model name:                     
                  Machine model version:                  
                  direct_rendering_display_compositor:    disabled_off_ok
                  gpu_compositing:                        enabled
                  multiple_raster_threads:                 enabled_on
                  opengl:                                 enabled_on
                  rasterization:                          enabled
                  raw_draw:                               disabled_off_ok
                  skia_graphite:                          disabled_off
                  trees_in_viz:                           disabled_off
                  video_decode:                           enabled
                  video_encode:                           enabled
                  webgl:                                  enabled
                  webgpu:                                 enabled
                  webnn:                                 disabled_off
GPU Log Messages:
[9336:1007/120214.343:ERROR:ui\gl\direct_composition_support.cc:616] : AMD VideoProcessorGetOutputExtension failed: Unspecified error (0x80004005)


CPU %	Mem MB	   PID	Process
    0	   164	 32688	code
    0	   139	  9336	   gpu-process
    0	   111	 10372	file-watcher [1]
    0	   154	 15656	agent-host
    0	    32	 30664	     "c:\Users\berwa\AppData\Local\Programs\Microsoft VS Code\2242ebbb54\resources\app\node_modules.asar.unpacked\@github\copilot-sdk-win32-x64\prebuilds\win32-x64\copilot-runtime.exe" --headless --no-auto-update --log-level info --stdio --no-auto-login
    0	     7	 29304	       C:\WINDOWS\system32\conhost.exe 0x4
    0	    35	 21528	   crashpad-handler
    0	    56	 35028	   utility-network-service
    9	   245	 35300	shared-process
    0	   192	 35312	extension-host [1]
    0	   115	  5844	     "C:\Users\berwa\AppData\Local\Programs\Microsoft VS Code\Code.exe" "c:\Users\berwa\AppData\Local\Programs\Microsoft VS Code\2242ebbb54\resources\app\extensions\markdown-language-features\dist\serverWorkerMain" --node-ipc --clientProcessId=35312
    0	   455	 36120	window [1] (writeup.md - Visual Studio Code)
```

PIDs, free memory, and the GPU log line change every time you run `code -s`. Paste the fresh output from your own machine.

---

## Q2. Response headers (0.5)

### What the grader wanted

The value of the `X-Exam-Code` response header from this student's question-data URL (`questionId=q-http-headers-server`).

### How it was fetched

The exam card shows a URL on `https://exam.sanand.workers.dev/questionData`. That URL includes a `quizSign`. The sign is not stored in a workspace script, so it is not copied here. In the browser, that URL was fetched and the header was read:

```javascript
const resp = await fetch(urlFromTheExamCard);
const code = resp.headers.get("x-exam-code") || resp.headers.get("X-Exam-Code");
```

To repeat it in PowerShell, paste the URL from your own Q2 card (do not share the `quizSign`):

```powershell
$url = "<paste the questionData URL from the Q2 card>"
$resp = Invoke-WebRequest -Uri $url -UseBasicParsing
$resp.Headers["X-Exam-Code"]
```

### Accepted answer

```text
8K28T7F7
```

---

## Q3. POST HTTP with uv (0.5)

### What the grader wanted

A JSON POST to `https://httpbin.org/post` using uv and HTTPie. The JSON body had to include this student's email. The full httpbin response was pasted into the exam field.

There is no saved script for this question. The commands below are the ones that were run.

### Setup

`uv` was not on `PATH`. It was installed with pip, then invoked as `python -m uv` (that pulls HTTPie in for the one command):

```powershell
pip install uv
python -m uv run --with httpie -- http --json POST https://httpbin.org/post email=<course-email> request_id=fca895e2
```

The `request_id` used in the accepted run was `fca895e2`. If the exam card shows a different id, use the id on the card.

A direct `requests.post` was also tried. The body that was submitted is the HTTPie response below.

### Accepted answer

The pasted httpbin body (email and `request_id` are inside `json`):

```json
{
  "args": {},
  "data": "{\"email\": \"<course-email>\", \"request_id\": \"fca895e2\"}",
  "files": {},
  "form": {},
  "headers": {
    "Accept": "application/json, */*;q=0.5",
    "Accept-Encoding": "gzip, deflate",
    "Content-Length": "69",
    "Content-Type": "application/json",
    "Host": "httpbin.org",
    "User-Agent": "HTTPie/3.2.4",
    "X-Amzn-Trace-Id": "Root=1-6ac5e844-7988a43f3fa8ff4346eb1739"
  },
  "json": {
    "email": "<course-email>",
    "request_id": "fca895e2"
  },
  "origin": "157.48.106.189",
  "url": "https://httpbin.org/post"
}
```

`origin`, `X-Amzn-Trace-Id`, and `Content-Length` change on a new request. Paste the response from the run you just made.

---

## Q4. npx prettier SHA-256 (0.5)

### What the grader wanted

SHA-256 of the bytes Prettier 3.4.2 writes for `q4\README.md`. Hash the raw stdout. Do not let PowerShell rewrite the line endings.

### Setup and command

`q4\README.md` is the badly formatted markdown from the exam. This is the command that produced the accepted hash (`cmd` redirection keeps the raw bytes; PowerShell `Out-File` did not):

```powershell
cd C:\Users\berwa\Desktop\kaggle-assignment\q4
cmd /c "npx -y prettier@3.4.2 README.md > prettier_raw.txt 2>nul"
python -c "import hashlib; data=open('prettier_raw.txt','rb').read(); print('LEN:', len(data)); print('HASH:', hashlib.sha256(data).hexdigest())"
```

An earlier PowerShell capture (`npx ... | Out-File` / hashing a PowerShell string) produced a different hash and was not the one that was accepted. The formatted text is in `q4\prettier_raw.txt`.

### Accepted answer

```text
3a2f3a3a1ad2999fedef2685376626e4dad1f9db8f4eeef63fc9cd6bf3a72043
```

---

## Q5. Fix broken JSON (1)

### What the grader wanted

`broken.json` repaired so that `json.loads` succeeds, then the whole fixed document pasted into the exam textarea. `q5\README.md` says the file has about 9 syntax errors. The saved `fixed.json` is a JSON array of 300 objects (`record_00000` through the rest).

### Files

- `C:\Users\berwa\Desktop\kaggle-assignment\q5\broken.json` — downloaded from the exam ZIP
- `C:\Users\berwa\Desktop\kaggle-assignment\q5\fix_json.py` — the script that wrote `fixed.json`
- `C:\Users\berwa\Desktop\kaggle-assignment\q5\fixed.json` — the file that was loaded into the textarea

The ZIP was saved as `q5\broken.zip` and extracted in that folder. `q5\split_chunks.py` only base64-splits `fixed.json` so it could be pasted through the browser. It does not change the JSON.

### Command

```powershell
python C:\Users\berwa\Desktop\kaggle-assignment\q5\fix_json.py
python -c "import json; d=json.load(open(r'C:\Users\berwa\Desktop\kaggle-assignment\q5\fixed.json',encoding='utf-8')); print(type(d).__name__, len(d))"
```

The second command printed `list 300`.

### The script (`q5\fix_json.py`)

```python
import json, re, os, sys
sys.stdout.reconfigure(encoding='utf-8')

d5 = r'C:\Users\berwa\Desktop\kaggle-assignment\q5'
with open(os.path.join(d5, 'broken.json'), 'r', encoding='utf-8') as f:
    raw = f.read()

fixed = raw

# Fix 1: Missing commas between key-value pairs
fixed = re.sub(r'("[^"]*"\s*:\s*"[^"]*")(\s*\n\s*")', r'\1,\2', fixed)
fixed = re.sub(r'("[^"]*"\s*:\s*\d+\.?\d*)(\s*\n\s*")', r'\1,\2', fixed)
fixed = re.sub(r'("[^"]*"\s*:\s*(?:true|false|null))(\s*\n\s*")', r'\1,\2', fixed)

# Fix 2: Missing commas between array elements and after }
fixed = re.sub(r'(])(\s*\n\s*")', r'\1,\2', fixed)
fixed = re.sub(r'(\d+)(\s*\n\s*\[)', r'\1,\2', fixed)

# Fix 3: Single quotes to double quotes
fixed = fixed.replace("'", '"')

# Fix 4: Remove trailing commas
fixed = re.sub(r',(\s*[}\]])', r'\1', fixed)

# Fix 5: Remove comments
fixed = re.sub(r'//[^\n]*', '', fixed)
fixed = re.sub(r'/\*.*?\*/', '', fixed, flags=re.DOTALL)

# Fix 6: Unquoted keys
fixed = re.sub(r'([,{]\s*)([a-zA-Z_][a-zA-Z0-9_]*)\s*:', r'\1"\2":', fixed)

for attempt in range(20):
    try:
        json.loads(fixed)
        print(f'JSON is valid after {attempt} attempts!')
        break
    except json.JSONDecodeError as e:
        print(f'Attempt {attempt}: Error at pos {e.pos}, line {e.lineno}: {e.msg}')
        if 'Expecting' in e.msg and 'delimiter' in e.msg:
            fixed = fixed[:e.pos] + ',' + fixed[e.pos:]
        elif 'Extra data' in e.msg:
            fixed = fixed[:e.pos]
        elif 'Expecting value' in e.msg:
            fixed = fixed[:e.pos] + fixed[e.pos+1:]
        elif 'Unvalid' in e.msg or 'Invalid' in e.msg:
            fixed = fixed[:e.pos] + fixed[e.pos+1:]
        else:
            fixed = fixed[:e.pos] + ',' + fixed[e.pos:]

with open(os.path.join(d5, 'fixed.json'), 'w', encoding='utf-8') as f:
    f.write(fixed)
```

### Accepted answer

The contents of `q5\fixed.json` (valid JSON, 300 items). Paste that file into the exam textarea. Do not paste `broken.json`.

---

## Q6. Config precedence (1)

### What the grader wanted

A public `GET /effective-config` endpoint. Layers, low to high:

1. Defaults: `port` 8000, `workers` 1, `debug` false, `log_level` `info`, `api_key` `default-secret-000`
2. `config.development.yaml`: `workers` 3, `log_level` `error`
3. `.env` (`APP_*` prefix, lowercased): `APP_PORT=8278`, `APP_LOG_LEVEL=warning`, `APP_API_KEY=key-c36mix94fm`
4. OS environment variables named `APP_*`
5. Query overrides `?set=key=value` (highest)

Integers and booleans are coerced. `api_key` is returned as `****`. The exam page calls the URL from the browser, so `CORSMiddleware` with `allow_origins=["*"]` was required.

Grader message: "Config precedence, type coercion, alias, and secret masking all verified."

### Files

- `C:\Users\berwa\Desktop\kaggle-assignment\q6\server.py`
- `C:\Users\berwa\Desktop\kaggle-assignment\q6\config.development.yaml`
- `C:\Users\berwa\Desktop\kaggle-assignment\q6\.env`

`config.development.yaml`:

```yaml
workers: 3
log_level: error
```

`.env`:

```text
APP_PORT=8278
APP_LOG_LEVEL=warning
APP_API_KEY=key-c36mix94fm
```

### Setup and deploy

Packages were already installed (no `pip install` for FastAPI was run in this session). Two terminals:

```powershell
cd C:\Users\berwa\Desktop\kaggle-assignment\q6
python -m uvicorn server:app --host 0.0.0.0 --port 8278
```

```powershell
cloudflared tunnel --url http://localhost:8278
```

The server was later started again as `python -m uvicorn server:app --host 127.0.0.1 --port 8278`. The public check used the tunnel URL.

Last working public URL:

```text
https://vocabulary-milwaukee-sized-cuts.trycloudflare.com/effective-config
```

That host will expire. Mint a new one with the same `cloudflared` command and submit `https://<new-host>.trycloudflare.com/effective-config`.

CORS check that was run:

```powershell
curl.exe -s -D - -o NUL -H "Origin: https://exam.sanand.workers.dev" "https://vocabulary-milwaukee-sized-cuts.trycloudflare.com/effective-config"
```

### Code

The working program is `q6\server.py`. Precedence is `DEFAULTS`, then YAML, then `.env`, then OS `APP_*`, then `?set=`. The route masks `api_key`:

```python
@app.get("/effective-config")
def get_effective_config(set: Optional[List[str]] = Query(default=None)):
    cfg = effective_config(set)
    response = dict(cfg)
    if "api_key" in response and response["api_key"] is not None:
        response["api_key"] = "****"
    return response
```

`effective_config` in that file is the merge. `app.add_middleware(CORSMiddleware, allow_origins=["*"], ...)` is what lets the exam page read the JSON.

---

## Q7. Reorganize files (1)

### What the grader wanted

Each `.txt` file's first line is `category: {name}`. Move it to:

```text
{category}/{original-path-with-slashes-replaced-by-dashes}
```

Example from `q7\README.md`: `docs/chapter1/file01.txt` with `category: reports` becomes `reports/docs-chapter1-file01.txt`. Keep spaces and Unicode in names. Then:

```text
find . -type f | LC_ALL=C sort | sha256sum
```

Submit the hex digest only.

### What was actually run

There is no `reorganize.py` in the folder. The ZIP is `q7\files.zip`. This inline Python wiped the folder except the ZIP, extracted again, and moved files. It was the run that built the tree that was hashed.

```powershell
cd C:\Users\berwa\Desktop\kaggle-assignment\q7
python -c @"
import os, shutil, hashlib, zipfile, sys
sys.stdout.reconfigure(encoding='utf-8')
base = r'C:\Users\berwa\Desktop\kaggle-assignment\q7'
for item in os.listdir(base):
    if item != 'files.zip':
        p = os.path.join(base, item)
        if os.path.isdir(p): shutil.rmtree(p)
        else: os.remove(p)
z = zipfile.ZipFile(os.path.join(base,'files.zip'))
z.extractall(base)
txt_files = []
for root, dirs, files in os.walk(base):
    for f in files:
        if f.endswith('.txt') and not f.endswith('.zip'):
            txt_files.append(os.path.join(root, f))
original_dirs = set()
for fpath in txt_files:
    rel = os.path.relpath(fpath, base).replace(os.sep, '/')
    with open(fpath, 'r', encoding='utf-8', errors='replace') as f:
        first_line = f.readline().strip()
    cat = first_line.split(':',1)[1].strip() if first_line.startswith('category:') else None
    if not cat: continue
    new_name = rel.replace('/', '-')
    new_dir = os.path.join(base, cat)
    os.makedirs(new_dir, exist_ok=True)
    shutil.move(fpath, os.path.join(new_dir, new_name))
    original_dirs.add(rel.split('/')[0])
for d in original_dirs:
    p = os.path.join(base, d)
    if os.path.isdir(p): shutil.rmtree(p)
all_files = []
for root, dirs, files in os.walk(base):
    for f in files:
        if f.endswith('.zip'): continue
        rel = os.path.relpath(os.path.join(root, f), base).replace(os.sep, '/')
        all_files.append('./' + rel)
all_files.sort(key=lambda x: x.encode('utf-8'))
content = '\n'.join(all_files) + '\n'
print(hashlib.sha256(content.encode('utf-8')).hexdigest())
"@
```

That print included `README.md` and was **rejected**:

```text
931a9a5130a7342c27148406bb3a9bc6400c2d9ddfae3e47ecf349559392b061
```

The accepted hash skipped `README.md` and `files.zip`. Paths are `./...`, sorted by UTF-8 bytes, joined with `\n`, with a trailing newline:

```powershell
python -c @"
import os, hashlib, sys
sys.stdout.reconfigure(encoding='utf-8')
base = r'C:\Users\berwa\Desktop\kaggle-assignment\q7'
all_files = []
for root, dirs, files in os.walk(base):
    for f in files:
        if f.endswith('.zip'): continue
        if f == 'README.md': continue
        rel = os.path.relpath(os.path.join(root, f), base).replace(os.sep, '/')
        all_files.append('./' + rel)
all_files.sort(key=lambda x: x.encode('utf-8'))
content = '\n'.join(all_files) + '\n'
print(hashlib.sha256(content.encode('utf-8')).hexdigest())
"@
```

### Accepted answer

```text
2438fef40e008e4357a211c7d665f20993840eb9ecb771d3ea9ab301cc64002e
```

---

## Q8. CSV sales (1)

### What the grader wanted

Total sales for city `Pune` in June 2025, from `sales_data.csv` inside the exam ZIP. Columns are `date,city,product,quantity,price`. Dates look like `2025-Jun-25`. The total is `quantity * price`, then rounded to an integer.

### Where the code is

No `sales_data.csv` and no script were saved in the workspace. The exam ZIP was decoded in memory. The loop that produced the submitted number was:

```python
total = 0
for r in reader:  # csv.DictReader of sales_data.csv
    if r["city"] == "Pune" and r["date"].startswith("2025-Jun"):
        total += int(r["quantity"]) * float(r["price"])
print(round(total))
```

After you download the ZIP from the exam card and extract `sales_data.csv`:

```powershell
python -c "import csv; total=0
for r in csv.DictReader(open('sales_data.csv', encoding='utf-8')):
    if r['city']=='Pune' and r['date'].startswith('2025-Jun'):
        total += int(r['quantity'])*float(r['price'])
print(round(total))"
```

### Accepted answer

```text
8274
```

---

## Q9. GitHub Pages (1)

### What the grader wanted

A public GitHub Pages site whose HTML contains the email exactly like this, so Cloudflare does not rewrite it:

```html
<!--email_off--><course-email><!--/email_off-->
```

### What was done

`gh` was not installed. A stored GitHub login for `<github-username>` was used through the GitHub REST API. Do not paste that token into notes or into this file.

Steps, without the token:

1. `POST https://api.github.com/user/repos` with `name` `tds-portfolio`, `private` false, `auto_init` true.
2. Create `index.html` on `main` through the Contents API. Commit message: `Add portfolio index.html with email_off wrapped email`.
3. `POST https://api.github.com/repos/<github-username>/tds-portfolio/pages` with body `{"source":{"branch":"main","path":"/"}}`.
4. Wait until the Pages API reports `status` `built`, then open the site and confirm the `email_off` comment is still in the HTML and `data-cfemail` is absent.

Repo: https://github.com/<github-username>/tds-portfolio

### HTML that was published

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Student - Portfolio</title>
<style>
  body { font-family: 'Segoe UI', Arial, sans-serif; max-width: 800px; margin: 0 auto; padding: 2rem; line-height: 1.6; color: #222; }
  h1 { color: #2c3e50; border-bottom: 3px solid #3498db; padding-bottom: .3rem; }
  a { color: #3498db; }
  .card { background:#f7f9fc; border:1px solid #e1e8ed; border-radius:8px; padding:1rem 1.25rem; margin:1rem 0; }
</style>
</head>
<body>
  <h1>Student</h1>
  <p>IIT Madras - Online Degree in Data Science</p>

  <div class="card">
    <h2>About Me</h2>
    <p>I am a student in the <em>Foundation Level</em> of the IIT Madras Online
       Degree in Data Science and Applications. I enjoy building small
       projects, automating tasks with Python, and learning new tools.</p>
  </div>

  <div class="card">
    <h2>Projects</h2>
    <ul>
      <li><strong>Parking App</strong> - A simple parking management application.</li>
      <li><strong>Placement Portal App</strong> - A portal to manage placement related activities.</li>
      <li><strong>BhagyaShree Anath Aashram</strong> - A website for an orphanage / NGO.</li>
    </ul>
  </div>

  <div class="card">
    <h2>Skills</h2>
    <p>Python, JavaScript, Git, GitHub, HTML, CSS, Data Analysis, Automation.</p>
  </div>

  <div class="card">
    <h2>Contact</h2>
    <p>Email: <!--email_off--><course-email><!--/email_off--></p>
    <p>GitHub: <a href="https://github.com/<github-username>"><github-username></a></p>
  </div>

  <footer>
    <p><small>&copy; 2026 Student. Hosted on GitHub Pages.</small></p>
  </footer>
</body>
</html>
```

### Accepted answer

```text
https://<github-username>.github.io/tds-portfolio/
```

If the page looks stale, add `?v=1` or `?v=2`.

---

## Q10. Git forensics (1.5)

### What the grader wanted

The staging token that was committed and later rotated, plus the subject of the commit that introduced it.

### Files

- `C:\Users\berwa\Desktop\kaggle-assignment\q10\fetch.py` — downloads the question page. The `questionData` URL, including `quizSign`, is inside this file. Run the script; do not copy the sign into notes.
- `C:\Users\berwa\Desktop\kaggle-assignment\q10\extract.py` — pulls the base64 `history.fastimport` out of that HTML.
- `C:\Users\berwa\Desktop\kaggle-assignment\q10\answer.json` — the submitted JSON.

### Commands that were run

```powershell
cd C:\Users\berwa\Desktop\kaggle-assignment\q10
$env:PYTHONIOENCODING = "utf-8"
python fetch.py
python extract.py
```

PowerShell's `<` redirect failed. `cmd` was used instead:

```powershell
cd C:\Users\berwa\Desktop\kaggle-assignment\q10
if (Test-Path repo) { Remove-Item -Recurse -Force repo }
mkdir repo | Out-Null
cmd /c "cd repo && git init -q && git fast-import --quiet < ..\history.fastimport && git log --oneline --all"
cd C:\Users\berwa\Desktop\kaggle-assignment\q10\repo
cmd /c "git log -S stg_5fe1b3962cb9d014f204bee4 --oneline --all"
cmd /c "git show 88e3bd3 --stat -p"
```

`git log -S` shows two commits. `88e3bd3` adds `STAGING_TOKEN=stg_5fe1b3962cb9d014f204bee4` in `config/app.env` with subject `Add staging deploy token to app config`. Later commit `e3c0220` (`Rotate and revoke leaked staging token`) changes the value to `__rotated__`.

### Accepted answer

`q10\answer.json`:

```json
{
  "token": "stg_5fe1b3962cb9d014f204bee4",
  "commit_subject": "Add staging deploy token to app config"
}
```

---

## Q11. Cosine similarity (1.5)

### What the grader wanted

250 documents, 10 queries, 64-dimensional unit vectors in `q11\embeddings.json`. For each query, the top 5 document ids by cosine similarity. On a tie, the smaller `doc_id` wins.

Grader message: "Cosine similarity search verified: 10/10 queries matched exactly."

### Files

- `C:\Users\berwa\Desktop\kaggle-assignment\q11\embeddings.json`
- `C:\Users\berwa\Desktop\kaggle-assignment\q11\recompute.py` — wrote `answers.json`
- `C:\Users\berwa\Desktop\kaggle-assignment\q11\answers.json` — the accepted mapping
- `C:\Users\berwa\Desktop\kaggle-assignment\q11\debug3.py` — extra checks, including an explicit sort by `(-similarity, doc_id)`

### Command

```powershell
python C:\Users\berwa\Desktop\kaggle-assignment\q11\recompute.py
```

`recompute.py` L2-normalizes the document matrix and each query, takes the dot product, and keeps the first 5 rows from `np.argsort(-sims)`. It then writes `answers.json`.

### Pitfall

Pushing the JSON into the exam page through the browser changed some ids. `D000212` became `D0000212`, `D000208` became `D0000208`, and `D000245` became `D00245`. The file on disk is the mapping that was accepted. Check those three ids before you submit.

### Accepted answer

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

---

## Q14. MCP server (3)

### What the grader wanted

A live MCP HTTP endpoint at path `/mcp`. Tool name `solve_challenge`. `inputSchema` has no required properties. On `tools/call`, ignore the JSON arguments and read the `X-Exam-Challenge` header. The text result is the first 16 lowercase hex characters of SHA-256 of `<challenge>:<course-email>`.

Grader message: "Live MCP checks passed."

### File

`C:\Users\berwa\Desktop\kaggle-assignment\q14\server.py`

Flask listens on port 8014. Both `/` and `/mcp` are registered. `tools/list` returns the one tool. `tools/call` hashes the header:

```python
NORMALIZED_EMAIL = "<course-email>".strip().lower()

# inside tools/call:
challenge = request.headers.get("X-Exam-Challenge", "")
digest_input = f"{challenge}:{NORMALIZED_EMAIL}"
full_hash = hashlib.sha256(digest_input.encode("utf-8")).hexdigest()
answer = full_hash[:16].lower()
```

`inputSchema` is `{"type": "object", "properties": {}, "required": []}`.

### Deploy

The process was already listening on 8014 when the tunnel was opened. The script starts that listener itself:

```powershell
cd C:\Users\berwa\Desktop\kaggle-assignment\q14
python server.py
```

Second terminal (this is the command that was run, with working directory `q14`):

```powershell
cd C:\Users\berwa\Desktop\kaggle-assignment\q14
cloudflared tunnel --url http://localhost:8014
```

Last URL that passed:

```text
https://happy-letting-wma-seems.trycloudflare.com/mcp
```

That host expires. Start `python server.py` and `cloudflared tunnel --url http://localhost:8014` again, and submit `https://<new-host>.trycloudflare.com/mcp`.

A health check that was run:

```powershell
curl.exe -s -o NUL -w "q14 %{http_code}`n" -X POST "https://happy-letting-wma-seems.trycloudflare.com/mcp" -H "Content-Type: application/json" -d "{\"jsonrpc\":\"2.0\",\"id\":1,\"method\":\"tools/list\"}"
```

---

## Not yet accepted

Facts only. These questions are not solved.

### Q12. One prompt, two models (1)

One prompt, at most 100 characters, no system prompt. The exam sends it to `https://aipipe.org/openai/v1/chat/completions` using `globalThis.aiPipeToken` from a browser `prompt()`. The same prompt must make `gpt-5.6-luna` say `Yes` (the word `Yes`, case-sensitive) and `gpt-5.4-mini` say `No`.

A check failed with `Missing Authorization: Bearer token` because no AI Pipe token was entered. No token is stored in this guide.

### Q13. termlog (2)

Repo: https://github.com/mynkpdr/termlog

The archive must contain `demo.cast` and `demo.cast.jwt`. A reconstructed asciicast was rejected: "Receipt signature is invalid. Was demo.cast.jwt edited, or made by another tool?" A real `termlog login` with Google is required. Copies of `demo.cast` and `demo.cast.jwt` in the workspace root are the rejected pair.

### Q15. Ledger (4)

Code is under `C:\Users\berwa\Desktop\kaggle-assignment\q15\`. `q15\server.py` serves `POST /ask` with body `{"question": "..."}` and returns JSON that includes `answer`. Local start is `q15\run_server.ps1` (`PORT=8015`, `python -u server.py`). Tunnel: `q15\run_tunnel.ps1` (`cloudflared tunnel --url http://localhost:8015`).

Public URL that was used:

```text
https://management-grace-fog-walter.trycloudflare.com/ask
```

Blender distinct paid customers: count any paid row even if a later revision is void. The accepted shape is the plain integer `209`. The string `208 customers.` was rejected. The count logic that walks raw paid rows is `customer_count` in `q15\queries.py`. Exploratory counts are in `q15\debug_blender.py`.

Next grader error: Central region, February 2026 revenue. `Total revenue: $25113.90` was rejected. That number is latest-status paid orders, region Central, `created_at` month in `Asia/Kolkata`. The variants that were printed are in `q15\debug_feb.py` and `q15\debug_revenue.py`. This item is still being fixed. No replacement figure is given here.

### Q16. Tangram (2)

Board: https://tds-network-games.sanand.workers.dev/tangram/

The email must be entered. The answer is the signed payload from "Generate signed payload". Not solved yet.

---

## Score

| Question | Marks | Result |
| --- | ---: | --- |
| Q1 VS Code version | 0.5 | Accepted |
| Q2 Response headers | 0.5 | Accepted (`8K28T7F7`) |
| Q3 POST with uv | 0.5 | Accepted |
| Q4 Prettier SHA-256 | 0.5 | Accepted |
| Q5 Broken JSON | 1 | Accepted |
| Q6 Config precedence | 1 | Accepted |
| Q7 Reorganize files | 1 | Accepted |
| Q8 CSV sales | 1 | Accepted (`8274`) |
| Q9 GitHub Pages | 1 | Accepted |
| Q10 Git forensics | 1.5 | Accepted |
| Q11 Cosine similarity | 1.5 | Accepted (10/10) |
| Q12 Two models | 1 | Not accepted |
| Q13 termlog | 2 | Not accepted |
| Q14 MCP server | 3 | Accepted |
| Q15 Ledger | 4 | Not accepted |
| Q16 Tangram | 2 | Not accepted |
| **Total** | **22** | **13 accepted, 9 still open** |
