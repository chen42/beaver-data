---
name: "beaver-data"
description: "Meticulous, reproducible data analysis pipeline using Bash orchestration, approved statistical engines and Linux analysis tools, independent cross-engine validation, and LaTeX reporting with mandatory stage grilling and decision tracking."
version: 7
created: "2026-08-26"
updated: "2026-09-22"
---
## When to Use
Use for end-to-end reproducible data analysis, statistical modeling, exploratory data analysis, data cleaning, or scientific reporting using Bash orchestration, an approved statistical engine or Linux analysis toolchain, independent cross-engine validation, and LaTeX reporting with strict stage gating and decision tracking.

## Mandatory Grilling Requirement
**Requirements Grilling is mandatory for EVERY analysis request.** This includes:
- New project or analysis setups.
- Reanalyses, model modifications, or parameter adjustments.
- Subsequent, follow-up, or secondary scientific questions.
- Retractions, branch pivots, or alternative exploratory attempts.

**Never start coding, modifying data, fitting models, or publishing results before completing a grilling interview round and securing explicit user confirmation.**

## Approved Analysis Engine and Toolchain
R is the default statistical engine, but it is not mandatory. During requirements grilling, ask whether the user requires or prefers a specific analysis tool, language, package, executable, container, proprietary application, laboratory workflow, or domain pipeline. The user may nominate tools not listed in this skill.

Permitted alternatives include:
- **Delimited data transformation**: Miller (`mlr`), `qsv`, `xsv`, or `csvkit`.
- **JSON processing**: `jq`.
- **Relational joins and aggregation**: DuckDB or SQLite.
- **Simple descriptive statistics**: `datamash`.
- **Validated TSV processing**: `awk`, `sort`, `join`, `cut`, and related GNU utilities.
- **General scientific computing**: Python with pinned packages, Julia, or GNU Octave.
- **Plotting**: `gnuplot`, Python plotting libraries, or another approved noninteractive engine.
- **Genetics and genomics**: PLINK 2, `bcftools`, `samtools`, or `bedtools`.
- **Neuroimaging**: AFNI, FSL, MRtrix3, or FreeSurfer.
- **Other tools**: Any user-specified or domain-specific tool approved during analysis planning when its algorithms match the scientific question.

The analysis plan must name each executable and assign it a specific role. For every tool, record its exact version or immutable container digest, installation source or environment, purpose, input and output schemas, missing-value behavior, statistical method, numerical options, factor coding, random seed, thread count, locale, precision settings, validation strategy, and known limitations.

Follow the user's selected tool when it can perform the approved analysis reproducibly. Do not silently replace it with R or another engine. If the requested tool is unavailable, unsuitable for the method, unable to produce required diagnostics, or insufficiently reproducible, explain the limitation, propose alternatives or supporting tools, and obtain a recorded user decision before proceeding. Do not install software, change environments, accept licenses, or transmit data to external services without user approval.

Bash may orchestrate the pipeline but must not implement inferential statistics or nontrivial floating-point algorithms through shell arithmetic. Use a tested statistical or domain-specific engine for those operations.

## Mandatory Diagnostic Visualization Standards
For both EDA and Modeling stages, **always generate multi-angle diagnostic visual suites**:
- **Multi-Angle Views**: Distributional geometry (raincloud, ECDF, strip), longitudinal subject spaghetti plots, multi-factor faceted grids, and missingness maps.
- **Model Diagnostics**: Residuals vs fitted with a smoother, Q-Q plots with confidence bands, scale-location, leverage/Cook's distance, random-effect BLUP caterpillar plots, and simulation-based residuals for count/GLMMs (`DHARMa` when R is used or a documented equivalent in another engine).
- **Aesthetic Excellence (Wilke / Scherer standards)**:
  - Clean themes: `theme_minimal(base_size = 12)` or `theme_light(base_size = 12)` with `plot.title.position = "plot"` and `panel.grid.minor = element_blank()`.
  - Descriptive titles and subtitles stating observation, context, and sample size ($N$). No hard-coded figure numbers.
  - Modern accessible color palettes: Okabe-Ito, `viridis`, `scico`, `MetBrewer`, `rcartocolor` (CVD-safe and grayscale readable).
  - Multi-panel composite figures assembled with `{patchwork}` or an equivalent approved tool, with semantic panel tags (`A`, `B`).

## Mandatory EDA Salient Findings Report
**An EDA Salient Findings Report is mandatory before proceeding to data cleaning or model fitting.**
Never advance directly from exploratory plotting to cleaning proposals or statistical modeling without presenting this report and alerting the user to:
- **Critical Data Issues**: Hardware glitches (sensor bounce, beam breaks, pump stalls), negative or boundary-violating values, duplicate IDs, missing baseline sessions, and systematic dropouts.
- **Salient Distributional Features**: Multimodality (sub-populations such as responder vs non-responder phenotypes), severe skewness, heavy tails, floor or ceiling bounds, and point masses at zero (zero-inflation).
- **Potential Assumption Violations**: Mean-variance dependency (heteroscedasticity), non-normality, non-linear trajectories over repeated measures, and hierarchical clustering (litter, operant chamber, cage, cohort, or experimenter effects).
- **Biological Patterns Deserving Exploration**: Subgroup bifurcation (escalation vs resistance), baseline imbalances across groups, decoupled behavioral measures (active vs inactive responses), and trajectory inflection points.
- **Diagnostic Visual Anomalies**: Erratic individual trajectories on spaghetti plots (loss of catheter patency, lever sticking), batch clustering on multivariate displays, and asymmetric attrition across treatment arms.
- **Actionable Decision Inquiries**: Concrete options for user approval regarding cleaning exclusions, distributional family choices, and random-effects structures.

Save the confirmed report to `reports/<run_id>__eda__<slug>__summary.md`. Present findings clearly to the user before generating cleaning proposals.

## Procedure
1. **Initialize Project and Approved Environment**: Create directories (`data/`, `scripts/`, `issues/`, `logs/`, `plans/`, `figures/`, `results/`, `reports/`, `validation/`; add `renv/` when R participates). Initialize local Git repo and `.gitignore`. Initialize `renv` for R or the appropriate pinned environment or version manifest for another toolchain. Commit baseline.
2. **Mandatory Grilling Interview**: Interview across decision tree (goals, estimands, exclusions, cleaning authority, statistical assumptions, displays, completion criteria). Mandatory for new analyses, reanalyses, and subsequent tasks. Save confirmed summary to `plans/<timestamp>__decisions__<slug>__grilling-summary.md`.
3. **Register Raw Data**: Place immutable raw data in `data/`. Create `plans/<timestamp>__intake__<slug>__source-record.md` and `plans/<timestamp>__dictionary__<slug>__variables.tsv`.
4. **Conduct Multi-Angle EDA & Deliver Findings Report**: Inspect raw distributions, missingness, outliers, artifacts, and design balance. Publish beautiful multi-angle diagnostic plot suites in `figures/`, summary tables in `results/`, and write mandatory salient findings report to `reports/<run_id>__eda__<slug>__summary.md`. Alert user to critical data issues, distribution properties, assumption risks, biological patterns, and diagnostic visual anomalies before proposing cleaning options. Never modify raw data.
5. **Clean Data**: Propose options in `plans/<timestamp>__cleaning__<slug>__options.md`. Obtain explicit user approval and save `plans/<timestamp>__cleaning__<slug>__approved.md`. Generate cleaned CSV/TSV in `data/` and row-level exclusion/correction audits in `results/`.
6. **Formulate Combined Analysis, Toolchain & Display Plan**: Save `plans/<timestamp>__analysis-plan__<slug>__models-and-displays.md` covering the primary analysis engine, each supporting tool, exact scripts or commands, model formulas, contrasts, link functions, assumption checks, sensitivity models, validation engine, and semantic figure/table specifications.
7. **Run Analysis & Multi-Angle Diagnostics**: Execute the approved toolchain in clean, noninteractive processes. R runs must activate `renv`; other engines must use their recorded environment or executable versions. Save substantive commands in stable semantic scripts rather than shell history. Generate model estimates and full diagnostic plot suites (residuals, Q-Q, leverage, BLUPs, or methodologically appropriate equivalents) to `figures/`, plot data and model tables to `results/`, write `logs/<run_id>__<stage>__<slug>__run.md`, and append a row to `logs/analysis-index.csv`.
8. **Manage Branching & Backtracking**: Mainline runs live on `main`. Exploratory tangents must use Git branches (`explore/<topic>`). If exploration fails, commit failing code, switch to `main`, and log `branch_abandon` / `retract` in index with `retracted_to_run_id`. When approved, merge to `main` and tag: `git tag -f accepted_<slug>_<run_id>`.
9. **Pause/Resume**: When pausing, write `logs/<timestamp>__checkpoint__<slug>__resume.md` and log `paused` in index. When resuming, read checkpoint, verify files, log `resumed`, and continue.
10. **Validate with an Independent Engine**: When requested, create a language-neutral and tool-neutral brief in `validation/<timestamp>__brief__<slug>__<target>.md` (goal, input data, required output, algorithm). Use an engine or independently implemented algorithm distinct from the primary analysis. Freeze both outputs before comparing. Save comparison table in `results/` and review in `validation/`.
11. **Finalize by User Decision**: Only when user explicitly declares completion: verify clean Git working tree, reproduce accepted workflow in clean session, compile final LaTeX report (`reports/<YYYY-MM-DD>_<HHMMSS>__final-report__<slug>__report.tex` on letter paper, 0.75in margins, sans-serif font) with file tracking comments, and log `finished` in index.

## Modular Reference Guides
For complete schemas, templates, and checklists, consult the bundled reference files:
- [01. Foundations, Git & Bash](references/01_foundations_git_and_bash.md): Project structure, Git branch discipline, analysis environments, analysis index, and Bash standards.
- [02. Grilling & Intake](references/02_grilling_and_intake.md): Grilling interview protocols, decision frontiers, raw data registration, and variable dictionaries.
- [03. Forensics & Cleaning](references/03_forensics_and_cleaning.md): 30-item EDA forensics checklist, multi-angle diagnostic plotting suites, cleaning approvals, and TSV type safety.
- [04. Inference & Displays](references/04_inference_and_displays.md): Analysis-engine selection, combined analysis and display plans, comprehensive model diagnostics, figure standards, and execution protocols.
- [05. Validation & Reports](references/05_validation_and_reports.md): Independent cross-engine validation briefs, LaTeX preamble, sans-serif setup, and file tracking comments.

## Pitfalls
- **Never skip Grill Me**: Do not assume user preferences or jump straight into code on new, reanalysis, or subsequent analysis requests.
- **Never skip the EDA findings report**: Do not jump straight from raw diagnostic plots into cleaning or modeling without documenting and alerting the user to distribution features, assumption risks, and data anomalies.
- **Never rely on single summary plots**: Always inspect distributions, raw observations, and model assumptions from multiple angles.
- Do not overwrite existing scripts, results, figures, or reports; use new timestamped IDs and Git commits.
- Do not embed hard-coded display numbers (like 'Figure 3') into filenames, plot titles, or captions; let LaTeX counters assign numbers.
- Do not rely on auto-detection for text/TSV data loading; explicitly specify column types and handle missing tokens like gene symbol 'NA' safely.
- Do not substitute analysis tools without user approval or describe a generic "Linux toolchain" without naming each executable and role.
- Do not treat two wrappers around the same underlying library as independent validation. Specify contrast coding, ML/REML, degrees of freedom methods, numerical tolerances, and other relevant defaults.
- Do not leave uncommitted code or untracked exploratory branches when pausing or finalizing.

## Verification
1. Confirmed grilling summary document exists in `plans/` before any substantive data modification or modeling run.
2. All generated figures reside in `figures/` and exact plotted/table data reside in `results/` with matching semantic names and run IDs.
3. Multi-angle diagnostic plots and composite figures generated with descriptive titles, subtitles, units, and modern palettes.
4. `logs/analysis-index.csv` accurately records all runs, branches, Git commits, decisions, checkpoints, and acceptance events.
5. Cleaned TSV/CSV files parse deterministically with explicit column types matching variable dictionary.
6. Analysis plan and run logs identify every executable, version, environment record, role, seed, thread count, locale, and relevant numerical setting.
7. Final LaTeX report compiles cleanly to PDF on letter paper with 0.75-inch margins, sans-serif typography, and accurate file tracking comments.
8. Mandatory EDA findings report exists in `reports/` and documents data issues, distribution properties, assumption risks, and diagnostic visual observations before cleaning plan creation.
