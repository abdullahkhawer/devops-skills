---
name: packer-cli
description: Perform HashiCorp Packer operations using the packer CLI. Use for validating, formatting, and building machine images from templates.
---

# Packer CLI Skill

Use the template table and rules below when constructing all `packer` commands.

## Configuration

Update the table below with your Packer template files and target build environments:

| Environment | Template File | Variables File |
|---|---|---|
| Production | `<your-prod-template>.pkr.hcl` | `<your-prod.pkrvars.hcl>` |
| Staging / Non-Prod | `<your-nonprod-template>.pkr.hcl` | `<your-nonprod.pkrvars.hcl>` |

If the target environment is unclear, ask the user before running any command.

### Prerequisites (one-time setup by user)

If `packer` is not installed, inform the user that setup is required and ask them to complete the following steps manually. Do not perform these steps yourself.

```bash
brew install packer
packer init <template.pkr.hcl>
```

## Command Format

```bash
packer <command> -var-file=<vars-file> <template.pkr.hcl>
```

### Examples

```bash
# Non-Prod — validate before building
packer validate -var-file=<your-nonprod.pkrvars.hcl> <your-nonprod-template>.pkr.hcl

# Non-Prod — build (only with user confirmation)
packer build -var-file=<your-nonprod.pkrvars.hcl> <your-nonprod-template>.pkr.hcl
```

## Available Commands

### Read-only (safe, no confirmation needed)

| Command | Description |
|---|---|
| `packer validate -var-file=<file> <template>` | Validate template syntax and configuration |
| `packer fmt -check -diff <template>` | Check formatting without modifying the file |
| `packer inspect <template>` | Show builders, provisioners, and variables defined |
| `packer console` | Open an interactive expression evaluator |
| `packer version` | Show Packer version |

### Mutating operations (confirm with user before running)

| Command | Description |
|---|---|
| `packer build -var-file=<file> <template>` | Build a new machine image — provisions real cloud/VM resources during the build |
| `packer fmt <template>` | Reformat a template file in place |
| `packer init <template>` | Install required plugins |

### Never run — requires explicit human approval

| Command | Reason |
|---|---|
| `packer build` against a production template/vars file | Builds and may publish a production-bound image, consuming real cloud resources and cost |
| `packer build -force` | Overwrites an existing image artifact, potentially breaking references to it |

## Rules

- NEVER target a template or vars file not listed in the configuration table above. If a command references an unfamiliar template, stop and ask the user to confirm.
- Always run `packer validate` and `packer fmt -check -diff` before ever suggesting a `build` — never build blind.
- NEVER run `packer build` against a production template without explicit user instruction — builds provision real, billable cloud resources and can publish images used by production infrastructure.
- NEVER print, display, or store cloud provider credentials embedded in variables files — treat any credential-like variable as sensitive.
- If a build fails partway, inform the user that orphaned temporary resources (instances, snapshots) may need manual cleanup in the cloud provider console — do not attempt automated cleanup yourself.
