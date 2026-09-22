---
name: appdynamics-cli
description: Perform AppDynamics operations using the AppDynamics HTTP API via curl. Use for querying applications, metrics, health rules, and events.
---

# AppDynamics CLI Skill

Use the instance and rules below when constructing all AppDynamics API commands.

## Instances

Update the table below with your AppDynamics controller URLs and credential file paths:

| Environment | Controller URL | Account | Credential File |
|---|---|---|---|
| Production | `https://<your-controller>.saas.appdynamics.com` | `<your-prod-account>` | `~/.appdynamics/prod.token` |
| Staging | `https://<your-controller>.saas.appdynamics.com` | `<your-staging-account>` | `~/.appdynamics/staging.token` |

If the target instance is unclear, ask the user before running any command.

## Prerequisites (one-time setup by user)

Generate an AppDynamics API client (**Administration → API Clients**) for each environment and save `<client-id>@<account>:<client-secret>` to the corresponding credential file. Never print or display credential values. Do not perform these steps yourself.

```bash
mkdir -p ~/.appdynamics && echo "<client-id>@<account>:<client-secret>" > <CREDENTIAL_FILE> && chmod 600 <CREDENTIAL_FILE>
```

## Command Format

AppDynamics uses OAuth — obtain a short-lived bearer token first, then call the API:

```bash
TOKEN=$(curl -s -X POST "<CONTROLLER_URL>/controller/api/oauth/access_token" \
  --data-urlencode "grant_type=client_credentials" \
  --data-urlencode "client_id=$(cut -d: -f1 <CREDENTIAL_FILE>)" \
  --data-urlencode "client_secret=$(cut -d: -f2 <CREDENTIAL_FILE>)" | jq -r '.access_token')

curl -s -H "Authorization: Bearer $TOKEN" \
  "<CONTROLLER_URL>/controller/rest/<endpoint>?output=JSON" | jq .
```

## Available API Endpoints

### Read-only (safe, no confirmation needed)

| Endpoint | Method | Description |
|---|---|---|
| `/controller/rest/applications` | GET | List applications |
| `/controller/rest/applications/<id>/metrics` | GET | Query metric data |
| `/controller/rest/applications/<id>/problems/healthrule-violations` | GET | List health rule violations |
| `/controller/rest/applications/<id>/events` | GET | List events |
| `/controller/rest/applications/<id>/nodes` | GET | List monitored nodes |

### Mutating operations (confirm with user before running)

| Endpoint | Method | Description |
|---|---|---|
| `/controller/rest/applications/<id>/healthrules` | POST | Create a health rule |
| `/controller/alerting/rest/v1/applications/<id>/policies` | POST | Create an alerting policy |

### Never run — requires explicit human approval

| Endpoint | Method | Reason |
|---|---|---|
| `/controller/rest/applications/<id>/healthrules/<id>` | DELETE | Irreversible health rule deletion |
| `/controller/rest/applications/<id>` | DELETE | Irreversibly deletes an application and its data |
| `/controller/rest/applications/<id>/nodes/<id>` | DELETE | Removes a monitored node |

## Rules

- NEVER target any AppDynamics controller not listed in the instance table above. If a URL references a different host, stop and ask the user to confirm.
- NEVER print, display, log, or store the client secret, credential file contents, or issued bearer token.
- Always fetch a fresh bearer token per session rather than caching it in a file — it is short-lived and sensitive.
- Always pass `-s` (silent) to `curl` and pipe through `jq` for readable output.
- NEVER run DELETE endpoints without explicit user instruction — these are irreversible.
- If an application ID or node ID is unfamiliar, look it up with a read-only call first.