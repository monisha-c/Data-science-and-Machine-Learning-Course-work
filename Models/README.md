# Machine Learning Models

This folder contains notebooks for expected-care regression, standardised care-intensity discordance feature engineering, readmission classification, model evaluation and supporting clustering/fairness analyses.

## 1. Environment setup

Use the shared virtual environment in the **parent project folder**. Follow the [parent README](../README.md) for the initial Python installation, environment creation, dependencies and Jupyter kernel registration. Do not create a separate virtual environment in this folder.

If the parent environment already exists, activate it from the parent folder:

**Windows (PowerShell)**

```powershell
.\.venv\Scripts\Activate.ps1
```

**macOS / Linux**

```bash
source .venv/bin/activate
```

## 2. Select the notebook kernel

Open the repository root in VS Code, or launch JupyterLab from the activated parent folder using:

```bash
python -m jupyterlab
```

1. Open the required `.ipynb` notebook inside `models/`.
2. In VS Code, click **Select Kernel** at the top right. In JupyterLab, use **Kernel > Change Kernel** or click the kernel name at the top right.
3. Select **Python (DSML CW2)**. In VS Code, the parent `.venv` Python environment is also a valid selection.
4. Save the notebook. Repeat this for every notebook in the folder when needed.

If the kernel is missing, activate the parent environment and register it again:

```bash
python -m ipykernel install --user --name dsml-cw2 --display-name "Python (DSML CW2)"
```

## 3. Verify the kernel

Run this cell before executing the notebook:

```python
import sys
from pathlib import Path

print("Python executable:", sys.executable)
print("Working directory:", Path.cwd())
```

The Python executable must point to the parent `.venv`, not a global Python installation. Check the working directory before loading relative file paths.

## 4. Run the notebook

1. Complete [data cleaning and preparation](../cleaning/README.md) first. Confirm the prepared data files and patient-level split assignments exist.
2. Open the required model notebook(s) and select the shared kernel. Restart the kernel and execute cells in order.
3. Run expected-care regression before generating discordance features. Use training-only fits and patient-grouped out-of-fold predictions for training residuals; apply the training-fitted models and standardisation to held-out patients. If tuning classifiers with cross-validation, nest feature generation within the training folds.
4. Compare readmission classifiers using the same patient-level splits and three feature sets: clinical features only; clinical plus observed care; clinical plus observed care plus discordance features.
5. Evaluate PR-AUC (state the chosen calculation, e.g. average precision), recall, F1-score and false-negative rate, and record confusion matrices, model settings and supporting analyses. Select thresholds on validation data, not the test set.

**Input:** Prepared data and split assignments from the cleaning stage (suggested location: `../data/processed/` when running from `models/`). Use the actual filenames produced by cleaning.

**Output:** Model results, metrics, plots and any explicitly saved trained models or intermediate features. Document actual output paths in the notebooks or this README once agreed.

**Research safeguard:** Observed inpatient care is available at discharge, so the prediction task is discharge-time readmission risk. Expected care is a statistical benchmark, not a clinical recommendation. Reserve the test set for final evaluation and avoid claims of causal effects.

## Troubleshooting

If imports fail, check `sys.executable` and install missing dependencies from the parent environment using the parent `requirements.txt`. If data files cannot be found, inspect `Path.cwd()` and the input paths. Do not create another environment in this folder or depend on variables created in another notebook session.

Return to the [parent README](../README.md) for full setup instructions.
