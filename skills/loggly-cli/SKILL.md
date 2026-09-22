---
name: loggly-cli
description: Perform Loggly log management operations using the Loggly HTTP API via curl. Use for searching logs and inspecting saved searches and source groups.
---

# Loggly CLI Skill

Use the instance and rules below when constructing all Loggly API commands.

## Configuration

Update the table below with your Loggly subdomain and credential file paths:

| Environment | Subdomain | Token File |
|---|---|---|
| Production | `<your-prod-subdomain>` | `~/.loggly/prod.token` |
| Staging | `<your-staging-subdomain>` | `~/.loggly/staging.token` |

If the target environment is unclear, ask the user before running any command.

## Prerequisites (one-time setup by user)

Generate a Loggly API token (**Account Settings → API Tokens**) for each environment and save it to the corresponding token file. Never print or display token values. Do not perform these steps yourself.

```bash
mkdir -p ~/.loggly && echo "<your-api-token>" > <TOKEN_FILE> && chmod 600 <TOKEN_FILE>
```

## Command Format

```bash
curl -s -u "$(cat <TOKEN_FILE>):" \
  "https://<subdomain>.loggly.com/apiv2/<endpoint>" | jq .
```

## Available API Endpoints

### Read-only (safe, no confirmation needed)

| Endpoint | Method | Description |
|---|---|---|
| `/apiv2/search` | GET | Run a search query, returns an `rsid` |
| `/apiv2/events?rsid=<id>` | GET | Fetch results for a search job |
| `/apiv2/search/list` | GET | List saved searches |
| `/apiv2/sources/groups` | GET | List source groups |
| `/apiv2/fields` | GET | List indexed fields |

### Example — search logs

```bash
RSID=$(curl -s -u "$(cat <TOKEN_FILE>):" \
  "https://<subdomain>.loggly.com/apiv2/search?q=<query>&from=-1h&until=now&size=50" | jq -r '.rsid.id')

curl -s -u "$(cat <TOKEN_FILE>):" \
  "https://<subdomain>.loggly.com/apiv2/events?rsid=$RSID" | jq .
```

### Mutating operations (confirm with user before running)

| Endpoint | Method | Description |
|---|---|---|
| `/apiv2/search/list` | POST | Create a saved search |
| `/apiv2/sources/groups` | POST | Create a source group |

### Never run — requires explicit human approval

| Endpoint | Method | Reason |
|---|---|---|
| `/apiv2/search/list/<id>` | DELETE | Irreversible saved search deletion |
| `/apiv2/sources/groups/<id>` | DELETE | Irreversible source group deletion |
| `/apiv2/customer/settings` | POST | Changes account-wide settings such as retention |

## Rules

- NEVER target a Loggly subdomain not listed in the configuration table above. If a request references an unfamiliar subdomain, stop and ask the user to confirm.
- NEVER print, display, log, or store the contents of any token file.
- Always read the token inline with `$(cat <TOKEN_FILE>)` — never hardcode a token value in a command.
- Always pass `-s` (silent) to `curl` and pipe through `jq` for readable output.
- NEVER run DELETE endpoints or account-wide settings changes without explicit user instruction.
- Always bound the search with `from`/`until` and a `size` limit — an unbounded query can be slow and return excessive data.
- If a saved search ID or source group ID is unfamiliar, look it up with a read-only call first.