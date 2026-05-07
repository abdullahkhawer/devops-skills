---
name: commit-code
description: Analyze code changes to prepare conventional commit message and commit to a new branch.
---

# Commit Code Skill

Run this skill when asked to commit code changes, create a commit message, or push changes to a new branch.

## Step 1 — Collect input

Ask the user the following question:

```
Kindly let me know:

1. Branch Name: What should I name the new branch? (name of the branch)
```

If the user has already provided the answer upfront, skip the question and use the provided value.

---

## Step 2 — Create a new branch

```bash
git checkout -b <BRANCH_NAME>
```

---

## Step 3 — Run pre-commit checks (if .pre-commit-config.yaml exists)

```bash
pre-commit run -a
```

If the command fails, try again only one more time. If it fails again, report the output to the user and stop. Do not continue until the user resolves any failures.

---

## Step 4 — Run Terraform formatting (if Terraform files exist)

```bash
terraform fmt -recursive
```

---

## Step 5 — Stage all changes, analyze them and commit

```bash
git add .
```

Before proceeding, list the staged files and check for potentially sensitive files (e.g. `.env`, `*.pem`, `*.key`, `*credentials*`, `*secret*`, `*token*`, `*password*`):

```bash
git diff --cached --name-only
```

If any staged file matches a sensitive pattern, pause and ask the user to confirm whether it should be included in the commit. Do not proceed until confirmed.

And then analyze the code changes in each staged file with the following command:

```bash
git diff --cached -- <FILE_PATH>
```

Then based on all the code changes, determine the appropriate conventional commit type, compose a single one-liner commit message, and commit the changes with the following command:

```bash
git commit -m "<TYPE>: <COMMIT_MESSAGE>"
```

### Commit message rules

- Single line only — no multi-line messages.
- Must be descriptive and mention all relevant changes.
- First letter of `<COMMIT_MESSAGE>` must be capitalized.
- Reference: https://www.conventionalcommits.org/en/v1.0.0/

### Conventional commit types

| Type | When to use |
|---|---|
| `feat` | A new feature |
| `fix` | A bug fix |
| `docs` | Documentation only changes |
| `style` | Formatting changes that do not affect code meaning |
| `refactor` | Code change that neither fixes a bug nor adds a feature |
| `perf` | Code change that improves performance |
| `test` | Adding or correcting tests |
| `chore` | Build process or auxiliary tool changes |
| `ci` | Changes to CI configuration files and scripts |
| `build` | Changes that affect the build system or external dependencies |

---

## Step 6 — Push the new branch

```bash
git push origin $(git rev-parse --abbrev-ref HEAD) --set-upstream
```

---

## Rules

- NEVER commit to `main` or `master` directly — always create a new branch.
- If `pre-commit run -a` fails, try again only one more time. If it fails again, stop and report the errors before proceeding.
- The commit message must be a single line — never use `\n` or multi-line `-m` flags.
- If the working directory has no staged or unstaged changes, report this and stop.
