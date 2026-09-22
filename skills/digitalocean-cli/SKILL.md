---
name: digitalocean-cli
description: Use the correct DigitalOcean CLI (doctl) context and region when running DigitalOcean commands based on the target environment.
---

# DigitalOcean CLI Skill

When running `doctl` commands, always pass the `--context` flag based on the target environment, and specify the region explicitly for resource-creating commands.

## Configuration

Update the context map below with your `doctl` auth context names:

| Environment | Context | Default Region |
|---|---|---|
| Production | `<your-prod-context>` | `<your-prod-region>` (e.g. `nyc3`) |
| Staging / Non-Prod | `<your-nonprod-context>` | `<your-nonprod-region>` (e.g. `sgp1`) |

### Prerequisites (one-time setup by user)

If not already authenticated, inform the user that setup is required and ask them to complete the following steps manually. Do not perform these steps yourself.

```bash
brew install doctl
doctl auth init --context <your-context-name>
doctl auth list
```

## Command Format

```bash
doctl <resource> <command> --context <context> [--region <region>] [options]
```

### Examples

```bash
# Production
doctl compute droplet list --context <your-prod-context>

# Staging / Non-Prod
doctl compute droplet list --context <your-nonprod-context>
```

## Available Commands

### Read-only (safe, no confirmation needed)

| Command pattern | Description |
|---|---|
| `doctl compute droplet list/get` | List or describe droplets |
| `doctl compute domain list` | List DNS domains |
| `doctl kubernetes cluster list/get` | List or describe Kubernetes clusters |
| `doctl databases list/get` | List or describe managed databases |
| `doctl monitoring alert-policy list` | List monitoring alert policies |
| `doctl account get` | Show account information |
| `doctl auth list` | List authenticated contexts |

### Mutating operations (confirm with user before running)

| Command pattern | Description |
|---|---|
| `doctl compute droplet create` | Create a new droplet |
| `doctl kubernetes cluster create` | Create a new Kubernetes cluster |
| `doctl databases create` | Create a new managed database |
| `doctl compute droplet resize` | Resize a droplet |

### Never run — requires explicit human approval

| Command pattern | Reason |
|---|---|
| `doctl compute droplet delete` | Irreversible resource deletion |
| `doctl kubernetes cluster delete` | Irreversibly deletes a Kubernetes cluster |
| `doctl databases delete` | Irreversibly destroys a managed database |
| `doctl compute droplet-action power-off/reboot` | Disrupts running workloads |
| `doctl compute firewall delete` | Removes network security controls |

## Rules

- Always specify `--context` explicitly — never rely on the default `doctl` context, as it may point to the wrong account.
- Always specify `--region` explicitly for resource-creating commands — never rely on defaults.
- If the target environment is unclear, ask the user before running the command.
- NEVER run any command in the "Never run" table above without explicit user instruction — a human must execute these, especially against production.
- NEVER read or output API tokens stored in `doctl` auth contexts or any credential value.
- If unsure which account a context belongs to, run `doctl account get --context <context>` to verify before proceeding.