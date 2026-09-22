---
name: postgresql-cli
description: Perform PostgreSQL operations using the psql CLI. Use for inspecting schemas, running read-only queries, and troubleshooting databases across environments.
---

# PostgreSQL CLI Skill

Use the connection table and rules below when constructing all `psql` commands.

## Configuration

Update the table below with your connection details before using this skill:

| Environment | Connection Service Name | Credential File |
|---|---|---|
| Production | `<your-prod-service>` | `~/.postgresql/prod.pgpass` |
| Staging / Non-Prod | `<your-nonprod-service>` | `~/.postgresql/staging.pgpass` |

If the target environment is unclear, ask the user before running any command.

### Prerequisites (one-time setup by user)

If `psql` is not installed or no connection service is configured, inform the user that setup is required and ask them to complete the following steps manually. Do not perform these steps yourself.

```bash
brew install libpq && brew link --force libpq
```

Configure `~/.pg_service.conf` with a named `[service]` section per environment, and store credentials in a `.pgpass` file (`hostname:port:database:username:password`, `chmod 600`). Never share credential file contents with the assistant.

## Command Format

Always use `service=<name>` from `~/.pg_service.conf` — never pass raw host/user/password on the command line, where they could leak into shell history or process listings:

```bash
psql "service=<connection-service-name>" -c "<SQL>"
```

### Examples

```bash
# Non-Prod — list tables
psql "service=<your-nonprod-service>" -c "\dt"

# Non-Prod — describe a table
psql "service=<your-nonprod-service>" -c "\d <table_name>"
```

## Available Commands

### Read-only (safe, no confirmation needed)

| Command | Description |
|---|---|
| `\l` | List databases |
| `\dt [pattern]` | List tables |
| `\d <table>` | Describe a table's schema |
| `\dn` | List schemas |
| `\du` | List roles/users |
| `\di` | List indexes |
| `SELECT ...` | Any read-only query |
| `EXPLAIN <query>` | Show a query plan without executing it |
| `\timing` | Toggle query timing (informational) |

### Mutating operations (confirm with user before running)

| Command | Description |
|---|---|
| `INSERT` / `UPDATE` | Insert or update rows |
| `CREATE TABLE` / `CREATE INDEX` | Create new schema objects |
| `ALTER TABLE` | Modify an existing table's structure |

### Never run — requires explicit human approval

| Command | Reason |
|---|---|
| `DELETE FROM ... ` (without a `WHERE` reviewed by the user) | Can irreversibly remove rows at scale |
| `DROP TABLE` / `DROP DATABASE` / `DROP SCHEMA` | Irreversible destruction of data or structure |
| `TRUNCATE` | Irreversibly wipes all rows from a table |
| `GRANT` / `REVOKE` | Alters access control, security-sensitive |
| Any DDL/DML against the production service | A human must execute changes against production data |

## Rules

- NEVER target a connection service not listed in the configuration table above. If a command references an unfamiliar service, stop and ask the user to confirm.
- NEVER pass `-h`, `-U`, or a password directly on the command line — always use a `service=` connection string backed by `~/.pg_service.conf` and `.pgpass`, so credentials never appear in shell history.
- NEVER print, display, or store the contents of `.pgpass` or any credential value.
- NEVER run `DELETE`, `DROP`, `TRUNCATE`, `GRANT`, or `REVOKE` without explicit user instruction — these are destructive or security-sensitive.
- Always run `EXPLAIN` on unfamiliar or complex queries before executing them against a large table, to catch missing indexes or full table scans.
- For any `UPDATE`/`DELETE`, always show the equivalent `SELECT ... WHERE ...` first and confirm the row count before proceeding.