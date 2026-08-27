# 03. Exploratory Data Forensics and Cleaning Protocols

## Exploratory Data Analysis (EDA Gate)

EDA inspects raw data before cleaning or filtering. Never alter raw data during EDA.

### Mandatory 30-Item Forensics Checklist
1. File readability, delimiter parsing, and encoding.
2. Exact row and column dimensions.
3. Variable names and naming consistency.
4. Data types and explicit column classes.
5. Units and measurement scales.
6. Impossible, implausible, or negative values where non-negative expected.
7. Duplicate rows.
8. Duplicate subject, animal, sample, or session identifiers.
9. Unexpected replication across repeated measures.
10. Expected vs observed key uniqueness and join cardinality.
11. Subject-level attributes changing unexpectedly across repeated observations.
12. Missing values and unrecognized missing tokens.
13. Missingness by group, treatment, sex, strain, batch, time, and session.
14. Structural missingness vs accidental missingness.
15. Univariate, multivariate, and within-subject outliers.
16. Range, quantiles, mean, median, standard deviation, and median absolute deviation (MAD).
17. Zero inflation, floor effects, and ceiling effects.
18. Skewness, kurtosis, and heavy tails.
19. Inconsistent factor levels and spelling variants.
20. Date and time parsing (ISO-8601 compliance).
21. Group sizes and unbalanced experimental designs.
22. Batch, cage, litter, sex, strain, treatment, session, and operator variables.
23. Time ordering and impossible temporal sequences.
24. Measurement limits, upper/lower detection limits, and censored values.
25. Internal consistency among related variables (e.g. active licks vs total infusions).
26. Attrition and sample-size tracking across acquisition stages.
27. Truncated fields, spreadsheet formula errors, and encoding artifacts.
28. Potential leakage of outcome information into predictor variables.
29. Independence of observations vs nested/clustering structure.
30. Acquisition hardware glitches, lickometer contact bounce, and recording dropouts.

### Diagnostic Plots
Plot raw observations before cleaning. Save all plots in `figures/` and matching plot data in `results/`:
- Histograms, density plots, and empirical CDFs.
- Jittered raw points overlaid on boxplots/violin plots.
- Q-Q plots for error normality inspection.
- Scatterplots with smoothers.
- Individual longitudinal trajectories for repeated measures.
- Missingness maps and batch-effect diagnostics.

### EDA Summary Report
Write: `reports/<run_id>__eda__<analysis_slug>__summary.md`.

---

## Cleaning Protocols and User Approvals

Never remove an observation solely because it is statistically extreme. An outlier is not automatically an artifact.

### 1. Propose Cleaning Options
Save proposed decisions in:
```text
plans/<timestamp>__cleaning__<analysis_slug>__options.md
```
For each issue, document:
- Problem detected and affected rows/subjects.
- Evidence and possible causes.
- Option A, Option B, and alternatives.
- Scientific advantages, disadvantages, and impact on sample size/balance.
- Required user decision.

### 2. Freeze Approved Cleaning Plan
Save user decisions in:
```text
plans/<timestamp>__cleaning__<analysis_slug>__approved.md
```

### 3. Generate Cleaned Data Files
Create: `data/<run_id>__cleaned__<analysis_slug>__<dataset>.<csv|tsv>`.
- Plain-text comma-delimited `.csv` or tab-delimited `.tsv`.
- Do not write R row names as an extra column.
- Accompany with machine-readable exclusion/correction audit tables in `results/`:
  - `results/<run_id>__cleaning__<slug>__exclusion-audit.tsv` (records every removed row ID and reason).
  - `results/<run_id>__cleaning__<slug>__correction-audit.tsv` (records old vs new values and row IDs).

### 4. Text and TSV Data Type Protection
- **Explicit Column Types**: Always pass explicit schemas (`readr::cols()` in R, `dtype={...}` in Python).
- **Missing Value Protection**: Explicitly declare `na = c("", "NA")`. If literal `"NA"` is a valid token (e.g. gene symbol), use `keep_default_na=False` in Python or handle `na.strings` explicitly in R.
- **Date Safety**: Explicitly parse date columns to prevent gene names (`MARCH1`, `SEPT2`) from turning into dates.
