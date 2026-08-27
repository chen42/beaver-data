# beaver-data

Meticulous, reproducible data analysis skill and pipeline framework for AI coding agents (Pi, Claude, and Agent Skills standard harnesses).

`beaver-data` enforces strict stage gating, requirements interrogation ("Grill Me"), Bash orchestration, R statistical modeling, Python independent cross-language validation, and publication-ready LaTeX reporting.

## Why Beaver?

The beaver is nature's premier hydraulic architect. It methodically constructs elaborate networks of canals and ponds to control water flow, builds watertight dams that withstand tremendous pressure without leaking, and constantly listens for the faint acoustic trickle of escaping water to immediately inspect and patch structural weak points. Through relentless discipline and organized layering of materials, it transforms chaotic, turbulent wild streams into stable, navigable, and productive ecosystems. `beaver-data` embodies many of these traits.

## Structured Project Layout

Every `beaver-data` project follows a strict, flat directory structure:

```text
project/
├── data/                  # Immutable raw data and generated cleaned CSV/TSV datasets
├── scripts/               # Stable semantic scripts tracked in Git (01_clean.R, 02_fit.R, etc.)
├── issues/                # Documented issues, edge cases, and resolution records
├── logs/                  # Minimal execution run logs, checkpoints, and analysis-index.csv
├── plans/                 # Grilling summaries, intake records, dictionaries, and analysis plans
├── figures/               # Diagnostic and report figures (flat, modern palettes, no figure numbers)
├── results/               # Exact plotted data, model summaries, and LaTeX table fragments
├── reports/               # EDA summaries and compiled standalone LaTeX reports (.tex and .pdf)
├── validation/            # Firewalled cross-language briefs, alternate code (Python), and reviews
└── renv/                  # Isolated R package environment activation and lockfile
```

## Key Features

- **Mandatory Requirements Grilling**: Never assumes requirements. Stress-tests assumptions, estimands, exclusions, and cleaning rules across decision trees before touching code.
- **Fail-Proof Lineage Tracking**: Git tracks code versions and branch evolution; flat timestamped directories manage data, figures, and results.
- **Exploratory Branching & Retraction**: Dedicated tracking for experimental modeling tangents, failed explorations, and retreats back to baseline without losing provenance.
- **Forensic EDA & Multi-Angle Diagnostics**: 30-item forensic data checklist; modern visual standards (Okabe-Ito, viridis, scico, patchwork composites) for raw distributions and model assumptions.
- **Type-Safe Clean Data**: Plain-text CSV/TSV data with explicit column typing to eliminate `NA` symbol and date auto-conversion bugs.
- **Firewalled Cross-Language Validation**: Generates language-neutral handoff briefs for independent Python/Bash validation without leaking original code or numbers.
- **Dynamic LaTeX Reports**: Generates standalone reports on letter paper with 0.75-inch margins, modern sans-serif typography, dynamic LaTeX counter numbering, and machine-readable file tracking comments.

## Repository Structure

```text
beaver-data/
├── SKILL.md                              # Core operational agent playbook
├── README.md                             # Documentation & overview
├── LICENSE                               # MIT License
└── references/
    ├── 01_foundations_git_and_bash.md    # Project layout, Git branches, renv, index, Bash
    ├── 02_grilling_and_intake.md         # Decision tree, intake records, variable dictionary
    ├── 03_forensics_and_cleaning.md      # 30-item EDA checklist, plots, cleaning approvals, TSV types
    ├── 04_inference_and_displays.md      # Analysis & display plan, assumption checks, GLMMs
    └── 05_python_validation_and_reports.md # Python validation, LaTeX report, Arial setup, tracking
```

## Installation

### In Pi Coding Agent (`pi`)
Clone or download this repository into your Pi skills directory:
```bash
git clone https://github.com/chen42/beaver-data.git ~/.pi/agent/skills/beaver-data
```
Or add the repository path to `~/.pi/agent/settings.json`:
```json
{
  "skills": [
    "/path/to/beaver-data"
  ]
}
```

### In Claude Code
Add to `~/.claude/skills` or your project `.claude/skills`:
```bash
git clone https://github.com/chen42/beaver-data.git ~/.claude/skills/beaver-data
```

## Author

Created by Hao Chen ([@chen42](https://github.com/chen42)).

## License

MIT License. Copyright (c) 2026 Hao Chen.
