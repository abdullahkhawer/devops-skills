---
name: kibana-cli
description: Perform Kibana operations using the Kibana HTTP API via curl. Use for querying saved objects, dashboards, index patterns, and alerts.
---

# Kibana CLI Skill

Use the instance and rules below when constructing all Kibana API commands.

## Instances

Update the table below with your Kibana instance URLs and corresponding credential file paths:

| Environment | Base URL | Credential File |
|---|---|---|
| Production | `https://<your-kibana-prod-url>` | `~/.kibana/prod.token` |
| Staging | `https://<your-kibana-staging-url>` | `~/.kibana/staging.token` |

If the target instance is unclear, ask the user before running any command.

## Prerequisites (one-time setup by user)

Generate a Kibana API key for each instance you intend to use and save it to the corresponding credential file (see instance table above). Never print or display credential values. Do not perform these steps yourself.

```bash
mkdir -p ~/.kibana && echo "<your-api-key>" > <CREDENTIAL_FILE> && chmod 600 <CREDENTIAL_FILE>
```

## Command Format

All API calls use `curl` with the API key read from the instance's credential file, and always require the `kbn-xsrf` header:

```bash
curl -s -H "Authorization: ApiKey $(cat <CREDENTIAL_FILE>)" \
  -H "kbn-xsrf: true" \
  "<BASE_URL>/api/<endpoint>" | jq .
```

Example (production):
```bash
curl -s -H "Authorization: ApiKey $(cat ~/.kibana/prod.token)" \
  -H "kbn-xsrf: true" \
  "https://<your-kibana-prod-url>/api/status" | jq .
```

## Available API Endpoints

### Read-only (safe, no confirmation needed)

| Endpoint | Method | Description |
|---|---|---|
| `/api/status` | GET | Check Kibana health/status |
| `/api/saved_objects/_find?type=<type>` | GET | Find saved objects (dashboards, visualizations, index-patterns) |
| `/api/saved_objects/<type>/<id>` | GET | Get a specific saved object |
| `/api/index_patterns/index_pattern/<id>` | GET | Get an index pattern |
| `/api/alerting/rules/_find` | GET | List alerting rules |
| `/api/alerting/rule/<id>` | GET | Get a specific alerting rule |
| `/api/spaces/space` | GET | List Kibana spaces |
| `/api/data_views` | GET | List data views |

### Mutating operations (confirm with user before running)

| Endpoint | Method | Description |
|---|---|---|
| `/api/saved_objects/<type>/<id>` | POST/PUT | Create or update a saved object (dashboard, visualization) |
| `/api/saved_objects/_import` | POST | Import saved objects from an NDJSON file |
| `/api/data_views/data_view` | POST | Create a data view |
| `/api/alerting/rule/<id>` | PUT | Update an alerting rule |
| `/api/alerting/rule/<id>/_disable` / `_enable` | POST | Enable or disable an alerting rule |

### Never run — requires explicit human approval

| Endpoint | Method | Reason |
|---|---|---|
| `/api/saved_objects/<type>/<id>` | DELETE | Irreversible deletion of a dashboard/visualization/index-pattern |
| `/api/saved_objects/_bulk_delete` | POST | Bulk-deletes saved objects — irreversible |
| `/api/alerting/rule/<id>` | DELETE | Irreversibly deletes an alerting rule |
| `/api/spaces/space/<id>` | DELETE | Irreversibly deletes a Kibana space and its contents |

## Rules

- NEVER target any Kibana instance not listed in the instance table above. If a URL references a different host, stop and ask the user to confirm.
- Update the instance table with your actual Kibana URLs before using this skill.
- NEVER print, display, log, or store the contents of any credential file.
- Always read the API key inline with `$(cat <CREDENTIAL_FILE>)` — never hardcode a credential value in a command.
- Always include the `kbn-xsrf: true` header on every request — Kibana rejects mutating requests without it.
- Always pass `-s` (silent) to `curl` and pipe through `jq` for readable output.
- NEVER run DELETE operations or bulk deletes without explicit user instruction — these are irreversible.
- If a saved object ID or type is unfamiliar, look it up with `_find` first before mutating.
- If the required credential file does not exist, inform the user and stop. Do not attempt to find or guess the credential.