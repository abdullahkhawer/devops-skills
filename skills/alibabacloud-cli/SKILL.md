---
name: alibabacloud-cli
description: Use the correct Alibaba Cloud CLI (aliyun) profile and region when running Alibaba Cloud commands based on the target environment.
---

# Alibaba Cloud CLI Skill

When running `aliyun` commands, always pass the `--profile` flag based on the target environment, and specify the region explicitly.

## Configuration

Update the profile map below with your Alibaba Cloud CLI profile names:

| Environment | Profile | Default Region |
|---|---|---|
| Production | `<your-prod-profile>` | `<your-prod-region>` (e.g. `cn-hangzhou`) |
| Staging / Non-Prod | `<your-nonprod-profile>` | `<your-nonprod-region>` (e.g. `ap-southeast-1`) |

### Prerequisites (one-time setup by user)

If not already authenticated, inform the user that setup is required and ask them to complete the following steps manually. Do not perform these steps yourself.

```bash
brew install aliyun-cli
aliyun configure --profile <your-profile-name>
aliyun configure list
```

## Command Format

```bash
aliyun <product> <action> --profile <profile> --region <region> [options]
```

### Examples

```bash
# Production
aliyun ecs DescribeInstances --profile <your-prod-profile> --region <your-prod-region>

# Staging / Non-Prod
aliyun ecs DescribeInstances --profile <your-nonprod-profile> --region <your-nonprod-region>
```

## Available Commands

### Read-only (safe, no confirmation needed)

| Command pattern | Description |
|---|---|
| `aliyun ecs DescribeInstances` | List ECS instances |
| `aliyun ecs DescribeInstanceAttribute` | Describe an ECS instance |
| `aliyun rds DescribeDBInstances` | List RDS database instances |
| `aliyun cs DescribeClusters` | List Kubernetes (ACK) clusters |
| `aliyun sts GetCallerIdentity` | Verify current identity |
| `aliyun cms ...` (Describe* actions) | Query CloudMonitor metrics/alerts |

### Mutating operations (confirm with user before running)

| Command pattern | Description |
|---|---|
| `aliyun ecs CreateInstance` | Create a new ECS instance |
| `aliyun ecs ModifyInstanceAttribute` | Modify an ECS instance |
| `aliyun rds CreateDBInstance` | Create a new RDS instance |
| `aliyun cs CreateCluster` | Create a new Kubernetes (ACK) cluster |

### Never run — requires explicit human approval

| Command pattern | Reason |
|---|---|
| `aliyun ecs DeleteInstance` | Irreversible resource deletion |
| `aliyun rds DeleteDBInstance` | Irreversibly destroys a database |
| `aliyun cs DeleteCluster` | Irreversibly deletes a Kubernetes cluster |
| `aliyun ecs StopInstance` / `RebootInstance` | Disrupts running workloads |
| `aliyun ram ...` (Create/Delete/Attach/Detach Policy) | Alters access control, security-sensitive |

## Rules

- Always specify `--profile` explicitly — never rely on the default `aliyun` profile, as it may point to the wrong account.
- Always specify `--region` explicitly — never rely on a default region.
- If the target environment is unclear, ask the user before running the command.
- NEVER run any command in the "Never run" table above without explicit user instruction — a human must execute these, especially against production.
- NEVER read or output AccessKey ID/Secret values stored in `aliyun` CLI profiles or any credential value.
- If unsure which account a profile belongs to, run `aliyun sts GetCallerIdentity --profile <profile>` to verify before proceeding.