# Q1. VS Code version (0.5)

**Exam id:** `q-vs-code-version`  
**Status:** Accepted. Saved 22/22.

## Requirements

Paste the full stdout of `code -s` (VS Code `--status`) from a running Visual Studio Code process. Not accepted: `code --version`, Cursor’s CLI, or the warning `The --status argument can only be used if Code is already running.`

The grader matches `Version:\s*Code\s*(\d+\.\d+\.\d+)` and `OS Version:`.

## Files

- `q1/code-s.txt` — the `code -s` dump currently in the exam field
- `q1/WRITEUP.md` — this file

## Packages

None. Needs the `code` CLI from VS Code (`C:\Users\berwa\AppData\Local\Programs\Microsoft VS Code\Code.exe`).

## Setup

```powershell
Start-Process "C:\Users\berwa\AppData\Local\Programs\Microsoft VS Code\Code.exe"
Start-Sleep -Seconds 12
code -s
```

Paste the entire output into `#q-vs-code-version`. PIDs, free memory, and the GPU log line change every run; that is expected.

## Code

There is no script. The command above is the solution. Saved copy: `q1/code-s.txt`.

## Accepted answer

Version line from the accepted run:

```text
Version:          Code 1.139.0 (2242ebbb54efeeb0129e08e919e7e8d43033cd83, 2026-09-22T15:13:34Z)
OS Version:       Windows_NT x64 10.0.26300
```
