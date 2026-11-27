# DS&ML CW2 — Diabetes Hospital Readmission

This repository contains the Python notebooks for the group coursework project:

**A Machine Learning Framework for Detecting Multidimensional Care-Intensity Discordance and Predicting 30-Day Hospital Readmission in Patients with Diabetes.**

The project uses the [UCI Diabetes 130-US Hospitals (1999–2008) dataset](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008).

## 1. Project structure

```text
project-root/
├── README.md
├── requirements.txt
├── .venv/                  # Created locally; do not commit
├── data/                   # Suggested location for data files
│   ├── raw/
│   │   ├── diabetic_data.csv
│   │   └── IDS_mapping.csv
│   └── processed/          # Suggested location for cleaning outputs
├── eda/
│   ├── README.md
│   └── *.ipynb
├── cleaning/
│   ├── README.md
│   └── *.ipynb
└── models/
    ├── README.md
    └── *.ipynb
```

The notebook names are represented by `*.ipynb` because actual names may differ. The `data/` layout is a suggested convention; if the existing notebooks use other paths, either update their paths or keep the existing documented layout consistently.

## 2. Prerequisites

- Python 3.11 (recommended for a consistent group environment).
- A terminal (PowerShell on Windows, Terminal on macOS/Linux).
- JupyterLab or VS Code with the Microsoft Python and Jupyter extensions.
- A local copy of the repository and the dataset.

Open a terminal in the **parent project folder** (the folder containing this README). All commands below are run there unless stated otherwise.

## 3. Create and activate the virtual environment

Create **one** virtual environment in the parent folder. Do not create separate environments in `eda`, `cleaning` or `models`.

### Windows (PowerShell)

```powershell
py -3.11 --version
py -3.11 -m venv .venv
.\.venv\Scripts\Activate.ps1
```

If the `py` launcher is unavailable, use `python -m venv .venv` after verifying that `python --version` reports Python 3.11.

If PowerShell blocks activation scripts, use Command Prompt with `.venv\Scripts\activate.bat`, or run the environment's Python executable directly (e.g. `.\.venv\Scripts\python.exe -m pip install -r requirements.txt`). No system-wide execution-policy change is required.

### macOS / Linux

```bash
python3.11 --version
python3.11 -m venv .venv
source .venv/bin/activate
```

If `python3.11` is not found, install Python 3.11 first. On some Linux distributions, the matching `venv` package may also be required.

## 4. Install project dependencies

With the environment activated, run:

```bash
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
python -m pip check
```

The supplied `requirements.txt` covers EDA, data preparation, statistical analysis, regression, classification, clustering and Jupyter. It is a starter dependency list, not a tested or version-pinned lock file. Add any dependencies required by the group's notebooks and agree on tested versions before final submission. Do not install packages separately in each notebook folder.

## 5. Register the shared Jupyter kernel

Still in the activated parent environment, run this command once per local setup:

```bash
python -m ipykernel install --user --name dsml-cw2 --display-name "Python (DSML CW2)"
```

This registers the parent `.venv` as a selectable Jupyter kernel for every notebook in the repository. Each team member must register it on their own computer; kernel registrations are local to the machine.

Verify it is available:

```bash
jupyter kernelspec list
```

## 6. Open notebooks and select the kernel

### Option A — JupyterLab

From the activated parent environment:

```bash
python -m jupyterlab
```

Open a notebook in `eda/`, `cleaning/` or `models/`. Click the kernel name at the top right, or choose **Kernel > Change Kernel**, and select **Python (DSML CW2)**. Repeat for each notebook when opening it for the first time, and save the notebook.

### Option B — VS Code

Open the parent project folder in VS Code, not just an individual child folder.

1. Open the Command Palette (`Ctrl+Shift+P` on Windows/Linux or `Cmd+Shift+P` on macOS).
2. Run **Python: Select Interpreter** and choose the interpreter inside the parent `.venv`.
3. Open a `.ipynb` file and click **Select Kernel** (or the existing kernel name) at the top right.
4. Choose **Python (DSML CW2)** under Jupyter kernels, or select the parent `.venv` under Python environments.
5. Save the notebook and repeat for each notebook as needed.

The editor interpreter and the notebook kernel are separate selections; selecting the interpreter alone is not sufficient to confirm which Python runs notebook cells.

### Verify the kernel in a notebook

Run the following cell in each notebook before running the analysis:

```python
import sys
from pathlib import Path

print("Python version:", sys.version)
print("Python executable:", sys.executable)
print("Working directory:", Path.cwd())
```

The executable should point to the **parent** `.venv` (`.venv/Scripts/python.exe` on Windows or `.venv/bin/python` on macOS/Linux). Do not assume the notebook's working directory is the parent folder: check the printed path when loading data.

## 7. Dataset setup

Download the source dataset from the [UCI repository](https://archive.ics.uci.edu/dataset/296/diabetes+130-us+hospitals+for+years+1999-2008) and extract `diabetic_data.csv` and `IDS_mapping.csv` into the agreed raw data folder. With the suggested layout, this is `data/raw/`.

For example, when the notebook working directory is `eda/` or `cleaning/`, `../data/raw/diabetic_data.csv` points to the raw CSV. When the working directory is the project root, the path is `data/raw/diabetic_data.csv`. Prefer `pathlib.Path` and avoid machine-specific absolute paths.

Keep the raw dataset unchanged. Use a separate location for processed data and intermediate outputs. Cite the original data source in the report.

## 8. Notebook execution order

| Order | Folder | Purpose | Expected input |
|---|---|---|---|
| 1 | [eda/](eda/README.md) | Initial data inspection, missingness and visualisation | Raw dataset |
| 2 | [cleaning/](cleaning/README.md) | Cohort definition, data validation, target creation and split preparation | Raw dataset |
| 3 | [models/](models/README.md) | Expected-care regression, discordance features, classification and evaluation | Prepared data and patient-level splits |

Select the shared kernel in every notebook. Run cells in order using **Run All** (or restart the kernel and run all cells). Each notebook must load its required inputs explicitly: Python variables from one notebook are not automatically available in another, even if both use the same kernel environment.

Keep preprocessing, feature engineering and hyperparameter selection restricted to training data. Reserve held-out patients for final evaluation; initial EDA on the full dataset is descriptive only.

## 9. Troubleshooting

| Issue | Solution |
|---|---|
| `ModuleNotFoundError` | Check `sys.executable` in the notebook. Activate the parent environment and install dependencies using `python -m pip install -r requirements.txt`. Restart the kernel. |
| Kernel not listed | Activate the parent `.venv`, repeat Step 5 and refresh/reopen the notebook editor. |
| Kernel points to an old location | Re-register the kernel from the current parent `.venv`. Virtual environments should be recreated rather than copied between machines. |
| `FileNotFoundError` | Check `Path.cwd()` and confirm the CSV or processed file is in the expected location. |
| Notebook cells fail after reopening | Restart the kernel and run all cells in sequence. Ensure previous-stage notebooks have produced the required files. |

When finished, stop the JupyterLab server with `Ctrl+C` in its terminal if applicable, then run `deactivate` to leave the environment. On future sessions, activate the existing `.venv`; there is no need to recreate or re-register it unless the setup changes.

## 10. Reproducibility and version control

Commit notebook source, READMEs and the agreed dependency list. Do not commit `.venv/`, `.ipynb_checkpoints/`, secrets or temporary caches. Agree on whether to track data and generated outputs, taking the dataset licence and file size into account. Record the Python and package versions used for final experiments; the starter requirements are not a reproducibility lock file.
