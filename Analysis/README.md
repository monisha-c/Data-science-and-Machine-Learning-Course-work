# Statistical analysis — DS&ML CW2

**Research topic:** A Machine Learning Framework for Detecting Multidimensional Care-Intensity Discordance and Predicting 30-Day Hospital Readmission in Patients with Diabetes.

These notebooks use the same patient-level splits, four care dimensions and locked models from the existing Python notebooks. They do not train new readmission classifiers or change the validation-selected thresholds.

## 1. Prerequisites

Finish `cleaning/cleaning.ipynb`, `cleaning/validation.ipynb`, `models/01_logistic_random_forest.ipynb` and `models/02_svm_xgboost.ipynb`. The latter must finish its final evaluation successfully (including the XGBoost import). The original model notebooks use the lowercase `models/outputs/` directory, even if the notebook folder is capitalised on your machine. Keep those outputs in place. Do not load joblib files from untrusted sources.

Required artifacts include `data/processed/feature_contract.json`, the validation report and split CSVs, and `models/outputs/{discordance_bundle.joblib,locked_classifiers.joblib,evaluation_lock.json,test_metrics_all.csv}`. The raw CSV is also checked against its saved checksum. Rerun the bridge whenever the upstream pipeline changes; do not mix runs.

## 2. Install R and register its Jupyter kernel

Install R from <https://cran.r-project.org/>. R is separate from the Python `.venv`; do not install it with pip. RStudio is optional. Open a terminal at the project root and activate the existing Python environment so that the `jupyter` executable is on PATH.

macOS/Linux:

```bash
source .venv/bin/activate
R
```

Windows PowerShell:

```powershell
.\.venv\Scripts\Activate.ps1
R.exe
```

If R is not on PATH, use its installed executable path. Inside the R console (not a Python notebook), run:

```r
install.packages(c("IRkernel", "jsonlite", "sandwich", "cluster"), repos="https://cloud.r-project.org")
IRkernel::installspec(name="dsml-r", displayname="R (DSML CW2)", user=TRUE)
q()
```

From the activated terminal, verify `jupyter kernelspec list`. In VS Code, reopen the notebook or reload the window if necessary. Use the top-right kernel selector. Select **Python (DSML CW2)** for notebook 00 and **R (DSML CW2)** for notebooks 01–03. In JupyterLab, choose the corresponding kernel via Kernel > Change Kernel.

If R installation or IRkernel registration fails, check `R --version`, the active terminal PATH, and whether `jupyter kernelspec list` works in the virtual environment. No second Python virtual environment is required.

## 3. Run in order

| Notebook | Kernel | Purpose |
|---|---|---|
| `00_export_for_R.ipynb` | Python | Reconstruct locked test scores, verify AP and hashes, export private R inputs |
| `01_RQ1_statistical_association.ipynb` | R | Descriptive analysis, adjusted logistic regression, patient-cluster robust Wald test and odds ratios |
| `02_RQ2_model_comparison.ipynb` | R | Compare 12 experiments, paired patient bootstrap CI for AP(C) − AP(B) |
| `03_clustering_and_fairness.ipynb` | R | Training-only K-means, held-out cluster summary and demographic error gaps |

Run all cells in each notebook. Stop if a check fails. The RQ1 model can fail on sparse categories, separation or singular covariance; investigate and document any changes rather than silently dropping predictors or reporting unreliable estimates. The code includes a training-frequency rare-category pooling rule. Use the same locked outputs throughout; never tune on the test set.

## 4. Outputs and interpretation

The generated `outputs/` folder contains aggregate CSV tables and PNG plots. For RQ1, report the joint Wald test and four adjusted odds ratios with 95% confidence intervals and Holm-adjusted p-values. For RQ2, report the validation-selected primary classifier family's AP difference with a paired patient-bootstrap 95% interval, then secondary metrics (recall, F1, FNR, precision). The R AP calculation should agree with Python. Clustering and fairness are supporting descriptive analyses, not new research questions.

The RQ1 inference conditions on the fitted benchmark and eligible cohort, not on a new random training sample. Associations do not prove causality, inadequate treatment or clinical benefit. The primary RQ2 CI quantifies test-patient sampling uncertainty conditional on fixed models. This is not independent replication of the Python evaluation. No costs or carbon emissions are measured.

## 5. Data protection and reproducibility

`private_exports/` contains row-level patient-cluster labels, clinical attributes and derived predictions. It is not guaranteed anonymous. Keep it local and access-controlled. The following can be added to the **parent** `.gitignore`:

```gitignore
statistical_analysis/private_exports/
models/outputs/*.joblib
data/raw/
data/processed/
.venv/
```

Review aggregate outputs for sparse groups and notebook outputs for sensitive content before sharing. Do not publish row-level data or model artifacts. Record R version and package versions (`sessionInfo()` in an R cell), Python versions, seed 42, data provenance and the actual evaluation outputs for the paper. The code uses a prespecified 3-cluster solution, 1,000 patient bootstrap replicates, 50 training encounters for rare-category pooling, and a 20-count subgroup suppression rule. These are proposed research design choices, not clinical standards.

## 6. Validation status

The notebooks were constructed against the actual earlier notebook code and its output contract. Notebook structure and Python syntax were checked. R execution and real-data end-to-end testing have not been performed in the creation environment (R and the group's generated model artefacts are unavailable). Run on your local project before reporting results. No numerical results are prefilled.
