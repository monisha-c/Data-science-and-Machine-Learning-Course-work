# Exploratory Data Analysis (EDA)

This folder contains notebooks for understanding the raw diabetes dataset: dataset structure, missing values, patient demographics, readmission distribution, clinical complexity and the four care-intensity dimensions.

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

1. Open the required `.ipynb` notebook inside `eda/`.
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

1. Download the dataset as described in the parent README. Confirm the raw CSV is available.
2. Open the EDA notebook(s), select the shared kernel, then restart the kernel and run all cells in order.
3. Review dataset dimensions, missing-value summaries, demographic distributions, class imbalance and the four care-intensity variables (`time_in_hospital`, `num_lab_procedures`, `num_procedures`, `num_medications`).
4. Save the notebook with its outputs and record observations for the cleaning stage and the IEEE report.

**Input:** Raw `diabetic_data.csv` (and `IDS_mapping.csv` if needed). With the suggested folder structure, use `../data/raw/diabetic_data.csv` if the working directory is `eda/`, or `data/raw/diabetic_data.csv` if it is the root.

**Output:** Descriptive tables, plots and documented data-quality observations. This stage does not overwrite the raw dataset. Treat full-dataset EDA as descriptive; use training data for subsequent model-directed decisions.

**Next:** [Data cleaning and preparation](../cleaning/README.md).

## Troubleshooting

If imports fail, check `sys.executable` and install missing dependencies from the parent environment using the parent `requirements.txt`. If data files cannot be found, inspect `Path.cwd()` and the input paths. Do not create another environment in this folder or depend on variables created in another notebook session.

Return to the [parent README](../README.md) for full setup instructions.
