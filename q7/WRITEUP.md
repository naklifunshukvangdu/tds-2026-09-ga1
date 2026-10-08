# Q7. Reorganize files (1)

**Exam id:** `q-rename-files-server`  
**Status:** Accepted. Saved 22/22.

## Requirements

Each `.txt` file starts with `category: {name}`. Move it to `{category}/{original-path-with-slashes-replaced-by-dashes}`. Keep spaces and Unicode. Then hash:

```text
find . -type f | LC_ALL=C sort | sha256sum
```

Submit the hex digest only. Including `README.md` in the hash was rejected.

## Files

- `q7/files.zip` — exam ZIP if you still have it (`reorganize.py` re-extracts). If the zip is gone, the category folders already in `q7/` are the working tree.
- `q7/reorganize.py` — wipe (except zip + scripts), extract, move
- `q7/hash_tree.py` — SHA-256 of `./path` lines, UTF-8 byte sort, trailing newline; skips zip, README, scripts
- `q7/WRITEUP.md`

## Packages

Python stdlib.

## Setup

```powershell
cd C:\Users\berwa\Desktop\kaggle-assignment\q7
python reorganize.py
python hash_tree.py
```

`reorganize.py` deletes other folders in `q7` before extract. Put `files.zip` in this folder first.

## Code

Move rule in `reorganize.py`:

```python
cat = first_line.split(":", 1)[1].strip()
new_name = rel.replace("/", "-")
shutil.move(fpath, os.path.join(BASE, cat, new_name))
```

Hash in `hash_tree.py`:

```python
all_files.sort(key=lambda x: x.encode("utf-8"))
content = "\n".join(all_files) + "\n"
print(hashlib.sha256(content.encode("utf-8")).hexdigest())
```

A hash that included `README.md` (`931a9a5130…`) was rejected.

## Accepted answer

```text
2438fef40e008e4357a211c7d665f20993840eb9ecb771d3ea9ab301cc64002e
```
