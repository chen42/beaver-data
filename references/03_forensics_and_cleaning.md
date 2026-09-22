# 03. Exploratory Data Forensics and Cleaning Protocols

## Exploratory Data Analysis (EDA Gate)

EDA inspects raw data before cleaning or filtering. Never alter raw data during EDA.

### Multi-Angle Diagnostic Plotting Suite
Always generate multi-angle diagnostic visual suites. Never rely on single aggregate plots (e.g. lone bar graphs).

Generate the following diagnostic suites:
1. **Distributional Geometry (Raincloud & Multi-view)**:
   - Combine half-density (`ggdist::stat_halfeye`), boxplots with outlier whiskers, and jittered raw observations (`geom_jitter` or `ggbeeswarm::geom_quasirandom`).
   - Empirical CDF plots (`stat_ecdf`) to detect distributional shifts and tail thickness.
2. **Longitudinal & Trajectory Tracking (Spaghetti Plots)**:
   - Individual animal/subject trajectories (`geom_line(aes(group = subject_id), alpha = 0.25)`) overlaid with group mean trend lines (`stat_summary(fun = mean, geom = "line", size = 1.2)`).
3. **Multi-Factor Faceting & Batch Structure**:
   - Faceted grids across experimental factors (e.g. `facet_grid(sex ~ treatment)` or `facet_wrap(~ strain, scales = "free_y")`).
   - Batch, cage, and session diagnostics to uncover acquisition artifacts or operator shifts.
4. **Missingness & Outlier Maps**:
   - Missingness heatmaps across time, subjects, and variables.
   - Multivariate outlier distance plots (e.g. Mahalanobis distance vs index).

### Aesthetic and Publication Standards for Diagnostic Figures
Apply the same scientific graphic design requirements regardless of plotting engine. The examples below use R, but an approved tool may provide equivalent output:
- **Themes**: `theme_minimal(base_size = 13)` or `theme_light(base_size = 13)` with:
  - `theme(plot.title.position = "plot", plot.caption.position = "plot")` (left-align titles to plot edge).
  - `theme(panel.grid.minor = element_blank(), panel.grid.major.x = element_blank())`.
  - Clean font hierarchy: bold title (`size = rel(1.2)`), informative subtitle (`size = rel(0.95)`, color = `"grey30"`), and clear axis titles with units.
- **Titles and Subtitles**:
  - Main Title: Descriptive statement of the observation (e.g. *Bimodal Distribution in Active Lick Counts*).
  - Subtitle: Methodological context and sample count (e.g. *Raw baseline session 1 across N=124 rats; points show individual animals*).
  - Never write fixed numbers (e.g. "Figure 1") in plot titles or subtitles.
- **Modern Accessible Color Palettes**:
  - Categorical (CVD-safe): Okabe-Ito (`c("#E69F00", "#56B4E9", "#009E73", "#F0E442", "#0072B2", "#D55E00", "#CC79A7", "#000000")`), `rcartocolor::scale_color_carto_d(palette = "Safe")`, `MetBrewer`, or `ggsci::scale_color_npg()`.
  - Continuous / Diverging: `viridis` (`magma`, `plasma`, `viridis`), `scico` (`scico::scale_color_scico(palette = "davos")`, `scico::scale_color_scico_d(palette = "hawaii")`).
  - Grayscale readable: Ensure luminance contrast separates groups even when printed in black and white.
- **Composite Panel Assembly**:
  - Combine multi-angle views into cohesive multi-panel figures using `{patchwork}` or an equivalent approved tool. R example:
    ```r
    library(patchwork)
    composite_p <- (p_distribution | p_trajectories) / (p_missingness | p_correlations) +
      plot_annotation(
        title = "Exploratory Forensics Overview: Oral Oxycodone Intake",
        subtitle = "Multi-angle inspection across strains, sessions, and missingness patterns",
        tag_levels = "A"
      )
    ```

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

Save all diagnostic plots in `figures/` and matching plotted data in `results/`.

---

## Mandatory EDA Salient Findings Report

Before formulating cleaning options or model plans, compile an EDA findings report and alert the user to all notable patterns:
```text
reports/<run_id>__eda__<analysis_slug>__summary.md
```

This report is a mandatory gate. It must document and alert the user to the following core dimensions:

### 1. Critical Data Issues Requiring Resolution
- **Acquisition Artifacts**: Sensor bounce, lickometer electrical noise, photobeam occlusions, negative timestamps, impossible session durations, and pump or telemetry dropouts.
- **Identifier Anomalies**: Duplicate animal or sample IDs, inconsistent sex or genotype designations across tables, non-sequential session numbering.
- **Domain Violations**: Negative counts where non-negative values are physically required, latencies below physiological limits, values exceeding apparatus capacities (such as syringe volume limits).
- **Missingness Structure**: Box-specific, chamber-specific, or operator-specific dropouts; missing baseline sessions; non-standard missing tokens (`999`, `-999`, `None`, empty strings).

### 2. Notable Distributional Features
- **Multimodality and Sub-Populations**: Bimodal or multimodal density profiles indicating distinct phenotypic responder subgroups (such as high- vs low-responders, resistant vs vulnerable animals).
- **Skewness and Kurtosis**: Severe positive or negative skewness, long tails, and extreme kurtosis requiring non-Gaussian distributional assumptions.
- **Zero Inflation**: Substantial point masses at zero (such as extinction sessions, inactive lever presses, abstinence periods), distinguishing structural zeros from sampling zeros.
- **Floor and Ceiling Effects**: Observations clustered at apparatus or protocol boundaries (such as maximum permitted infusions per session, fixed timeout cutoffs).

### 3. Potential Violations of Statistical Modeling Assumptions
- **Heteroscedasticity and Mean-Variance Scaling**: Variance increasing with the mean level across groups or sessions, indicating that linear models with constant error variance will yield biased inferences; generalized linear models (Poisson, Negative Binomial, Gamma) required.
- **Residual Non-Normality Risks**: Heavy-tailed or asymmetric distributions in candidate response variables indicating that Gaussian residual assumptions will fail without transformation or appropriate link functions.
- **Non-Linear Functional Forms**: Behavioral trajectories showing non-linear phases across repeated sessions (such as rapid acquisition, inflection points, asymptotic plateaus, within-session habituation).
- **Hierarchical Clustering and Non-Independence**: Obvious clustering by litter, operant chamber, cage, cohort, or experimenter requiring random effects or cluster-robust standard errors.

### 4. Biological Patterns Deserving Further Exploration
- **Phenotypic Divergence**: Groups separating over chronic exposure (such as progressive divergence of intake during extended access self-administration).
- **Baseline Imbalances**: Pre-existing differences across treatment groups or genotypes in baseline measures (such as initial body weight, locomotor activity, or baseline lick rate) that could confound treatment effects.
- **Decoupled Behavioral Endpoints**: Discrepancies between related variables (such as active lever presses increasing while delivered infusions remain static due to timeout limitations; dissociation between lick contact count and consumed volume).
- **Sex or Genotype Disparities**: Differences in variance, baseline level, or trajectory steepness across sexes or strains.

### 5. Unexpected Observations from Diagnostic Visualizations
- **Spaghetti Plot Trajectory Anomalies**: Individual animals exhibiting abrupt trajectory collapses (indicating catheter failure, illness, or headcap detachment) or sudden spikes (indicating lever malfunction).
- **Multivariate and Spatial Clusters**: Outlier clusters on PCA or Mahalanobis distance plots corresponding to unrecorded experimental batches or test rooms.
- **Asymmetric Attrition**: Disproportionate loss of subjects in specific doses, strains, or surgical groups (informative dropout and non-random censoring).

### 6. Actionable Inquiries for Cleaning and Modeling Plans
- Specific recommendations and decision questions presented to the user regarding exclusions, data corrections, choice of link functions, and random-effects structures before advancing to cleaning plan formulation.

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
- Do not write language-generated row names, indexes, or record counters as extra columns unless they are defined variables in the data dictionary.
- Accompany with machine-readable exclusion/correction audit tables in `results/`:
  - `results/<run_id>__cleaning__<slug>__exclusion-audit.tsv` (records every removed row ID and reason).
  - `results/<run_id>__cleaning__<slug>__correction-audit.tsv` (records old vs new values and row IDs).

### 4. Text and TSV Data Type Protection
- **Explicit Column Types**: Always pass explicit schemas (`readr::cols()` in R, `dtype={...}` in Python).
- **Missing Value Protection**: Explicitly declare `na = c("", "NA")`. If literal `"NA"` is a valid token (e.g. gene symbol), use `keep_default_na=False` in Python or handle `na.strings` explicitly in R.
- **Date Safety**: Explicitly parse date columns to prevent gene names (`MARCH1`, `SEPT2`) from turning into dates.
