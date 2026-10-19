# 🍁 Canada PR Admissions: Economic Impact Dashboard

A research-grade Streamlit dashboard analysing the economic effects of Canada's
Permanent Residency admissions (2015–2025), testing five research questions with
proper econometric methods.

## About this fork

Forked from
[smitjaiswal2001/canada-pr-economic-dashboard](https://github.com/smitjaiswal2001/canada-pr-economic-dashboard).
The upstream repo holds the original dashboard. The work below is
Jesun's, added in this fork in June 2026, from the commit record:

- **v2.0 redesign.** A complete dashboard rebuild: 12 tabs, 41
  analytical tables, 36 figures, aligned with the final report and
  the notebook.
- **Model performance matrix.** Added to the Analytics tab, first as a
  summary, then rebuilt as exact detailed tables with an interactive
  Plotly chart.
- **Plotly fixes.** Two fixes for undefined chart text and title
  fonts.

Commit count: 8 commits under his account out of 21 in the fork
(GitHub API, author filter). Two are dev container setup commits and
one is a merge. The feature work is the redesign, the matrix, and the
fixes.

## Run it

```bash
python -m pip install -r requirements.txt
python -m streamlit run app.py
```

Opens at http://localhost:8501 . On Windows use `python -m streamlit run app.py`
(not bare `streamlit run`) if the command isn't on your PATH.

## What's new in this version

- **Monthly analysis (n≈132)** instead of annual aggregates (n≈10): far more statistical power.
- **Multivariate OLS** with controls: time trend, COVID-19 dummy, prime rate, CPI.
- **HAC (Newey–West) standard errors** robust to autocorrelation in time series.
- **Toggle naive vs. controlled** specifications to see how confounding changes results.
- **Lag analysis** (contemporaneous, 3/6/12-month) for delayed immigration effects.
- **Full regression output** tables with SE, t-stats, p-values, 95% CIs.
- **Correlation matrix**, hypothesis scorecard, per-province regressions.
- Cohesive professional design system, consistent chart theme.

## Research Questions

| Tab | Outcome | Hypothesis |
|-----|---------|-----------|
| RQ1 | GDP growth (YoY) | PR → higher GDP growth |
| RQ2 | Employment level | PR → higher employment |
| RQ3 | Unemployment rate | PR → lower (H₁a) or higher (H₁b) |
| RQ4 | Hours worked | PR → more aggregate hours |
| RQ5 | Provincial performance | Higher-intake provinces perform better |

## Key finding

Bivariate models show strong positive PR associations, but once the time trend and
macro controls are added, most shrink toward zero: the raw correlations are largely a
**shared upward trend**. GDP growth and unemployment retain significance under controls;
employment and hours do not. The **Methodology** tab explains this in full.

## Data layout

```
data/
  processed/  master_national_monthly.csv, master_provincial_monthly.csv
  interim/    ircc_pr_monthly_by_province_category.csv
  raw/        IRCC country-of-citizenship..., Interprovincial migration 17100022.csv
```

*Estimates are associational, not causal.*

---

## Production hardening (this version)

- **Path-independent data loading**: CSVs resolve relative to `app.py`, so it runs from any working directory and on cloud hosts.
- **Graceful failure**: friendly messages instead of crashes when data is missing or a selected window is too short.
- **Robust regressions**: automatically drops constant control columns (e.g. the COVID dummy in a pre-2020 window), scales HAC lags to sample size, and returns a clear "insufficient data" state rather than throwing.
- **pandas 3 compatible**: uses `Styler.map` (not the removed `applymap`).
- **`matplotlib` pinned** in requirements (needed by table gradients).
- Ships `.streamlit/config.toml` (matching theme), `.gitignore`, and `.python-version`.

## Deploy to Streamlit Community Cloud (free)

1. Push this folder to GitHub.
2. Go to https://share.streamlit.io → **Create app** → **Deploy from GitHub**.
3. Repository: your repo · Branch: `main` · Main file path: `app.py`.
4. Deploy. It reads `requirements.txt` automatically.

> Note: Streamlit apps need a long-running server and do **not** run on Vercel.
> Use Streamlit Community Cloud, Render, Railway, or Hugging Face Spaces.
