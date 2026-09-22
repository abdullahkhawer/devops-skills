---
name: oci-cli
description: Use the correct Oracle Cloud Infrastructure CLI (oci) profile and region when running OCI commands based on the target environment.
---

# Oracle Cloud Infrastructure (OCI) CLI Skill

When running `oci` commands, always pass the `--profile` flag based on the target environment, and specify the region explicitly.

## Configuration

Update the profile map below with your OCI CLI profile names as configured in `~/.oci/config`:

| Environment | Profile | Compartment OCID |
|---|---|---|
| Production | `<your-prod-profile>` | `<your-prod-compartment-ocid>` |
| Staging / Non-Prod | `<your-nonprod-profile>` | `<your-nonprod-compartment-ocid>` |

## Default Region

Set your default region here: **`<your-default-region>`** (e.g. `us-ashburn-1`, `eu-frankfurt-1`).

Always pass `--region <your-default-region>` unless the user specifies a different region.

### Prerequisites (one-time setup by user)

If not already authenticated, inform the user that setup is required and ask them to complete the following steps manually. Do not perform these steps yourself.

```bash
brew install oci-cli
oci setup config
oci iam region list --profile <your-profile>
```

## Command Format

```bash
oci <service> <resource> <action> --profile <profile> --region <region> [--compartment-id <compartment-ocid>] [options]
```

### Examples

```bash
# Production
oci compute instance list --profile <your-prod-profile> --region <your-default-region> --compartment-id <your-prod-compartment-ocid>

# Staging / Non-Prod
oci compute instance list --profile <your-nonprod-profile> --region <your-default-region> --compartment-id <your-nonprod-compartment-ocid>
```

## Available Commands

### Read-only (safe, no confirmation needed)

| Command pattern | Description |
|---|---|
| `oci * list` | List any resource type |
| `oci * get` | Get details of a resource |
| `oci iam region list` | List available regions |
| `oci iam compartment list` | List compartments |
| `oci os object list` | List Object Storage bucket contents |

### Mutating operations (confirm with user before running)

| Command pattern | Description |
|---|---|
| `oci * create` | Create a new resource |
| `oci * update` | Update an existing resource |
| `oci compute instance launch` | Launch a new compute instance |

### Never run — requires explicit human approval

| Command pattern | Reason |
|---|---|
| `oci * delete` | Irreversible resource deletion |
| `oci compute instance terminate` | Irreversibly terminates a compute instance |
| `oci iam policy delete` / `oci iam policy update` | Alters access control, security-sensitive |
| `oci compute instance action --action STOP/RESET` | Disrupts running workloads |
| `oci db database delete` | Irreversibly destroys a database |

## Rules

- Always specify `--profile` explicitly — never rely on the `DEFAULT` profile in `~/.oci/config`.
- Always specify `--region` explicitly — never rely on the default region.
- If the target environment is unclear, ask the user before running the command.
- NEVER run any command in the "Never run" table above without explicit user instruction — a human must execute these, especially against production.
- NEVER read or output the contents of `~/.oci/config`, private key files (`.pem`), or any credential value.
- If unsure which tenancy/compartment a profile belongs to, run `oci iam compartment list --profile <profile>` to verify before proceeding.