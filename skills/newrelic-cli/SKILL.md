---
name: newrelic-cli
description: Perform New Relic operations using the newrelic CLI and NerdGraph API. Use for querying entities, applications, alerts, and NRQL data.
---

# New Relic CLI Skill

Use the profile table and rules below when constructing all `newrelic` CLI commands.

## Configuration

Update the table below with your New Relic CLI profile names:

| Environment | Profile | Account ID |
|---|---|---|
| Production | `<your-prod-profile>` | `<your-prod-account-id>` |
| Staging | `<your-staging-profile>` | `<your-staging-account-id>` |

If the target environment is unclear, ask the user before running any command.

### Prerequisites (one-time setup by user)

If not already authenticated, inform the user that setup is required and ask them to complete the following steps manually. Do not perform these steps yourself.

```bash
brew install newrelic-cli
newrelic profile configure --profile <your-profile-name>
newrelic profile list
```

## Command Format

Always pass `--profile` explicitly — never rely on the default profile:

```bash
newrelic <command> --profile <profile> [options]
```

### Examples

```bash
# Run a NRQL query
newrelic nrql query --profile <your-staging-profile> --accountId <your-staging-account-id> --query "SELECT count(*) FROM Transaction SINCE 1 hour ago"

# List entities
newrelic entity search --profile <your-staging-profile> --name "<service-name>"
```

## Available Commands

### Read-only (safe, no confirmation needed)

| Command | Description |
|---|---|
| `newrelic nrql query` | Run a NRQL query |
| `newrelic entity search` | Search for entities (services, hosts) |
| `newrelic entity get` | Get details of an entity |
| `newrelic apm application list/get` | List or describe APM applications |
| `newrelic alerts policy list` | List alert policies |
| `newrelic alerts condition list` | List alert conditions |
| `newrelic profile list` | List configured CLI profiles |

### Mutating operations (confirm with user before running)

| Command | Description |
|---|---|
| `newrelic alerts policy create` | Create an alert policy |
| `newrelic alerts condition create` | Create an alert condition |
| `newrelic edge create` | Create an edge tracing/ingest resource |
| `newrelic workload create` | Create a workload |

### Never run — requires explicit human approval

| Command | Reason |
|---|---|
| `newrelic alerts policy delete` | Irreversible policy deletion |
| `newrelic alerts condition delete` | Irreversible condition deletion |
| `newrelic apm application delete` | Removes an application entity |
| `newrelic entity delete` | Irreversibly deletes an entity |

## Rules

- NEVER target a profile not listed in the configuration table above. If a command references an unfamiliar profile, stop and ask the user to confirm.
- Always pass `--profile` explicitly on every command — never rely on the default profile, which may point to the wrong account.
- NEVER run delete operations without explicit user instruction — these are irreversible.
- NEVER print, display, or store New Relic API/license/ingest keys.
- For NRQL queries, always include a `SINCE`/`UNTIL` time bound and a reasonable `LIMIT` to avoid excessive result sets.
- If an entity GUID, policy ID, or account ID is unfamiliar, look it up with `entity search` or `alerts policy list` first.