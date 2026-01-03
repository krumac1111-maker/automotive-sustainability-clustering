# Automotive Sustainability Clustering (2020+)

This repository contains a reproducible workflow that uses unsupervised machine learning to identify distinct **environmental sustainability performance profiles** among automotive manufacturers. Vehicle-level WLTP/Euro 6 records are cleaned and aggregated to manufacturer-level indicators, then standardised and analysed using PCA and K-Means clustering.

**GitHub repo:** https://github.com/krumac1111-maker/automotive-sustainability-clustering

## Research Question
Can cluster analysis reveal distinct groups of automotive companies based on their environmental sustainability performance indicators—carbon emissions, renewable energy use, and energy efficiency?

> Dataset note: the WLTP/Euro 6 file used here does **not** contain manufacturer-level renewable electricity use. Therefore, the notebook constructs a clean-transition proxy using **powertrain adoption shares** (BEV/Hybrid/ICE). Findings should be interpreted as segmentation by *tailpipe emissions, efficiency, and electrification mix*, not by full operational renewable energy adoption.

## Data
- **Raw input file (example name):** `Euro_6_latest.csv`
- **Raw data location (recommended):** `data/raw/Euro_6_latest.csv`
- **Derived manufacturer dataset (created in Notebook 01/02):** `data/derived/manufacturer_level_dataset.csv` (recommended filename)

### Raw data sharing
If the raw dataset has licensing/redistribution constraints, do **not** upload it to GitHub. Instead:
- keep it locally in `data/raw/`
- upload only derived/aggregated outputs in `data/derived/`
- document the source in `data/raw/README.md`

## Method Overview (Notebook pipeline)
1. **01_data_cleaning.ipynb**
   - removes duplicates, cleans column names, converts key columns to numeric
   - creates a clean adoption category `fuel_cat` from `Fuel Type`
2. **02_feature_engineering.ipynb**
   - aggregates vehicle-level rows to manufacturer-level indicators
3. **03_pca_kmeans_clustering.ipynb**
   - standardises features (z-score), runs PCA, fits K-Means, selects k with diagnostics
4. **04_results_validation.ipynb**
   - cluster profiling + statistical tests (e.g., ANOVA/Tukey) + stability checks (ARI)

## Key modelling features (manufacturer-level)
These match the feature names used in your notebook:
- `mean_co2` = mean **WLTP CO2** (carbon emissions proxy)
- `efficiency` = mean **WLTP Metric Combined** (energy efficiency proxy)
- `share_bev` = share of BEV rows (from `fuel_cat`)
- `share_hybrid` = share of Hybrid rows (from `fuel_cat`)
- `share_ice` = share of ICE rows (from `fuel_cat`)
- `n_variants` = number of rows per manufacturer (sample size)

See `data/data_dictionary.md` for definitions and construction rules.

## Repository structure (recommended)
```
automotive-sustainability-clustering/
├─ README.md
├─ requirements.txt
├─ data/
│  ├─ raw/                 # raw not uploaded if restricted
│  ├─ derived/              # manufacturer-level outputs
│  └─ data_dictionary.md
├─ notebooks/
│  ├─ 01_data_cleaning.ipynb
│  ├─ 02_feature_engineering.ipynb
│  ├─ 03_pca_kmeans_clustering.ipynb
│  └─ 04_results_validation.ipynb
└─ outputs/
   ├─ figures/
   └─ tables/
```

## How to run (quick)
```bash
git clone https://github.com/krumac1111-maker/automotive-sustainability-clustering.git
cd automotive-sustainability-clustering
python -m venv .venv
# Windows:
.venv\Scripts\activate
# Mac/Linux:
source .venv/bin/activate
pip install -r requirements.txt
```

1) Put the raw CSV in `data/raw/Euro_6_latest.csv` (or update the notebook path).  
2) Run notebooks in order: `01` → `04`.  
3) Outputs should save into `outputs/figures/` and `outputs/tables/`.

## Transparency & limitations
- K-Means is distance-based, so **scaling** is required and results can be sensitive to outliers.
- Renewable electricity use is not directly observed in WLTP/Euro 6, so transition is approximated using BEV/Hybrid/ICE shares.
