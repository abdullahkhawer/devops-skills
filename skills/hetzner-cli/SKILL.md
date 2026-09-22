---
name: hetzner-cli
description: Use the correct Hetzner Cloud CLI (hcloud) context when running Hetzner Cloud commands based on the target environment.
---

# Hetzner Cloud CLI Skill

When running `hcloud` commands, always pass the `--context` flag based on the target environment.

## Configuration

Update the context map below with your `hcloud` CLI context names:

| Environment | Context |
|---|---|
| Production | `<your-prod-context>` |
| Staging / Non-Prod | `<your-nonprod-context>` |

### Prerequisites (one-time setup by user)

If not already authenticated, inform the user that setup is required and ask them to complete the following steps manually. Do not perform these steps yourself.

```bash
brew install hcloud
hcloud context create <your-context-name>
hcloud context list
```

## Command Format

```bash
hcloud --context <context> <resource> <command> [options]
```

### Examples

```bash
# Production
hcloud --context <your-prod-context> server list

# Staging / Non-Prod
hcloud --context <your-nonprod-context> server list
```

## Available Commands

### Read-only (safe, no confirmation needed)

| Command | Description |
|---|---|
| `server list` / `server describe <name>` | List or describe servers |
| `volume list` / `volume describe <name>` | List or describe volumes |
| `load-balancer list` / `load-balancer describe <name>` | List or describe load balancers |
| `network list` / `network describe <name>` | List or describe networks |
| `firewall list` / `firewall describe <name>` | List or describe firewalls |
| `context list` / `context active` | Show CLI context information |

### Mutating operations (confirm with user before running)

| Command | Description |
|---|---|
| `server create` | Create a new server |
| `volume create` | Create a new volume |
| `load-balancer create` | Create a new load balancer |
| `server resize` | Resize a server |

### Never run — requires explicit human approval

| Command | Reason |
|---|---|
| `server delete` | Irreversible resource deletion |
| `volume delete` | Irreversibly deletes a volume and its data |
| `load-balancer delete` | Irreversibly deletes a load balancer |
| `server reset` / `server poweroff` | Disrupts running workloads |
| `firewall delete` | Removes network security controls |

## Rules

- Always specify `--context` explicitly — never rely on the active `hcloud` context, as it may point to the wrong project.
- If the target environment is unclear, ask the user before running the command.
- NEVER run any command in the "Never run" table above without explicit user instruction — a human must execute these, especially against production.
- NEVER read or output the API token stored in `hcloud` CLI contexts or any credential value.
- If unsure which project a context belongs to, run `hcloud --context <context> context active` to verify before proceeding.
