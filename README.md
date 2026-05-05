# Where, When, and Why? Spatial Heterogeneity in Police-Reported Casualty Severity for Vulnerable Road Users in Greater London (2022–2024)

CASA0006 *Data Science for Spatial Systems* — final coursework, UCL CASA MSc Urban Spatial Science.

> A four-method spatial-statistics + machine-learning pipeline modelling the **conditional severity** P(severe | casualty) of pedestrian and cyclist casualties on STATS19 records, with explicit calibration, spatial cross-validation, and counterfactual policy simulation.

## Research question

> How do environmental, temporal, and spatial factors — net of socio-economic deprivation — shape the **police-reported severity** of pedestrian and cyclist casualties in Greater London (2022–2024); to what extent can we predict this risk with calibrated uncertainty; and what risk reductions could be expected under counterfactual policy scenarios?

## Key findings

- Spatial autocorrelation of conditional severity is statistically significant but **modest** (Moran's I = 0.054 at LSOA, 0.058 at MSOA; both p ≤ 0.005), and is robust to scale (MAUP test) and to City of London exclusion.
- Within-week temporal structure (hour × day-of-week) is far stronger than the spatial signal.
- Logistic regression and XGBoost SHAP **independently reproduce** the dominant predictors: pedestrian age 60+, dark-unlit conditions, 40 mph+ roads.
- IMD deprivation enters with a small **counterintuitively negative** odds ratio (OR ≈ 0.94), consistent with either differential under-reporting in deprived LSOAs or compositional exposure differences — the conditional-on-being-recorded framing cannot distinguish them.
- XGBoost achieves AUC = 0.661 with **negligible spatial leakage** (Borough-CV gap = 0.005); Mondrian split-conformal returns **near-nominal coverage** at every level tested.
- The four-scenario counterfactual surfaces a structural limit of the conditional-severity framing: most road-safety interventions reduce casualty severity primarily by reducing **crash occurrence**, not severity given a crash. Future work should target a complementary marginal-risk P(crash) model.

## Methods

Four-stage analytical pipeline (see Methodology flow chart, Figure 0):

| Stage | Method | Key tools | Outputs |
|---|---|---|---|
| 1 | **ESDA** — Global Moran's I + LISA at LSOA / MSOA / Borough; City of London sensitivity | `libpysal`, `esda`, `splot` | Figs 2, 3, 4, 4b, 4c, 5 |
| 2 | **Logistic GLM** — 11 features + standardised IMD; Borough cluster-robust SE; residual Moran's I diagnostic | `statsmodels` | Fig 6 |
| 3 | **XGBoost + SHAP + Conformal** — random vs Borough-grouped 5-fold CV; SHAP attribution; Mondrian split-conformal at four confidence levels (80–95%) | `xgboost`, `shap`, `mapie` | Figs 7, 8, 9 |
| 4 | **Counterfactual** — four scenarios (speed 30→20 mph, dark-unlit→dark-lit, rain→fine, wet→dry) on the held-out test set; reports ARR, RRR, NNT | sklearn prediction perturbation | Fig 10 |

## Data sources

| Dataset | Provider | Coverage | Repo location |
|---|---|---|---|
| STATS19 Road Safety Open Data | Department for Transport | 2022–2024, Great Britain | `data/raw/` (gitignored) |
| English Indices of Deprivation 2019 | Ministry of Housing, Communities & Local Government | England, LSOA-level | `data/external/` (gitignored) |
| LSOA 2011 boundaries | Greater London Authority | Greater London | `data/boundaries/` (gitignored) |
| Borough boundaries | Greater London Authority | Greater London | `data/boundaries/` (gitignored) |

The **cleaned, derived analytical dataset** (`data/clean/vru_clean.csv` + LSOA GeoJSON + IMD slim) is tracked in this repo and read directly by the submission notebook from a public GitHub raw URL — cloning + running suffices to reproduce.

## Reproducing the analysis

```bash
pip install -r requirements.txt
jupyter lab notebooks/analysis.ipynb
# Restart Kernel & Run All — completes in ~5 minutes on 32 GB RAM
```

## Repository structure

```
casa0006-vru-london/
├── analysis.pdf                    # final submission PDF (Moodle Tab 2)
├── data/
│   ├── raw/                        # STATS19 (gitignored)
│   ├── boundaries/                 # LSOA / Borough shapefiles (gitignored)
│   ├── external/                   # IMD 2019 (gitignored)
│   └── clean/                      # derived analytical dataset (tracked)
│       ├── vru_clean.csv
│       ├── london_lsoa.geojson
│       └── imd2019_london.csv
├── notebooks/
│   ├── 00_preprocessing.ipynb      # STATS19 → vru_clean.csv pipeline (run once)
│   └── analysis.ipynb              # final submission notebook (Moodle Tab 1)
├── figures/                        # 13 publication-quality PNGs (Arial, 600 DPI)
├── docs/                           # planning notes
├── requirements.txt
└── README.md
```

## Submission deliverables (CASA0006 Moodle, two tabs)

- **Part 1 (Tab 1)** — `notebooks/analysis.ipynb`
- **Part 2 (Tab 2)** — `analysis.pdf` (HTML-export → browser print-to-PDF, text-selectable, 51 pages)

Narrative word count: **1,497 / 1,500** (excluding code, comments, tables, figure captions, and references).
References: **14 verified** entries (all DOI / CrossRef / Scholar verified before insertion).

## Statement on AI use

Claude (Anthropic, model `claude-opus-4-7`) was used to assist with project structure scaffolding, code debugging, and English translation and editorial polishing of prose. All academic references were verified by the author on Google Scholar / CrossRef / DOI lookup before insertion; all methodological decisions, analytical interpretations, and conclusions are the author's own work.

## License

- **Code**: MIT
- **Data**: original providers' licenses apply (Open Government Licence v3.0 for STATS19 and IMD 2019; LSOA boundaries © Office for National Statistics / Crown Copyright)
- **This repository**: code under MIT; the cleaned derived dataset under OGL v3.0 (consistent with source).

## Author

**Yuxiang Fan** — UCL CASA, MSc Urban Spatial Science, 2025–2026
