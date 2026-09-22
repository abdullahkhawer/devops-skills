---
name: go-linting
description: Lint and format Go code using golangci-lint, gofmt, and go vet according to standard conventions. Use when asked to lint, format, or check style issues in Go files.
---

# Go Linting Skill

Run this skill when asked to lint, format, or check style for Go code.

## Step 1 — Detect available tooling

```bash
which golangci-lint go 2>/dev/null
ls .golangci.yml .golangci.yaml 2>/dev/null
```

If `golangci-lint` is not installed, inform the user and ask before installing:

```bash
brew install golangci-lint
```

`gofmt` and `go vet` ship with the Go toolchain, so no separate install is needed for those.

---

## Step 2 — Run formatting check

```bash
gofmt -l .
```

This lists files that are not correctly formatted (empty output means everything is formatted). Show the diff for any listed file:

```bash
gofmt -d <file>
```

---

## Step 3 — Run vet (correctness checks)

```bash
go vet ./...
```

---

## Step 4 — Run the linter

```bash
golangci-lint run ./...
```

Respect the project's `.golangci.yml`/`.golangci.yaml` if present — do not pass extra flags that override its enabled/disabled linters.

Report all findings grouped by file, with line numbers and linter names.

---

## Step 5 — Apply fixes (only with user confirmation)

```bash
gofmt -w <file>
golangci-lint run --fix ./...
```

Only run auto-fix commands after showing the user what will change and getting explicit confirmation. `go vet` findings and most `golangci-lint` findings require manual code changes — present each with a suggested fix.

---

## Standard conventions enforced

- Formatting is always `gofmt`-compliant — this is non-negotiable in Go and never a matter of style preference.
- Package names: short, lowercase, no underscores.
- Exported identifiers (`PascalCase`) must have a doc comment starting with the identifier's name.
- Errors are returned, not panicked, except at true program-fatal boundaries; always check `if err != nil`.
- No unused imports or variables (enforced by the compiler itself, but `golangci-lint` catches shadowed/ignored errors too).

## Rules

- NEVER install `golangci-lint` without asking the user first.
- Always run in check mode first — only apply `--fix`/`-w` after the user confirms.
- Respect the project's existing `.golangci.yml` — do not suggest a different linter set without asking.
- Report the exact command run and its full output — do not summarize away findings.
- Flag any ignored error (`_ = someFunc()` where the error matters) or unchecked type assertion as a correctness finding, even if the linter configuration does not flag it.
