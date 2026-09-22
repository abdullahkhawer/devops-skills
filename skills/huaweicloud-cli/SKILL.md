---
name: huaweicloud-cli
description: Use the correct Huawei Cloud CLI (hcloud) profile and region when running Huawei Cloud commands based on the target environment.
---

# Huawei Cloud CLI Skill

When running `hcloud` commands, always pass the `--cli-profile` flag based on the target environment, and specify the region explicitly.

## Configuration

Update the profile map below with your Huawei Cloud CLI profile names:

| Environment | Profile | Default Region |
|---|---|---|
| Production | `<your-prod-profile>` | `<your-prod-region>` (e.g. `cn-north-4`) |
| Staging / Non-Prod | `<your-nonprod-profile>` | `<your-nonprod-region>` (e.g. `ap-southeast-1`) |

### Prerequisites (one-time setup by user)

If not already authenticated, inform the user that setup is required and ask them to complete the following steps manually. Do not perform these steps yourself.

```bash
brew install huaweicloud-cli
hcloud configure set --cli-profile <your-profile-name>
hcloud configure list
```

## Command Format

```bash
hcloud <service> <action> --cli-profile <profile> --cli-region <region> [options]
```

### Examples

```bash
# Production
hcloud ECS ListServersDetails --cli-profile <your-prod-profile> --cli-region <your-prod-region>

# Staging / Non-Prod
hcloud ECS ListServersDetails --cli-profile <your-nonprod-profile> --cli-region <your-nonprod-region>
```

## Available Commands

### Read-only (safe, no confirmation needed)

| Command pattern | Description |
|---|---|
| `hcloud ECS ListServersDetails` | List ECS instances |
| `hcloud ECS ShowServer` | Describe an ECS instance |
| `hcloud RDS ListInstances` | List RDS database instances |
| `hcloud CCE ListClusters` | List Kubernetes (CCE) clusters |
| `hcloud IAM ShowUser` / `KeystoneListAuthProjects` | Verify current identity/permissions |
| `hcloud CES ...` (List/Show actions) | Query Cloud Eye monitoring metrics/alarms |

### Mutating operations (confirm with user before running)

| Command pattern | Description |
|---|---|
| `hcloud ECS CreateServers` | Create new ECS instances |
| `hcloud RDS CreateInstance` | Create a new RDS instance |
| `hcloud CCE CreateCluster` | Create a new Kubernetes (CCE) cluster |
| `hcloud ECS ResizeServer` | Resize an ECS instance |

### Never run — requires explicit human approval

| Command pattern | Reason |
|---|---|
| `hcloud ECS DeleteServers` | Irreversible resource deletion |
| `hcloud RDS DeleteInstance` | Irreversibly destroys a database |
| `hcloud CCE DeleteCluster` | Irreversibly deletes a Kubernetes cluster |
| `hcloud ECS BatchStopServers` / `BatchRebootServers` | Disrupts running workloads |
| `hcloud IAM ...` (Create/Delete/Update policy or user) | Alters access control, security-sensitive |

## Rules

- Always specify `--cli-profile` explicitly — never rely on the default profile, as it may point to the wrong account.
- Always specify `--cli-region` explicitly — never rely on a default region.
- If the target environment is unclear, ask the user before running the command.
- NEVER run any command in the "Never run" table above without explicit user instruction — a human must execute these, especially against production.
- NEVER read or output AK/SK (Access Key/Secret Key) values stored in `hcloud` CLI profiles or any credential value.
- If unsure which account a profile belongs to, run `hcloud IAM ShowUser --cli-profile <profile>` to verify before proceeding.