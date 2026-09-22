---
name: rollbar-cli
description: Perform Rollbar error monitoring operations using the Rollbar HTTP API via curl. Use for querying items, occurrences, and deploys.
---

# Rollbar CLI Skill

Use the instance and rules below when constructing all Rollbar API commands.

## Configuration

Update the table below with your Rollbar credential file paths:

| Environment | Read Token File | Project |
|---|---|---|
| Production | `~/.rollbar/prod.token` | `<your-prod-project>` |
| Staging | `~/.rollbar/staging.token` | `<your-staging-project>` |

If the target environment is unclear, ask the user before running any command.

## Prerequisites (one-time setup by user)

Generate a Rollbar project access token with `read` scope (**Project Settings → Access Tokens**) for each environment and save it to the corresponding token file. Never print or display token values. Do not perform these steps yourself.

```bash
mkdir -p ~/.rollbar && echo "<your-read-token>" > <TOKEN_FILE> && chmod 600 <TOKEN_FILE>
```

## Command Format

```bash
curl -s "https://api.rollbar.com/api/1/<endpoint>?access_token=$(cat <TOKEN_FILE>)" | jq .
```

## Available API Endpoints

### Read-only (safe, no confirmation needed)

| Endpoint | Method | Description |
|---|---|---|
| `/api/1/items` | GET | List items (grouped errors) |
| `/api/1/item/<id>` | GET | Get a specific item |
| `/api/1/item/<id>/instances` | GET | List occurrences of an item |
| `/api/1/deploys` | GET | List deploys |
| `/api/1/project` | GET | Get project details |

### Mutating operations (confirm with user before running)

| Endpoint | Method | Description |
|---|---|---|
| `/api/1/item/<id>` | PATCH | Update item status (e.g. resolve, mute) |
| `/api/1/deploy` | POST | Report a new deploy |

### Never run — requires explicit human approval

| Endpoint | Method | Reason |
|---|---|---|
| `/api/1/item/<id>` | DELETE | Irreversible item deletion |
| `/api/1/instances/<id>` | DELETE | Irreversible occurrence deletion |
| `/api/1/project/<id>` | DELETE | Irreversibly deletes the entire project |

## Rules

- NEVER target a Rollbar project not listed in the configuration table above. If a request references an unfamiliar project, stop and ask the user to confirm.
- NEVER print, display, log, or store the contents of any token file.
- Always read the token inline with `$(cat <TOKEN_FILE>)` — never hardcode a token value in a command.
- Always pass `-s` (silent) to `curl` and pipe through `jq` for readable output.
- NEVER run DELETE endpoints or change item status (resolve/mute) without explicit user instruction.
- Prefer a read-only project access token for this skill — never use a `write`/`post_server_item` scoped token for read operations.
- If an item ID is unfamiliar, list items first before fetching instances.