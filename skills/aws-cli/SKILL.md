---
name: aws-cli
description: Use the correct AWS CLI profile when running AWS commands based on the target environment.
---

# AWS CLI Skill

When running AWS CLI commands, always pass the `--profile` flag based on the target environment.

## Configuration

Update the profile map below with your AWS profile names as configured in `~/.aws/config`:

| Environment | Profile |
|---|---|
| Production | `<your-prod-profile>` |
| Staging / Non-Prod | `<your-nonprod-profile>` |

## Command Format

```bash
aws <service> <command> --profile <profile> [options]
```

### Examples

```bash
# Production
aws s3 ls --profile my-prod-profile

# Staging / Non-Prod
aws s3 ls --profile my-nonprod-profile
```

## Rules

- Always specify `--profile` explicitly — never rely on the default profile.
- If the target environment is unclear, ask the user before running the command.
