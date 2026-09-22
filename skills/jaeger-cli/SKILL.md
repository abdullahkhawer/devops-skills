---
name: jaeger-cli
description: Query Jaeger distributed tracing data using the Jaeger HTTP API via curl. Use for finding traces, services, operations, and dependency graphs.
---

# Jaeger CLI Skill

Use the instance and rules below when constructing all Jaeger API commands.

## Instances

Update the table below with your Jaeger Query instance URLs before using this skill:

| Environment | Base URL | Token File (if auth is enforced) |
|---|---|---|
| Production | `https://<your-jaeger-prod-url>` | `~/.jaeger/prod.token` |
| Staging | `https://<your-jaeger-staging-url>` | `~/.jaeger/staging.token` |

If the target instance is unclear, ask the user before running any command.

## Prerequisites (one-time setup by user)

If the Jaeger instance is behind an authentication proxy, obtain a token and save it to the corresponding token file (see instance table above). Never print or display token values. Do not perform these steps yourself.

```bash
mkdir -p ~/.jaeger && echo "<your-api-token>" > <TOKEN_FILE> && chmod 600 <TOKEN_FILE>
```

## Command Format

All API calls use `curl` against the Jaeger Query Service API:

```bash
curl -s -H "Authorization: Bearer $(cat <TOKEN_FILE>)" \
  "<BASE_URL>/api/<endpoint>" | jq .
```

If the instance has no auth in front of it, omit the `Authorization` header entirely.

Example (production):
```bash
curl -s -H "Authorization: Bearer $(cat ~/.jaeger/prod.token)" \
  "https://<your-jaeger-prod-url>/api/services" | jq .
```

## Available API Endpoints

### Read-only (safe, no confirmation needed)

| Endpoint | Description |
|---|---|
| `/api/services` | List all known service names |
| `/api/services/<service>/operations` | List operations for a service |
| `/api/traces?service=<service>&operation=<op>&start=<us>&end=<us>&limit=<n>` | Find traces matching filters |
| `/api/traces/<trace-id>` | Get a specific trace by ID |
| `/api/dependencies?endTs=<ms>&lookback=<ms>` | Get the service dependency graph |

### Example Queries

```bash
# List services
curl -s "<BASE_URL>/api/services" | jq .

# Find traces for a service in the last hour, with a minimum duration filter
curl -s "<BASE_URL>/api/traces?service=<service-name>&start=$(( $(date -u +%s) - 3600 ))000000&end=$(date -u +%s)000000&minDuration=100ms&limit=20" | jq .

# Get a specific trace
curl -s "<BASE_URL>/api/traces/<trace-id>" | jq .
```

## Rules

- NEVER target any Jaeger instance not listed in the instance table above. If a URL references a different host, stop and ask the user to confirm.
- Update the instance table with your actual Jaeger instance URLs before using this skill.
- NEVER print, display, log, or store the contents of any token file.
- Always pass `-s` (silent) to `curl` and pipe through `jq` for readable output.
- Timestamps for `/api/traces` (`start`/`end`) are in **microseconds since epoch**; `/api/dependencies` (`endTs`/`lookback`) are in **milliseconds** — do not mix the units.
- Always set a `limit` when searching for traces — an unbounded search on a busy service can return excessive data.
- Jaeger's Query API is read-only by design (it does not accept span ingestion or deletion via this API) — there are no mutating or destructive endpoints to guard against here.
- If a trace ID or service name is unfamiliar, use `/api/services` and `/api/services/<service>/operations` to explore before querying traces.
