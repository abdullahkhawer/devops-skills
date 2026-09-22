---
name: azure-cli
description: Use the correct Azure CLI subscription and region when running Azure commands based on the target environment.
---

# Azure CLI Skill

When running `az` commands, always pass the `--subscription` flag based on the target environment, and specify the region explicitly for resource-creating commands.

## Configuration

Update the subscription map below with your Azure subscription IDs/names:

| Environment | Subscription | Resource Group |
|---|---|---|
| Production | `<your-prod-subscription>` | `<your-prod-resource-group>` |
| Staging / Non-Prod | `<your-nonprod-subscription>` | `<your-nonprod-resource-group>` |
| Sandbox / Dev | `<your-sandbox-subscription>` | `<your-sandbox-resource-group>` |

## Default Region

Set your default region here: **`<your-default-region>`** (e.g. `eastus`, `westeurope`).

Always pass `--location <your-default-region>` unless the user specifies a different region.

### Prerequisites (one-time setup by user)

If not already authenticated, inform the user that setup is required and ask them to complete the following steps manually. Do not perform these steps yourself.

```bash
brew install azure-cli
az login
az account list
```

## Command Format

```bash
az <group> <command> --subscription <subscription> [--resource-group <rg>] [--location <region>] [options]
```

### Examples

```bash
# Production
az vm list --subscription <your-prod-subscription> --resource-group <your-prod-resource-group>

# Staging / Non-Prod
az vm list --subscription <your-nonprod-subscription> --resource-group <your-nonprod-resource-group>
```

## Available Commands

### Read-only (safe, no confirmation needed)

| Command pattern | Description |
|---|---|
| `az * list` | List any resource type |
| `az * show` | Show details of a resource |
| `az account list` / `az account show` | Show subscription/account info |
| `az group list` | List resource groups |
| `az monitor log-analytics query` | Query Log Analytics |
| `az monitor metrics list` | List metrics for a resource |

### Mutating operations (confirm with user before running)

| Command pattern | Description |
|---|---|
| `az * create` | Create a new resource |
| `az * update` | Update an existing resource |
| `az webapp deploy` | Deploy code to an App Service |
| `az deployment group create` | Deploy an ARM/Bicep template |

### Never run — requires explicit human approval

| Command pattern | Reason |
|---|---|
| `az * delete` | Irreversible resource deletion |
| `az group delete` | Irreversibly deletes a resource group and everything in it |
| `az role assignment create/delete` | Alters access control, security-sensitive |
| `az vm stop/restart/deallocate` | Disrupts running workloads |
| `az sql server delete` / `az sql db delete` | Irreversibly destroys a database |

## Rules

- Always specify `--subscription` explicitly — never rely on the default subscription set via `az account set`, as it may point to the wrong account.
- Always specify `--location`/`--resource-group` explicitly for resource-creating commands — never rely on defaults.
- If the target environment is unclear, ask the user before running the command.
- NEVER run any command in the "Never run" table above without explicit user instruction — a human must execute these, especially against production.
- NEVER read or output the contents of `az account get-access-token`, service principal secrets, or any credential value.
- If unsure which subscription an account belongs to, run `az account show --subscription <subscription>` to verify before proceeding.