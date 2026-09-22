---
name: graylog-cli
description: Perform Graylog operations using the Graylog HTTP API via curl. Use for searching logs, and inspecting streams, dashboards, and alerts.
---

# Graylog CLI Skill

Use the instance and rules below when constructing all Graylog API commands.

## Instances

Update the table below with your Graylog instance URLs and credential file paths:

| Environment | Base URL | Token File |
|---|---|---|
| Production | `https://<your-graylog-prod-url>` | `~/.graylog/prod.token` |
| Staging | `https://<your-graylog-staging-url>` | `~/.graylog/staging.token` |

If the target instance is unclear, ask the user before running any command.

## Prerequisites (one-time setup by user)

Generate a Graylog API token (**System → Users → Edit Tokens**) for each instance and save it to the corresponding token file. Never print or display token values. Do not perform these steps yourself.

```bash
mkdir -p ~/.graylog && echo "<your-api-token>" > <TOKEN_FILE> && chmod 600 <TOKEN_FILE>
```

## Command Format

Graylog uses the API token as the username with `token` as the password:

```bash
curl -s -u "$(cat <TOKEN_FILE>):token" -H "X-Requested-By: cli" \
  "<BASE_URL>/api/<endpoint>" | jq .
```

## Available API Endpoints

### Read-only (safe, no confirmation needed)

| Endpoint | Method | Description |
|---|---|---|
| `/api/search/universal/relative` | GET | Run a relative-time search query |
| `/api/streams` | GET | List streams |
| `/api/streams/<id>` | GET | Get a specific stream |
| `/api/dashboards` | GET | List dashboards |
| `/api/alerts` | GET | List triggered alerts |
| `/api/system/indices/index_sets` | GET | List index sets |
| `/api/system/cluster/node` | GET | Show cluster node info |

### Example — search query

```bash
curl -s -u "$(cat <TOKEN_FILE>):token" -H "X-Requested-By: cli" \
  "<BASE_URL>/api/search/universal/relative?query=<query>&range=3600&limit=50" | jq .
```

### Mutating operations (confirm with user before running)

| Endpoint | Method | Description |
|---|---|---|
| `/api/streams` | POST | Create a stream |
| `/api/streams/<id>` | PUT | Update a stream |
| `/api/dashboards` | POST | Create a dashboard |
| `/api/streams/<id>/pause` / `/resume` | POST | Pause or resume a stream |

### Never run — requires explicit human approval

| Endpoint | Method | Reason |
|---|---|---|
| `/api/streams/<id>` | DELETE | Irreversible stream deletion |
| `/api/dashboards/<id>` | DELETE | Irreversible dashboard deletion |
| `/api/system/indices/index_sets/<id>` | DELETE | Irreversibly deletes an index set and its data |

## Rules

- NEVER target any Graylog instance not listed in the instance table above. If a URL references a different host, stop and ask the user to confirm.
- NEVER print, display, log, or store the contents of any token file.
- Always read the token inline with `$(cat <TOKEN_FILE>)` — never hardcode a token value in a command.
- Always include the `X-Requested-By` header on every request — Graylog rejects requests without it.
- Always pass `-s` (silent) to `curl` and pipe through `jq` for readable output.
- NEVER run DELETE endpoints without explicit user instruction — these are irreversible.
- For search queries, always set a bounded `range` and a reasonable `limit` — an unbounded search can be slow and return excessive data.