# 02. Requirements Grilling, Intake Records, and Data Dictionaries

## Requirements Grilling Protocol

Before touching code, cleaning data, or fitting models, pressure-test unresolved decisions across the design tree.

### Decision Dimensions
- **Scientific Goal**: Target estimand, scientific question, confirmatory vs exploratory claim.
- **Scope & Non-Goals**: Exclusions, boundaries, out-of-scope analyses.
- **Data & Structure**: Experimental unit, observational unit, identifiers, repeated measures, nesting.
- **Cleaning Authority**: Actions requiring explicit user decision vs automated rules.
- **Statistical Strategy**: Models, contrasts, multiplicity adjustment, sensitivity analyses.
- **Display Outputs**: Required figures, tables, accessibility needs, report structure.
- **Reproducibility & Constraints**: Privacy, software versions, LaTeX constraints.

### Interview Rules
1. Work through the active decision frontier (unresolved questions whose prerequisites are settled).
2. Ask tree-shaping questions first.
3. Maximum 5 independent questions per round. Ask 1 question at a time when decisions are tightly coupled.
4. For each question:
   - State why it affects the analysis.
   - Provide bounded, mutually exclusive options.
   - Provide the recommended option with scientific justification.
5. The agent researches project facts, code, and documentation; the user makes the decisions.
6. If an answer is unknown, propose a bounded exploratory check or pilot step.

### Grilling Summary Document
Save the confirmed interview summary as:
```text
plans/<timestamp>__decisions__<analysis_slug>__grilling-summary.md
```
Contains: decisions made, recommended workflow, assumptions/risks, rejected alternatives, open items, definition of done, and user confirmation.

---

## Raw Data Registration (Intake Gate)

Raw data files in `data/` are immutable. Never edit, open-and-save, or overwrite raw files.

### Registration Procedure
1. Copy raw file into `data/<timestamp>__raw__<source>.<ext>` or register external path.
2. Set read-only file permissions where the platform permits.
3. Assign a stable source row identifier if the source lacks one.
4. Create the Data Intake Record:
   ```text
   plans/<timestamp>__intake__<analysis_slug>__source-record.md
   ```
   Records: source name, acquisition date, local ingestion timestamp, owner contact, format, delimiter, encoding, sheet name, dimensions, units, and access restrictions.

### Variable Dictionary
Create an explicit, typed data dictionary alongside raw data:
```text
plans/<timestamp>__dictionary__<analysis_slug>__variables.tsv
```
Required columns:
- `variable_name`: Exact column name.
- `data_type`: Explicit class (`character`, `integer`, `numeric`, `factor`, `Date`, `POSIXct`).
- `description`: Biological/experimental meaning.
- `units`: Measurement scale or unit (e.g. `mg/kg`, `licks`, `seconds`).
- `allowed_values_or_range`: Valid ranges or factor levels.
- `missing_codes`: Recognized missing tokens (e.g. `""`, `"NA"`, `"-999"`).
- `role`: Role in analysis (`id`, `predictor`, `outcome`, `covariate`, `batch_factor`).
