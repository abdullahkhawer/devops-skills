---
name: argocd-cli
description: Perform ArgoCD GitOps operations using the argocd CLI. Use for inspecting applications, sync status, and deployment history across Kubernetes clusters.
---

# ArgoCD CLI Skill

Use the server table and rules below when constructing all `argocd` commands.

## Configuration

Update the table below with your ArgoCD server addresses before using this skill:

| Environment | Server Address | Auth Token File |
|---|---|---|
| Production | `<your-prod-argocd-server>` | `~/.argocd/prod.token` |
| Staging / Non-Prod | `<your-nonprod-argocd-server>` | `~/.argocd/staging.token` |

If the target environment is unclear, ask the user before running any command.

### Prerequisites (one-time setup by user)

If not already authenticated, inform the user that setup is required and ask them to complete the following steps manually. Do not perform these steps yourself.

```bash
brew install argocd
argocd login <server-address> --sso
```

Generate an auth token via `argocd account generate-token` and save it to the corresponding token file (see table above). Never share credential file contents with the assistant.

## Command Format

Always pass `--server` and `--auth-token` explicitly — never rely on a cached local session context, which may point to the wrong cluster:

```bash
argocd <command> --server <server-address> --auth-token "$(cat <TOKEN_FILE>)" [options]
```

## Available Commands

### Read-only (safe, no confirmation needed)

| Command | Description |
|---|---|
| `app list` | List applications |
| `app get <app>` | Show application details and sync status |
| `app history <app>` | Show deployment history |
| `app diff <app>` | Show diff between live and desired state |
| `app manifests <app>` | Show rendered manifests |
| `app logs <app>` | Show logs for an application's pods |
| `proj list` / `proj get <proj>` | List or show projects |
| `cluster list` | List registered clusters |
| `repo list` | List registered repositories |

### Mutating operations (confirm with user before running)

| Command | Description |
|---|---|
| `app sync <app>` | Sync an application to its desired state |
| `app set <app>` | Update application configuration |
| `repo add <url>` | Register a new repository |
| `cluster add <context>` | Register a new cluster |

### Never run — requires explicit human approval

| Command | Reason |
|---|---|
| `app delete <app>` | Irreversible application deletion (may also delete deployed resources) |
| `app sync <app> --prune` against production | Deletes live resources no longer in Git, affecting production |
| `app rollback <app> <revision>` | Reverts a live production deployment |
| `proj delete <proj>` | Irreversible project deletion |
| `cluster rm <context>` | Removes a registered cluster |

## Rules

- NEVER target a server not listed in the configuration table above. If a command references an unfamiliar server, stop and ask the user to confirm.
- Always pass `--server` and `--auth-token` explicitly on every command — never rely on `argocd login`'s cached local context.
- NEVER run `app sync`, `app sync --prune`, or `app rollback` against a production application without explicit user instruction.
- NEVER print, display, log, or store the contents of any token file or `ARGOCD_AUTH_TOKEN` value.
- Always run `app diff` and show the output before ever suggesting a `sync` — never sync blind.
- If an application or project name is unfamiliar, look it up with `app list`/`app get` first.
