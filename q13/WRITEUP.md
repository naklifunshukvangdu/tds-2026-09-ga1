# Q13. Decode it in the terminal (termlog) (2)

**Exam id:** `q-termlog-rec-server`  
**Status:** Accepted. Live URL to `submission.tar.gz`; keep the file server and tunnel up.

## Requirements

Public HTTPS URL of a `.tar.gz` that contains `demo.cast` and `demo.cast.jwt`. The recording must be made with real [termlog](https://github.com/mynkpdr/termlog) after `termlog login` as `<course-email>`. Inside the recording: fetch the exam challenge and decode it with three visible shell steps. A handmade JWT is rejected (“Receipt signature is invalid.”).

Grader: **Verified recording.**

## Files

- `q13/decode.sh` — fetch + reverse / hex / ROT13 (the `questionData` URL in this script includes `quizSign`; do not copy that into notes)
- `q13/record.sh` — WSL wrapper around `termlog rec` + `termlog verify`
- `q13/demo.cast` and `q13/demo.cast.jwt` — signed recording
- `q13/submission.tar.gz` — `tar -czf` of the two files
- `q13/WRITEUP.md`

## Packages

- Termlog Linux binary (run under WSL `docker-desktop`; Google login callback is `127.0.0.1`)
- `cloudflared` to publish the archive
- Python `http.server` (or any static file host)

## Setup

Login (once):

```powershell
wsl -d docker-desktop -- sh -c "mkdir -p /mnt/host/c/Users/berwa/Desktop/kaggle-assignment/q13/termlog-config && export TERMLOG_CONFIG_HOME=/mnt/host/c/Users/berwa/Desktop/kaggle-assignment/q13/termlog-config && export HOME=/root && /tmp/termlog login"
```

Record and pack:

```sh
termlog rec --overwrite --window-size 120x40 --command 'sh decode.sh' demo.cast
termlog verify --expect-email <course-email> demo.cast
tar -czf submission.tar.gz demo.cast demo.cast.jwt
```

`record.sh` is the WSL wrapper that ran that recording. Host the archive:

```powershell
cd C:\Users\berwa\Desktop\kaggle-assignment\q13
python -m http.server 8013
```

```powershell
cloudflared tunnel --url http://localhost:8013
```

Submit `https://<host>.trycloudflare.com/submission.tar.gz`.

## Code

`decode.sh` tries both transform orders and keeps the one that yields `TL-…`:

1. Reverse the challenge string
2. Hex-decode (`xxd -r -p`)
3. ROT13 (`tr A-Za-z N-ZA-Mn-za-m`)

The other order is hex-decode, then ROT13, then reverse. Each step is printed as its own command so the asciicast shows three steps.

Paste the `questionData` URL from the exam card into `decode.sh`; do not commit a fresh `quizSign` into the writeup.

## Accepted answer

Working public URL on 7 Oct 2026:

```text
https://metallic-files-springs-cell.trycloudflare.com/submission.tar.gz
```
