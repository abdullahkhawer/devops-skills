---
name: prometheus-cli
description: Query Prometheus metrics using the HTTP API via curl. Use for instant queries, range queries, metadata lookups, and alert/rule inspection.
---

# Prometheus CLI Skill

Use the instance and rules below when constructing all Prometheus API commands.

## Instances

Update the table below with your Prometheus instance URLs before using this skill:

| Environment | Base URL | Token File |
|---|---|---|
| Production | `https://<your-prometheus-prod-url>` | `~/.prometheus/prod.token` |
| Staging | `https://<your-prometheus-staging-url>` | `~/.prometheus/staging.token` |

If the target instance is unclear, ask the user before running any command.

## Prerequisites (one-time setup by user)

Obtain a Prometheus API token for each instance you intend to use and save it to the corresponding token file (see instance table above). Never print or display token values. Do not perform these steps yourself.

For each instance:
1. Obtain an API token (via your team or the monitoring platform)
2. Save it:

```bash
mkdir -p ~/.prometheus && echo "<your-api-token>" > <TOKEN_FILE> && chmod 600 <TOKEN_FILE>
```

## Command Format

All API calls use `curl` with the token read from the instance's token file (see instance table above):

```bash
curl -s -H "Authorization: Bearer $(cat <TOKEN_FILE>)" \
  "<BASE_URL>/api/v1/<endpoint>" | jq .
```

Example (production):
```bash
curl -s -H "Authorization: Bearer $(cat ~/.prometheus/prod.token)" \
  "https://<your-prometheus-prod-url>/api/v1/query?query=up" | jq .
```

## Available API Endpoints

### Read-only (safe, no confirmation needed)

| Endpoint | Description |
|---|---|
| `/api/v1/query?query=<expr>` | Instant query — evaluate a PromQL expression at current time |
| `/api/v1/query_range?query=<expr>&start=<ts>&end=<ts>&step=<dur>` | Range query — evaluate PromQL over a time range |
| `/api/v1/labels` | List all label names |
| `/api/v1/label/<name>/values` | List values for a specific label |
| `/api/v1/series?match[]=<selector>` | Find time series matching a selector |
| `/api/v1/metadata` | List metric metadata (type, help text) |
| `/api/v1/rules` | List all recording and alerting rules |
| `/api/v1/alerts` | List currently firing alerts |
| `/api/v1/targets` | List scrape targets and their health |
| `/api/v1/targets/metadata` | List metadata for scraped targets |
| `/api/v1/status/config` | Show current Prometheus configuration |
| `/api/v1/status/flags` | Show runtime flags |
| `/api/v1/status/runtimeinfo` | Show runtime information |
| `/api/v1/status/buildinfo` | Show build information |
| `/api/v1/status/tsdb` | Show TSDB stats |

### Example Queries

Replace `<TOKEN_FILE>` and `<BASE_URL>` with the values for your target instance (see instance table above).

```bash
# Instant query — CPU usage across all instances
curl -s -H "Authorization: Bearer $(cat <TOKEN_FILE>)" \
  "<BASE_URL>/api/v1/query?query=rate(process_cpu_seconds_total[5m])" | jq .

# Range query — memory usage over the last hour (60s step)
curl -s -H "Authorization: Bearer $(cat <TOKEN_FILE>)" \
  "<BASE_URL>/api/v1/query_range?query=process_resident_memory_bytes&start=$(date -u -v-1H +%s)&end=$(date -u +%s)&step=60" | jq .

# List all firing alerts
curl -s -H "Authorization: Bearer $(cat <TOKEN_FILE>)" \
  "<BASE_URL>/api/v1/alerts" | jq '.data.alerts[]'

# Find series matching a selector
curl -s -H "Authorization: Bearer $(cat <TOKEN_FILE>)" \
  "<BASE_URL>/api/v1/series?match[]=up{job=\"node-exporter\"}" | jq .
```

### Never run — requires explicit human approval

| Endpoint | Method | Reason |
|---|---|---|
| `/api/v1/admin/tsdb/delete_series` | POST | Permanently deletes time series data |
| `/api/v1/admin/tsdb/clean_tombstones` | POST | Purges deleted data from disk |
| `/api/v1/admin/tsdb/snapshot` | POST | Triggers a snapshot (may affect disk/storage) |

## Rules

- NEVER target any Prometheus instance not listed in the instance table above. If a URL references a different host, stop and ask the user to confirm.
- Update the instance table with your actual Prometheus URLs before using this skill.
- NEVER print, display, log, or store the contents of any token file or credential value.
- Always read the token inline with `$(cat <TOKEN_FILE>)` — never hardcode a token value in a command.
- Always pass `-s` (silent) to `curl` to suppress progress output, and pipe through `jq` for readable output.
- NEVER run admin/mutating endpoints (`/api/v1/admin/...`) without explicit user instruction — these can permanently alter or delete metric data.
- If a PromQL expression or metric name is unfamiliar, use `/api/v1/metadata` or `/api/v1/labels` to explore available metrics before querying.
- For range queries, always verify that the time window (`start`/`end`) and `step` are reasonable — very wide windows with fine steps can produce extremely large responses.
- If the required token file does not exist, inform the user and stop. Do not attempt to find or guess the token.
