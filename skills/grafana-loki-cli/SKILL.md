---
name: grafana-loki-cli
description: Query Grafana Loki logs using the Loki HTTP API via curl or logcli. Use for LogQL queries, label exploration, and tailing log streams.
---

# Grafana Loki CLI Skill

Use the instance and rules below when constructing all Loki API/`logcli` commands.

## Instances

Update the table below with your Loki instance URLs and token file paths:

| Environment | Base URL | Token File |
|---|---|---|
| Production | `https://<your-loki-prod-url>` | `~/.loki/prod.token` |
| Staging | `https://<your-loki-staging-url>` | `~/.loki/staging.token` |

If the target instance is unclear, ask the user before running any command.

## Prerequisites (one-time setup by user)

Obtain a Loki API token (or basic-auth credentials, depending on setup) for each instance and save it to the corresponding token file. Never print or display token values. Do not perform these steps yourself.

```bash
brew install logcli
mkdir -p ~/.loki && echo "<your-api-token>" > <TOKEN_FILE> && chmod 600 <TOKEN_FILE>
```

## Command Format

Via `curl`:
```bash
curl -s -H "Authorization: Bearer $(cat <TOKEN_FILE>)" \
  "<BASE_URL>/loki/api/v1/<endpoint>" | jq .
```

Via `logcli`:
```bash
logcli --addr="<BASE_URL>" --bearer-token="$(cat <TOKEN_FILE>)" <command>
```

## Available API Endpoints / Commands

### Read-only (safe, no confirmation needed)

| Endpoint / `logcli` command | Description |
|---|---|
| `/loki/api/v1/query` / `logcli query '<LogQL>'` | Run an instant LogQL query |
| `/loki/api/v1/query_range` / `logcli query '<LogQL>' --from=<t> --to=<t>` | Run a LogQL range query |
| `/loki/api/v1/labels` / `logcli labels` | List all label names |
| `/loki/api/v1/label/<name>/values` / `logcli labels <name>` | List values for a label |
| `/loki/api/v1/series?match[]=<selector>` / `logcli series '<selector>'` | Find streams matching a selector |
| `/ready` | Check readiness/health |
| `logcli query '<LogQL>' --tail` | Tail a live log stream (read-only) |

### Example Queries

```bash
# Instant query — errors in the last 5 minutes for a job
curl -s -H "Authorization: Bearer $(cat <TOKEN_FILE>)" \
  "<BASE_URL>/loki/api/v1/query_range?query={job=\"<job-name>\"}|=\"error\"&start=$(( $(date -u +%s) - 300 ))000000000&end=$(date -u +%s)000000000&limit=100" | jq .

# logcli equivalent
logcli --addr="<BASE_URL>" --bearer-token="$(cat <TOKEN_FILE>)" \
  query '{job="<job-name>"} |= "error"' --since=5m --limit=100
```

## Rules

- NEVER target any Loki instance not listed in the instance table above. If a URL references a different host, stop and ask the user to confirm.
- NEVER print, display, log, or store the contents of any token file.
- Always read the token inline with `$(cat <TOKEN_FILE>)` — never hardcode a token value in a command.
- Always pass `-s` (silent) to `curl` and pipe through `jq` for readable output.
- Loki's public API is read-only for queries (no delete/mutate endpoints exposed) — no destructive operations to guard against here, other than avoiding excessive queries.
- Always set a bounded time range (`start`/`end` or `--since`) and a reasonable `limit` — an unbounded LogQL query over a busy stream can be slow and return excessive data.
- If a label name or stream selector is unfamiliar, use `/loki/api/v1/labels` and `label/<name>/values` to explore before querying.