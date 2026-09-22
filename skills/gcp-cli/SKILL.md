---
name: gcp-cli
description: Use the correct GCP CLI (gcloud) project, account, and region when running Google Cloud commands based on the target environment.
---

# GCP CLI Skill

When running `gcloud` commands, always pass the `--project` flag based on the target environment, and configure the region/zone explicitly.

## Configuration

Update the project map below with your GCP project IDs:

| Environment | Project ID | Account |
|---|---|---|
| Production | `<your-prod-project-id>` | `<your-prod-account>` |
| Staging / Non-Prod | `<your-nonprod-project-id>` | `<your-nonprod-account>` |
| Sandbox / Dev | `<your-sandbox-project-id>` | `<your-sandbox-account>` |

## Default Region / Zone

Set your default region and zone here: **`<your-default-region>`** / **`<your-default-zone>`** (e.g. `us-central1` / `us-central1-a`).

Always pass `--region <your-default-region>` or `--zone <your-default-zone>` unless the user specifies a different location.

### Prerequisites (one-time setup by user)

If not already authenticated, inform the user that setup is required and ask them to complete the following steps manually. Do not perform these steps yourself.

```bash
brew install --cask google-cloud-sdk
gcloud auth login
gcloud auth list
```

## Command Format

```bash
gcloud <group> <command> --project <project-id> --account <account> [--region <region> | --zone <zone>] [options]
```

### Examples

```bash
# Production
gcloud compute instances list --project <your-prod-project-id> --account <your-prod-account>

# Staging / Non-Prod
gcloud compute instances list --project <your-nonprod-project-id> --account <your-nonprod-account>
```

## Available Commands

### Read-only (safe, no confirmation needed)

| Command pattern | Description |
|---|---|
| `gcloud * list` | List any resource type |
| `gcloud * describe <name>` | Describe a resource |
| `gcloud projects describe <project-id>` | Show project metadata |
| `gcloud auth list` | Show authenticated accounts |
| `gcloud config list` | Show current CLI configuration |
| `gcloud logging read <query>` | Read Cloud Logging entries |
| `gcloud monitoring ...` (read subcommands) | Query Cloud Monitoring |

### Mutating operations (confirm with user before running)

| Command pattern | Description |
|---|---|
| `gcloud * create` | Create a new resource |
| `gcloud * update` | Update an existing resource |
| `gcloud deploy releases create` | Trigger a deployment |
| `gcloud run deploy` | Deploy a Cloud Run service |

### Never run — requires explicit human approval

| Command pattern | Reason |
|---|---|
| `gcloud * delete` | Irreversible resource deletion |
| `gcloud projects delete` | Irreversibly deletes an entire project |
| `gcloud iam ... add-iam-policy-binding / remove-iam-policy-binding` | Alters access control, security-sensitive |
| `gcloud compute instances stop/reset` | Disrupts running workloads |
| `gcloud sql instances delete/restart` | Disrupts or destroys a database |

## Rules

- Always specify `--project` and `--account` explicitly — never rely on the default `gcloud config` values, as they may point to the wrong project.
- Always specify `--region`/`--zone` explicitly — never rely on the default configured region.
- If the target environment is unclear, ask the user before running the command.
- NEVER run any command in the "Never run" table above without explicit user instruction — a human must execute these, especially against production.
- NEVER read or output service account key files, `gcloud auth print-access-token` output, or any credential value.
- If unsure which project an account belongs to, run `gcloud projects describe <project-id> --account <account>` to verify before proceeding.