---
name: helm-cli
description: Perform Helm operations using the helm CLI. Use for inspecting, templating, and managing chart releases across Kubernetes contexts.
---

# Helm CLI Skill

Use the context table and rules below when constructing all `helm` commands.

## Configuration

Update the table below with your kubeconfig context names before using this skill:

| Environment | Context Name | Default Namespace |
|---|---|---|
| Production | `<your-prod-context>` | `<your-prod-namespace>` |
| Staging / Non-Prod | `<your-nonprod-context>` | `<your-nonprod-namespace>` |
| Sandbox / Dev | `<your-sandbox-context>` | `<your-sandbox-namespace>` |

If the target environment is unclear, ask the user before running any command.

### Prerequisites (one-time setup by user)

If `helm` is not installed or no repositories are configured, inform the user that setup is required and ask them to complete the following steps manually. Do not perform these steps yourself.

```bash
brew install helm
helm repo add <repo-name> <repo-url>
helm repo update
```

## Command Format

Always pass `--kube-context` and `--namespace` explicitly — never rely on the current kubeconfig context:

```bash
helm <command> --kube-context <context-name> --namespace <namespace> [options]
```

### Examples

```bash
# List releases in non-prod
helm list --kube-context <your-nonprod-context> --namespace <your-nonprod-namespace>

# Render a chart's templates locally without touching the cluster
helm template <release-name> <chart> --values <values-file>
```

## Available Commands

### Read-only (safe, no confirmation needed)

| Command | Description |
|---|---|
| `list [-A]` | List releases in a namespace (or all namespaces with `-A`) |
| `status <release>` | Show the status of a release |
| `get values <release>` | Show the values used for a release |
| `get manifest <release>` | Show the rendered manifest of a release |
| `get hooks <release>` | Show hooks associated with a release |
| `history <release>` | Show revision history for a release |
| `template <chart>` | Render chart templates locally — no cluster interaction |
| `show chart/values/readme <chart>` | Show chart metadata, default values, or README |
| `diff upgrade <release> <chart>` | Preview changes an upgrade would make (requires `helm-diff` plugin) |
| `lint <chart>` | Validate a chart for issues |
| `search repo/hub <keyword>` | Search for charts |
| `repo list` | List configured repositories |
| `version` | Show Helm client/server version |

### Mutating operations (confirm with user before running)

| Command | Description |
|---|---|
| `install <release> <chart>` | Install a new release |
| `upgrade <release> <chart>` | Upgrade an existing release |
| `upgrade --install <release> <chart>` | Install or upgrade in one step |
| `repo add/update` | Add or refresh a chart repository |
| `dependency update/build` | Resolve and download chart dependencies |

### Never run — requires explicit human approval

| Command | Reason |
|---|---|
| `uninstall <release>` | Removes the release and its resources from the cluster |
| `rollback <release> <revision>` | Reverts a release to a prior revision, affecting live workloads |
| `upgrade --install` against a production release | Mutates live production infrastructure |
| `install`/`upgrade` with `--force` | Forces resource replacement, which can cause downtime |

## Rules

- NEVER target a context not listed in the configuration table above. If a command references an unfamiliar context, stop and ask the user to confirm.
- Always pass `--kube-context` and `--namespace` explicitly on every command — never rely on the current kubeconfig context.
- NEVER run `helm install`, `helm upgrade`, `helm rollback`, or `helm uninstall` against a production context without explicit user instruction — a human must execute these.
- Always run `helm template` or `helm diff upgrade` and show the output before ever suggesting an `install`/`upgrade` — never apply blind.
- NEVER print, display, or store the contents of values files that may contain secrets — treat any value that looks like a credential as sensitive and do not display it.
- If a chart or release name looks unfamiliar, look it up with `helm list`/`helm status` first before mutating.
- Prefer `--dry-run` on `install`/`upgrade` commands when previewing changes against a live cluster is unavoidable.
