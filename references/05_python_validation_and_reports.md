# 05. Cross-Language Validation and LaTeX Reporting

## Independent Cross-Language Validation

When requested, validate a result independently in Python (or Bash for file operations) to confirm computational correctness.

### Language-Neutral Handoff Brief
Write and freeze the brief before alternate implementation begins:
```text
validation/<timestamp>__brief__<analysis_slug>__<target>.md
```
The brief must contain ONLY:
1. **Goal**: Target estimand or computation to reproduce.
2. **Input Data**: Path, dictionary path, column data types, key variables, units, inclusion/exclusion rules.
3. **Required Output**: Output filename, structure, columns, precision, comparison criteria.
4. **Statistical & Algorithmic Specification**:
   - Formulas and link functions.
   - Contrast coding (e.g. treatment contrast with explicit reference group).
   - Estimation method (e.g. REML vs ML, OLS).
   - Random-effects covariance structure (independent vs unstructured).
   - Degrees of freedom method (Satterthwaite, Kenward-Roger, Wald).
   - Random seed value and RNG method.

*Strict Firewall*: Do not include R code, package calls, intermediate outputs, or target numerical answers.

### Alternate Implementation & Comparison
1. Implement independently in `validation/python__<slug>__<target>.py` and pin dependencies in `validation/python__<slug>__requirements.txt`.
2. Run validation under a new `run_id` with stage `validation`.
3. Freeze alternate outputs in `results/` and alternate figures in `figures/`.
4. Generate comparison table: `results/<run_id>__validation__<slug>__<target>-comparison.tsv`.
5. Write review: `validation/<run_id>__review__<slug>__<target>.md`.
6. Classification: `matched`, `matched-within-tolerance`, `mismatched`, or `inconclusive`. A mismatch opens an issue.

---

## Final LaTeX Report Compilation

An analysis is finished ONLY when the user explicitly declares it finished.

### Report Specifications
- **Location**: `reports/<YYYY-MM-DD>_<HHMMSS>__final-report__<analysis_slug>__report.<tex|pdf>`
- **Paper & Margins**: Letter paper, 0.75-inch margins.
- **Typography**: Clean sans-serif font (e.g. `\renewcommand{\familydefault}{\sfdefault}` or sans system font).
- **Style**: Restrained, objective scientific tone; informative color palettes; no decorative bolding.
- **Dynamic Numbering**: LaTeX counters assign all figure and table numbers. Use `\label{fig:...}` and `\ref{fig:...}`. Never write "Figure 3" in captions or filenames.
- **Graphics Path**: Include `\graphicspath{{../figures/}}` in preamble.

### Standard Preamble
```latex
\documentclass[11pt]{article}
\usepackage[letterpaper,margin=0.75in]{geometry}
\usepackage{fontspec}
\renewcommand{\familydefault}{\sfdefault}
\usepackage{xcolor}
\usepackage{graphicx}
\graphicspath{{../figures/}}
\usepackage{booktabs}
\usepackage{longtable}
\usepackage{amsmath}
\usepackage{hyperref}
\usepackage{cleveref}
\usepackage{caption}
\usepackage{float}
```

### Mandatory Provenance / File Tracking Block
Every final report source must end with an invisible LaTeX comment block listing all files and Git commits:

```latex
% Tracking record begin
% analysis_slug: oxycodone-escalation
% accepted_run_id: 20260826_093210
% accepted_git_commit: 8f3c2a1
% role=raw-data | path=data/20260825_120000__raw__oxycodone.csv
% role=data-dictionary | path=plans/20260825_123000__dictionary__oxycodone-escalation__variables.tsv
% role=cleaned-data | path=data/20260826_090000__cleaned__oxycodone-escalation__primary.tsv
% role=decision-record | path=plans/20260826_083000__decisions__oxycodone-escalation__grilling-summary.md
% role=cleaning-plan | path=plans/20260826_084500__cleaning__oxycodone-escalation__approved.md
% role=analysis-plan | path=plans/20260826_091000__analysis-plan__oxycodone-escalation__models-and-displays.md
% role=cleaning-script | path=scripts/01_clean_data.R | git_commit=7e2b1a4
% role=analysis-script | path=scripts/02_fit_models.R | git_commit=8f3c2a1
% role=figure-script | path=scripts/03_figures_tables.R | git_commit=8f3c2a1
% role=figure | path=figures/20260826_093210__analysis__oxycodone-escalation__intake-by-strain.pdf
% role=plot-data | path=results/20260826_093210__analysis__oxycodone-escalation__intake-by-strain.csv
% role=model-summary | path=results/20260826_093210__analysis__oxycodone-escalation__model-summary.csv
% role=run-log | path=logs/20260826_093210__analysis__oxycodone-escalation__run.md
% role=environment | path=renv.lock
% Tracking record end
```

Compile with:
```bash
latexmk -xelatex -interaction=nonstopmode -halt-on-error -file-line-error -cd reports/<report_file>.tex
```
Publish only `.tex` and `.pdf` to `reports/`. Record compilation status in the report run log.
