# Who's "Steeling" the Show? Layoffs and Local Voices in U.S. IRA Rulemaking

Final project for **DSAN 6750 / PPOL 6805: GIS for Spatial Data Science** (Georgetown University, Fall 2025).

**Read the paper:** https://prdough.github.io/gis-final-project/

## Overview

When the Inflation Reduction Act (IRA) is written into federal rules, which U.S. counties "show up" in the public comments? This project links **county-level IRA comment participation in 2022** to **steel-related exposure**: a lagged steel value-chain layoff proxy, plant locations, and a contiguity-based "steel corridor". It also adds **Registered Apprenticeship intensity** as a measure of local workforce capacity.

Main findings:

- IRA commenting intensity is **strongly spatially clustered**. Spatial-lag (SAR) models fit the data better than OLS and absorb most of the leftover residual clustering.
- Steel layoffs are associated with more commenting on their own, but the link **disappears once county controls are added**.
- A multi-county **steel corridor** predicts excess commenting better than whether a county hosts a plant. A broader "industrial belt" comparator does not.
- In a layoff × apprenticeship 2×2 typology, **high-layoff / high-apprenticeship** counties comment *less* than the low/low baseline after controls.

## Methods

- County geometries from TIGER/Line (2019 cartographic boundary, Lower 48), projected to EPSG:5070
- Per-capita comment rate and relative risk ("excess commenting")
- Global Moran's I and LISA cluster maps
- OLS and spatial-lag (SAR) regressions with queen-contiguity weights
- Text-as-data pipeline: ZIP geocoding → SBERT embeddings → DBSCAN de-duplication → LDA topic models by typology group

## Repository structure

```
.
├── index.qmd               # Manuscript source (text + R code)
├── GIS_Manuscript.bib      # References
├── _quarto.yml             # Quarto manuscript project config
├── fips_*.csv              # County FIPS lists for each layoff × apprenticeship group
└── docs/                   # Rendered site served by GitHub Pages (do not edit by hand)
```

## Reproducing

The input data are **not included** in this repository. To re-render, place these files in a `data/` folder at the project root:

| File | Contents |
|---|---|
| `master_panel_fips_year_25fin.dta` | County-year panel of covariates (layoffs, apprenticeships, controls) |
| `ira_dbscan_fips_year_features_2022.csv` | 2022 IRA comment features by county |
| `us_steel_plants.csv` | U.S. steel plant locations |
| `topic_words_all_groups_k30.csv` | LDA top words by typology group |

Requirements: [Quarto](https://quarto.org/) (≥ 1.4) and R with these packages:

```r
install.packages(c(
  "tidyverse", "sf", "spdep", "spatialreg", "tigris", "haven",
  "modelsummary", "broom", "knitr", "DT", "leaflet", "mapview", "viridis"
))
```

Then render:

```bash
quarto render
```

Computed results are frozen (`execute: freeze: true`). Delete the local `_freeze/` folder to force a full re-run.

## Author

Puran Dou, Georgetown University

## License

Code is released under the [MIT License](LICENSE).
