---
name: bash-linting
description: Lint and format Bash/shell scripts using shellcheck and shfmt according to standard conventions. Use when asked to lint, format, or check style issues in shell scripts.
---

# Bash Linting Skill

Run this skill when asked to lint, format, or check style for Bash/shell scripts.

## Step 1 — Detect available tooling

```bash
which shellcheck shfmt 2>/dev/null
```

If neither tool is installed, inform the user and ask before installing:

```bash
brew install shellcheck shfmt
```

---

## Step 2 — Find shell scripts

```bash
find . -type f \( -name "*.sh" -o -name "*.bash" \) -not -path "*/node_modules/*" -not -path "*/.git/*"
```

Also lint any file with a `#!/usr/bin/env bash` or `#!/bin/bash` shebang but no `.sh` extension:

```bash
grep -rl '^#!.*\(bash\|sh\)' . --include="*" -l 2>/dev/null | grep -v -E '\.(sh|bash)$'
```

---

## Step 3 — Run linting (read-only check)

```bash
shellcheck <file>
```

Report all findings grouped by file, with line numbers and SC rule codes (e.g. `SC2086`).

---

## Step 4 — Run formatting check

```bash
shfmt -d -i 2 -ci <file>
```

Show the proposed diff to the user before applying any formatting changes. `-i 2` sets 2-space indentation and `-ci` indents switch-case bodies — adjust to match the project's existing style if it differs.

---

## Step 5 — Apply formatting (only with user confirmation)

```bash
shfmt -w -i 2 -ci <file>
```

`shellcheck` findings almost always require a manual code change (not a safe auto-fix) — present each finding with its suggested fix and apply only after user confirmation.

---

## Standard conventions enforced

- Always quote variable expansions: `"$var"`, not `$var` (SC2086).
- Use `[[ ... ]]` over `[ ... ]` for conditionals in Bash scripts.
- Use `set -euo pipefail` at the top of scripts to fail fast on errors, unset variables, and pipeline failures.
- Use `$(...)` command substitution, not backticks.
- Declare function-local variables with `local`.
- Indentation: 2 spaces, no tabs (unless the project's `.editorconfig` specifies otherwise).

## Rules

- NEVER install `shellcheck`/`shfmt` without asking the user first.
- Always run in check/diff mode first — only apply auto-fixes after the user confirms.
- Respect the project's existing `.shellcheckrc` or `.editorconfig` if present — do not override its settings.
- Report the exact command run and its full output — do not summarize away findings.
- Flag any `eval`, unquoted glob expansion, or command injection risk (e.g. unsanitized input passed to a shell command) as a security finding, even if `shellcheck` does not flag it directly.
