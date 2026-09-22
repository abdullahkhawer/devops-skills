---
name: kubernetes-cli
description: Perform Kubernetes operations using the kubectl CLI. Use for inspecting, managing, and troubleshooting workloads, nodes, and cluster resources across contexts.
---

# Kubernetes CLI Skill

Use the context table and rules below when constructing all `kubectl` commands.

## Configuration

Update the table below with your kubeconfig context names before using this skill:

| Environment | Context Name | Default Namespace |
|---|---|---|
| Production | `<your-prod-context>` | `<your-prod-namespace>` |
| Staging / Non-Prod | `<your-nonprod-context>` | `<your-nonprod-namespace>` |
| Sandbox / Dev | `<your-sandbox-context>` | `<your-sandbox-namespace>` |

If the target environment is unclear, ask the user before running any command.

### Prerequisites (one-time setup by user)

If `kubectl` is not installed or no context is configured for the target cluster, inform the user that setup is required and ask them to complete the following steps manually. Do not perform these steps yourself.

```bash
brew install kubectl
aws eks update-kubeconfig --name <cluster-name> --profile <profile> --region <region> --alias <your-context-name>
# or for other providers, follow the provider-specific kubeconfig setup
kubectl config get-contexts
```

## Command Format

Always pass `--context` and `--namespace` explicitly — never rely on the current context or namespace set in kubeconfig:

```bash
kubectl <command> --context <context-name> --namespace <namespace> [options]
```

### Examples

```bash
# Production
kubectl get pods --context <your-prod-context> --namespace <your-prod-namespace>

# Staging / Non-Prod
kubectl get pods --context <your-nonprod-context> --namespace <your-nonprod-namespace>
```

## Available Commands

### Read-only (safe, no confirmation needed)

| Command | Description |
|---|---|
| `get <resource> [name]` | List or show resources (pods, deployments, services, nodes, etc.) |
| `describe <resource> <name>` | Show detailed information and recent events for a resource |
| `logs <pod> [-c container] [--previous] [--tail=N]` | Show container logs |
| `top nodes` / `top pods` | Show resource usage (requires metrics-server) |
| `get events --sort-by=.lastTimestamp` | Show recent cluster events |
| `explain <resource>` | Show API resource schema documentation |
| `api-resources` / `api-versions` | List available resource types and API versions |
| `config get-contexts` / `config current-context` | Inspect kubeconfig contexts |
| `version` | Show client and server version |
| `cluster-info` | Show cluster endpoint information |
| `diff -f <file>` | Show what would change if a manifest were applied (dry run, no cluster mutation) |
| `get <resource> -o yaml/json` | Dump full resource manifest |

### Mutating operations (confirm with user before running)

| Command | Description |
|---|---|
| `apply -f <file>` | Create or update resources from a manifest |
| `create -f <file>` / `create <resource>` | Create a new resource |
| `patch <resource> <name>` | Apply a strategic/JSON merge patch |
| `scale <resource> <name> --replicas=N` | Scale a deployment/statefulset/replicaset |
| `rollout restart/undo <resource> <name>` | Restart or roll back a deployment |
| `label` / `annotate` | Add or modify labels/annotations |
| `cordon <node>` / `uncordon <node>` | Mark a node unschedulable or schedulable |

### Never run — requires explicit human approval

| Command | Reason |
|---|---|
| `delete <resource> <name>` | Irreversible deletion of a resource |
| `delete namespace <name>` | Deletes the namespace and everything inside it |
| `drain <node>` | Evicts all pods from a node, affecting live workloads |
| `exec -it <pod> -- <command>` | Opens an interactive shell/session inside a running container |
| `edit <resource> <name>` | Opens an interactive editor against a live resource |
| `replace --force -f <file>` | Force-deletes and recreates a resource, causing downtime |
| `taint <node>` | Alters node scheduling behavior cluster-wide |

## Rules

- NEVER target a context not listed in the configuration table above. If a command references an unfamiliar context, stop and ask the user to confirm.
- Always pass `--context` and `--namespace` explicitly on every command — never rely on the current kubeconfig context, as this may silently target the wrong cluster.
- NEVER run commands in the "Never run" table above without explicit user instruction — this includes any command against a production context, even if listed as a mutating operation above.
- If a resource name, namespace, or context looks unfamiliar, look it up with a read-only command first (`get`, `describe`) before mutating.
- Prefer `kubectl diff -f <file>` before any `apply` to preview changes.
- NEVER print, display, or store the contents of Secrets (`kubectl get secret -o yaml` exposes base64-encoded values) — if a Secret must be inspected, only confirm its existence and keys, not its values.
- All list commands may be paginated or truncated by the API server for large clusters — use `--chunk-size` or narrow the query with label selectors (`-l`) if results seem incomplete.
