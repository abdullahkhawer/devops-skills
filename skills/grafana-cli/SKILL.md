---
name: grafana-cli
description: Perform Grafana operations using the Grafana HTTP API via curl. Use for querying dashboards, datasources, alerts, and annotations.
---

# Grafana CLI Skill

Use the instance and rules below when constructing all Grafana API commands.

## Instances

Update the table below with your Grafana instance URLs and corresponding token file paths:

| Environment | Base URL | Token File |
|---|---|---|
| Production | `https://<your-grafana-prod-url>` | `~/.grafana/prod.token` |
| Staging | `https://<your-grafana-staging-url>` | `~/.grafana/staging.token` |

If the target instance is unclear, ask the user before running any command.

## Prerequisites (one-time setup by user)

Generate a Grafana API token for each instance you intend to use and save it to the corresponding token file (see instance table above). Never print or display token values. Do not perform these steps yourself.

For each instance:
1. Navigate to `<BASE_URL>` → **Administration** → **Users and access** → **Service accounts** → **Add service account**
2. Enter a display name and role, then click **Create**
3. On the service account page, click **Add service account token** and copy the generated token
4. Save it:

```bash
mkdir -p ~/.grafana && echo "<your-api-token>" > <TOKEN_FILE> && chmod 600 <TOKEN_FILE>
```

## Command Format

All API calls use `curl` with the token read from the instance's token file (see instance table above):

```bash
curl -s -H "Authorization: Bearer $(cat <TOKEN_FILE>)" \
  "<BASE_URL>/api/<endpoint>" | jq .
```

Example (production):
```bash
curl -s -H "Authorization: Bearer $(cat ~/.grafana/prod.token)" \
  "https://<your-grafana-prod-url>/api/health" | jq .
```

## Available API Endpoints

### Read-only (safe, no confirmation needed)

| Endpoint | Method | Description |
|---|---|---|
| `/api/org` | GET | Get current organisation info |
| `/api/user` | GET | Get current authenticated user |
| `/api/datasources` | GET | List all datasources |
| `/api/datasources/<id>` | GET | Get datasource by ID |
| `/api/dashboards/home` | GET | Get the home dashboard |
| `/api/search?query=<term>` | GET | Search dashboards and folders |
| `/api/dashboards/uid/<uid>` | GET | Get dashboard by UID |
| `/api/folders` | GET | List all folders |
| `/api/folders/<uid>` | GET | Get folder by UID |
| `/api/alerts` | GET | List legacy alerts |
| `/api/v1/provisioning/alert-rules` | GET | List all alert rules |
| `/api/v1/provisioning/alert-rules/<uid>` | GET | Get alert rule by UID |
| `/api/annotations` | GET | List annotations |
| `/api/health` | GET | Check Grafana health |

### Mutating operations (confirm with user before running)

| Endpoint | Method | Description |
|---|---|---|
| `/api/dashboards/db` | POST | Create or update a dashboard |
| `/api/folders` | POST | Create a folder |
| `/api/folders/<uid>` | PUT | Update a folder |
| `/api/annotations` | POST | Create an annotation |
| `/api/annotations/<id>` | PUT | Update an annotation |
| `/api/v1/provisioning/alert-rules` | POST | Create an alert rule |
| `/api/v1/provisioning/alert-rules/<uid>` | PUT | Update an alert rule |

### Never run — requires explicit human approval

| Endpoint | Method | Reason |
|---|---|---|
| `/api/dashboards/uid/<uid>` | DELETE | Irreversible dashboard deletion |
| `/api/folders/<uid>` | DELETE | Deletes folder and all its dashboards |
| `/api/datasources/<id>` | DELETE | Removes datasource used by dashboards |
| `/api/annotations/<id>` | DELETE | Irreversible annotation deletion |
| `/api/v1/provisioning/alert-rules/<uid>` | DELETE | Removes alert rule permanently |
| `/api/admin/users/<id>` | DELETE | Deactivates a user account |

## Rules

- NEVER target any Grafana instance not listed in the instance table above. If a URL references a different host, stop and ask the user to confirm.
- Update the instance table with your actual Grafana URLs before using this skill.
- NEVER print, display, log, or store the contents of any token file or credential value.
- Always read the token inline with `$(cat <TOKEN_FILE>)` — never hardcode a token value in a command.
- Always pass `-s` (silent) to `curl` to suppress progress output, and pipe through `jq` for readable output.
- NEVER run DELETE or destructive PUT/POST operations without explicit user instruction.
- If a dashboard UID, datasource ID, or folder UID is unfamiliar, look it up with a read-only call first.
- If the required token file does not exist, inform the user and stop. Do not attempt to find or guess the token.
- All list endpoints may be paginated — check the response for `page` and `perpage` fields and iterate if needed.
