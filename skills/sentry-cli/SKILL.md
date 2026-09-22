---
name: sentry-cli
description: Perform Sentry error tracking operations using the sentry-cli tool and Sentry HTTP API. Use for querying issues, events, releases, and organizations.
---

# Sentry CLI Skill

Use the configuration and rules below when constructing all `sentry-cli`/API commands.

## Configuration

Update the table below with your Sentry organization and credential file paths:

| Environment | Organization | Project | Token File |
|---|---|---|---|
| Production | `<your-org-slug>` | `<your-prod-project-slug>` | `~/.sentry/prod.token` |
| Staging | `<your-org-slug>` | `<your-staging-project-slug>` | `~/.sentry/staging.token` |

If the target environment is unclear, ask the user before running any command.

### Prerequisites (one-time setup by user)

Generate a Sentry auth token (**Settings → Auth Tokens**) with `project:read`/`org:read` scope for each environment and save it to the corresponding token file. Never print or display token values. Do not perform these steps yourself.

```bash
brew install getsentry/tools/sentry-cli
mkdir -p ~/.sentry && echo "<your-auth-token>" > <TOKEN_FILE> && chmod 600 <TOKEN_FILE>
```

## Command Format

Via `sentry-cli`:
```bash
SENTRY_AUTH_TOKEN="$(cat <TOKEN_FILE>)" sentry-cli --org <org-slug> <command>
```

Via API (`curl`), for operations `sentry-cli` doesn't cover:
```bash
curl -s -H "Authorization: Bearer $(cat <TOKEN_FILE>)" \
  "https://sentry.io/api/0/<endpoint>" | jq .
```

## Available Commands / Endpoints

### Read-only (safe, no confirmation needed)

| Command / Endpoint | Description |
|---|---|
| `sentry-cli releases list --org <org> -p <project>` | List releases |
| `sentry-cli releases info <version> --org <org> -p <project>` | Show release details |
| `/api/0/projects/<org>/<project>/issues/` | List issues |
| `/api/0/issues/<id>/` | Get a specific issue |
| `/api/0/issues/<id>/events/` | List events for an issue |
| `/api/0/organizations/<org>/projects/` | List projects in the org |

### Mutating operations (confirm with user before running)

| Command / Endpoint | Description |
|---|---|
| `sentry-cli releases new <version>` | Create a new release |
| `sentry-cli releases deploys <version> new` | Register a deploy for a release |
| `/api/0/issues/<id>/` (PUT) | Update issue status (resolve, ignore, assign) |

### Never run — requires explicit human approval

| Command / Endpoint | Reason |
|---|---|
| `sentry-cli releases delete <version>` | Irreversible release deletion |
| `/api/0/issues/<id>/` (DELETE) | Irreversible issue deletion |
| `/api/0/projects/<org>/<project>/` (DELETE) | Irreversibly deletes the entire project |

## Rules

- NEVER target an organization/project not listed in the configuration table above. If a command references an unfamiliar org or project, stop and ask the user to confirm.
- NEVER print, display, log, or store the contents of any token file or `SENTRY_AUTH_TOKEN` value.
- Always read the token inline (`$(cat <TOKEN_FILE>)`) — never hardcode it in a command or shell profile.
- NEVER delete a release, issue, or project without explicit user instruction — these are irreversible.
- NEVER change issue status (resolve/ignore) in bulk without explicit user instruction.
- All list endpoints are paginated via the `Link` response header — follow the `next` relation until absent to fetch all pages.