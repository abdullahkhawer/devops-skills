---
name: code-review
description: Analyze code changes between two local Git branches and perform a comprehensive code review. Use ONLY for local git branch comparisons. Do NOT use when a GitLab or GitHub MR/PR URL or MR ID is provided — use gitlab-cli or github-cli skill instead.
---

# Code Review Skill

Run this skill when asked to review code, compare branches, or perform a code review between a source and target branch using **local git**. Do NOT use this skill if the user provides a GitLab or GitHub MR/PR URL or MR ID — use the `gitlab-cli` or `github-cli` skill instead.

## Step 1 — Collect inputs

Ask the user the following two questions together if not already provided:

```
Kindly let me know:

1. Source Branch: What is the name of the source branch (the branch with new changes)?
2. Target Branch: What is the name of the target branch (usually main/master)?
```

If the user has already provided either value upfront, skip the corresponding question.

---

## Step 2 — Find changed files

```bash
git diff --name-only <TARGET_BRANCH> <SOURCE_BRANCH>
```

If the output is empty, report that there are no differences between the two branches and stop.

---

## Step 3 — Analyze each changed file

For each file returned in Step 2, read the full diff:

```bash
git diff <TARGET_BRANCH> <SOURCE_BRANCH> -- <FILE_PATH>
```

Perform a comprehensive code review across all changed files covering:

- **Security**: Secrets exposure, API keys, tokens, SQL injection, XSS, command injection, insecure dependencies.
- **Performance**: Inefficient algorithms, memory leaks, unnecessary computations, N+1 queries.
- **Code quality**: Code smells, duplicate code, overly complex functions, trailing whitespace, dead code.
- **Error handling**: Missing or incorrect error handling, unhandled exceptions, silent failures.
- **Logging**: Missing or excessive logging, sensitive data in logs.
- **Documentation**: Missing or outdated comments, unclear variable/function names.

---

## Step 4 — Report findings

For each issue found:

- State the **file path and line number(s)**.
- Show the **existing code snippet** that needs to change.
- Provide the **suggested fix or improvement**.

Only include code snippets that actually require changes — do not repeat code that looks fine.

If no issues are found, state clearly that the code looks good and summarize what was reviewed.

---

## Rules

- Always use `git diff <TARGET_BRANCH> <SOURCE_BRANCH>` — never checkout branches or modify the working tree.
- Report file paths relative to the repository root.
- Do not suggest changes for code outside the diff scope.
- If a branch name does not exist, report the error and ask the user to verify the branch names.
