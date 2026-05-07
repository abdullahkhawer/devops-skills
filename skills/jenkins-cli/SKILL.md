---
name: jenkins-cli
description: Perform Jenkins operations using the jenkins-cli.
---

# Jenkins CLI Skill

Use the CLI setup and command format below when constructing all jenkins-cli commands.

## Instances

Update the table below with your Jenkins server URLs and auth file paths:

| Environment | Server URL | Auth File |
|---|---|---|
| Production | `https://<your-jenkins-prod-url>` | `jenkins-cli/prod.auth` |
| Staging | `https://<your-jenkins-staging-url>` | `jenkins-cli/staging.auth` |

If the target server is unclear, ask the user before running any command.

## CLI Setup

- JAR File: `jenkins-cli/jenkins-cli.jar` (working directory's root)
- Auth Files: one per server — see instance table above. File contents format: `username:api-token`

All commands must be run from the working directory's root (where `jenkins-cli/` directory exists).

### Prerequisites (one-time setup by user)

If the `jenkins-cli/` directory does not exist yet, or `jenkins-cli.jar` or the auth file for the target server is missing, inform the user that setup is required and ask them to complete the following steps manually. Do not perform these steps yourself.

1. Create the directory `jenkins-cli` in the working directory's root.
2. Download the JAR from `<YOUR_JENKINS_URL>/jnlpJars/jenkins-cli.jar` and save it as `jenkins-cli/jenkins-cli.jar`.
3. Create an auth file for each server you intend to use, with your credentials in this exact format: `username:api-token`
   - `jenkins-cli/prod.auth`
   - `jenkins-cli/staging.auth`

## Command Format

```
java -jar jenkins-cli/jenkins-cli.jar -s <SERVER_URL> -auth @<AUTH_FILE> [command]
```

Example (production):
```bash
java -jar jenkins-cli/jenkins-cli.jar -s https://<your-jenkins-prod-url> -auth @jenkins-cli/prod.auth who-am-i
```

## Available Commands

### Read-only (safe, no confirmation needed)

| Command | Description |
|---|---|
| `who-am-i` | Verify auth and show permissions |
| `version` | Show Jenkins version |
| `list-jobs [folder]` | List all jobs (optionally in a folder/view) |
| `get-job <job>` | Dump job config XML to stdout |
| `console <job> [build#]` | Get console output of a build; `build#` accepts integers **or** Jenkins specifiers like `lastSuccessfulBuild`, `lastBuild`, `lastFailedBuild` |
| `list-changes <job> [build#]` | Show changelog for a build; `build#` must be an **integer** — specifiers like `lastSuccessfulBuild` are not accepted |
| `get-node <node>` | Dump node config XML |
| `get-view <view>` | Dump view config XML |
| `list-plugins` | List installed plugins |
| `session-id` | Show current session ID |
| `export-configuration` | Export Jenkins config as YAML |
| `check-configuration` | Validate YAML configuration |
| `declarative-linter` | Validate a Declarative Jenkinsfile (reads from stdin) |
| `list-credentials <store>` | List credentials in a store |
| `get-credentials-as-xml <store> <domain> <id>` | Get credential XML (secrets redacted) |
| `get-credentials-domain-as-xml <store> <domain>` | Get credentials domain XML |
| `list-credentials-context-resolvers` | List credentials context resolvers |
| `list-credentials-providers` | List credentials providers |
| `help [command]` | List all commands or describe one command |
| `reload-job <job>` | Reload job config from disk (read-only, no state change) |

### Build operations (confirm if job looks like production)

| Command | Description |
|---|---|
| `build <job> [-s] [-v] [-p key=val]` | Trigger a build (`-s` waits, `-v` streams output) |
| `stop-builds <job>` | Stop all running builds for a job |
| `keep-build <job> <build#>` | Mark a build to keep forever |
| `set-build-description <job> <build#> <desc>` | Set build description |
| `set-build-display-name <job> <build#> <name>` | Set build display name |
| `set-next-build-number <job> <number>` | Set next build number |
| `replay-pipeline <job> [build#]` | Replay a pipeline with edited script from stdin |
| `restart-from-stage <job> <build#> <stage>` | Restart a Declarative Pipeline from a given stage |

### Job / view management (ask user before running)

| Command | Description |
|---|---|
| `create-job <job>` | Create a new job (XML from stdin) |
| `update-job <job>` | Update job config (XML from stdin) |
| `copy-job <src> <dst>` | Copy a job |
| `enable-job <job>` | Enable a job |
| `disable-job <job>` | Disable a job |
| `create-view <view>` | Create a new view (XML from stdin) |
| `update-view <view>` | Update view config (XML from stdin) |
| `add-job-to-view <view> <job>` | Add a job to a view |
| `remove-job-from-view <view> <job>` | Remove a job from a view |
| `connect-node <node>` | Reconnect a node |
| `disconnect-node <node>` | Disconnect a node |
| `offline-node <node>` | Take a node offline temporarily |
| `online-node <node>` | Bring a node back online |
| `create-node <node>` | Create a new node (XML from stdin) |
| `update-node <node>` | Update node config (XML from stdin) |
| `reload-configuration` | Reload all config from disk |
| `reload-jcasc-configuration` | Reload JCasC YAML config |

### Never run — requires explicit human approval

| Command | Reason |
|---|---|
| `delete-job <job>` | Irreversible deletion |
| `delete-node <node>` | Irreversible deletion |
| `delete-view <view>` | Irreversible deletion |
| `delete-credentials ...` | Irreversible deletion |
| `delete-credentials-domain ...` | Irreversible deletion |
| `clear-queue` | Cancels all queued builds |
| `quiet-down` / `cancel-quiet-down` | Affects all builds instance-wide |
| `restart` / `safe-restart` | Restarts Jenkins for all users |
| `shutdown` / `safe-shutdown` | Shuts down Jenkins for all users |
| `delete-builds <job> <range>` | Irreversible deletion of build records |
| `apply-configuration` | Applies YAML config changes to the entire Jenkins instance |
| `groovy` / `groovysh` | Arbitrary code execution on Jenkins master |
| `install-plugin` / `enable-plugin` / `disable-plugin` | Modifies Jenkins instance globally |
| `import-credentials-as-xml` / `update-credentials-by-xml` | Modifies stored credentials |
| `mail` | Sends email to real recipients |
| `support` | Generates and may upload diagnostic bundles |

## Rules

- Always use `jenkins-cli.jar` for all Jenkins operations. Do **not** use `curl` or other HTTP clients against the Jenkins REST API.
- NEVER target a server not listed in the instance table above. If asked to run against an unfamiliar server URL, stop and ask the user to confirm.
- Update the instance table with your actual Jenkins URLs before using this skill.
- If a job name, parameter, or folder path references an unfamiliar environment, stop and ask the user before running.
- NEVER run commands in the "Never run" table above without explicit user instruction.
