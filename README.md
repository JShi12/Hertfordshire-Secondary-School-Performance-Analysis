# Hertfordshire Secondary School Performance Analysis

This project analyses Hertfordshire state-funded secondary school performance using Department for Education school performance data for academic years `2021-2022` to `2024-2025`.

The main analysis is in `Hertfordshire-Secondary-School-Performance-Analysis.ipynb`.

## Scope

- Schools: state-funded secondary schools only
- Outcomes: Attainment 8 (`ATT8SCR`) and Progress 8 (`P8MEA`)
- Context variables: disadvantage, attendance, SEND, school size, admissions policy, gender mix, and roll composition
- Geography: latest-year ranked schools are mapped using school postcodes geocoded to latitude/longitude

## Data

The notebook uses local files under `data/`, including:

- `919_ks4final.csv` for KS4 outcomes
- `919_census.csv` for school roll and pupil composition
- `919_abs.csv` for attendance where available
- `919_school_information.csv` for school postcodes

`2024-2025` has no attendance file and Progress 8 is not populated, so regression models using `PERCTOT` and `P8MEA` exclude that year.

## Analysis Workflow

1. Load and clean four years of DfE data.
2. Build a school-year panel for state-funded secondary schools.
3. Audit coverage, missingness, and core outcome trends.
4. Explore context relationships using scatter plots and a correlation heatmap.
5. Fit descriptive OLS regression models for `ATT8SCR` and `P8MEA` using `statsmodels.OLS`.
6. Produce ranked school tables for high deprivation and high attainment.
7. Map latest-year ranked schools with Folium.

## Regression Summary

The regression models use standardised numeric predictors:

- `PNUMFSMEVER`
- `PERCTOT`
- `PSEN_ALL4`
- `NOR`

`PPERSABS10` is omitted because it is strongly related to `PERCTOT`, and `PNORG` is omitted because it has a weak relationship with attainment in the correlation check.

Key findings:

- `PNUMFSMEVER` and `PERCTOT` are statistically clear negative predictors for `ATT8SCR`.
- `PERCTOT` and `PNUMFSMEVER` are statistically clear negative predictors for `P8MEA`.
- `NOR` and `PSEN_ALL4` are weak after deprivation, attendance, year, gender, and admissions controls are included.

These models are descriptive, not causal.

## Mapping

The notebook uses `folium` to map latest-year ranked schools. Postcodes are geocoded through postcodes.io and cached in:

```text
data/postcode_coordinates_cache.csv
```

If the Folium map does not render inside the notebook, trust the notebook in your editor or save the map to HTML and open it in a browser.

## Environment

Recommended packages:

```text
pandas
numpy
matplotlib
statsmodels
folium
requests
```

Install missing packages into the project virtual environment, for example:

```powershell
.\.venv\Scripts\python.exe -m pip install statsmodels folium requests
```

## Notes

- The ranked tables are shortlists for review, not standalone performance judgements.
- Independent and special-school provision is outside the analysis scope.
- Regression and correlation results should be interpreted alongside local context, admissions patterns, and catchment effects.
