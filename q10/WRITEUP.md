# Q10. Git forensics (1.5)

**Exam id:** `q-git-forensics-token-server`  
**Status:** Accepted. Saved 22/22.

## Requirements

From the exam `history.fastimport`, find the staging token that was committed and later rotated, plus the subject of the commit that introduced it. Submit JSON:

```json
{"token": "stg_…", "commit_subject": "…"}
```

## Files

- `q10/fetch.py` — downloads the question HTML (`quizSign` lives in this script; do not copy it into notes)
- `q10/extract.py` — writes `history.fastimport` from the data URI
- `q10/history.fastimport`
- `q10/answer.json` — submitted JSON
- `q10/repo/` — git repo after `git fast-import`
- `q10/requirements.txt`
- `q10/WRITEUP.md`

## Packages

```powershell
cd C:\Users\berwa\Desktop\kaggle-assignment\q10
pip install -r requirements.txt
```

Also needs `git`.

## Setup

```powershell
cd C:\Users\berwa\Desktop\kaggle-assignment\q10
$env:PYTHONIOENCODING = "utf-8"
python fetch.py
python extract.py
if (Test-Path repo) { Remove-Item -Recurse -Force repo }
mkdir repo | Out-Null
cmd /c "cd repo && git init -q && git fast-import --quiet < ..\history.fastimport && git log --oneline --all"
cd repo
cmd /c "git log -S stg_ --oneline --all"
cmd /c "git show 88e3bd3 --stat -p"
```

PowerShell `<` redirect failed; use `cmd /c` as above.

## Code

`q10/extract.py`:

```python
m = re.search(r'href="data:application/octet-stream;base64,([^"]+)"', html)
data = base64.b64decode(m.group(1))
open("history.fastimport", "wb").write(data)
```

`git log -S` shows two commits. `88e3bd3` adds `STAGING_TOKEN=stg_5fe1b3962cb9d014f204bee4` in `config/app.env` with subject `Add staging deploy token to app config`. Later `e3c0220` (`Rotate and revoke leaked staging token`) sets the value to `__rotated__`.

## Accepted answer

`q10/answer.json`:

```json
{
  "token": "stg_5fe1b3962cb9d014f204bee4",
  "commit_subject": "Add staging deploy token to app config"
}
```
