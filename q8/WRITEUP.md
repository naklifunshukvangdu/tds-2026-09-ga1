# Q8. CSV sales (1)

**Exam id:** `q-csv-sales-analysis-server`  
**Status:** Accepted. Saved 22/22.

## Requirements

From `sales_data.csv` in the exam ZIP, total `quantity * price` for city `Pune` and dates starting `2025-Jun`. Round to an integer. Columns: `date,city,product,quantity,price`. Dates look like `2025-Jun-25`.

## Files

- `q8/total.py` — computes the total once `sales_data.csv` is next to it
- `q8/WRITEUP.md`

The ZIP was not kept in the repo. Download it from the exam card and extract `sales_data.csv` into `q8/`.

## Packages

Python stdlib (`csv`).

## Setup

```powershell
cd C:\Users\berwa\Desktop\kaggle-assignment\q8
python total.py
```

Or pass a path: `python total.py C:\path\to\sales_data.csv`.

## Code

`q8/total.py`:

```python
total = 0.0
for row in csv.DictReader(f):
    if row["city"] == "Pune" and row["date"].startswith("2025-Jun"):
        total += int(row["quantity"]) * float(row["price"])
print(round(total))
```

## Accepted answer

```text
8274
```
