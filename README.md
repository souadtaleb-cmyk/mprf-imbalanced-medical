# MPRF — Minority-guided Pruning Random Forest

Code accompanying the manuscript *"Towards Compact and Interpretable Random Forests for Imbalanced Medical Classification"* (IJDMMM-342531): MPRF forward selection of trees from a cost-sensitive random forest (CSRF), the OBP and IBEP baselines, and the minority-class rule extraction and simplification pipeline.

## Contents

| Notebook | Purpose | Manuscript |
|---|---|---|
| `notebooks/01_original_experiments.ipynb` | Original experiments: 10-fold CV, CSRF / OBP / IBEP / MPRF, rule extraction and simplification | Tables 1–3, 11–15 |
| `notebooks/02_five_seeds_alpha_ablation.ipynb` | Five seeds, sensitivity to α and k, ablations, paired tests with Holm correction | Tables 2, 3, 5–8 |
| `notebooks/03_calibration.ipynb` | Brier score, ECE, Platt recalibration | Table 9 |
| `notebooks/04_pruning_baselines.ipynb` | Comparison with simple pruning rules (top-k F1, clustering, random) | Table 10 |
| `notebooks/05_nested_alpha_robustness.ipynb` | α chosen by nested validation; in-fold imputation and removal of examination variables (Cervical Cancer), five seeds | Section 5.7, Table 4 |
| `notebooks/06_rules_test_folds_and_robustness.ipynb` | Rules evaluated on the test folds, fidelity to the pruned forest, one-seed robustness checks | Tables 16–17 |
| `notebooks/07_env_timing.ipynb` | Software and hardware report, measured times | Section 4.5 |

Cells of the original notebook: 1 Cervical Cancer, 3 Hepatitis C, 5 Mammography, 7 Pima, 13 rule extraction and simplification, 15 rule extraction and evaluation. Cells 9 (Thyroid *sick*) and 11 (Ecoli) are exploratory and are **not** reported in the manuscript.

## Datasets

Data files are not redistributed here; download them and place them in `data/` (or in `/content` on Colab) under the file names expected by the notebooks.

| Dataset | Source | Size | Expected file name |
|---|---|---|---|
| Cervical Cancer (Risk Factors) | UCI ID 383, https://doi.org/10.24432/C5Z310 (released 2 Mar 2017) | 858 × 36 | `1. risk_factors_cervical_cancer.csv` |
| Hepatitis C (HCV data) | UCI ID 571, https://doi.org/10.24432/C5D612 (released 9 Jun 2020) | 615 × 12 | `2. HepatitisCdata.csv` |
| Mammography | OpenML ID 310, https://www.openml.org/d/310 | 11,183 × 6 | `phpn1jVwe.arff` |
| Pima Indians Diabetes | Smith et al. (1988); Kaggle copy, https://www.kaggle.com/datasets/uciml/pima-indians-diabetes-database | 768 × 9 | `3. diabetes.csv` |

## Running (Google Colab)

1. Open a notebook in Colab (or locally with Jupyter) and upload the data files to `/content`.
2. Install the requirements (`pip install -r requirements.txt`) — Colab's defaults work.
3. Run the cell of the notebook of interest; for the rules, run cells 13 and 15 of notebook 01 on the Cervical Cancer data.

Environment used for the reported results: Google Colab, Python 3.13, scikit-learn 1.6.1, numpy 2.4.6, pandas 2.2.3, scipy 1.16.3; Intel Xeon 2.20 GHz, 2 logical cores, 12.7 GB RAM, no GPU.

## Protocol notes

- The five-seed results use the forest seed 42 + s and the validation split seed fold + 1000·s for s = 0…4 (seed 0 is the protocol of the original notebook).
- Stratified 10-fold CV (`shuffle=True`, `random_state=42`); the 20% validation set is a random, non-stratified split of each training portion (seed = fold index).
- CSRF = `RandomForestClassifier(n_estimators=50, class_weight="balanced")`.
- MPRF score = α·F1_min + (1−α)·G-mean with α = 0.6 (chosen empirically), k = 10 trees.
- Missing values are replaced by the feature-wise median computed on the full dataset (an in-fold variant is in notebooks 05 and 06).
- Notebook comments are partly in French.

## Citation and license

Taleb Zouggar, S., Merzoug, H. and Adla, A. *Towards Compact and Interpretable Random Forests for Imbalanced Medical Classification: Application to HPV-Related Cervical Cancer Detection.* International Journal of Data Mining, Modelling and Management (accepted; submission IJDMMM-342531).

License: MIT (see the `LICENSE` file).
