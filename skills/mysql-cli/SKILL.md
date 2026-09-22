---
name: mysql-cli
description: Perform MySQL operations using the mysql CLI. Use for inspecting schemas, running read-only queries, and troubleshooting databases across environments.
---

# MySQL CLI Skill

Use the connection table and rules below when constructing all `mysql` commands.

## Configuration

Update the table below with your connection details before using this skill:

| Environment | Config Group Name (in `~/.my.cnf`) |
|---|---|
| Production | `<your-prod-group>` |
| Staging / Non-Prod | `<your-nonprod-group>` |

If the target environment is unclear, ask the user before running any command.

### Prerequisites (one-time setup by user)

If `mysql` is not installed or no connection group is configured, inform the user that setup is required and ask them to complete the following steps manually. Do not perform these steps yourself.

```bash
brew install mysql-client && brew link --force mysql-client
```

Configure `~/.my.cnf` with a named `[<group>]` section per environment (`host`, `port`, `database`, `user`, `password`), and `chmod 600 ~/.my.cnf`. Never share credential file contents with the assistant.

## Command Format

Always use `--defaults-group-suffix` or `--login-path` from `~/.my.cnf` — never pass raw host/user/password on the command line, where they could leak into shell history or process listings:

```bash
mysql --defaults-group-suffix=<group> -e "<SQL>"
```

### Examples

```bash
# Non-Prod — list tables
mysql --defaults-group-suffix=<your-nonprod-group> -e "SHOW TABLES;"

# Non-Prod — describe a table
mysql --defaults-group-suffix=<your-nonprod-group> -e "DESCRIBE <table_name>;"
```

## Available Commands

### Read-only (safe, no confirmation needed)

| Command | Description |
|---|---|
| `SHOW DATABASES;` | List databases |
| `SHOW TABLES;` | List tables |
| `DESCRIBE <table>;` | Describe a table's schema |
| `SHOW INDEX FROM <table>;` | List indexes for a table |
| `SHOW GRANTS FOR <user>;` | Show a user's grants (informational, not a mutation) |
| `SELECT ...` | Any read-only query |
| `EXPLAIN <query>;` | Show a query plan without executing it |
| `SHOW PROCESSLIST;` | List currently running queries |

### Mutating operations (confirm with user before running)

| Command | Description |
|---|---|
| `INSERT` / `UPDATE` | Insert or update rows |
| `CREATE TABLE` / `CREATE INDEX` | Create new schema objects |
| `ALTER TABLE` | Modify an existing table's structure |

### Never run — requires explicit human approval

| Command | Reason |
|---|---|
| `DELETE FROM ...` (without a `WHERE` reviewed by the user) | Can irreversibly remove rows at scale |
| `DROP TABLE` / `DROP DATABASE` | Irreversible destruction of data or structure |
| `TRUNCATE` | Irreversibly wipes all rows from a table |
| `GRANT` / `REVOKE` | Alters access control, security-sensitive |
| Any DDL/DML against the production group | A human must execute changes against production data |

## Rules

- NEVER target a connection group not listed in the configuration table above. If a command references an unfamiliar group, stop and ask the user to confirm.
- NEVER pass `-h`, `-u`, or `-p<password>` directly on the command line — always use `--defaults-group-suffix` backed by `~/.my.cnf`, so credentials never appear in shell history or process listings.
- NEVER print, display, or store the contents of `~/.my.cnf` or any credential value.
- NEVER run `DELETE`, `DROP`, `TRUNCATE`, `GRANT`, or `REVOKE` without explicit user instruction — these are destructive or security-sensitive.
- Always run `EXPLAIN` on unfamiliar or complex queries before executing them against a large table, to catch missing indexes or full table scans.
- For any `UPDATE`/`DELETE`, always show the equivalent `SELECT ... WHERE ...` first and confirm the row count before proceeding.