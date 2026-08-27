---
name: "beaver-data"
description: "Meticulous, reproducible data analysis pipeline using Bash orchestration, R modeling, Python cross-validation, and LaTeX reporting with mandatory stage grilling and decision tracking."
version: 5
created: "2026-08-26"
updated: "2026-08-27"
---
## When to Use
Use for end-to-end reproducible data analysis, statistical modeling, exploratory data analysis, data cleaning, or scientific reporting using Bash orchestration, R statistical modeling, Python cross-language validation, and LaTeX reporting with strict stage gating and decision tracking.

## Mandatory Grilling Requirement
**Requirements Grilling is mandatory for EVERY analysis request.** This includes:
- New project or analysis setups.
- Reanalyses, model modifications, or parameter adjustments.
- Subsequent, follow-up, or secondary scientific questions.
- Retractions, branch pivots, or alternative exploratory attempts.

**Never start coding, modifying data, fitting models, or publishing results before completing a grilling interview round and securing explicit user confirmation.**

## Mandatory Diagnostic Visualization Standards
For both EDA and Modeling stages, **always generate multi-angle diagnostic visual suites**:
- **Multi-Angle Views**: Distributional geometry (raincloud, ECDF, strip), longitudinal subject spaghetti plots, multi-factor faceted grids, and missingness maps.
- **Model Diagnostics**: Residuals vs fitted with loess smoother, Q-Q plots with confidence bands, scale-location, leverage/Cook's distance, random-effect BLUP caterpillar plots, and `DHARMa` simulated residuals for count/GLMMs.
- **Aesthetic Excellence (Wilke / Scherer standards)**:
  - Clean themes: `theme_minimal(base_size = 12)` or `theme_light(base_size = 12)` with `plot.title.position = "plot"` and `panel.grid.minor = element_blank()`.
  - Descriptive titles and subtitles stating observation, context, and sample size ($N$). No hard-coded figure numbers.
  - Modern accessible color palettes: Okabe-Ito, `viridis`, `scico`, `MetBrewer`, `rcartocolor` (CVD-safe and grayscale readable).
  - Multi-panel composite figures stitched via `{patchwork}` with semantic panel tags (`A`, `B`).

## Procedure
1. **Initialize Project**: Create directories (`data/`, `scripts/`, `issues/`, `logs/`, `plans/`, `figures/`, `results/`, `reports/`, `validation/`, `renv/`). Initialize local Git repo, `.gitignore`, and `renv`. Commit baseline.
2. **Mandatory Grilling Interview**: Interview across decision tree (goals, estimands, exclusions, cleaning authority, statistical assumptions, displays, completion criteria). Mandatory for new analyses, reanalyses, and subsequent tasks. Save confirmed summary to `plans/<timestamp>__decisions__<slug>__grilling-summary.md`.
3. **Register Raw Data**: Place immutable raw data in `data/`. Create `plans/<timestamp>__intake__<slug>__source-record.md` and `plans/<timestamp>__dictionary__<slug>__variables.tsv`.
4. **Conduct Multi-Angle EDA**: Inspect raw distributions, missingness, outliers, artifacts, and design balance. Publish beautiful multi-angle diagnostic plot suites in `figures/`, summary tables in `results/`, and write `reports/<run_id>__eda__<slug>__summary.md`. Never modify raw data.
5. **Clean Data**: Propose options in `plans/<timestamp>__cleaning__<slug>__options.md`. Obtain explicit user approval and save `plans/<timestamp>__cleaning__<slug>__approved.md`. Generate cleaned CSV/TSV in `data/` and row-level exclusion/correction audits in `results/`.
6. **Formulate Combined Analysis & Display Plan**: Save `plans/<timestamp>__analysis-plan__<slug>__models-and-displays.md` covering model formulas, contrasts, link functions, assumption checks, sensitivity models, and semantic figure/table specifications.
7. **Run Analysis & Multi-Angle Diagnostics**: Execute in clean R session (sourcing `renv/activate.R`). Generate model estimation and full diagnostic plot suites (residuals, Q-Q, leverage, BLUPs) to `figures/`, plot data and model tables to `results/`, write `logs/<run_id>__<stage>__<slug>__run.md`, and append a row to `logs/analysis-index.csv`.
8. **Manage Branching & Backtracking**: Mainline runs live on `main`. Exploratory tangents must use Git branches (`explore/<topic>`). If exploration fails, commit failing code, switch to `main`, and log `branch_abandon` / `retract` in index with `retracted_to_run_id`. When approved, merge to `main` and tag: `git tag -f accepted_<slug>_<run_id>`.
9. **Pause/Resume**: When pausing, write `logs/<timestamp>__checkpoint__<slug>__resume.md` and log `paused` in index. When resuming, read checkpoint, verify files, log `resumed`, and continue.
10. **Validate (Independent Cross-Language)**: When requested, create language-neutral brief in `validation/<timestamp>__brief__<slug>__<target>.md` (goal, input data, required output, algorithm). Implement independently in `validation/` (e.g. Python). Freeze outputs to `results/` before comparing. Save comparison table in `results/` and review in `validation/`.
11. **Finalize by User Decision**: Only when user explicitly declares completion: verify clean Git working tree, reproduce accepted workflow in clean session, compile final LaTeX report (`reports/<YYYY-MM-DD>_<HHMMSS>__final-report__<slug>__report.tex` on letter paper, 0.75in margins, sans-serif font) with file tracking comments, and log `finished` in index.

## Modular Reference Guides
For complete schemas, templates, and checklists, consult the bundled reference files:
- [01. Foundations, Git & Bash](references/01_foundations_git_and_bash.md): Project structure, Git branch discipline, renv, analysis index, and Bash standards.
- [02. Grilling & Intake](references/02_grilling_and_intake.md): Grilling interview protocols, decision frontiers, raw data registration, and variable dictionaries.
- [03. Forensics & Cleaning](references/03_forensics_and_cleaning.md): 30-item EDA forensics checklist, multi-angle diagnostic plotting suites, cleaning approvals, and TSV type safety.
- [04. Inference & Displays](references/04_inference_and_displays.md): Combined analysis and display plan, comprehensive model diagnostics (DHARMa, BLUPs, Cook's distance), modern figure aesthetics, patchwork composite layouts, and GLMMs.
- [05. Python Validation & Reports](references/05_python_validation_and_reports.md): Cross-language validation briefs, LaTeX preamble, sans-serif setup, and file tracking comments.

## Pitfalls
- **Never skip Grill Me**: Do not assume user preferences or jump straight into code on new, reanalysis, or subsequent analysis requests.
- **Never rely on single summary plots**: Always inspect distributions, raw observations, and model assumptions from multiple angles.
- Do not overwrite existing scripts, results, figures, or reports; use new timestamped IDs and Git commits.
- Do not embed hard-coded display numbers (like 'Figure 3') into filenames, plot titles, or captions; let LaTeX counters assign numbers.
- Do not rely on auto-detection for text/TSV data loading; explicitly specify column types and handle missing tokens like gene symbol 'NA' safely.
- Do not perform cross-language validation with identical default statistical settings without specifying contrast coding, ML/REML, and degrees of freedom methods.
- Do not leave uncommitted code or untracked exploratory branches when pausing or finalizing.

## Verification
1. Confirmed grilling summary document exists in `plans/` before any substantive data modification or modeling run.
2. All generated figures reside in `figures/` and exact plotted/table data reside in `results/` with matching semantic names and run IDs.
3. Multi-angle diagnostic plots and composite figures generated with descriptive titles, subtitles, units, and modern palettes.
4. `logs/analysis-index.csv` accurately records all runs, branches, Git commits, decisions, checkpoints, and acceptance events.
5. Cleaned TSV/CSV files parse deterministically with explicit column types matching variable dictionary.
6. Final LaTeX report compiles cleanly to PDF on letter paper with 0.75-inch margins, sans-serif typography, and accurate file tracking comments.