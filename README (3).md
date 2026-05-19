# UCSF Health — Ambulatory Role Sizing Rubric

A single-file, static web app for scoring ambulatory medical director roles by cost center, built for the UCSF Health Ambulatory Physician Leadership Harmonization workgroup.

## What it does

- **Select a cost center** from a dropdown of all 342 active ambulatory cost centers
- **Score 8 standardized metrics** across Clinical Scope and Structural Complexity
  - Visit volume and Clinicians supervised are weighted ×1.5 as primary drivers
- **See a recommended tier and cFTE band** computed in real time
- **Save scores** to the browser and **export to CSV** for the workgroup

## Quick start

1. Open `index.html` in any modern browser — that's it
2. Or deploy to GitHub Pages: push to a repo → Settings → Pages → Source: `main` / root

## Updating the cost center list

The 342 cost center names are embedded as a JavaScript array inside `index.html`. Search for `const COST_CENTERS = [` near the bottom of the file.

To refresh the list from a new spreadsheet:

```python
import pandas as pd, json
df = pd.read_excel('All_Medical_Directors_Ambulatory_BU.xlsx',
                   sheet_name='All Medical Directors Ambulator')
names = sorted(df['Cost Center Name'].dropna().unique().tolist())
print(f"const COST_CENTERS = {json.dumps(names, separators=(',', ':'))};")
```

Replace the existing `const COST_CENTERS = [...]` line in `index.html` with the output.

## Tuning the rubric

Open `index.html` and edit:

- **Metric weights** — the `data-weight="1.5"` attribute on each `<select>` element
- **Tier thresholds** — `RUBRIC_CONFIG.tierThresholds` (score ranges → tier labels and cFTE bands)
- **Tier option text** — the `<option>` labels inside each metric `<select>`

**Default tier bands:**

| Weighted score | Tier | Suggested cFTE |
|---:|---|---:|
| 0–12 | Tier 4 · Focused MD | 0.10–0.20 |
| 13–18 | Tier 3 · Medical Director | 0.20–0.40 |
| 19–24 | Tier 2 · AEMD | 0.40–0.60 |
| 25+ | Tier 1 · Exec/CMO scope | 0.60–1.00 |

The workgroup should validate these bands against a few known roles before relying on them for decisions.

## Privacy

Everything runs client-side. Saved scores live only in the user's browser (`localStorage`) and never leave their device unless explicitly exported via the **Export CSV** button.

## File structure

```
/
├── index.html      ← entire app: HTML + CSS + JS + 342 cost center names
├── README.md       ← this file
└── .gitignore
```

No build step, no dependencies, no backend.
