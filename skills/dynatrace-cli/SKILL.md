---
name: dynatrace-cli
description: Perform Dynatrace operations using the Dynatrace HTTP API via curl. Use for querying entities, metrics, problems, and events.
---

# Dynatrace CLI Skill

Use the instance and rules below when constructing all Dynatrace API commands.

## Instances

Update the table below with your Dynatrace environment URLs and token file paths:

| Environment | Base URL | Token File |
|---|---|---|
| Production | `https://<your-env-id>.live.dynatrace.com` | `~/.dynatrace/prod.token` |
| Staging | `https://<your-env-id>.live.dynatrace.com` | `~/.dynatrace/staging.token` |

If the target instance is unclear, ask the user before running any command.

## Prerequisites (one-time setup by user)

Generate a Dynatrace API token with the required scopes (**Settings → Integration → Dynatrace API**) for each environment and save it to the corresponding token file. Never print or display token values. Do not perform these steps yourself.

```bash
mkdir -p ~/.dynatrace && echo "<your-api-token>" > <TOKEN_FILE> && chmod 600 <TOKEN_FILE>
```

## Command Format

```bash
curl -s -H "Authorization: Api-Token $(cat <TOKEN_FILE>)" \
  "<BASE_URL>/api/v2/<endpoint>" | jq .
```

## Available API Endpoints

### Read-only (safe, no confirmation needed)

| Endpoint | Method | Description |
|---|---|---|
| `/api/v2/entities` | GET | List monitored entities (hosts, services, etc.) |
| `/api/v2/entities/<id>` | GET | Get a specific entity |
| `/api/v2/metrics/query` | GET | Query metric data points |
| `/api/v2/problems` | GET | List problems |
| `/api/v2/problems/<id>` | GET | Get a specific problem |
| `/api/v2/events` | GET | List events |
| `/api/v2/synthetic/monitors` | GET | List synthetic monitors |

### Mutating operations (confirm with user before running)

| Endpoint | Method | Description |
|---|---|---|
| `/api/v2/problems/<id>/comments` | POST | Add a comment to a problem |
| `/api/config/v1/anomalyDetection/metricEvents` | POST | Create a metric event/alert rule |
| `/api/v2/synthetic/monitors` | POST | Create a synthetic monitor |

### Never run — requires explicit human approval

| Endpoint | Method | Reason |
|---|---|---|
| `/api/v2/settings/objects/<id>` | DELETE | Irreversible settings object deletion |
| `/api/config/v1/anomalyDetection/metricEvents/<id>` | DELETE | Irreversible alert rule deletion |
| `/api/v2/synthetic/monitors/<id>` | DELETE | Irreversible monitor deletion |

## Rules

- NEVER target any Dynatrace environment not listed in the instance table above. If a URL references a different host, stop and ask the user to confirm.
- NEVER print, display, log, or store the contents of any token file.
- Always read the token inline with `$(cat <TOKEN_FILE>)` — never hardcode a token value in a command.
- Always pass `-s` (silent) to `curl` and pipe through `jq` for readable output.
- NEVER run DELETE endpoints without explicit user instruction — these are irreversible.
- All list endpoints are paginated (`nextPageKey` field) — follow it until absent to fetch all pages.
- If an entity ID or problem ID is unfamiliar, look it up with a read-only call first.