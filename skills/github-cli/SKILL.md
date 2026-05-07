---
name: github-cli
description: Perform GitHub operations on GitHub using the gh CLI. Use for issues, pull requests, pipelines, repositories, and project management. ALSO use this skill when asked to review a GitHub PR — when a GitHub PR URL or PR ID is provided, use gh to fetch and review the diff instead of the code-review skill.
---

# GitHub CLI Skill

Use the instance and rules below when constructing all `gh` commands.

## Instance

All `gh` CLI and `git` operations use host: `github.com`

### Prerequisites (one-time setup by user)

If not already authenticated, inform the user that authentication is required and ask them to complete the following steps manually. Do not perform these steps yourself.

1. Install the `gh` CLI if not already installed:

```bash
brew install gh
```

2. Run the following command to authenticate with GitHub:

```bash
gh auth login
```

## PR Code Review

When asked to review a GitHub PR (URL or PR ID provided), extract the `<owner/repo>` and `<PR_ID>` from the URL (ask the user if the repo is missing), then use `gh api` to fetch the PR metadata (`repos/<owner>/<repo>/pulls/<PR_ID>`) and its diff (`repos/<owner>/<repo>/pulls/<PR_ID>/files --paginate`). To fully understand the context and impact of all code changes, read the complete source branch codebase — the diff alone does not capture the surrounding logic, dependencies, or broader design that the changes interact with. Report the PR title, author, and branches, then review all changed files for: secrets/injection/XSS (security), inefficient algorithms/memory leaks (performance), code smells/dead code (quality), missing error handling, excessive or sensitive logging, and missing/outdated documentation. For each finding, include the file path, line number(s), the problematic snippet, and a suggested fix. If nothing is wrong, summarize what was reviewed and state it looks good.

## Rules

- Always pass `--repo <owner>/<repo>` explicitly for any `gh` subcommand that accepts it (e.g., `pr list`, `issue list`, `run list`) — without it, the command infers the repo from the current git directory, which may silently return wrong results.
- When using `gh api`, use the path relative to `https://api.github.com` — e.g., `gh api repos/<owner>/<repo>/pulls`.
- All `gh api` calls that return lists are paginated (default 30/page). Always pass `--paginate` to fetch all pages: `gh api --paginate "repos/<owner>/<repo>/pulls"`.
- For `gh pr list`, prefer explicit state flags: use `--state closed` for closed, `--state merged` for merged, or `--state all` for all states. Omitting `--state` returns open PRs by default.
- NEVER target any GitHub Enterprise instance unless the user explicitly provides the hostname and confirms it is correct. If a URL references an unfamiliar hostname, stop and ask the user to confirm.
