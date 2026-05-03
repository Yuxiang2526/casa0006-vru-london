# Where, When, and Why? Spatial Heterogeneity in Police-Reported Casualty Severity for Vulnerable Road Users in Greater London (2022–2024)

CASA0006 *Data Science for Spatial Systems* coursework, UCL CASA, MSc Urban Spatial Science.

## Research question

How do environmental, temporal, and spatial factors — net of socio-economic deprivation — shape the **police-reported severity** of pedestrian and cyclist casualties in Greater London (2022–2024); to what extent can we predict this risk with calibrated uncertainty; and what risk reductions could be expected under counterfactual policy scenarios?

## Methods

Four-stage analytical pipeline:

1. **Diagnose** — Exploratory Spatial Data Analysis (Moran's I + LISA, with EB smoothing and outlier sensitivity)
2. **Explain** — Logistic regression with cluster-robust SE, controlling for IMD 2019 deprivation
3. **Predict** — XGBoost with Stratified Group K-Fold spatial CV; SHAP interpretation; conformal prediction intervals (MAPIE)
4. **Simulate** — Counterfactual policy scenarios (30→20 mph, street lighting), reported as risk-elasticity (not causal effects)

## Data sources

| Dataset | Provider | Coverage | Repo location |
|---|---|---|---|
| STATS19 Road Safety Open Data | DfT | 2022–2024, GB | `data/raw/` (not tracked) |
| English Indices of Deprivation 2019 | MHCLG | England, LSOA-level | `data/external/` (not tracked) |
| LSOA 2011 boundaries | London Datastore | Greater London | `data/boundaries/` (not tracked) |
| Borough boundaries | London Datastore | Greater London | `data/boundaries/` (not tracked) |

The cleaned, derived analytical dataset (`data/clean/vru_clean.csv`) **is** tracked in this repo.

## Reproducing the analysis

```bash
pip install -r requirements.txt
jupyter lab notebooks/analysis.ipynb
# Restart Kernel & Run All — completes in ~30 minutes on 16 GB RAM
```

The submission notebook reads its inputs from this repo's raw URL, so cloning + running is sufficient — no manual data download required to reproduce results.

## Repository structure

```
casa0006-vru-london/
├── data/
│   ├── raw/         # original STATS19 (gitignored)
│   ├── boundaries/  # LSOA / Borough geojson (gitignored)
│   ├── external/    # IMD 2019 (gitignored)
│   └── clean/       # derived analytical dataset (tracked)
├── notebooks/
│   ├── 00_preprocessing.ipynb … 05_method4_counterfactual.ipynb  (development)
│   └── analysis.ipynb   # final submission notebook
├── figures/         # exported PNGs
├── src/             # reusable utility functions
├── outputs/         # final notebook + PDF for Moodle submission
└── docs/            # planning notes
```

## License

Code: MIT. Data: see original providers (Open Government Licence v3.0 for STATS19 and IMD).

## Author

Yuxiang Fan — UCL CASA MSc, 2025–2026
