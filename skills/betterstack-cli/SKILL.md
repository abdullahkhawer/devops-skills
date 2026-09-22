---
name: betterstack-cli
description: Perform Better Stack (Logs, Uptime, Incidents) operations using the Better Stack HTTP API via curl. Use for querying logs, monitors, and incidents.
---

# Better Stack CLI Skill

Use the instance and rules below when constructing all Better Stack API commands.

## Configuration

Update the table below with your Better Stack credential file paths:

| Environment | Token File | Team |
|---|---|---|
| Production | `~/.betterstack/prod.token` | `<your-prod-team>` |
| Staging | `~/.betterstack/staging.token` | `<your-staging-team>` |

If the target environment is unclear, ask the user before running any command.

## Prerequisites (one-time setup by user)

Generate a Better Stack API token (**Settings → API tokens**) for each environment and save it to the corresponding token file. Never print or display token values. Do not perform these steps yourself.

```bash
mkdir -p ~/.betterstack && echo "<your-api-token>" > <TOKEN_FILE> && chmod 600 <TOKEN_FILE>
```

## Command Format

```bash
curl -s -H "Authorization: Bearer $(cat <TOKEN_FILE>)" \
  "https://<your-betterstack-api-host>/api/v<version>/<endpoint>" | jq .
```

Logs querying and Uptime/Incidents management use different API hosts — confirm the correct one for the operation with the user if unsure (`telemetry API` vs `Uptime API`).

## Available API Endpoints

### Read-only (safe, no confirmation needed)

| Endpoint | Method | Description |
|---|---|---|
| `/api/v2/monitors` | GET | List uptime monitors |
| `/api/v2/monitors/<id>` | GET | Get a specific monitor |
| `/api/v2/incidents` | GET | List incidents |
| `/api/v1/heartbeats` | GET | List heartbeat checks |
| `/api/v1/query` (telemetry API) | GET/POST | Query log data |

### Mutating operations (confirm with user before running)

| Endpoint | Method | Description |
|---|---|---|
| `/api/v2/monitors` | POST | Create a monitor |
| `/api/v2/monitors/<id>` | PATCH | Update a monitor |
| `/api/v2/incidents/<id>/acknowledge` | POST | Acknowledge an incident |
| `/api/v2/incidents/<id>/resolve` | POST | Resolve an incident |

### Never run — requires explicit human approval

| Endpoint | Method | Reason |
|---|---|---|
| `/api/v2/monitors/<id>` | DELETE | Irreversible monitor deletion |
| `/api/v2/heartbeats/<id>` | DELETE | Irreversible heartbeat deletion |
| `/api/v2/monitors/<id>/pause` | POST | Stops monitoring silently, risking missed real outages |

## Rules

- NEVER target a Better Stack team/environment not listed in the configuration table above. If a request references an unfamiliar team, stop and ask the user to confirm.
- NEVER print, display, log, or store the contents of any token file.
- Always read the token inline with `$(cat <TOKEN_FILE>)` — never hardcode a token value in a command.
- Always pass `-s` (silent) to `curl` and pipe through `jq` for readable output.
- NEVER run DELETE endpoints, pause a monitor, or resolve/acknowledge an incident without explicit user instruction.
- All list endpoints are paginated (`pagination.next` field) — follow it until absent to fetch all pages.
- If a monitor ID or incident ID is unfamiliar, look it up with a read-only call first.