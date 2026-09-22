---
name: salesforce-cli
description: Perform Salesforce operations using the Salesforce CLI (sf). Use for querying orgs, records, metadata, and deployments.
---

# Salesforce CLI Skill

Use the org alias table and rules below when constructing all `sf` commands.

## Configuration

Update the table below with your Salesforce org aliases before using this skill:

| Environment | Org Alias |
|---|---|
| Production | `<your-prod-org-alias>` |
| Staging / Sandbox | `<your-sandbox-org-alias>` |

If the target org is unclear, ask the user before running any command.

### Prerequisites (one-time setup by user)

If not already authenticated, inform the user that setup is required and ask them to complete the following steps manually. Do not perform these steps yourself.

```bash
brew install sf
sf org login web --alias <your-org-alias> --instance-url <your-instance-url>
sf org list
```

## Command Format

Always pass `--target-org` explicitly — never rely on the default org:

```bash
sf <topic> <command> --target-org <org-alias> [options]
```

### Examples

```bash
# Query records (SOQL)
sf data query --query "SELECT Id, Name FROM Account LIMIT 10" --target-org <your-sandbox-org-alias>

# List installed metadata components
sf project retrieve start --metadata ApexClass --target-org <your-sandbox-org-alias>
```

## Available Commands

### Read-only (safe, no confirmation needed)

| Command | Description |
|---|---|
| `sf org list` | List authenticated orgs |
| `sf org display --target-org <alias>` | Show org details |
| `sf data query --query "<SOQL>"` | Run a SOQL query |
| `sf data get record --sobject <type> --record-id <id>` | Get a specific record |
| `sf project retrieve start --metadata <type>` | Retrieve metadata (read from org, writes locally) |
| `sf apex run --file <file>` (read-only Apex, e.g. `System.debug`) | Execute anonymous Apex — only if the script has no DML |
| `sf org open --target-org <alias> --url-only` | Get the org login URL without opening a browser |

### Mutating operations (confirm with user before running)

| Command | Description |
|---|---|
| `sf data create record` | Create a new record |
| `sf data update record` | Update an existing record |
| `sf project deploy start` | Deploy metadata to an org |
| `sf apex run --file <file>` (with DML statements) | Execute anonymous Apex that modifies data |

### Never run — requires explicit human approval

| Command | Reason |
|---|---|
| `sf data delete record` | Irreversible record deletion |
| `sf data delete bulk` | Bulk-deletes records — irreversible |
| `sf project deploy start` against production org | Deploys code/config changes to live production |
| `sf org delete scratch` | Irreversibly deletes a scratch org and its data |
| `sf force:data:tree:import` (bulk load) against production | Bulk-mutates production data |

## Rules

- NEVER target an org alias not listed in the configuration table above. If a command references an unfamiliar org, stop and ask the user to confirm.
- Always pass `--target-org` explicitly on every command — never rely on the default org set via `sf config set target-org`.
- NEVER run `sf project deploy start` or any bulk data mutation against the production org alias without explicit user instruction.
- NEVER print, display, or store OAuth tokens, session IDs, or client secrets from `sf org display --verbose`.
- If a record ID, object type, or org alias is unfamiliar, look it up with a read-only `sf data query` first.
- Always wrap SOQL queries in double quotes and use `LIMIT` when exploring unfamiliar data to avoid excessive result sets.