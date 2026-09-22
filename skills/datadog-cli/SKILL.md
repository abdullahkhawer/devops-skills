---
name: datadog-cli
description: Perform Datadog operations using the Datadog HTTP API via curl. Use for querying metrics, logs, monitors, dashboards, and events.
---

# Datadog CLI Skill

Use the instance and rules below when constructing all Datadog API commands.

## Configuration

Update the table below with your Datadog site and credential file paths:

| Environment | Site | API Key File | App Key File |
|---|---|---|---|
| Production | `<your-datadog-site>` (e.g. `datadoghq.com`, `datadoghq.eu`) | `~/.datadog/prod.api_key` | `~/.datadog/prod.app_key` |
| Staging | `<your-datadog-site>` | `~/.datadog/staging.api_key` | `~/.datadog/staging.app_key` |

If the target environment is unclear, ask the user before running any command.

## Prerequisites (one-time setup by user)

Generate a Datadog API key and Application key for each environment (**Organization Settings → API Keys / Application Keys**) and save them to the corresponding files. Never print or display key values. Do not perform these steps yourself.

```bash
mkdir -p ~/.datadog && echo "<your-api-key>" > <API_KEY_FILE> && echo "<your-app-key>" > <APP_KEY_FILE> && chmod 600 ~/.datadog/*
```

## Command Format

```bash
curl -s -H "DD-API-KEY: $(cat <API_KEY_FILE>)" \
  -H "DD-APPLICATION-KEY: $(cat <APP_KEY_FILE>)" \
  "https://api.<your-datadog-site>/api/<endpoint>" | jq .
```

## Available API Endpoints

### Read-only (safe, no confirmation needed)

| Endpoint | Method | Description |
|---|---|---|
| `/api/v1/query?query=<metric-query>&from=<ts>&to=<ts>` | GET | Query timeseries metrics |
| `/api/v2/logs/events/search` | POST | Search log events |
| `/api/v1/monitor` | GET | List monitors |
| `/api/v1/monitor/<id>` | GET | Get a specific monitor |
| `/api/v1/dashboard` | GET | List dashboards |
| `/api/v1/dashboard/<id>` | GET | Get a specific dashboard |
| `/api/v2/incidents` | GET | List incidents |
| `/api/v1/events` | GET | List events |
| `/api/v1/hosts` | GET | List hosts |

### Mutating operations (confirm with user before running)

| Endpoint | Method | Description |
|---|---|---|
| `/api/v1/monitor` | POST | Create a monitor |
| `/api/v1/monitor/<id>` | PUT | Update a monitor |
| `/api/v1/dashboard` | POST | Create a dashboard |
| `/api/v1/monitor/<id>/mute` | POST | Mute a monitor |
| `/api/v1/events` | POST | Post a custom event |

### Never run — requires explicit human approval

| Endpoint | Method | Reason |
|---|---|---|
| `/api/v1/monitor/<id>` | DELETE | Irreversible monitor deletion |
| `/api/v1/dashboard/<id>` | DELETE | Irreversible dashboard deletion |
| `/api/v2/logs/config/indexes/<name>` | DELETE/PUT | Alters log retention/indexing globally |
| `/api/v1/monitor/<id>/unmute` combined with bulk operations | Can silence/unsilence alerting broadly |

## Rules

- NEVER target any Datadog site not listed in the configuration table above. If a command references an unfamiliar site, stop and ask the user to confirm.
- NEVER print, display, log, or store the contents of any API/App key file.
- Always read keys inline with `$(cat <FILE>)` — never hardcode key values in a command.
- Always pass `-s` (silent) to `curl` and pipe through `jq` for readable output.
- NEVER run DELETE endpoints or global config changes (log indexes, retention) without explicit user instruction.
- All list endpoints may be paginated — check the response for a `page` cursor or `next` link and iterate if needed.
- If a monitor ID, dashboard ID, or metric name is unfamiliar, look it up with a read-only call first.