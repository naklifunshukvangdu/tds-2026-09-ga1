# Q4. npx prettier SHA-256 (0.5)

**Exam id:** `q-npx-prettier`  
**Status:** Accepted. Saved 22/22.

## Requirements

Run Prettier **3.4.2** on the exam markdown (`q4/README.md`). SHA-256 the raw stdout bytes. Submit the hex digest only. PowerShell `Out-File` / piping through a .NET string rewrites line endings and produces a different hash.

## Files

- `q4/README.md` — badly formatted markdown from the exam
- `q4/hash.ps1` — `cmd` redirect + Python hash
- `q4/prettier_raw.txt` — captured Prettier stdout
- `q4/WRITEUP.md`

## Packages

Node.js (`npx`). Python `hashlib` (stdlib).

## Setup

```powershell
cd C:\Users\berwa\Desktop\kaggle-assignment\q4
.\hash.ps1
```

## Code

```powershell
cmd /c "npx -y prettier@3.4.2 README.md > prettier_raw.txt 2>nul"
python -c "import hashlib; data=open('prettier_raw.txt','rb').read(); print(hashlib.sha256(data).hexdigest())"
```

## Accepted answer

```text
3a2f3a3a1ad2999fedef2685376626e4dad1f9db8f4eeef63fc9cd6bf3a72043
```
