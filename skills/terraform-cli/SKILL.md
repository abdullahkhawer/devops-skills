---
name: terraform-cli
description: Perform Terraform operations using the terraform CLI. Use for planning, validating, and inspecting infrastructure-as-code state and workspaces.
---

# Terraform CLI Skill

Use the workspace table and rules below when constructing all `terraform` commands.

## Configuration

Update the table below with your Terraform workspace/backend names before using this skill:

| Environment | Workspace | Backend Config / Var File |
|---|---|---|
| Production | `<your-prod-workspace>` | `<your-prod.tfvars>` |
| Staging / Non-Prod | `<your-nonprod-workspace>` | `<your-nonprod.tfvars>` |
| Sandbox / Dev | `<your-sandbox-workspace>` | `<your-sandbox.tfvars>` |

If the target environment is unclear, ask the user before running any command.

### Prerequisites (one-time setup by user)

If `terraform` is not installed or the backend is not initialized, inform the user that setup is required and ask them to complete the following steps manually. Do not perform these steps yourself.

```bash
brew install terraform
terraform init
terraform workspace list
```

## Command Format

Always run commands from the directory containing the relevant Terraform root module, and pass `-var-file` explicitly for the target environment:

```bash
terraform <command> -var-file=<var-file> [options]
```

### Examples

```bash
# Select the correct workspace before running any command
terraform workspace select <your-nonprod-workspace>

# Non-Prod
terraform plan -var-file=<your-nonprod.tfvars> -out=tfplan

# Show a saved plan in human-readable form
terraform show tfplan

# Show a saved plan in JSON (for programmatic inspection)
terraform show -json tfplan
```

## Available Commands

### Read-only (safe, no confirmation needed)

| Command | Description |
|---|---|
| `plan -var-file=<file> [-out=tfplan]` | Show or save a proposed set of changes — does not mutate infrastructure |
| `validate` | Check configuration syntax and internal consistency |
| `fmt -recursive [-check] [-diff]` | Format or check formatting of `.tf` files |
| `show [tfplan]` | Show the current state or a saved plan in human-readable form |
| `show -json [tfplan]` | Show the current state or a saved plan as JSON |
| `state list` | List all resources tracked in state |
| `state show <resource>` | Show attributes of a resource in state |
| `output [-json]` | Show output values from the root module |
| `workspace list` / `workspace show` | List or show the current workspace |
| `graph` | Generate a visual dependency graph (DOT format) |
| `providers` | List provider requirements and versions |
| `version` | Show Terraform and provider versions |
| `console` | Open an interactive expression evaluator (read-only against current state) |

### Mutating operations (confirm with user before running)

| Command | Description |
|---|---|
| `apply tfplan` / `apply -var-file=<file>` | Apply a plan and provision/modify real infrastructure |
| `workspace new <name>` | Create a new workspace |
| `workspace select <name>` | Switch the active workspace |
| `refresh -var-file=<file>` | Reconcile state with real infrastructure (can drift outputs) |
| `state mv <src> <dst>` | Move a resource within state |

### Never run — requires explicit human approval

| Command | Reason |
|---|---|
| `destroy` / `apply -destroy` | Irreversibly destroys provisioned infrastructure |
| `apply` against a production var file/workspace | Mutates live production infrastructure |
| `state rm <resource>` | Removes a resource from state without destroying it — causes drift and orphaned resources |
| `import <resource> <id>` | Binds existing infrastructure into state; mistakes can corrupt state |
| `taint <resource>` / `untaint <resource>` | Forces recreation of a resource on the next apply |
| `force-unlock <lock-id>` | Removes the state lock — risks concurrent, corrupting writes |
| `workspace delete <name>` | Irreversible deletion of a workspace and its state |

## Rules

- NEVER target a workspace or var file not listed in the configuration table above. If a command references an unfamiliar environment, stop and ask the user to confirm.
- NEVER run `terraform apply`, `terraform destroy`, or any command in the "Never run" table above without explicit user instruction — a human must execute these, especially against production.
- Always run `terraform plan` and show the output before ever suggesting an `apply` — never apply blind.
- Always confirm the active workspace with `terraform workspace show` before running any command that reads or writes state, to avoid targeting the wrong environment.
- NEVER print, display, or store the contents of `.tfvars` files or state files that may contain secrets — treat any `sensitive = true` output as a credential and do not display its value.
- If state appears locked (`Error: Error acquiring the state lock`), do NOT run `force-unlock` — report this to the user and let them confirm no other operation is in progress.
- Prefer `-out=tfplan` and `terraform show -json tfplan` when a plan must be inspected programmatically, so it can be re-applied exactly as reviewed.
