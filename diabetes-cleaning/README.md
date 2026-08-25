# diabetes-cleaning

Data-cleaning workspace for the Pima Indians Diabetes dataset.

## Structure

```
diabetes-cleaning/
├── data/
│   ├── raw/diabetes.csv          # source data — never edited
│   └── clean/diabetes_clean.csv  # output of notebooks/cleaning.ipynb (not generated yet)
├── notebooks/
│   └── cleaning.ipynb
├── README.md
└── .gitignore
```

## Raw dataset

`data/raw/diabetes.csv` — 768 rows, 9 columns, copied byte-for-byte from the original
file (SHA-1 `f3e442b2a54522a7708b2992af76ccf1abb6183a`). It is the immutable source of
truth: read it, never write to it.

| Column | Description |
| --- | --- |
| `Pregnancies` | Number of times pregnant |
| `Glucose` | Plasma glucose concentration (2-hr oral glucose tolerance test) |
| `BloodPressure` | Diastolic blood pressure (mm Hg) |
| `SkinThickness` | Triceps skin fold thickness (mm) |
| `Insulin` | 2-hour serum insulin (mu U/ml) |
| `BMI` | Body mass index (kg/m²) |
| `DiabetesPedigreeFunction` | Diabetes pedigree function |
| `Age` | Age in years |
| `Outcome` | Target: 1 = diabetic, 0 = not diabetic |

## Status

**No cleaning has been applied.** `notebooks/cleaning.ipynb` currently only *inspects*
the raw data (shape, dtypes, duplicates, zero-valued sentinels) and leaves the cleaning
decisions to you. `data/clean/` is an empty placeholder — `diabetes_clean.csv` appears
only once you fill in and run the cleaning section of the notebook.

## Usage

```bash
python -m venv .venv && source .venv/bin/activate
pip install pandas jupyter
jupyter notebook notebooks/cleaning.ipynb
```

## Convention

Raw in, clean out. Every transformation lives in the notebook so the cleaned file can
always be rebuilt from `data/raw/diabetes.csv` alone.
