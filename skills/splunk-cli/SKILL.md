---
name: splunk-cli
description: Perform Splunk operations using the Splunk REST API via curl. Use for running search queries, and inspecting saved searches, indexes, and alerts.
---

# Splunk CLI Skill

Use the instance and rules below when constructing all Splunk API commands.

## Instances

Update the table below with your Splunk instance URLs and credential file paths:

| Environment | Base URL | Token File |
|---|---|---|
| Production | `https://<your-splunk-prod-url>:8089` | `~/.splunk/prod.token` |
| Staging | `https://<your-splunk-staging-url>:8089` | `~/.splunk/staging.token` |

If the target instance is unclear, ask the user before running any command.

## Prerequisites (one-time setup by user)

Generate a Splunk authentication token (**Settings → Tokens**) for each instance and save it to the corresponding token file. Never print or display token values. Do not perform these steps yourself.

```bash
mkdir -p ~/.splunk && echo "<your-auth-token>" > <TOKEN_FILE> && chmod 600 <TOKEN_FILE>
```

## Command Format

```bash
curl -s -H "Authorization: Bearer $(cat <TOKEN_FILE>)" \
  "<BASE_URL>/services/<endpoint>?output_mode=json" | jq .
```

## Available API Endpoints

### Read-only (safe, no confirmation needed)

| Endpoint | Method | Description |
|---|---|---|
| `/services/search/jobs/export` | GET/POST | Run a search query and stream results |
| `/services/search/jobs/<sid>/results` | GET | Get results of a completed search job |
| `/services/saved/searches` | GET | List saved searches |
| `/services/data/indexes` | GET | List indexes |
| `/services/alerts/fired_alerts` | GET | List fired alerts |
| `/services/server/info` | GET | Show server/instance info |

### Example — run a search

```bash
curl -s -H "Authorization: Bearer $(cat <TOKEN_FILE>)" \
  --data-urlencode "search=search index=<index> error | head 20" \
  "<BASE_URL>/services/search/jobs/export?output_mode=json" | jq .
```

### Mutating operations (confirm with user before running)

| Endpoint | Method | Description |
|---|---|---|
| `/services/saved/searches` | POST | Create a saved search |
| `/services/saved/searches/<name>` | POST | Update a saved search |
| `/services/data/indexes` | POST | Create a new index |

### Never run — requires explicit human approval

| Endpoint | Method | Reason |
|---|---|---|
| `/services/saved/searches/<name>` | DELETE | Irreversible deletion of a saved search |
| `/services/data/indexes/<name>` | DELETE | Irreversibly deletes an index and its data |
| `/services/data/indexes/<name>` | POST (with `clean` action) | Irreversibly clears all data from an index |

## Rules

- NEVER target any Splunk instance not listed in the instance table above. If a URL references a different host, stop and ask the user to confirm.
- NEVER print, display, log, or store the contents of any token file.
- Always read the token inline with `$(cat <TOKEN_FILE>)` — never hardcode a token value in a command.
- Always pass `-s` (silent) to `curl`, use `output_mode=json`, and pipe through `jq` for readable output.
- NEVER run DELETE endpoints or index-clean operations without explicit user instruction — these are irreversible.
- For search queries, always set a reasonable time range and `head`/`| head N` limit — an unbounded search over a large index can be slow and return excessive data.
- If a saved search name or index name is unfamiliar, look it up with a read-only call first.