---
name: bitbucket-cli
description: Perform Bitbucket operations using the Bitbucket REST API via curl. Use for repositories, pull requests, pipelines, and issues. ALSO use this skill when asked to review a Bitbucket PR — when a Bitbucket PR URL or PR ID is provided, fetch and review the diff instead of the code-review skill.
---

# Bitbucket CLI Skill

Bitbucket Cloud has no official first-party CLI equivalent to `gh`/`glab` — use the REST API via `curl` for all operations.

## Configuration

Update the workspace below before using this skill:

| Setting | Value |
|---|---|
| Bitbucket Workspace | `<your-workspace>` |
| Credential File | `~/.bitbucket/token` |

If the target workspace is unclear, ask the user before running any command.

### Prerequisites (one-time setup by user)

Create an [App Password](https://bitbucket.org/account/settings/app-passwords/) with `Repositories: Read` (and `Write`/`Pull requests: Write` if mutations are needed), then save `<username>:<app-password>` to the credential file. Never print or display the credential value. Do not perform these steps yourself.

```bash
mkdir -p ~/.bitbucket && echo "<username>:<app-password>" > ~/.bitbucket/token && chmod 600 ~/.bitbucket/token
```

## Command Format

All API calls use `curl` with basic auth read from the credential file:

```bash
curl -s -u "$(cat ~/.bitbucket/token)" \
  "https://api.bitbucket.org/2.0/<endpoint>" | jq .
```

## PR Code Review

When asked to review a Bitbucket PR (URL or PR ID provided), extract the `<workspace/repo>` and `<PR_ID>` from the URL (ask the user if the repo is missing), then fetch the PR metadata (`GET /2.0/repositories/<workspace>/<repo>/pullrequests/<PR_ID>`) and its diff (`GET /2.0/repositories/<workspace>/<repo>/pullrequests/<PR_ID>/diff`). To fully understand the context and impact of all code changes, read the complete source branch codebase — the diff alone does not capture the surrounding logic, dependencies, or broader design that the changes interact with. Report the PR title, author, and branches, then review all changed files for: secrets/injection/XSS (security), inefficient algorithms/memory leaks (performance), code smells/dead code (quality), missing error handling, excessive or sensitive logging, and missing/outdated documentation. For each finding, include the file path, line number(s), the problematic snippet, and a suggested fix. If nothing is wrong, summarize what was reviewed and state it looks good.

## Available API Endpoints

### Read-only (safe, no confirmation needed)

| Endpoint | Method | Description |
|---|---|---|
| `/2.0/repositories/<workspace>` | GET | List repositories in the workspace |
| `/2.0/repositories/<workspace>/<repo>` | GET | Get repository details |
| `/2.0/repositories/<workspace>/<repo>/pullrequests` | GET | List pull requests |
| `/2.0/repositories/<workspace>/<repo>/pullrequests/<id>` | GET | Get a pull request |
| `/2.0/repositories/<workspace>/<repo>/pullrequests/<id>/diff` | GET | Get PR diff |
| `/2.0/repositories/<workspace>/<repo>/pullrequests/<id>/commits` | GET | List PR commits |
| `/2.0/repositories/<workspace>/<repo>/pipelines/` | GET | List pipeline runs |
| `/2.0/repositories/<workspace>/<repo>/issues` | GET | List issues |

### Mutating operations (confirm with user before running)

| Endpoint | Method | Description |
|---|---|---|
| `/2.0/repositories/<workspace>/<repo>/pullrequests` | POST | Create a pull request |
| `/2.0/repositories/<workspace>/<repo>/pullrequests/<id>` | PUT | Update a pull request |
| `/2.0/repositories/<workspace>/<repo>/pullrequests/<id>/comments` | POST | Comment on a pull request |
| `/2.0/repositories/<workspace>/<repo>/pullrequests/<id>/merge` | POST | Merge a pull request |
| `/2.0/repositories/<workspace>/<repo>/pipelines/` | POST | Trigger a pipeline run |

### Never run — requires explicit human approval

| Endpoint | Method | Reason |
|---|---|---|
| `/2.0/repositories/<workspace>/<repo>/pullrequests/<id>/decline` | POST | Closes a PR without merging |
| `/2.0/repositories/<workspace>/<repo>` | DELETE | Irreversibly deletes the repository |
| `/2.0/repositories/<workspace>/<repo>/pullrequests/<id>` | DELETE | Irreversible PR deletion |
| `/2.0/repositories/<workspace>/<repo>/branch-restrictions/<id>` | DELETE | Removes branch protection rules |

## Rules

- NEVER target any Bitbucket workspace other than the one configured above. If a URL references a different workspace, stop and ask the user to confirm.
- NEVER print, display, log, or store the contents of `~/.bitbucket/token` or any credential value.
- Always read the credential inline with `$(cat ~/.bitbucket/token)` — never hardcode it in a command.
- All list endpoints are paginated (`next` field in the response) — follow the `next` URL until it is absent to fetch all pages.
- NEVER merge, decline, or delete a pull request/repository without explicit user instruction.
- If a repo slug or PR ID is unfamiliar, look it up with a read-only call first.
