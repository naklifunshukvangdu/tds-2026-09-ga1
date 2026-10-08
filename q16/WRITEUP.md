# Q16. Tangram silhouette (2)

**Exam id:** `q-visual-game-tangram`  
**Status:** Accepted. Saved 22/22. Do not resubmit unless the token is missing.

## Requirements

Place all seven standard Tangram pieces on https://tds-network-games.sanand.workers.dev/tangram/ so the union matches this student’s silhouette. Then **Generate signed payload** and paste that JWT into the exam. The checker is translation-invariant and ignores piece identity; it compares the union boundary.

- Target: `tg-018`
- `expected_match_key`: `7b6ae4721c625c42eb65cd0cdd4dd7ab0358ef62f16100b566f94321df5bf792`

A forged JWT is not accepted. Only `/api/visual/attest` on the game page signs a valid receipt.

Grader: **Correct**

## Files

- `q16/poses.json` — working placement + share link
- Search/explore scripts (`solve.py`, `search.py`, …) — not what the grader reads
- `q16/WRITEUP.md`

The signed token is not stored here.

## Packages

None required for the working path (place pieces on the official board). Search scripts used local Python.

## Setup

1. Open the Tangram board and set the email to `<course-email>`.
2. Load the share link in `q16/poses.json`, or place pieces by inventory order:

```text
large-a        x=18  y=231  orientation=6  reflected=false
large-b        x=29  y=112  orientation=0  reflected=false
medium         x=38  y=89   orientation=1  reflected=false
small-a        x=66  y=183  orientation=2  reflected=false
small-b        x=44  y=16   orientation=0  reflected=false
square         x=29  y=48   orientation=0  reflected=false
parallelogram  x=29  y=80   orientation=0  reflected=false
```

3. Board status **Valid candidate** and match key `7b6ae472…`.
4. Click **Generate signed payload** and paste the token into `#q-visual-game-tangram`.

Share link:

```text
https://tds-network-games.sanand.workers.dev/tangram/#v=1&s=W1sxOCwyMzEsNiwwXSxbMjksMTEyLDAsMF0sWzM4LDg5LDEsMF0sWzY2LDE4MywyLDBdLFs0NCwxNiwwLDBdLFsyOSw0OCwwLDBdLFsyOSw4MCwwLDBdXQ
```

## Code

The pose list in `q16/poses.json` is the arrangement the board signed. Search scripts under `q16/` were used to explore placements only.

## Accepted answer

The signed payload from **Generate signed payload** (not stored in git). After paste, Check this question once.
