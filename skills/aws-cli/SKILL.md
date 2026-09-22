---
name: aws-cli
description: Use the correct AWS CLI profile and region when running AWS commands based on the target environment.
---

# AWS CLI Skill

When running AWS CLI commands, always pass the `--profile` and `--region` flags based on the target environment.

## Configuration

Update the profile and region map below with your AWS profile names as configured in `~/.aws/config`:

| Environment | Profile |
|---|---|
| Production | `<your-prod-profile>` |
| Staging / Non-Prod | `<your-nonprod-profile>` |
| Sandbox / Dev | `<your-sandbox-profile>` |

## Default Region

Set your default region here: **`<your-default-region>`** (e.g. `eu-west-1`, `us-east-1`).

Always pass `--region <your-default-region>` unless the user specifies a different region.

## Command Format

```bash
aws <service> <command> --profile <profile> --region <region> [options]
```

### Examples

```bash
# Production
aws s3 ls --profile <your-prod-profile> --region <your-default-region>

# Staging / Non-Prod
aws s3 ls --profile <your-nonprod-profile> --region <your-default-region>

# Sandbox / Dev
aws s3 ls --profile <your-sandbox-profile> --region <your-default-region>
```

## Rules

- Always specify `--profile` explicitly — never rely on the default profile.
- Always specify `--region` explicitly — never rely on the default region.
- If the target environment is unclear, ask the user before running the command.
- If the target region is unclear, use the configured default region — but only if `<your-default-region>` has been set to a real region value above.
- If unsure which account a profile belongs to, run `aws sts get-caller-identity --profile <profile>` to verify identity before proceeding.
