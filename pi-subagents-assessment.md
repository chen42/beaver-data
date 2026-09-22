# Pi-subagents Assessment for Beaver-data

## Conclusion

Beaver-data can benefit from selective subagent use. Best uses are independent review and cross-engine validation, not parallel analysis writers.

## Recommended Uses by Stage

| Beaver-data stage | Subagent use |
|---|---|
| Grilling | Parent only. User decisions remain with parent. |
| EDA execution | One writer only. |
| EDA findings report | Two fresh, read-only reviewers: one for data integrity and visual anomalies, one for statistical assumptions. Parent synthesizes findings. |
| Cleaning plan | Reviewer or oracle challenges proposed exclusions. Parent asks user and records decisions. |
| Analysis plan | Fresh reviewer checks estimand, model, contrasts, diagnostics, and displays. |
| Modeling | One writer, followed by fresh diagnostic review. |
| Python validation | Fresh validator receives only language-neutral brief and designated inputs, not R implementation or results. |
| Final report | Read-only traceability reviewer checks claims against plans, outputs, and logs. |

## Constraints

- Keep one writer per working directory.
- Do not run parallel workers against shared `data/`, `results/`, `figures/`, or `logs/analysis-index.csv` paths.
- Do not assume `worktree: true` will provide analysis isolation. Ignored data and generated outputs may be absent from managed worktrees.
- Parent retains scientific decisions, user communication, finding disposition, and final acceptance.
- Use fresh context for independent reviewers to reduce bias from prior analysis discussion.
- Treat child reports as evidence, not authority.
- Keep Pi integration optional. Core skill claims compatibility across models and agent platforms, so mandatory `pi-subagents` syntax would reduce portability.

## Scientific Review Benefits

Fresh statistical review could identify overstatements in `references/03_forensics_and_cleaning.md`:

- Multimodality does not establish responder subpopulations. It may also reflect batches, conditioning variables, measurement artifacts, or sampling variation.
- Heteroscedasticity does not necessarily bias coefficient estimates or require a generalized linear model. Consequences depend on model specification and inferential method.
- Heavy-tailed raw outcomes do not establish residual non-normality. Residual behavior must be evaluated after fitting an appropriate model.
- Abrupt trajectory changes may be consistent with equipment or health problems, but diagnostic plots alone do not identify their cause.

These statements should use measured language and require evaluation of alternative explanations.

## Proposed Skill Integration

1. Add a generic `Optional Independent-Agent Review` section to `SKILL.md`.
2. Add `references/06_optional_agent_orchestration.md`.
3. Document Pi-specific recipes in that reference:
   - Fresh parallel EDA review
   - Analysis-plan review
   - Fresh Python validation
   - Final traceability audit
4. Initially use built-in `reviewer`, `worker`, and `oracle` roles.
5. Add custom `eda-auditor`, `python-validator`, and `traceability-auditor` agents only if repeated use justifies maintaining them.

## Suggested Delegation Boundaries

### EDA Review

Run EDA with one analysis owner. After outputs are frozen, launch independent read-only reviews with distinct scopes:

1. Data integrity, missingness, acquisition artifacts, and unexpected diagnostic-plot features.
2. Distributional structure, dependence, design balance, and risks to planned statistical assumptions.

Parent verifies each finding against source data and plots, then writes and presents the mandatory EDA findings report.

### Cleaning Review

Subagent may challenge whether proposed exclusions are supported and whether alternatives or sensitivity analyses are needed. It must not approve exclusions or modify data. User retains cleaning authority.

### Analysis-Plan Review

Fresh reviewer checks whether estimand, model family, link function, repeated-measures structure, contrasts, diagnostics, and displays match study design. Oracle use should be reserved for material unresolved design choices.

### Cross-Engine Validation

This stage offers strongest subagent benefit. Launch validator with fresh context and provide only:

- Language-neutral validation brief
- Approved input dataset
- Required output schema
- Algorithm specification
- Numerical tolerance and comparison rules

Do not provide original R code or expected numerical results before independent output is frozen. Filesystem separation or sandboxing is needed if strict implementation isolation is required because fresh conversational context alone does not restrict file access.

### Final Traceability Audit

Read-only reviewer checks that:

- Report claims match recorded results.
- Figures and tables map to exact files.
- Cleaning and exclusion decisions have approval records.
- Diagnostic limitations and unresolved risks are reported.
- Validation results are described accurately.

Parent retains publication authority.

## Current Pi State

`pi-subagents` version `0.70.1` exists at:

```text
~/.pi/agent/npm/node_modules/pi-subagents
```

It is not listed in `~/.pi/agent/settings.json`, and current session does not expose a `subagent` tool. Package appears present but inactive.

Activation command:

```bash
pi install npm:pi-subagents
```

Run command outside current read-only harness.
