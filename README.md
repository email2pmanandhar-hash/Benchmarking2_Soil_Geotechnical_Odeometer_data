# GEOAI Benchmark 2: soil mechanical parameter prediction

This repository provides data and Jupyter notebooks for predicting missing soil mechanical parameters in GEOAI Benchmark Problem 2. The material is organized in `Data/`, `Scripts/`, and this `README.md`. The required inputs are supplied in separate folders under `Data/`; use the files designated for the study being reproduced. The `Scripts/` folder contains the Study A and Study B analysis notebooks shown below. Study C uses its separately supplied data and its borehole-exclusion evaluation protocol.

## Studies and evaluation sets

| Study | Purpose | Data partition |
| --- | --- | --- |
| A | Reproduce the Benchmark 2 evaluation using its prescribed partition. | 2,766 local records, 110 verification records, and 20 blind-test records. Use five-fold cross-validation within the local development data. |
| B | Evaluate prediction at boreholes withheld from development using a larger, approximately 80:20 borehole-disjoint split. | 2,219 development records from 184 boreholes and 555 test records from 52 different boreholes. |
| C | Evaluate at the designated benchmark test boreholes after excluding those boreholes from development. | 2,668 development records from 228 boreholes and 106 test records from 8 different boreholes. The Study C input data are provided separately under `Data/`. |

The studies use the same offshore reclamation dataset but answer different evaluation questions. Report each study's results with its own partition; do not combine records across study folders or treat the Study B and Study C test sets as interchangeable. These are within-site evaluations, including the borehole-disjoint evaluations in Studies B and C.

## Data and prediction task

Benchmark 2 contains index and laboratory soil measurements identified by sample and borehole. The available variables include degree of saturation (`S_r`), total unit weight (`gamma_t`), void ratio (`e`), liquid limit (`LL`), plastic limit (`PL`), and water content (`w`), alongside mechanical parameters: undrained shear strength (`S_u`), secant stiffness (`E_50`), preconsolidation pressure (`P_c`), compression index (`C_c`), and coefficient of consolidation (`C_v`). Spatial analyses use matched location and depth descriptors (`x`, `y`, and depth) where available. Check the column names and units in the supplied data before running a notebook.

The prediction scenarios represent different sets of laboratory results available at inference time:

| Missing type | Available mechanical measurements |
| --- | --- |
| 1 | None of the five target mechanical parameters |
| 2 | `S_u` |
| 3 | `S_u` and `E_50` |
| 4 | `P_c`, `C_c`, and `C_v` |

Targets already observed in a given scenario are inputs where appropriate; predictions concern the remaining missing parameters. Spatial comparisons consider no spatial descriptors, horizontal coordinates (`XY`), depth (`Z`), and combined coordinates and depth (`XYZ`).

## Scripts

The following filenames are shown in the repository's `Scripts/` folder:

| Notebook | Study | Role |
| --- | --- | --- |
| `StudyA_40runs_fixed_dataset_seeded_ensemble_OK_RK.ipynb` | A | Fixed-dataset, seeded ensemble analysis across repeated runs. |
| `StudyA_40runs_reproducible_tables_mapped_test_rows_revised.ipynb` | A | Repeated-run tables and test-row mapping. |
| `StudyA_40runs_with_spatial_comparators_MultipleRuns.ipynb` | A | Repeated-run comparisons involving spatial predictors. |
| `StudyB_40runs_fixed_dataset_seeded_ensemble_OK_RK.ipynb` | B | Fixed-dataset, seeded ensemble analysis across repeated runs. |
| `StudyB_40runs_reproducible_tables_mapped_test_rows_revised.ipynb` | B | Repeated-run tables and test-row mapping. |
| `StudyB_40run_with_spatial_comparators.ipynb` | B | Comparisons involving spatial predictors. |

Study B notebooks belong with the Study B data partition. Study C data belong with the Study C partition; no Study C notebook is visible in the supplied `Scripts/` screenshot. If Study C scripts are supplied separately, run those scripts against the Study C data and preserve the exclusion of all test boreholes from model development.

## Running the notebooks

1. Download or clone the repository and keep the `Data/` and `Scripts/` directories together.
2. Open the notebook corresponding to the intended study and set its input paths to the matching files under `Data/`.
3. Install the Python packages imported by that notebook in a Jupyter-capable environment. Check notebook imports for the exact dependencies and versions.
4. Run cells in order. Keep each study's train, verification, and test assignments fixed across repeated runs unless the notebook explicitly specifies a different protocol.
5. Fit any preprocessing and model selection steps using development data only. Evaluate the held-out records only after the fitting procedure is complete.

The notebook names indicate repeated 40-run analyses, but exact random seeds, model settings, output files, and execution requirements should be taken from the notebooks themselves. The README does not substitute one study's data for another's.
