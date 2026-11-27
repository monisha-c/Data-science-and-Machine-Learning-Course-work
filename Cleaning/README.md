# Data Cleaning and Preparation

This folder contains notebooks for validating the raw data, defining eligible discharge encounters, handling missing values, creating the binary 30-day readmission target and preparing patient-level data splits.

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

1. Open the required `.ipynb` notebook inside `cleaning/`.
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

1. Review the [EDA notebook results](../eda/README.md) before cleaning.
2. Confirm the raw dataset and mapping file are available. Load them explicitly; do not rely on variables from an EDA notebook.
3. Select the shared kernel, restart it and run all cells in order.
4. Validate missing values and encounter IDs, document cohort eligibility, preserve the distinction between unperformed tests and missing entries, create the 30-day target, and prepare patient-grouped training/validation/test data as required by the chosen validation design.
5. Save the prepared data and split assignments to the agreed output location; document the actual filenames and schema for the models notebooks.

**Input:** Raw `diabetic_data.csv` and `IDS_mapping.csv` as needed; the suggested raw location is `../data/raw/` when running from `cleaning/`.

**Output:** Prepared cohort and patient-level split assignments (suggested location: `../data/processed/`). Actual output filenames must match what the notebooks save and what the models notebooks load.

**Leakage prevention:** Keep all encounters for the same patient in the same split. Fit learned transformations such as imputation, encoding and scaling on training data only (within training folds during cross-validation). Preserve `patient_nbr` for grouping, not as a predictor, and exclude `encounter_id` and outcome-derived fields from model inputs.

**Next:** [Machine learning models](../models/README.md).

## Troubleshooting

If imports fail, check `sys.executable` and install missing dependencies from the parent environment using the parent `requirements.txt`. If data files cannot be found, inspect `Path.cwd()` and the input paths. Do not create another environment in this folder or depend on variables created in another notebook session.

Return to the [parent README](../README.md) for full setup instructions.
