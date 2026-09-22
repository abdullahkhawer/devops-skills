---
name: chef-cli
description: Perform Chef configuration management operations using the knife and chef CLI tools. Use for inspecting nodes, cookbooks, roles, and environments on a Chef server.
---

# Chef CLI Skill

Use the server/organization table and rules below when constructing all `knife`/`chef` commands.

## Configuration

Update the table below with your Chef server organizations before using this skill:

| Environment | Chef Server URL | Organization | Config File |
|---|---|---|---|
| Production | `<your-prod-chef-server-url>` | `<your-prod-org>` | `~/.chef/prod_knife.rb` |
| Staging / Non-Prod | `<your-nonprod-chef-server-url>` | `<your-nonprod-org>` | `~/.chef/staging_knife.rb` |

If the target environment is unclear, ask the user before running any command.

### Prerequisites (one-time setup by user)

If `knife`/`chef` is not installed or no config exists, inform the user that setup is required and ask them to complete the following steps manually. Do not perform these steps yourself.

```bash
brew install --cask chef-workstation
knife configure --config-file <config-file>
```

## Command Format

Always pass `--config` explicitly — never rely on the default `~/.chef/knife.rb`, which may point to the wrong organization:

```bash
knife <command> --config <config-file> [options]
```

### Examples

```bash
# Non-Prod
knife node list --config ~/.chef/staging_knife.rb
knife node show <node-name> --config ~/.chef/staging_knife.rb
```

## Available Commands

### Read-only (safe, no confirmation needed)

| Command | Description |
|---|---|
| `knife node list` / `knife node show <name>` | List or show node details |
| `knife cookbook list` / `knife cookbook show <name>` | List or show cookbooks |
| `knife role list` / `knife role show <name>` | List or show roles |
| `knife environment list` / `knife environment show <name>` | List or show environments |
| `knife search node "<query>"` | Search nodes |
| `knife status` | Show recent chef-client run status across nodes |
| `chef exec chef-client --why-run` | Dry-run a cookbook against local resources, showing what would change |

### Mutating operations (confirm with user before running)

| Command | Description |
|---|---|
| `knife cookbook upload <name>` | Upload a cookbook to the Chef server |
| `knife role create/from file` | Create or update a role |
| `knife environment from file` | Create or update an environment |
| `knife node run_list add <node> <item>` | Modify a node's run list |

### Never run — requires explicit human approval

| Command | Reason |
|---|---|
| `knife node delete <name>` | Irreversible node deregistration |
| `knife cookbook delete <name>` | Irreversible cookbook deletion |
| `knife environment delete <name>` | Irreversible environment deletion |
| `knife ssh <query> "<command>"` | Executes arbitrary commands on live nodes |
| `chef-client` run directly against production nodes | Applies real configuration changes to live production infrastructure |

## Rules

- NEVER target a Chef server/organization not listed in the configuration table above. If a command references an unfamiliar organization, stop and ask the user to confirm.
- Always pass `--config` explicitly on every command — never rely on the default `knife.rb`.
- NEVER run `knife ssh`, `chef-client` against production nodes, or any deletion command without explicit user instruction.
- NEVER print, display, or store the contents of `.pem` client key files used for Chef server authentication.
- Always run `chef-client --why-run` (dry-run) before suggesting a real cookbook run against nodes.
- If a node name or cookbook version is unfamiliar, look it up with `knife node show`/`knife cookbook show` first.
