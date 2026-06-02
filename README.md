# CRAPQuants Skill for Claude

A Claude skill that teaches Claude how to analyze Python code quality using [CRAPQuants](https://github.com/dockyardtechlabs/crapquants).

## What This Skill Does

When activated, Claude can:
- Run CRAP score analysis on any Python codebase
- Interpret diagnostic tags from 6 software engineering books
- Recommend specific named refactorings (Extract Method, Decompose Conditional, etc.)
- Set up baseline regression detection for CI/CD
- Generate reports in 5 formats (table, JSON, Markdown, SARIF, GitHub Actions)

## Repository Structure

```
crapquants-skill/         <- this repo (GitHub README here)
└── crapquants/           <- the skill folder — UPLOAD THIS
    ├── SKILL.md          <- skill instructions
    └── references/
        ├── scoring.md    <- CRAP formula, thresholds
        └── tags.md       <- 30+ diagnostic tags
```

**Important:** When installing, use the inner `crapquants/` folder — not the repo root. The skill folder is kept clean of README files per the Agent Skills standard.

## Installation

### Option A: Claude.ai (Upload)
1. Download this repo
2. Zip the inner `crapquants/` folder (the one containing SKILL.md)
3. Go to Claude.ai → Settings → Capabilities → Skills → Upload skill
4. Select the zipped `crapquants/` folder

### Option B: Claude Code (Personal — all your projects)
```bash
git clone https://github.com/dockyardtechlabs/crapquants-skill.git
cp -r crapquants-skill/crapquants ~/.claude/skills/crapquants
```

### Option C: Project-level (for your team)
```bash
# Inside your project repo
mkdir -p .claude/skills
git clone https://github.com/dockyardtechlabs/crapquants-skill.git /tmp/cq-skill
cp -r /tmp/cq-skill/crapquants .claude/skills/crapquants
git add .claude/skills/crapquants/
```

## Trigger Phrases

The skill activates when you say things like:
- "Check code quality of this project"
- "Analyze complexity of src/"
- "Which functions should I refactor first?"
- "Is this code safe to change?"
- "Run CRAP analysis"
- "Set up quality gates for CI"

## Prerequisites

CRAPQuants must be installed in the project being analyzed:
```bash
pip install --only-binary :all: radon pydantic structlog rich typer polars
pip install -e .  # from the CRAPQuants repo
```

## Related

- [CRAPQuants](https://github.com/dockyardtechlabs/crapquants) — The main tool
- [Agent Skills Open Standard](https://github.com/anthropics/skills) — Anthropic's skill specification

## License

MIT

---

*Built by [Dockyard Techlabs](https://github.com/dockyardtechlabs) — Dedicated as Seva to Lord Sri Krishna*

## Name & Branding

The skill **content** is MIT-licensed and freely reusable. However, **"CRAPQuants" as a project name and brand belongs to Dockyard Techlabs.** You may fork this skill and ship it under your own name, and you may state your work is "based on the CRAPQuants skill" — but you may not redistribute a fork as the official "CRAPQuants skill."

Canonical projects:
- **Skill:** https://github.com/dockyardtechlabs/crapquants-skill
- **Tool:** https://github.com/dockyardtechlabs/crapquants

## Provenance

This is the original CRAPQuants skill, authored by Tushar Ghorpade / Dockyard Techlabs. Note that per the Agent Skills standard, the inner `crapquants/` skill folder is kept clean of LICENSE/NOTICE/README files — those live at the repo root. The skill folder contains only `SKILL.md` and `references/`.
