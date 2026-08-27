# 01. Foundations, Git Lineage, and Bash Orchestration

## Project Structure

```text
project/
├── data/                  # Immutable raw data and generated cleaned data files (flat)
├── scripts/               # Stable semantic scripts tracked in Git
├── issues/                # Documented issues and resolution records
├── logs/                  # Minimal run logs, checkpoints, and analysis-index.csv
├── plans/                 # Grilling summaries, intake records, dictionaries, and plans
├── figures/               # All generated diagnostic and report plots (flat, no display numbers)
├── results/               # Plotted data CSVs, model summaries, table fragments (flat)
├── reports/               # EDA reports and compiled LaTeX reports (.tex and .pdf)
├── validation/            # Cross-language validation briefs, code, and reviews
└── renv/                  # Approved R environment infrastructure only
```

Root-level control files allowed: `README.md`, `.gitignore`, `.here`, R project file, and `renv.lock`.

## File Naming and Local Timestamps

Use compact, sortable local timestamps: `YYYYMMDD_HHMMSS` (e.g. `20260826_093210`).
- This timestamp serves as the `run_id` for each execution.
- If multiple runs start in the same second, append `_01`, `_02`.
- Script files use stable semantic names without timestamps (e.g. `scripts/clean_data.R`).
- Final reports use readable date-time: `reports/<YYYY-MM-DD>_<HHMMSS>__final-report__<slug>__report.<tex|pdf>`.

## Git Tracking, Branches, and Exploratory Lineage

Git tracks code, plans, issues, and environment definitions (`renv.lock`).
- **.gitignore**: Must ignore `data/`, `figures/`, `results/`, `logs/`, `reports/`, `renv/library/`, `validation/.venv/`, `.Rhistory`, `.RData`, temporary files, and secrets.
- **Mainline vs Exploration**:
  - Mainline accepted work lives on `main`.
  - Non-trivial exploratory tangents (alternative models, transforms, outlier bounds) MUST use a Git branch (e.g. `explore/oxycodone-negbin`).
- **Commits Required**: Code, plans, and `renv.lock` must be committed before executing substantive runs. Record the commit hash in the run log.
- **Abandoning Branches**: If exploration fails (non-convergence, singular fit, rejected by user):
  1. Commit failing code with descriptive message (e.g. `explore: failed convergence for negbin`).
  2. Switch back to baseline: `git checkout main`.
  3. Log `branch_abandon` or `retract` in `logs/analysis-index.csv` with `retracted_to_run_id`.
- **Accepting Branches**: When user accepts a branch, merge to `main` and tag: `git tag -f accepted_<slug>_<run_id>`.

## Minimal Run Tracking

Each execution writes one Markdown log: `logs/<run_id>__<stage>__<slug>__run.md`.

```markdown
# Run record
Run ID: 20260826_093210
Local time: 2026-08-26 09:32:10
Time zone: America/Chicago (-05:00)
Analysis slug: oxycodone-escalation
Stage: analysis
Status: completed
Branch: main
Parent run IDs: 20260826_090000
Forked from run ID: N/A
Retracted to run ID: N/A
Git commit: 8f3c2a1
Git tag: accepted_oxycodone-escalation_20260826_093210
Purpose: Fit primary generalized linear mixed model
Decision record: plans/20260826_083000__decisions__oxycodone-escalation__grilling-summary.md
Analysis plan: plans/20260826_091000__analysis-plan__oxycodone-escalation__models-and-displays.md
Input data: data/20260826_090000__cleaned__oxycodone-escalation__primary.tsv
Scripts: scripts/02_fit_models.R
Command: Rscript --vanilla scripts/02_fit_models.R --run_id 20260826_093210
R version: 4.4.1
Random seed: 20260826
Outputs: results/20260826_093210__analysis__oxycodone-escalation__model-estimates.tsv | figures/20260826_093210__analysis__oxycodone-escalation__residuals.pdf
Validation: N/A
Warnings or errors: None. Model converged cleanly.
Next step: Generate report-ready figures and tables.
```

### Analysis Index (`logs/analysis-index.csv`)
Append-only CSV tracking every run and lifecycle event:
`timestamp,timezone,event,analysis_slug,branch,run_id,stage,status,parent_run_ids,git_commit,git_tag,decision_record,checkpoint,script,input_data,outputs,report,notes`

Events: `run`, `branch_fork`, `branch_abandon`, `retract`, `paused`, `resumed`, `accepted`, `finished`, `superseded`.

## Pause and Resume Checkpoints

When pausing an analysis, write: `logs/<timestamp>__checkpoint__<slug>__resume.md` and log `paused` in index.
To resume:
1. Inspect newest index event. If `paused`, read checkpoint, decision record, plan, and last run log.
2. Confirm files exist.
3. Summarize status to user.
4. Log `resumed` event in index and continue with new timestamped files.

## Bash Scripting Standards

Bash scripts (`.sh`) may orchestrate pipelines or perform file validation:
- Must begin with `#!/usr/bin/env bash` and use `set -euo pipefail`.
- Quote all variables and paths; use arrays for argument lists.
- Check required commands before execution.
- Use `mktemp` and `trap` for cleanup of temporary resources.
- Pass computation to R or Python when statistical methods or floating-point math are needed.

## renv Dependency Management

- `renv` records exact package versions in `renv.lock`.
- Keep package cache outside project to avoid duplicating binaries.
- Always explicitly source `renv/activate.R` when invoking `Rscript --vanilla`.
- Record exact R version for every run. Treat R upgrades as new environments and new runs.
