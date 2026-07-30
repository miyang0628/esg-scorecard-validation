# ESG Scorecard Validation — Analysis Notebooks

Reproducible analysis for the paper *"Beyond Discrimination Power: A
Validation Framework for Target-less Expert-Judgment ESG Rating Models."*

## Repository layout

```
.
├── notebooks/
│   ├── 01_eda.ipynb                     # data inspection, missingness, scale checks
│   ├── 02_experiment.ipynb              # convergent validity + cut-off analysis
│   └── 03_paper_tables_figures.ipynb    # LaTeX tables + publication figures
├── data/
│   ├── Dow30-DATASET-Q2_2024.csv        # 3-rater ESG (S&P, Sustainalytics, MSCI)
│   └── data.csv                         # 700+ firm single-source ESG scores
└── results/
    ├── table/                           # *.csv (intermediate) + *.tex (paper)
    └── figures/                         # *.png (preview) + *.pdf (LaTeX)
```

## How to run

Run the notebooks in order (01 → 02 → 03). Notebook 02 writes CSVs that
notebook 03 consumes.

```bash
pip install pandas numpy scipy matplotlib
jupyter notebook
```

## Data sources

Both datasets are public (Kaggle). See the paper's Data section for
provenance. Because both are public and one is single-source, the
empirical results are positioned as an **illustrative demonstration** of
the validation framework, not a confirmatory large-sample estimation.

## Key methodological note

The three raters use different scale directions. **Sustainalytics is an
ESG *risk* score (lower = better)** and is sign-reversed so that all
three measures increase in ESG quality before any correlation is
computed. The dataset's shipped `Sustainalytics_normalized` column is
*not* direction-aligned; do not use it without flipping.
