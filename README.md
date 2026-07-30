# ESG Scorecard Validation

Reproducible analysis for the paper *"Beyond Discrimination Power: A
Validation Framework for Target-less Expert-Judgment ESG Rating Models"*
(author and venue anonymized for peer review).

This repository operationalizes two axes of a proposed validation
framework for ESG rating models that lack an observable target variable
(Y): **convergent validity** across raters and **cut-off justification**.
Because standard discrimination metrics (KS, AUC) are undefined without a
target, the framework substitutes psychometric validity evidence.

---

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

Run the notebooks in order (01 -> 02 -> 03). Notebook 02 writes CSVs that
notebook 03 consumes.

```bash
pip install pandas numpy scipy matplotlib
jupyter notebook
```

## Data

Both datasets are public (Kaggle). Because both are public and one is
single-source, the empirical results are positioned as an **illustrative
demonstration** of the validation framework, not a confirmatory
large-sample estimation.

- **Dow 30 (n = 30; 26 complete cases):** three major raters (S&P Global,
  Sustainalytics, MSCI), Q2 2024.
- **Broad dataset (n = 722):** single-source ESG scores with letter grades,
  used for cut-off / distribution analysis.

## Key methodological note

The three raters use different scale directions. **Sustainalytics is an
ESG *risk* score (lower = better)** and is sign-reversed so that all three
measures increase in ESG quality before any correlation is computed. The
dataset's shipped `Sustainalytics_normalized` column is *not*
direction-aligned (it still increases with risk); do not use it without
flipping.

---

## Results summary

### 1. Convergent validity across raters (Dow 30, n = 26, direction-aligned)

| Rater pair | Pearson r | Spearman rho |
|---|---:|---:|
| S&P vs Sustainalytics | -0.163 | 0.021 |
| S&P vs MSCI | 0.251 | 0.266 |
| Sustainalytics vs MSCI | 0.285 | 0.295 |
| **Mean** | **0.124** | **0.194** |

Mean pairwise agreement is far below the credit-rating benchmark (~0.99)
and below the large-sample ESG benchmark reported in prior work
(~0.53-0.56). The negative S&P-vs-Sustainalytics correlation indicates
that firms rated favorably by one provider can be rated unfavorably by
another. Individual pairwise correlations are not statistically
significant at n = 26; the direction and magnitude are nonetheless
consistent with large-sample prior evidence, which is why the results are
framed as an illustrative demonstration.

**Quadratic-weighted Cohen's kappa (4 ordinal bins):**

| Rater pair | Weighted kappa |
|---|---:|
| S&P vs Sustainalytics | 0.072 |
| S&P vs MSCI | 0.072 |
| Sustainalytics vs MSCI | 0.449 |

**Convergent validity by pillar (S&P pillar vs other raters, Spearman):**

| Pillar | n | vs Sustainalytics | vs MSCI |
|---|---:|---:|---:|
| Environmental (E) | 25 | 0.450 | 0.117 |
| Social (S) | 25 | 0.136 | 0.164 |
| Governance (G) | 25 | 0.572 | 0.415 |

The Social pillar shows the lowest cross-rater agreement in both
comparisons, independently reproducing the pattern of higher S-pillar
disagreement documented in the divergence literature.

### 2. Cut-off justification (broad dataset, n = 722)

Grade boundaries are hard-coded at round numbers and are not aligned with
the score distribution:

| Cut-off scheme | Lower | Middle | Upper |
|---|---:|---:|---:|
| Hard-coded (observed) | 750 | 900 | 1200 |
| Distribution-based (quartiles) | 763 | 1046 | 1144 |

Consequences for the grade distribution:

| Grade | Share |
|---|---:|
| BBB | 51.0% |
| B | 23.1% |
| BB | 14.4% |
| A | 11.5% |

Absolute, distribution-blind cut-offs compress a majority of firms (51%)
into a single grade (BBB), reducing the rating's ability to discriminate
across firms.

---

## Outputs

Notebook 03 writes paper-ready artefacts:

- `results/table/table_convergent_validity.tex`, `results/table/table_cutoff.tex`
- `results/figures/*.png` (repository preview) and `*.pdf` (LaTeX inclusion)

## License

Anonymized for review. License to be added upon publication.
