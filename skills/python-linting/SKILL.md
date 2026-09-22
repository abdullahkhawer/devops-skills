---
name: python-linting
description: Lint and format Python code using ruff, flake8, black, and mypy according to standard conventions. Use when asked to lint, format, or check style/type issues in Python files.
---

# Python Linting Skill

Run this skill when asked to lint, format, check style, or check types for Python code.

## Step 1 — Detect available tooling

Check which linting/formatting tools are configured or installed, in order of preference:

```bash
# Config files (indicate which tool the project already uses)
ls pyproject.toml setup.cfg .flake8 mypy.ini 2>/dev/null

# Installed tools
which ruff black flake8 mypy isort 2>/dev/null
```

Prefer `ruff` if configured or installed — it replaces `flake8`, `isort`, and much of `pylint` with a single fast tool. Fall back to `flake8` + `black` + `isort` if `ruff` is not available. Do not install new tools without asking the user first.

---

## Step 2 — Run linting (read-only check)

```bash
# Preferred
ruff check .

# Fallback
flake8 .
```

Report all findings grouped by file, with line numbers and rule codes.

---

## Step 3 — Run formatting check

```bash
# Preferred
ruff format --check --diff .

# Fallback
black --check --diff .
isort --check-only --diff .
```

Show the proposed diff to the user before applying any formatting changes.

---

## Step 4 — Run type checking (if type hints are used)

```bash
mypy .
```

Skip this step if the project has no type hints and no `mypy` configuration.

---

## Step 5 — Apply fixes (only with user confirmation)

```bash
# Preferred
ruff check --fix .
ruff format .

# Fallback
black .
isort .
```

Only run auto-fix commands after showing the user what will change and getting explicit confirmation. Never auto-fix `mypy` type errors — these require manual code changes.

---

## Standard conventions enforced

- Line length: 88 (black/ruff default) or 79 (PEP 8 strict) — follow whatever the project's config specifies; do not override it.
- Import order: standard library, then third-party, then local — grouped and alphabetized (`isort`/`ruff` default).
- Naming: `snake_case` for functions/variables, `PascalCase` for classes, `UPPER_SNAKE_CASE` for constants (PEP 8).
- No unused imports or variables.
- No bare `except:` clauses — always catch specific exceptions.

## Rules

- NEVER install a new linter, formatter, or dependency without asking the user first.
- Always run in check/diff mode first — only apply auto-fixes after the user confirms.
- Respect the project's existing configuration (`pyproject.toml`, `setup.cfg`, `.flake8`) — do not suggest a different tool or ruleset than what is already configured.
- If no configuration exists, ask the user whether to add one or use tool defaults, rather than silently picking one.
- Report the exact command run and its full output — do not summarize away findings.
- If a finding requires a logic change (not just style), explain the risk before suggesting a fix.
