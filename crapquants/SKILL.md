---
name: crapquants
description: Analyzes Python code quality using CRAP scores and book-derived diagnostics. Use when the user asks to check code quality, analyze complexity, review code for maintainability, find risky functions, compute CRAP scores, assess test coverage gaps, detect security smells, or asks "is this code safe to change". Also use when user mentions "CRAP", "code quality", "complexity analysis", "refactoring priorities", "technical debt", "code health", or "risk assessment". Use for any Python project where understanding which functions are risky, undertested, or need refactoring would be valuable — even if the user doesn't explicitly ask for CRAP analysis.
metadata:
  author: Dockyard Techlabs
  version: 1.0.0
  category: code-quality
  tags: [python, crap, complexity, coverage, refactoring, static-analysis]
---

# CRAPQuants — Code Quality Analysis Skill

CRAPQuants computes CRAP (Change Risk Anti-Patterns) scores for Python code by combining cyclomatic complexity with test coverage, then enriches results with diagnostic tags from six software engineering books.

**CRAP formula:** `CC² × (1 − coverage/100)³ + CC`
**Threshold:** Score above 30 = CRAPpy (high risk to change)

## When to Use This Skill

- User asks to analyze code quality, complexity, or maintainability
- User wants to know which functions are risky or need refactoring
- User asks about test coverage gaps or technical debt
- Before a refactoring session — identify what to fix first
- During code review — flag risky functions in a PR
- User asks "is this safe to change?" or "what should I refactor first?"

## Prerequisites

Check if CRAPQuants is installed:

```bash
pip show radon 2>/dev/null && echo "radon: installed" || echo "radon: MISSING"
```

If not installed, set up:

```bash
python3 -m venv .venv && . .venv/bin/activate
pip install --only-binary :all: radon pydantic structlog rich typer polars
```

If user has CRAPQuants source, use it directly with `PYTHONPATH=src`.

## Analysis Workflow

### Step 1: Discover What to Analyze

Ask the user or determine from context:
- **Path:** Which directory or file? (default: `./src` or `./`)
- **Coverage:** Is there a coverage report? Check for `coverage.json`, `coverage.xml`, `.coverage`, `lcov.info`
- **Level:** How deep? (default: standard)

```bash
# Check for coverage data
ls coverage.json coverage.xml lcov.info .coverage 2>/dev/null
```

If no coverage report exists:
```bash
# Generate one (if pytest is available)
pytest --cov=src --cov-report=json -q 2>/dev/null
```

### Step 2: Run Analysis

```bash
# Standard analysis (CRAP + 6 framework tags)
PYTHONPATH=src python -m crapquants.cli analyze {path} --level standard

# With coverage data
PYTHONPATH=src python -m crapquants.cli analyze {path} -c coverage.json

# Quick mode (CRAP only, fastest)
PYTHONPATH=src python -m crapquants.cli analyze {path} --level quick

# JSON output for programmatic use
PYTHONPATH=src python -m crapquants.cli analyze {path} -f json -o report.json

# Markdown for documentation
PYTHONPATH=src python -m crapquants.cli analyze {path} -f markdown -o report.md
```

### Step 3: Interpret Results

After running, explain results to the user using these thresholds:

**CRAP Score:**
- 1–10: Clean, low risk
- 11–30: Moderate, manageable
- 31–60: CRAPpy, prioritize for refactoring or testing
- 61+: Critical, dangerous to change

**Diagnostic Tags** come from six books — always mention the source:
- `[Feathers]` — testability barriers (from "Working Effectively with Legacy Code")
- `[Ousterhout]` — design quality issues (from "A Philosophy of Software Design")
- `[Hunt & Thomas]` — engineering culture (from "The Pragmatic Programmer")
- `[Fowler]` — code smells with named refactorings (from "Refactoring")
- `[Tornhill]` — behavioral risk (from "Your Code as a Crime Scene")
- `[Ford]` — architectural fitness functions (from "Building Evolutionary Architectures")

### Step 4: Recommend Actions

For each CRAPpy function, provide actionable guidance:

1. **If high CC + low coverage:** "Write characterization tests first, then refactor" (Feathers approach)
2. **If high CC + high coverage:** "Safe to refactor — coverage is your safety net" (Hunt & Thomas)
3. **If MONSTER_SNARLED tag:** "Use Replace Method with Method Object to untangle state"
4. **If EDIT_AND_PRAY tag:** "Write characterization tests documenting actual behavior before any change"
5. **If NONOBVIOUS tag:** "Extract Method to name code sections by intent, add type annotations"

Always prioritize: CRITICAL tags first, then HIGH, then WARNING.

## Baseline Regression Workflow

For CI/CD or ongoing tracking:

```bash
# Save baseline (do this on main branch)
PYTHONPATH=src python -m crapquants.cli analyze {path} -c coverage.json --save-baseline data/baseline.json

# Compare against baseline (do this on PR branch)
PYTHONPATH=src python -m crapquants.cli analyze {path} -c coverage.json --baseline data/baseline.json
```

Exit code 1 = regression detected. Use in CI to block merging CRAPpy code.

## Security Scanning

CRAPQuants includes AST-based security smell detection (always on):
- eval/exec usage (CWE-95)
- pickle deserialization (CWE-502)
- Hardcoded secrets (CWE-798)
- Shell injection (CWE-78)
- Unsafe YAML loading (CWE-502)

For comprehensive SAST, ensure Semgrep is installed and use `--level full`.

## Common Scenarios

**"How risky is this function?"**
→ Run quick analysis on the specific file, report CRAP score and what it means.

**"What should I refactor first?"**
→ Run standard analysis on the project, sort by CRAP score descending, recommend top 3 with specific refactoring techniques from the Fowler tag mappings.

**"Is this codebase healthy?"**
→ Run standard analysis, report PHS (Pragmatic Health Score 0-100), highlight broken windows and coincidence code.

**"Help me prepare for a code review"**
→ Run analysis with markdown output, generate report the reviewer can read. Include the glossary section so reviewers understand the metrics.

**"Set up quality gates for CI"**
→ Generate baseline, provide GitHub Actions workflow YAML with SHA-pinned actions (Rule #51), configure SARIF output for GitHub Code Scanning.

## Important Notes

- CRAPQuants is pure static analysis — no AI, no network calls, works offline
- Coverage data dramatically improves accuracy — always try to include it
- Without coverage, all functions are assumed 0% covered (pessimistic mode)
- Use `--missing-policy optimistic` during local dev if no coverage available
- Reports include a "How to Read This Report" glossary — remind users it's there

For detailed framework documentation, see `references/` directory.
