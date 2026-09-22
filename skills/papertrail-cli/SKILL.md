---
name: papertrail-cli
description: Perform Papertrail log management operations using the Papertrail HTTP API via curl. Use for searching logs and inspecting saved searches and systems.
---

# Papertrail CLI Skill

Use the instance and rules below when constructing all Papertrail API commands.

## Configuration

Update the table below with your Papertrail credential file paths:

| Environment | Token File |
|---|---|
| Production | `~/.papertrail/prod.token` |
| Staging | `~/.papertrail/staging.token` |

If the target environment is unclear, ask the user before running any command.

## Prerequisites (one-time setup by user)

Generate a Papertrail API token (**Settings → Profile → API Token**) for each environment and save it to the corresponding token file. Never print or display token values. Do not perform these steps yourself.

```bash
mkdir -p ~/.papertrail && echo "<your-api-token>" > <TOKEN_FILE> && chmod 600 <TOKEN_FILE>
```

## Command Format

```bash
curl -s -H "X-Papertrail-Token: $(cat <TOKEN_FILE>)" \
  "https://papertrailapp.com/api/v1/<endpoint>.json" | jq .
```

## Available API Endpoints

### Read-only (safe, no confirmation needed)

| Endpoint | Method | Description |
|---|---|---|
| `/api/v1/events/search.json?q=<query>` | GET | Search log events |
| `/api/v1/searches.json` | GET | List saved searches |
| `/api/v1/systems.json` | GET | List registered systems |
| `/api/v1/groups.json` | GET | List system groups |

### Example — search logs

```bash
curl -s -H "X-Papertrail-Token: $(cat <TOKEN_FILE>)" \
  "https://papertrailapp.com/api/v1/events/search.json?q=<query>&system_id=<id>" | jq .
```

### Mutating operations (confirm with user before running)

| Endpoint | Method | Description |
|---|---|---|
| `/api/v1/searches.json` | POST | Create a saved search |
| `/api/v1/groups.json` | POST | Create a system group |

### Never run — requires explicit human approval

| Endpoint | Method | Reason |
|---|---|---|
| `/api/v1/searches/<id>.json` | DELETE | Irreversible saved search deletion |
| `/api/v1/systems/<id>.json` | DELETE | Removes a registered system and stops its ingestion |
| `/api/v1/groups/<id>.json` | DELETE | Irreversible group deletion |

## Rules

- NEVER print, display, log, or store the contents of any token file.
- Always read the token inline with `$(cat <TOKEN_FILE>)` — never hardcode a token value in a command.
- Always pass `-s` (silent) to `curl` and pipe through `jq` for readable output.
- NEVER run DELETE endpoints without explicit user instruction — these are irreversible.
- Papertrail's search API only returns the most recent matching events within retention — always narrow the query (`q`, `system_id`) rather than fetching broadly.
- If a system ID or saved search ID is unfamiliar, look it up with a read-only call first.