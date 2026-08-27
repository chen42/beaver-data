# 04. Statistical Inference, Modeling, and Display Planning

## Combined Analysis and Display Plan

Before fitting confirmatory models, create and freeze:
```text
plans/<timestamp>__analysis-plan__<analysis_slug>__models-and-displays.md
```

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
- **Palette**: Color-vision deficiency accessible (e.g. viridis, Okabe-Ito) and readable in grayscale.
- **Output Paths**: Figures saved in `figures/` as PDF/PNG; plotted data saved in `results/` as TSV/CSV.

---

## Assumption Evaluation and Diagnostics

Evaluate assumptions before and after fitting:
- **Independence**: Nested designs, batch effects, litter effects, repeated measures.
- **Linearity & Functional Form**: Residual vs fitted plots, polynomial/spline alternatives.
- **Homoscedasticity**: Scale-location plots, robust standard errors, weighted regression.
- **Error Normality**: Q-Q plots of model residuals (not raw data).
- **Overdispersion & Zero-Inflation**: Dispersion tests for count data (Poisson vs Negative Binomial / Conway-Maxwell-Poisson).
- **Singular Fits & Boundary Estimates**: Random-effects variance-covariance matrix checks in mixed models (`lme4::isSingular()`).
- **Influential Cases**: Cook's distance, leverage, DFBETAS.

### Documenting Deviations
If post-fit diagnostics require modifying the model:
1. Do not replace the primary model silently.
2. Commit the new code under a Git branch (`explore/<topic>`).
3. Retain both models and report sensitivity comparisons in `results/`.
4. Document the scientific reason in the run log.

---

## Execution Protocol

Run pipelines noninteractively:
```bash
Rscript --vanilla scripts/02_fit_models.R --run_id 20260826_093210
```
- Always source `renv/activate.R` explicitly at script entry.
- Generate outputs atomically with the active `run_id`.
- Figures go to `figures/`; model summaries, contrasts, and plot data go to `results/`.
- Write the run log (`logs/<run_id>__<stage>__<slug>__run.md`) and append to `logs/analysis-index.csv`.
