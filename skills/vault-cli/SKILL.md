---
name: vault-cli
description: Perform HashiCorp Vault operations using the vault CLI. Use for reading secret metadata, checking policies, and inspecting auth methods across environments.
---

# HashiCorp Vault CLI Skill

Use the address table and rules below when constructing all `vault` commands.

## Configuration

Update the table below with your Vault cluster addresses before using this skill:

| Environment | VAULT_ADDR | Token File |
|---|---|---|
| Production | `https://<your-vault-prod-addr>:8200` | `~/.vault/prod.token` |
| Staging / Non-Prod | `https://<your-vault-staging-addr>:8200` | `~/.vault/staging.token` |

If the target environment is unclear, ask the user before running any command.

### Prerequisites (one-time setup by user)

If `vault` is not installed or not authenticated, inform the user that setup is required and ask them to complete the following steps manually. Do not perform these steps yourself.

```bash
brew install vault
vault login -address=<VAULT_ADDR>
```

Save the resulting token to the corresponding token file (see table above), `chmod 600`. Never share credential file contents with the assistant.

## Command Format

Always set `VAULT_ADDR` and `VAULT_TOKEN` explicitly per command — never rely on shell-exported environment variables that could leak into other sessions:

```bash
VAULT_ADDR=<vault-addr> VAULT_TOKEN=$(cat <TOKEN_FILE>) vault <command>
```

## Available Commands

### Read-only (safe, no confirmation needed)

| Command | Description |
|---|---|
| `vault status` | Show seal/HA status |
| `vault secrets list` | List enabled secrets engines |
| `vault list <path>` | List keys at a path (not the secret values) |
| `vault kv metadata get <path>` | Show a KV secret's metadata (versions, timestamps) — not its value |
| `vault policy list` / `vault policy read <name>` | List or show policies |
| `vault auth list` | List enabled auth methods |
| `vault token lookup` | Show info about the current token (not other tokens) |

### Mutating operations (confirm with user before running)

| Command | Description |
|---|---|
| `vault kv put <path> <data>` | Write/update a secret value |
| `vault policy write <name> <file>` | Create or update a policy |
| `vault secrets enable <type>` | Enable a new secrets engine |

### Never run — requires explicit human approval

| Command | Reason |
|---|---|
| `vault kv get <path>` | Reads a secret's actual value — a credential |
| `vault kv delete/destroy <path>` | Irreversible secret deletion |
| `vault delete <path>` | Irreversible deletion of stored data |
| `vault operator seal/unseal` | Alters cluster availability instance-wide |
| `vault token revoke/revoke-orphan` | Invalidates tokens, potentially breaking other sessions |
| `vault policy delete <name>` | Removes access control, security-sensitive |
| `vault auth disable <path>` | Disables an authentication method cluster-wide |

## Rules

- NEVER target a Vault address not listed in the configuration table above. If a command references an unfamiliar address, stop and ask the user to confirm.
- NEVER run `vault kv get`, `vault read` on a secret path, or any command that would print a secret value — this skill is for metadata/structure inspection only, not for retrieving credential contents.
- NEVER print, display, log, or store the contents of any token file or `VAULT_TOKEN` value.
- NEVER run `vault kv delete`, `destroy`, `operator seal/unseal`, or `token revoke` without explicit user instruction — these are destructive or availability-impacting.
- Always pass `VAULT_ADDR` explicitly on every command — never rely on a pre-exported environment variable, which may point to the wrong cluster.
- If a secret path or policy name is unfamiliar, use `vault list`/`vault kv metadata get` to explore structure without exposing values.
