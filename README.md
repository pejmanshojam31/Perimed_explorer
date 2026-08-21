# PERIMED Study — Data Explorer

A single-file, offline web app for exploring the **PERIMED** clinical cohort. Open it in any browser, load the study's Excel workbook, and it cleans the data and builds an interactive dashboard — **entirely in your browser**. No server, no installation, and no data ever leaves your machine.

> ⚠️ **No data is included in this repository.** The app reads *your local* Excel file at runtime. Nothing is uploaded, stored, or committed here.

## Features

- **Cohort overview** — participant count, sex ratio, age and BMI, group sizes, and a descriptive-statistics table for every numeric variable.
- **Glycemic group + filter** — group sizes for NGT / Prediabetes / Diabetes with an **age and sex filter** (e.g. `age > 40`, `Female`) and a live per-group breakdown (n, %, median age, female/male).
- **Correlation** — pick any two variables; Pearson or Spearman; colour by group; regression line, *r*, *p*, *n*, R².
- **Heatmap** — correlation matrix for chosen variables, with missing-value handling (pairwise, drop rows, mean- or median-impute).
- **Association** — rank how every variable correlates with one target, sorted, with significance stars.
- **PCA** — standardized principal-component analysis with a scores plot, scree plot and loadings.
- **Immune cell profile** — standardized (z-score) blood-count values as a line-per-group plot with SEM / SD / 95% CI error bars, and a Kruskal–Wallis test per cell type to flag differences between glycemic states.
- **Glucose–insulin dynamics** — the full 5-point OGTT (glucose & insulin at 0/30/60/90/120 min) averaged per glycemic group, with glucose/insulin AUC, 1-hour-glucose phenotype, time-to-peak insulin, and a disposition-index plot (β-cell secretion vs. insulin sensitivity on log axes).
- **Inflammation ratios** — neutrophil-to-lymphocyte (NLR) and monocyte-to-lymphocyte (MLR) ratios by glycemic group, and their correlation with insulin resistance, adiposity and liver-fat markers.
- **Per-page export** — every analysis page has an export bar: download that chart as PNG or SVG and its results/data as CSV (the cleaned dataset lives on the Overview page). Heatmap and Association also let you restrict the analysis to one, two, or all glycemic groups.

## How to use

1. Download **`index.html`** (green **Code ▸ Download ZIP**, or use the live link below).
2. Double-click it to open in any modern browser (Chrome, Edge, Firefox, Safari).
3. Drag the PERIMED master workbook onto the drop zone (or click to choose it).
4. Explore. Use **“Load a different file”** in the sidebar to switch datasets.

### Live version (optional)

If GitHub Pages is enabled for this repository (**Settings ▸ Pages ▸ Deploy from branch ▸ `main` / root**), the app is available at:

```
https://<your-username>.github.io/perimed-explorer/
```

Anyone with the link can use it in their browser with their **own** local file — no data is shared.

## Expected input

The app is tailored to the PERIMED master workbook and expects these sheets: `Tabelle1`, `Tabelle2`, `Tabelle4`, and `Insulin Sensitivität`. Participants are matched across sheets by their number.

During loading it automatically:

- treats text such as `n.d.`, `NA`, `PA` and Excel error cells (`#DIV/0!`, `#REF!`) as missing;
- splits blood pressure (`129/76`) into systolic/diastolic;
- converts steatosis (`0`–`III`) and fibrosis (`F0`–`F4`) to ordinal scores.

### Glycemic group classification

The **Glycemic group** uses the study's adjudicated case classification (**Tabelle4, column C**: normal / prediabetes / diabetes), cross-checked against the marker criteria:

| Group | Criteria (any one) |
|---|---|
| **Diabetes** | HbA1c ≥ 6.5% · fasting glucose ≥ 7.0 mmol/L (126 mg/dL) · OGTT-120 ≥ 11.1 mmol/L (200 mg/dL) |
| **Prediabetes** | HbA1c 5.7–6.4% · fasting 5.55–6.99 mmol/L (100–125 mg/dL) · OGTT-120 7.77–11.1 mmol/L (140–199 mg/dL) |
| **NGT (normal)** | all available markers below the prediabetes thresholds |

If a record is labelled *Diabetes* in column C but **no** marker meets a diabetes threshold **and** the participant is not on glucose-lowering medication, it is reclassified to the marker-based category (this corrects one data-entry case while keeping medication-controlled diabetics as diabetes).

## Privacy

- Runs 100% client-side; there is no backend and no network request with your data.
- The repository contains **only the app** — no participant data.
- The included `.gitignore` blocks common spreadsheet formats so data files can't be committed by accident.

## Built with

Vanilla JavaScript, hand-drawn SVG charts, and [SheetJS](https://sheetjs.com/) (inlined) for reading `.xlsx` in the browser. Everything is bundled into the single `index.html`.

## Copyright & licence

**Software** © 2026 **Pejman Shojaee**. All rights reserved. Free to use, copy and modify for non-commercial academic/research purposes with attribution; any other use needs written permission. See [`LICENSE`](LICENSE). (Bundles [SheetJS](https://sheetjs.com/) Community Edition, Apache-2.0.)

**Data** — the PERIMED study data are the property of the **Perakakis Lab**. They are **not** part of this repository and **not** covered by the software licence; the app reads a file supplied locally by the user at runtime. Any use of the data requires the Perakakis Lab's permission and remains subject to its ethics/consent and data-governance agreements.

