---
name: terraform-linting
description: Lint and format Terraform code using tflint and terraform fmt/validate according to standard conventions. Use when asked to lint, format, or check style issues in Terraform files.
---

# Terraform Linting Skill

Run this skill when asked to lint, format, or check style for Terraform code.

## Step 1 — Detect available tooling

```bash
which terraform tflint 2>/dev/null
ls .tflint.hcl 2>/dev/null
```

If `tflint` is not installed, inform the user and ask before installing:

```bash
brew install tflint
```

`terraform fmt` and `terraform validate` ship with the Terraform CLI, so no separate install is needed for those.

---

## Step 2 — Run formatting check

```bash
terraform fmt -recursive -check -diff
```

Show the diff to the user before applying any formatting changes.

---

## Step 3 — Run validation

```bash
terraform validate
```

Requires `terraform init` to have been run in the directory first — if it fails with a missing-provider error, inform the user rather than running `init` yourself if it would touch a real backend.

---

## Step 4 — Run the linter

```bash
tflint --recursive
```

Respect the project's `.tflint.hcl` if present (enabled plugins/rules) — do not override it. If no `.tflint.hcl` exists, run with default rules and mention that a config file could be added.

Report all findings grouped by file, with line numbers and rule names (e.g. `terraform_deprecated_index`, `aws_instance_invalid_type`).

---

## Step 5 — Apply fixes (only with user confirmation)

```bash
terraform fmt -recursive
tflint --fix --recursive
```

Only run auto-fix commands after showing the user what will change and getting explicit confirmation. Most `tflint` findings (e.g. deprecated syntax, unused variables, invalid instance types) require manual review even when a `--fix` is available.

---

## Standard conventions enforced

- Formatting is always `terraform fmt`-compliant (2-space indentation, aligned `=` signs).
- Resource and variable names: `snake_case`, descriptive, no redundant type prefixes (e.g. `aws_instance.web`, not `aws_instance.aws_instance_web`).
- All variables have a `description` and, where sensible, a `type` constraint.
- No hardcoded credentials, account IDs, or ARNs — use variables or data sources.
- Provider versions are pinned via `required_providers` in a `versions.tf` or the root module.

## Rules

- NEVER install `tflint` without asking the user first.
- NEVER run `terraform init` against a real backend without asking the user first — offer to run `validate` only if the working directory is already initialized.
- Always run in check/diff mode first — only apply auto-fixes after the user confirms.
- Respect the project's existing `.tflint.hcl` — do not suggest a different ruleset without asking.
- Report the exact command run and its full output — do not summarize away findings.
- Flag any hardcoded secret-looking string (access keys, tokens, passwords) as a security finding regardless of whether `tflint` catches it.
