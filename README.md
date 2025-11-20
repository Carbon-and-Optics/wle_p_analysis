# Western Lake Erie Phosphorus Analysis

End-to-end notebook (`analysis/manuscript_and_SI_figures_and_analysis.ipynb`) for reproducing figures and statistics on sediment resuspension and phosphorus release in western Lake Erie. All required input files live under `data/`, and outputs (figures/derived tables) are written back under `data/figures` and `data/sentinel_data`.

## Repository layout
- `analysis/manuscript_and_SI_figures_and_analysis.ipynb` — single, fully reproducible analysis notebook.
- `data/` — all inputs (no downloads needed):
  - `sentinel_data/` (POLYMER L2A NetCDF for May 18/26 2023, plus derived SPM output)
  - `lake_erie_shapefiles/` (Lake Erie contours/shorelines)
  - `sediment_data/` (Be-7, P fractions, porosity/bulk density, metals)
  - `water_data/` (in-situ SPM/TP/SRP/Chl for validation)
  - `THOR1_WIND_DATA/` (wind time series text files)
  - `GSHHS/` (coastline polygons for masking)
- `config.yml` — project paths; update if your data live elsewhere.
- `requirements.txt` — Python dependencies.

## Quick start
1) Python 3.9+ recommended. Create a virtual env in the repo root:
```
python3 -m venv .venv
source .venv/bin/activate
pip install --upgrade pip
pip install -r requirements.txt
```
If Cartopy/GEOS/PROJ wheels are unavailable for your platform, install system packages first (e.g., `brew install geos proj gdal` on macOS or `apt-get install libgeos-dev libproj-dev gdal-bin` on Debian/Ubuntu).

2) Confirm `config.yml` points to your data:
- `data_dir` — root folder containing the inputs above (defaults to `data` in this repo).
- `figures_dir` — where plots/tables are written (defaults to `data/figures`).

3) Run the notebook:
- Interactive: `jupyter lab` (or `jupyter notebook`), open `analysis/manuscript_and_SI_figures_and_analysis.ipynb`, and run all cells.
- Non-interactive/test run (saves figures and tables): 
```
jupyter nbconvert --execute analysis/manuscript_and_SI_figures_and_analysis.ipynb \
  --to notebook --inplace --ExecutePreprocessor.timeout=0
```
Outputs land in `data/figures` and `data/sentinel_data`.

## What the notebook does
- Loads Sentinel-3 POLYMER L2A scenes (May 18/26, 2023), applies Nechad SPM algorithm, masks land with GSHHS shorelines, and saves derived SPM NetCDF.
- Computes spatial SPM change and erosion depth estimates over western/whole Lake Erie using station porosity and depth data.
- Maps TP/SRP inventories and generates manuscript figures (main + supplements) for SPM, Be-7, P fractions, metals, and windrose.
- Validates SPM algorithm against in-situ SPM/TP/Chl data and performs ANOVA/Tukey tests for spatial/temporal variability.

## Notes for collaborators
- Figures overwrite files in `data/figures`; delete contents if you want a clean regeneration.
- Keep the directory names inside `data` unchanged unless you also update `config.yml`.
- The notebook is self-contained; no external downloads are triggered during execution.
