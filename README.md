# Execution scripts for Studies A, B and C

This README accompanies `Execution_Scripts_Studies_A_B_C.zip`. Place it in the extracted `Execution_Scripts_Studies_A_B_C/` folder.

The archive contains 11 original Python scripts and Jupyter notebooks, copied unchanged from the consolidated supplementary package. It contains no input datasets, results, fitted models, dependency lock or execution-configuration JSON files. The companion `Journal_Supplementary_Files_Studies_A_B_C_Updated.zip` supplies retained configurations, identifiers, results and available provenance records.

## 1. Files and roles

Paths below are relative to the extracted package root.

| Study | File | Role |
| --- | --- | --- |
| A | `S1_StudyA/code/StudyA_retuned_fast_resume.py` | Primary learner pipeline, empirical OK/RK, ablation, diagnostics and SHAP. |
| A | `S1_StudyA/code/original/StudyA_executed_notebook.ipynb` | Supplied notebook containing the study configuration and execution calls. |
| B | `S2_StudyB/code/StudyC_fast_resume.py` | Shared primary engine, configured for Study B inputs and its 555-record test population. |
| B | `S2_StudyB/code/StudyB_OK_RK_tuning_Table7_compatible.py` | Separate empirical OK/RK tuning using saved Study B primary outputs. |
| B | `S2_StudyB/code/original/StudyB_executed_notebook.ipynb` | Study B primary configuration and execution calls. |
| B | `S2_StudyB/code/original/StudyB_empirical_OK_RK_reference.ipynb` | Supplied empirical comparator reference notebook. Use the accompanying Python module for the retained source identity. |
| C | `S3_StudyC/code/StudyC_fast_resume.py` | Primary engine for the 106-record Study C test population. |
| C | `S3_StudyC/code/StudyC_empirical_source_as_hashed.py` | Empirical OK/RK source whose bytes match the retained tuning manifest's recorded source hash. |
| C | `S3_StudyC/code/StudyC_empirical_definitions_from_notebook.py` | Separately preserved notebook definitions, with revision/default-path differences. |
| C | `S3_StudyC/code/original/StudyC_primary_supplied.ipynb` | Supplied Study C primary configuration and execution calls. |
| C | `S3_StudyC/code/original/StudyC_empirical_tuning_supplied.ipynb` | Supplied empirical tuning notebook, retaining historical example paths and configuration cells. |

Study B's `StudyC` filenames and some inherited labels identify the shared engine. Its input files, configuration and split identifiers determine that the run is Study B.

## 2. Environment and inputs

Use Python 3.12 with NumPy, pandas, SciPy, scikit-learn, XGBoost, SHAP, joblib, threadpoolctl, Matplotlib and openpyxl. Jupyter is needed for notebooks. Consult the full supplement's environment records for recorded versions. This dependency list is not a complete environment lock.

| Study | Required source datasets | Retained evaluation population |
| --- | --- | --- |
| A | `BM2_Local_BID_11_minus_Table1_XYZ_merged.csv`; `BM2_Local_validation_without_74_matched_records.csv`; `BM2_Table_1_md_XYZ_merged_rows.csv` | Five records per applicable task; four boreholes for MT1 and five for MT2 to MT4. Development and test share boreholes. |
| B | `BM2_Train_borehole_grouped_80.csv`; `BM2_Test_borehole_grouped_20.csv` | 555 test records from 52 boreholes, disjoint from development boreholes. |
| C | `BM2_Data_All_without_106_records.csv`; `BM2_Data_All_with_106_records.csv` | 106 test records from eight boreholes, disjoint from development boreholes. |

Provide the original datasets with the expected columns and identifiers. Use absolute paths when relocating inputs and outputs. For empirical companions, use `input_overrides` when necessary to point to identical moved inputs; retain their original contents and checksums.

Run each study from its own `code/` directory. Notebooks are stored one directory below it in `original/`; set the kernel working directory to the corresponding `code/` directory before executing import cells, or add that directory to the Python import path. A fresh kernel avoids carrying another study's `CONFIG` into notebooks that only define it when absent.

## 3. Retained configuration

Use the recorded configuration rather than class defaults or explanatory notebook prose. All studies retain 40 fitting seeds, 2026 to 2065, tuning seed 1729 and five grouped development folds.

| Stage | Settings requiring particular attention |
| --- | --- |
| Study A | `verification_role="pooled_grouped_cv"`, `exclude_test_boreholes=False`, `search_iterations=18`, `boundary_candidates=2`, `geo_candidates=20`, `geo_inner_folds=5`. The 18 initial learner candidates plus two probes give 20 candidates. |
| Studies B/C primary | `search_iterations=20`, `cv_folds=5`, `runs=40`; set the correct train/test files and `expected_test_records=555` for B or `106` for C. |
| Study B empirical OK/RK | 20 candidates, five folds, neighbours `(8,16,32,64,128)`, Ridge penalties `(0.01,0.1,1,10,100)` and lag counts `(12,20)`. |
| Study C empirical OK/RK | 20 candidates, five folds, neighbours `(8,16,32,64,128,256)`, Ridge penalties `(0.01,0.1,1,10,100,1000)` and lag counts `(12,20)`. The expanded neighbour/Ridge domain must be passed explicitly. |

The retained Study C empirical implementation uses variance-normalized semivariogram-form kriging, diagonal jitter `1e-10`, at most 1,000 function evaluations per variogram fit and no exhaustion retries. Numerical jitter is distinct from the fitted nugget.

The primary B/C engines preserve an earlier heuristic comparator stage. For the retained comparisons, use the separate empirical tuning outputs to replace those comparator results while retaining the primary learner results. Notebook displays and historical table names do not define the final manuscript tables.

## 4. Execution order

### Study A

The Study A script includes empirical OK/RK; no separate comparator companion is needed. Obtain `S1_StudyA/provenance/executed_configuration.json` from the full supplement and review its input/output paths. From `S1_StudyA/code/`, validate and then run:

```bash
python StudyA_retuned_fast_resume.py --config /path/to/executed_configuration.json --validate-only
python StudyA_retuned_fast_resume.py --config /path/to/executed_configuration.json --workers 4 --stage all
```

Alternatively, use the supplied Study A notebook with its recorded configuration cell. Some surrounding notebook prose describes earlier defaults; the retained configuration above governs the intended analysis.

### Studies B and C: primary pipelines

Use the relevant primary notebook, or pass a JSON object containing the relevant primary manifest's `config` fields to the shared engine. For Study B, that record is `S2_StudyB/provenance/original/run_manifest.json` in the full supplement; for Study C, it is the corresponding S3 path. Pass the `config` object, not the entire manifest.

From the study's own `code/` directory:

```bash
python StudyC_fast_resume.py --config /path/to/primary_config.json --validate-only
python StudyC_fast_resume.py --config /path/to/primary_config.json --workers 4 --stage all
```

Keep B and C in separate output directories. The retained source directories were `StudyB_reconciled` and `StudyC_reconciled_25Nov` respectively. Select an appropriate worker count for available memory and CPUs.

### Study B: empirical comparators

After primary outputs are available, run the separate empirical companion. It can run in a fresh process; it does not require the primary notebook's live kernel.

```bash
python StudyB_OK_RK_tuning_Table7_compatible.py --study-b-output /path/to/StudyB_reconciled --output-dir /path/to/StudyB_empirical_rerun --validate-only
python StudyB_OK_RK_tuning_Table7_compatible.py --study-b-output /path/to/StudyB_reconciled --output-dir /path/to/StudyB_empirical_rerun
```

The retained empirical output location was `StudyB_reconciled/OK_RK_tuning_Table7_compatible`. A rerun should use its own output location unless intentionally resuming the identical configuration.

### Study C: empirical comparators

Use `StudyC_empirical_source_as_hashed.py` with an explicit JSON configuration. For example, create `study_c_empirical_config.json` outside the source files:

```json
{
  "study_c_output_dir": "/path/to/StudyC_reconciled_25Nov",
  "output_dir": "/path/to/StudyC_empirical_rerun",
  "n_candidates": 20,
  "inner_folds": 5,
  "tuning_seed": 1729,
  "neighbor_counts": [8, 16, 32, 64, 128, 256],
  "ridge_alphas": [0.01, 0.1, 1.0, 10.0, 100.0, 1000.0],
  "lag_counts": [12, 20],
  "max_variogram_evaluations": 1000,
  "diagonal_jitter": 1e-10,
  "final_seeds": null,
  "resume": true
}
```

Then run from `S3_StudyC/code/`:

```bash
python StudyC_empirical_source_as_hashed.py --config /path/to/study_c_empirical_config.json --validate-only
python StudyC_empirical_source_as_hashed.py --config /path/to/study_c_empirical_config.json
```

`final_seeds=null` uses the original primary run's seeds. The retained empirical output directory was `OKRK_OP_25Nov`. The two preserved C source versions are not interchangeable provenance records: the embedded-source version matches the recorded hash, while exact historical live-kernel attribution remains qualified in the full supplement.

