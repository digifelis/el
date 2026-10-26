# Reviewer Reproducibility Package — v03

The package has been prepared and validated by running the experiments. The manuscript and original research code were not modified; this was confirmed by SHA-256 comparisons.

## Deliverables

- `reviewer_reproducibility_v03.zip`: research code, processed inputs, the original Lu Excel datasets, pinned dependencies, reference results, an automatic comparison tool, English usage instructions, and validation reports.
- `reviewer_reproducibility_v03_CORA_raw.zip`: 410 original Parquet files, the source README, and a manifest; approximately 1.38 GB. Extract this archive into the main package folder to repeat calibration from the raw CORA data. It is not required for the quick check.
- `reviewer_reproducibility_v03_SHA256SUMS.txt`: integrity checksums for both archives.

## Validation scope and results

All 36 reference result CSV files were regenerated and matched numerically. Four additional processed-data CSV files were regenerated from raw inputs and checked. Comparisons use an absolute tolerance of 1e-9 and a relative tolerance of 1e-7; machine-dependent execution times are excluded.

- All six optimization algorithms were rerun with 30 replicates each; the resulting 180 runs and convergence histories were compared with the references.
- Controller comparisons, regeneration experiments, and sensitivity ablations were reoptimized.
- The Pareto search, controller/myopic grids containing 19,584 rows, SoH equivalence analysis, finer-grid comparison, shared-setting experiment, resistance-information error analysis, and transfer experiments were rerun.
- CORA calibration, voltage-validation and thermal-validation outputs, together with the Lu cell analyses, were regenerated from raw data.
- In MATLAB, all 18 main runs and eight BMS estimation-error runs completed successfully and matched the references. These 26 runs took approximately 16.7 minutes.
- Five physical consistency tests passed. After the ZIP was extracted into a new folder, the quick check passed in approximately 12 seconds.
- The dependencies in `requirements.txt` were installed into a fresh Python virtual environment, and the dependency check passed. The quick, Lu, and CORA stages were validated again in that environment.
- Negative tests confirmed that corrupted results and modified files were detected. The checksums of all 410 files in the raw-data archive were verified individually.

The stages were executed separately; the combined `all-python` command was not additionally tested as a single end-to-end run. Detailed evidence is provided in `validation/`, with the coverage inventory in `validation/coverage.csv`. MATLAB validation used a disposable copy of the author's existing configured model; a new Composer model was not created through the GUI during validation.

## Getting started

Extract the main ZIP into a separate folder and run the following commands with Python 3.11:

```powershell
python -m venv .venv
.venv\Scripts\python -m pip install -r requirements.txt
.venv\Scripts\python reproduce.py verify
.venv\Scripts\python reproduce.py quick
```

The `quick` stage re-simulates saved controller vectors; it does not repeat optimization. To rerun the searches, use `benchmark`, `controllers`, `pareto`, and the other stages listed in the README. New results are written to a separate working directory, leaving the reference results unchanged.

The MATLAB experiments require R2025b, Simulink, and Powertrain Blockset. The proprietary reference model is not included in the ZIP; configuration instructions, model-file checksums, and the author-written integration scripts are provided. The MSVC C2099 error encountered on this machine was resolved using the `_UCRT_NOISY_NAN` definition documented by MathWorks. The `--msvc-nan-workaround` option of `run_matlab.py` applies this definition only to the relevant execution process. See `MATLAB_REPRODUCTION.md` for the complete command and the supporting MathWorks documentation.

## Two manuscript details requiring clarification

1. `objective.py` adds **+0.5 °C** to both the numerator and denominator of the thermal objective ratio, but the manuscript equation does not show this term. The package preserves the calculation that produced the existing results. The manuscript should state the implemented definition explicitly; no manuscript changes were made here.
2. The local CORA README cites **version 1.0**, whereas the manuscript's data-availability paragraph requests confirmation of **version 3.0**. The package identifies the exact files used through SHA-256 checksums; it does not claim that a version-3 match has been verified.

These points do not indicate a failure to reproduce the results. They concern alignment between the method and data-provenance descriptions and the implementation actually used. Author placeholders were not treated as scientific deficiencies.

## Data availability statement

A draft statement is provided in `DATA_AVAILABILITY_DRAFT.md`. No public DOI or repository link has yet been created for this local package. The actual link can be added to the statement after deposition. No files were uploaded to an external service or sent to a reviewer.
