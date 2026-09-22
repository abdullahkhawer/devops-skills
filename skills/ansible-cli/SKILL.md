---
name: ansible-cli
description: Perform Ansible operations using ansible and ansible-playbook. Use for running playbooks, checking inventory, and validating configuration against target environments.
---

# Ansible CLI Skill

Use the inventory table and rules below when constructing all `ansible`/`ansible-playbook` commands.

## Configuration

Update the table below with your inventory file paths before using this skill:

| Environment | Inventory File | Vault Password File |
|---|---|---|
| Production | `<your-prod-inventory>` | `<your-prod-vault-pass-file>` |
| Staging / Non-Prod | `<your-nonprod-inventory>` | `<your-nonprod-vault-pass-file>` |

If the target environment is unclear, ask the user before running any command.

### Prerequisites (one-time setup by user)

If `ansible` is not installed or no inventory is configured, inform the user that setup is required and ask them to complete the following steps manually. Do not perform these steps yourself.

```bash
brew install ansible
ansible-inventory -i <inventory-file> --list
```

## Command Format

Always pass `-i <inventory-file>` explicitly — never rely on a default inventory:

```bash
ansible-playbook -i <inventory-file> <playbook.yml> [options]
```

### Examples

```bash
# Dry run against non-prod
ansible-playbook -i <your-nonprod-inventory> <playbook.yml> --check --diff

# List hosts targeted by a play
ansible-playbook -i <your-nonprod-inventory> <playbook.yml> --list-hosts
```

## Available Commands

### Read-only (safe, no confirmation needed)

| Command | Description |
|---|---|
| `ansible-inventory -i <inv> --list/--graph` | Show inventory structure |
| `ansible <group> -i <inv> -m ping` | Check host connectivity (no state change) |
| `ansible <group> -i <inv> -m setup` | Gather facts about hosts |
| `ansible-playbook -i <inv> <playbook> --check --diff` | Dry-run a playbook, showing what would change |
| `ansible-playbook -i <inv> <playbook> --list-hosts` | List hosts a playbook would target |
| `ansible-playbook -i <inv> <playbook> --list-tasks` | List tasks a playbook would run |
| `ansible-lint <playbook>` | Lint a playbook for issues |
| `ansible-doc <module>` | Show documentation for a module |
| `ansible-vault view <file>` | View encrypted vault contents (still a credential — handle per Rules) |

### Mutating operations (confirm with user before running)

| Command | Description |
|---|---|
| `ansible-playbook -i <inv> <playbook>` | Run a playbook, applying real changes to target hosts |
| `ansible <group> -i <inv> -m <module> -a "<args>"` | Run an ad-hoc module against hosts |
| `ansible-vault encrypt/decrypt <file>` | Encrypt or decrypt a vault file |

### Never run — requires explicit human approval

| Command | Reason |
|---|---|
| `ansible-playbook -i <inv> <playbook>` against production inventory | Mutates live production infrastructure |
| `ansible <group> -m reboot/shutdown` | Disrupts running workloads |
| `ansible <group> -m service -a "state=stopped"` | Stops services on live hosts |
| `ansible-playbook --force-handlers` combined with destructive tasks | Forces handlers to run even after failures, bypassing safety checks |

## Rules

- NEVER target an inventory file not listed in the configuration table above. If a command references an unfamiliar inventory, stop and ask the user to confirm.
- Always pass `-i <inventory-file>` explicitly on every command — never rely on `ansible.cfg` defaults, which may silently target the wrong hosts.
- Always run with `--check --diff` first and show the output before ever suggesting a real run — never apply blind, especially against production.
- NEVER run a playbook or ad-hoc command against a production inventory without explicit user instruction.
- NEVER print, display, or store the contents of `ansible-vault`-encrypted files or vault password files — treat them as credentials.
- If a host group or variable name is unfamiliar, inspect it with `ansible-inventory` first.
- Prefer idempotent modules; flag any `shell`/`command` module usage that could be replaced with a dedicated idempotent module.
