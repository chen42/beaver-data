# 04. Statistical Inference, Modeling, and Display Planning

## Combined Analysis and Display Plan

Before fitting confirmatory models, create and freeze:
```text
plans/<timestamp>__analysis-plan__<analysis_slug>__models-and-displays.md
```

### Analysis Engine and Toolchain Section
R is the default statistical engine, but it is not mandatory. Ask whether the user requires or prefers a particular analysis tool, language, package, executable, container, proprietary application, or established laboratory workflow. User-specified tools may fall outside the examples below.

Selection priority:
1. User-required tool when scientifically and operationally suitable.
2. User-approved domain-specific tool.
3. R when no tool is specified.
4. Supporting engine for diagnostics, plotting, or reporting when the primary tool lacks required functions.

Suitable tools include:

| Task | Suitable tools | Restrictions |
|---|---|---|
| CSV/TSV transformation | Miller (`mlr`), `qsv`, `xsv`, `csvkit` | Explicit types, delimiters, and missing tokens required |
| Simple TSV extraction | `awk`, `cut`, `sort`, `join` | Do not use `awk` for quoted CSV or embedded newlines |
| JSON extraction | `jq` | Preserve types and document missing fields |
| Joins and aggregation | DuckDB, SQLite | Record SQL, types, null handling, collation, and ordering |
| Descriptive summaries | `datamash`, DuckDB | Verify grouping, precision, and missing-value behavior |
| Statistical modeling | R, Python, Julia, GNU Octave, approved domain tool | Selected method and required diagnostics must be supported |
| Genetic association | PLINK 2 | Record genome build, allele coding, covariates, filters, and test |
| Sequence or variant processing | `bcftools`, `samtools`, `bedtools` | Record reference build and coordinate convention |
| Neuroimaging | AFNI, FSL, MRtrix3, FreeSurfer | Record software version, coordinate space, preprocessing, and contrasts |
| Plot generation | R, Python, `gnuplot`, approved plotting tool | Existing diagnostic and accessibility standards still apply |

For every selected tool, record:
- Exact executable and version or immutable container digest.
- Installation source, environment, and lockfile or version manifest.
- Scientific or data-processing purpose.
- Input and output paths, schemas, units, and coordinate conventions.
- Missing-value, delimiter, factor-coding, and reference-level behavior.
- Statistical method, optimizer, convergence rules, and numerical options.
- Random seed, RNG method, thread count, and parallelization settings.
- Locale, time zone, sorting, collation, and numerical precision.
- Required diagnostics, validation strategy, and known limitations.

Follow a user-selected tool when it can perform the approved analysis reproducibly. Do not silently substitute R or another engine. If the selected tool is unavailable, methodologically unsuitable, unable to produce required diagnostics, or insufficiently reproducible, explain the specific limitation and ask the user whether to retain, supplement, or replace it. Record the decision in the grilling summary and analysis plan. Do not install software, change environments, accept licenses, or transmit data to external services without user approval.

### Statistical Modeling Section
- **Status**: Confirmatory vs exploratory.
- **Estimand**: Population estimand, primary outcome, secondary outcomes.
- **Model Specification**: Full formula, link function, family (Gaussian, Poisson, Negative Binomial, Binomial).
- **Contrasts & Reference Levels**: Explicit contrast matrix (e.g. treatment contrasts with baseline group vs sum/deviation contrasts).
- **Design Structure**: Fixed effects, random effects, repeated measures, clustering (litter, cage, batch).
- **Estimation Method**: OLS, ML, REML, IRLS; optimizer and convergence tolerance.
- **Degrees of Freedom**: Satterthwaite, Kenward-Roger, or asymptotic Wald approximations.
- **Multiplicity Adjustment**: Family-wise error rate or false discovery rate strategy (Holm, Tukey, Benjamini-Hochberg).
- **Sensitivity Analyses**: Robust regressions, alternative distributions, influential observation trimming.
- **Reproducibility**: Random seed and parallel generator method.

### Display Plan Section (Figures & Tables)
For each planned figure and table:
- **Semantic Name**: Descriptive kebab-case (e.g. `oxycodone-intake-by-sex-and-access`). No fixed numbers in filenames or titles.
- **Scientific Purpose**: Target hypothesis or diagnostic check.
- **Input Data**: Exact cleaned file and model result used.
- **Grouping & Aesthetics**: X-axis, Y-axis, fill/color, faceting variables.
- **Raw Points**: Raw observations overlaid on summaries whenever possible.
- **Palette**: Modern accessible palette (e.g. Okabe-Ito, viridis, MetBrewer, rcartocolor).
- **Output Paths**: Figures saved in `figures/` as PDF/PNG; plotted data saved in `results/` as TSV/CSV.

---

## Comprehensive Model Diagnostic Visual Suite

Every statistical model must be diagnosed from multiple angles. Never rely on p-values alone.

### Multi-Angle Diagnostic Plots
Generate and combine model diagnostics using the approved statistical and plotting engines. Tool selection cannot weaken diagnostic requirements:
1. **Residuals vs Fitted Values**: Check for non-linearity, heteroscedasticity, and mean-variance relationships. Include loess smoother line.
2. **Q-Q Residual Normality Plot**: Quantile-quantile plot with 95% confidence bands (`car::qqPlot` or `ggplot2::stat_qq_line`).
3. **Scale-Location / Spread-Location**: Square root of absolute standardized residuals vs fitted values to detect changing variance.
4. **Residuals vs Leverage / Cook's Distance**: Detect highly influential subjects or observations (`cooks.distance(model)`).
5. **Random Effects Diagnostics (GLMMs)**: Caterpillar plots or Q-Q plots of Best Linear Unbiased Predictors (BLUPs) / conditional modes to check normality and shrinkage across subjects/strains.
6. **Observed vs Predicted / Calibration**: Scatter of observed vs model-predicted values with identity line ($y=x$) to evaluate calibration.
7. **Simulated Residuals (Count / GLMMs)**: Use `DHARMa::simulateResiduals()` when R is used. For another engine, use a methodologically equivalent simulation-based residual procedure and record its algorithm, simulation count, seed, and interpretation.

If the primary engine cannot generate a required diagnostic, use a separate approved diagnostic engine without changing primary model estimates. Save the exported fitted values, residuals, influence statistics, or simulation inputs used by that engine in `results/`.

---

## High-Quality Figure Aesthetic Guidelines

Follow modern scientific visualization best practices (Claus Wilke *Fundamentals of Data Visualization* / Cédric Scherer standards):

### 1. Structure & Layout
- **Aspect Ratio & White Space**: Avoid distorted squashed plots; set balanced dimensions (e.g. 6x4 in for single panels, 10x7 in for composite figures).
- **Multi-Panel Composition**: Use `{patchwork}`, `{cowplot}`, Python composition tools, `gnuplot`, or another approved engine to group related sub-plots with aligned axes and shared legends. R example:
  ```r
  library(patchwork)
  fig_composite <- (p_main + p_effects) / (p_diagnostics_res + p_diagnostics_qq) +
    plot_layout(heights = c(1.2, 1), guides = "collect") +
    plot_annotation(
      title = "Escalation of Oral Oxycodone Self-Administration Across Rat Strains",
      subtitle = "Mixed-effects model estimates (top) and residual diagnostics (bottom)",
      tag_levels = "A"
    ) &
    theme_light(base_size = 12) &
    theme(
      plot.title.position = "plot",
      plot.caption.position = "plot",
      panel.grid.minor = element_blank(),
      legend.position = "top"
    )
  ```

### 2. Typography & Labeling
- **Hierarchical Titles**:
  - Title: Clear scientific conclusion or key pattern.
  - Subtitle: Method, sample size ($N$), and statistical annotation details.
  - Axis Labels: Always state variable name and physical units (e.g. `Oxycodone Intake (mg/kg / 2 h)`).
- **Direct Labeling**: Prefer direct text labels (`ggrepel::geom_text_repel()`) over crowded legends when highlighting specific strains, outlier rats, or treatment groups.

### 3. Color & Accessible Palettes
- **Categorical Data**:
  - Okabe-Ito Palette: `c("#E69F00", "#56B4E9", "#009E73", "#F0E442", "#0072B2", "#D55E00", "#CC79A7")`.
  - `rcartocolor::scale_color_carto_d(palette = "Safe")` or `MetBrewer::scale_color_met_d("Hiroshige")`.
- **Continuous & Ordered Data**:
  - `viridis` (`magma`, `plasma`, `viridis`), `scico` (`scico::scale_fill_scico(palette = "batlow")`).
- **Grayscale Test**: Ensure high luminance contrast so figures remain legible when photocopied or printed in black and white.

---

## Execution Protocol

Run every pipeline noninteractively with stable semantic scripts. Examples:
```bash
Rscript --vanilla scripts/fit_models.R --run_id 20260826_093210
python scripts/fit_models.py --run-id 20260826_093210
julia --project=. scripts/fit_models.jl --run-id 20260826_093210
octave --quiet scripts/fit_models.m 20260826_093210
LC_ALL=C mlr --tsv stats1 -a count,mean,sd -f outcome -g treatment data/input.tsv
duckdb -readonly data/analysis.duckdb < scripts/summarize_outcomes.sql
gnuplot scripts/plot_diagnostics.gp
plink2 --pfile data/genotypes --glm ...
```

- R scripts must explicitly source `renv/activate.R`; other engines must use their approved environment or recorded executable versions.
- Save substantive shell commands in `.sh`, SQL in `.sql`, `awk` programs in `.awk`, and plotting instructions in suitable tracked scripts. Shell history is not an analysis record.
- Bash may orchestrate tools and validate files, but it must not implement inferential statistics or nontrivial floating-point algorithms through shell arithmetic.
- Generate outputs atomically with the active `run_id`.
- Figures go to `figures/`; model summaries, contrasts, diagnostics, and plot data go to `results/`.
- Record the primary engine, complete toolchain, environment records, locale, thread count, numerical precision, random seed, and exact command in `logs/<run_id>__<stage>__<slug>__run.md`.
- Append the run to `logs/analysis-index.csv`.
