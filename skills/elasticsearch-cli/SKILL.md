---
name: elasticsearch-cli
description: Perform Elasticsearch operations using the Elasticsearch HTTP API via curl. Use for querying indices, documents, mappings, and cluster health.
---

# Elasticsearch CLI Skill

Use the instance and rules below when constructing all Elasticsearch API commands.

## Instances

Update the table below with your Elasticsearch instance URLs and corresponding credential file paths:

| Environment | Base URL | Credential File |
|---|---|---|
| Production | `https://<your-es-prod-url>` | `~/.elasticsearch/prod.token` |
| Staging | `https://<your-es-staging-url>` | `~/.elasticsearch/staging.token` |

If the target instance is unclear, ask the user before running any command.

## Prerequisites (one-time setup by user)

Generate an Elasticsearch API key for each instance you intend to use and save it to the corresponding credential file (see instance table above). Never print or display credential values. Do not perform these steps yourself.

```bash
mkdir -p ~/.elasticsearch && echo "<your-api-key>" > <CREDENTIAL_FILE> && chmod 600 <CREDENTIAL_FILE>
```

## Command Format

All API calls use `curl` with the API key read from the instance's credential file:

```bash
curl -s -H "Authorization: ApiKey $(cat <CREDENTIAL_FILE>)" \
  "<BASE_URL>/<endpoint>" | jq .
```

Example (production):
```bash
curl -s -H "Authorization: ApiKey $(cat ~/.elasticsearch/prod.token)" \
  "https://<your-es-prod-url>/_cluster/health" | jq .
```

## Available API Endpoints

### Read-only (safe, no confirmation needed)

| Endpoint | Method | Description |
|---|---|---|
| `/_cluster/health` | GET | Show cluster health status |
| `/_cat/indices?v` | GET | List all indices with stats |
| `/_cat/nodes?v` | GET | List cluster nodes |
| `/_cat/shards?v` | GET | List shard allocation |
| `/<index>/_mapping` | GET | Show field mappings for an index |
| `/<index>/_settings` | GET | Show index settings |
| `/<index>/_search` | GET/POST | Search documents (query in body) |
| `/<index>/_count` | GET/POST | Count matching documents |
| `/<index>/_doc/<id>` | GET | Get a document by ID |
| `/_cat/aliases?v` | GET | List index aliases |
| `/_nodes/stats` | GET | Show node-level statistics |

### Example query

```bash
curl -s -H "Authorization: ApiKey $(cat <CREDENTIAL_FILE>)" \
  -H "Content-Type: application/json" \
  -X POST "<BASE_URL>/<index>/_search" \
  -d '{"query": {"match": {"field": "value"}}, "size": 10}' | jq .
```

### Mutating operations (confirm with user before running)

| Endpoint | Method | Description |
|---|---|---|
| `/<index>/_doc/<id>` | PUT/POST | Create or update a document |
| `/<index>` | PUT | Create a new index |
| `/<index>/_mapping` | PUT | Add fields to an index mapping |
| `/<index>/_settings` | PUT | Update index settings |
| `/_aliases` | POST | Update index aliases |

### Never run — requires explicit human approval

| Endpoint | Method | Reason |
|---|---|---|
| `/<index>` | DELETE | Irreversibly deletes the index and all its documents |
| `/<index>/_doc/<id>` | DELETE | Irreversible document deletion |
| `/<index>/_delete_by_query` | POST | Bulk-deletes documents matching a query — irreversible |
| `/_cluster/settings` | PUT | Changes cluster-wide configuration |
| `/<index>/_close` / `/<index>/_open` | POST | Changes index availability cluster-wide |

## Rules

- NEVER target any Elasticsearch instance not listed in the instance table above. If a URL references a different host, stop and ask the user to confirm.
- Update the instance table with your actual Elasticsearch URLs before using this skill.
- NEVER print, display, log, or store the contents of any credential file.
- Always read the API key inline with `$(cat <CREDENTIAL_FILE>)` — never hardcode a credential value in a command.
- Always pass `-s` (silent) to `curl` and pipe through `jq` for readable output.
- NEVER run DELETE endpoints or `_delete_by_query` without explicit user instruction — these are irreversible.
- For search queries, always set a reasonable `size` limit — an unbounded query on a large index can return excessive data.
- If the required credential file does not exist, inform the user and stop. Do not attempt to find or guess the credential.