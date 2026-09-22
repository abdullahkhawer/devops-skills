# DevOps Skills

Author: [Abdullah Khawer - LinkedIn](https://www.linkedin.com/in/abdullah-khawer)

# 💡 Introduction

`devops-skills` is a curated collection of **DevOps Skills** that enhance your development using AI to work smarter.

**Skills** are reusable, step-by-step instruction files (`SKILL.md`) that can be used across almost every major AI agent product including [Claude Code](https://claude.ai/code), [GitHub Copilot](https://github.com/features/copilot), [OpenCode](https://opencode.ai), [Mistral AI](https://mistral.ai), [Cursor](https://www.cursor.com), and more.

Each skill builds a specialized AI assistant with targeted knowledge and abilities for your DevOps workflow, significantly cutting development time by eliminating repetitive manual tasks.

CLI-based skills (GitHub, GitLab, Jenkins, Grafana, Prometheus, Jira, Confluence, Slack, AWS, and more) let your AI agent interact with those platforms directly through their native CLI tools or HTTP APIs — **no MCP server required**, whether remote or local.

Once a skill is loaded into your AI agent, it is automatically invoked whenever your prompt contains relevant context that matches the skill's purpose — no explicit command needed.

# 📚 Official Documentation

- [Agent Skills](https://agentskills.io/home)
- [Claude Code Skills](https://docs.anthropic.com/en/docs/claude-code/skills)

# 🚀 Available Skills

## 🛠️ Development Tools

- [Code Review](skills/code-review/SKILL.md) — Analyze code changes between two local Git branches and perform a comprehensive code review
- [Commit Code](skills/commit-code/SKILL.md) — Analyze code changes, prepare conventional commit messages, and commit to a new branch
- [Create Dockerfile](skills/create-dockerfile/SKILL.md) — Create optimized, secure, production-ready Dockerfiles based on user requirements and application context
- [GitHub CLI](skills/github-cli/SKILL.md) — Perform GitHub operations using the `gh` CLI — issues, pull requests, pipelines, repositories, and PR code reviews
- [GitLab CLI](skills/gitlab-cli/SKILL.md) — Perform GitLab operations using the `glab` CLI — issues, merge requests, pipelines, repositories, and MR code reviews
- [Bitbucket CLI](skills/bitbucket-cli/SKILL.md) — Perform Bitbucket operations using the REST API via `curl` — repositories, pull requests, pipelines, and PR code reviews

## 🧹 Code Quality & Linting

- [Python Linting](skills/python-linting/SKILL.md) — Lint and format Python code using `ruff`, `flake8`, `black`, and `mypy` according to standard conventions
- [Bash Linting](skills/bash-linting/SKILL.md) — Lint and format Bash/shell scripts using `shellcheck` and `shfmt` according to standard conventions
- [Go Linting](skills/go-linting/SKILL.md) — Lint and format Go code using `golangci-lint`, `gofmt`, and `go vet` according to standard conventions
- [Dockerfile Linting](skills/dockerfile-linting/SKILL.md) — Lint Dockerfiles using `hadolint` according to standard best practices
- [Terraform Linting](skills/terraform-linting/SKILL.md) — Lint and format Terraform code using `tflint` and `terraform fmt`/`validate` according to standard conventions

## 🏗️ Infrastructure & DevOps

- [Create Terraform Helm Upgrade Plan](skills/create-terraform-helm-upgrade-plan/SKILL.md) — Create a detailed upgrade plan for a Helm release managed by Terraform, comparing chart versions including breaking changes
- [Maintenance Check AWS EKS](skills/maintenance-check-aws-eks/SKILL.md) — Check available upgrades across an AWS EKS cluster and output a decision file with Jira-formatted tickets
- [Kubernetes CLI](skills/kubernetes-cli/SKILL.md) — Perform Kubernetes operations using the `kubectl` CLI — workloads, nodes, and cluster resources across contexts
- [Terraform CLI](skills/terraform-cli/SKILL.md) — Perform Terraform operations using the `terraform` CLI — planning, validating, and inspecting infrastructure-as-code state
- [Helm CLI](skills/helm-cli/SKILL.md) — Perform Helm operations using the `helm` CLI — inspecting, templating, and managing chart releases
- [Ansible CLI](skills/ansible-cli/SKILL.md) — Perform Ansible operations using `ansible`/`ansible-playbook` — running playbooks and validating configuration
- [HashiCorp Vault CLI](skills/vault-cli/SKILL.md) — Perform HashiCorp Vault operations using the `vault` CLI — secret metadata, policies, and auth methods
- [ArgoCD CLI](skills/argocd-cli/SKILL.md) — Perform ArgoCD GitOps operations using the `argocd` CLI — applications, sync status, and deployment history
- [Packer CLI](skills/packer-cli/SKILL.md) — Perform HashiCorp Packer operations using the `packer` CLI — validating, formatting, and building machine images
- [Chef CLI](skills/chef-cli/SKILL.md) — Perform Chef configuration management operations using `knife`/`chef` — nodes, cookbooks, roles, and environments
- [Jenkins CLI](skills/jenkins-cli/SKILL.md) — Perform Jenkins operations using the `jenkins-cli` JAR — jobs, builds, nodes, and pipeline management

## ☁️ Cloud Providers

- [AWS CLI](skills/aws-cli/SKILL.md) — Use the correct AWS CLI profile and region when running AWS commands based on the target environment
- [GCP CLI](skills/gcp-cli/SKILL.md) — Use the correct GCP CLI (`gcloud`) project, account, and region when running Google Cloud commands
- [Azure CLI](skills/azure-cli/SKILL.md) — Use the correct Azure CLI (`az`) subscription and region when running Azure commands
- [OCI CLI](skills/oci-cli/SKILL.md) — Use the correct Oracle Cloud Infrastructure CLI (`oci`) profile and region when running OCI commands
- [DigitalOcean CLI](skills/digitalocean-cli/SKILL.md) — Use the correct DigitalOcean CLI (`doctl`) context and region when running DigitalOcean commands
- [Alibaba Cloud CLI](skills/alibabacloud-cli/SKILL.md) — Use the correct Alibaba Cloud CLI (`aliyun`) profile and region when running Alibaba Cloud commands
- [Huawei Cloud CLI](skills/huaweicloud-cli/SKILL.md) — Use the correct Huawei Cloud CLI (`hcloud`) profile and region when running Huawei Cloud commands
- [Hetzner Cloud CLI](skills/hetzner-cli/SKILL.md) — Use the correct Hetzner Cloud CLI (`hcloud`) context when running Hetzner Cloud commands

## 📊 Monitoring & Observability

- [Grafana CLI](skills/grafana-cli/SKILL.md) — Perform Grafana operations using the Grafana HTTP API via `curl` — dashboards, datasources, alerts, and annotations
- [Prometheus CLI](skills/prometheus-cli/SKILL.md) — Query Prometheus metrics using the HTTP API via `curl` — instant queries, range queries, metadata, and alerts
- [Kibana CLI](skills/kibana-cli/SKILL.md) — Perform Kibana operations using the HTTP API via `curl` — saved objects, dashboards, index patterns, and alerts
- [Elasticsearch CLI](skills/elasticsearch-cli/SKILL.md) — Perform Elasticsearch operations using the HTTP API via `curl` — indices, documents, mappings, and cluster health
- [Jaeger CLI](skills/jaeger-cli/SKILL.md) — Query Jaeger distributed tracing data using the HTTP API via `curl` — traces, services, and dependency graphs
- [Datadog CLI](skills/datadog-cli/SKILL.md) — Perform Datadog operations using the HTTP API via `curl` — metrics, logs, monitors, dashboards, and events
- [Splunk CLI](skills/splunk-cli/SKILL.md) — Perform Splunk operations using the REST API via `curl` — search queries, saved searches, indexes, and alerts
- [New Relic CLI](skills/newrelic-cli/SKILL.md) — Perform New Relic operations using the `newrelic` CLI and NerdGraph API — entities, applications, alerts, and NRQL
- [AWS CloudWatch CLI](skills/aws-cloudwatch-cli/SKILL.md) — Perform AWS CloudWatch operations using the AWS CLI — metrics, logs, alarms, and dashboards
- [Grafana Loki CLI](skills/grafana-loki-cli/SKILL.md) — Query Grafana Loki logs using the HTTP API via `curl` or `logcli` — LogQL queries and label exploration
- [Graylog CLI](skills/graylog-cli/SKILL.md) — Perform Graylog operations using the HTTP API via `curl` — searching logs, streams, dashboards, and alerts
- [Better Stack CLI](skills/betterstack-cli/SKILL.md) — Perform Better Stack (Logs, Uptime, Incidents) operations using the HTTP API via `curl`
- [Loggly CLI](skills/loggly-cli/SKILL.md) — Perform Loggly log management operations using the HTTP API via `curl` — searches and source groups
- [Papertrail CLI](skills/papertrail-cli/SKILL.md) — Perform Papertrail log management operations using the HTTP API via `curl` — searches and systems
- [Rollbar CLI](skills/rollbar-cli/SKILL.md) — Perform Rollbar error monitoring operations using the HTTP API via `curl` — items, occurrences, and deploys
- [Dynatrace CLI](skills/dynatrace-cli/SKILL.md) — Perform Dynatrace operations using the HTTP API via `curl` — entities, metrics, problems, and events
- [Sentry CLI](skills/sentry-cli/SKILL.md) — Perform Sentry error tracking operations using `sentry-cli` and the HTTP API — issues, events, and releases
- [AppDynamics CLI](skills/appdynamics-cli/SKILL.md) — Perform AppDynamics operations using the HTTP API via `curl` — applications, metrics, health rules, and events

## 💾 Databases

- [PostgreSQL CLI](skills/postgresql-cli/SKILL.md) — Perform PostgreSQL operations using the `psql` CLI — inspecting schemas and running read-only queries
- [MySQL CLI](skills/mysql-cli/SKILL.md) — Perform MySQL operations using the `mysql` CLI — inspecting schemas and running read-only queries

## 📋 Project Management

- [Jira and Confluence CLI](skills/jira-and-confluence-cli/SKILL.md) — Perform Jira and Confluence operations using the Atlassian CLI (`acli`) — issues, sprints, boards, pages, and spaces
- [Maintenance Create Ticket on Jira](skills/maintenance-create-ticket-on-jira/SKILL.md) — Create or update a Jira ticket for a workload maintenance upgrade based on maintenance upgrade check files

## 💼 Business Platforms

- [Salesforce CLI](skills/salesforce-cli/SKILL.md) — Perform Salesforce operations using the `sf` CLI — orgs, records, metadata, and deployments

## 💬 Collaboration

- [Slack CLI](skills/slack-cli/SKILL.md) — Perform Slack operations using the Slack CLI and Slack Web API — messaging, channels, users, and app management
- [Microsoft Teams CLI](skills/ms-teams-cli/SKILL.md) — Perform Microsoft Teams operations using the Microsoft Graph API via `curl` — channel messages, teams, and users

# 📖 How to Use Skills

Skills are placed in different directories depending on your AI agent:

| Agent | Skill path |
| ----- | ---------- |
| Claude Code | `.claude/skills/<skill-name>/SKILL.md` |
| GitHub Copilot | `.github/skills/<skill-name>/SKILL.md` |
| Other agents | `skills/<skill-name>/SKILL.md` |

## ⚙️ Setup

1. Copy the desired `SKILL.md` file from the `/skills` directory into the correct path for your AI agent (see table above)
2. If the skill contains `<placeholder>` values, replace them with your actual configuration
3. Invoke the skill by name in your AI agent conversation

## 🎯 Tips for Best Results

- Review each skill's **Configuration** section and update any placeholder values before use
- Be specific about your requirements when the skill asks for clarification
- Ensure you have the necessary CLI tools installed and authenticated (each skill lists prerequisites)

**Note: For more details, refer to the chosen skill's `SKILL.md` file.**

# 🛡️ Claude Code Guardrails

The `.claude/` directory contains security guardrails for [Claude Code](https://claude.ai/code) that enforce safe, read-only behaviour by default and prevent destructive or irreversible actions.

| File | Description |
| ---- | ----------- |
| [.claude/settings.json](.claude/settings.json) | Hardcoded `deny` and `allow` lists for Bash, Read, and Edit tools |
| [.claude/rules/security.md](.claude/rules/security.md) | Non-negotiable safety rules loaded at every session start |
| [.claude/rules/aws.md](.claude/rules/aws.md) | AWS-specific rules: credential safety, allowed read-only operations, and blocked destructive/provisioning/IAM commands |

## 🚫 What is blocked

`settings.json` hard-blocks the following via the `deny` list — Claude Code will never execute these regardless of instructions:

- **AWS destructive operations**: `delete*`, `remove*`, `destroy*`, `terminate*`, `deregister*`, `purge*`, `s3 rm`, `s3 rb`, `cloudformation delete-stack`
- **AWS provisioning**: `ec2 run-instances`, `ec2 stop-instances`, autoscaling mutations, `ecs update-service`
- **AWS IAM mutations**: `iam create*`, `iam delete*`, `iam put*`, `iam attach*`
- **Kubernetes mutations**: `kubectl delete`, `apply`, `create`, `patch`, `scale`, `rollout`, `drain`, `cordon`, `exec`, `edit`
- **Terraform mutations**: `apply`, `destroy`, `import`, `state rm`, `taint`
- **Helm mutations**: `install`, `upgrade`, `delete`, `uninstall`, `rollback`
- **Dangerous git operations**: force push, push to `main`/`master`, `reset --hard`, `clean -f`
- **System-level danger**: `sudo`, `chmod 777`, `chown root`, `rm -rf` on system paths, `mkfs`, `dd`, `ssh`, `scp`, pipe-to-shell (`curl | bash`, `wget | sh`)
- **Credential file reads**: `~/.aws/**`, `~/.kube/**`, `~/.ssh/**`, `.env`, `**/secrets/**`, `**/*credentials*`

## ✅ What is always allowed

The `allow` list pre-approves safe, read-only operations without requiring user confirmation:

- **Git read operations**: `log`, `status`, `diff`, `branch`, `show`, `rev-parse`, `remote -v`
- **AWS read-only**: `describe*`, `list*`, `get*`, `s3 ls`, `sts get-caller-identity`
- **Kubernetes read-only**: `kubectl get`, `kubectl describe`, `kubectl logs`
- **Terraform safe operations**: `plan`, `validate`, `fmt`
- **Test runners**: npm, npx, pnpm, go, mvn, gradle, pytest
- **File operations**: `Read(./**)`, `Edit(./**)` within the project directory

## 🔧 Setup for Claude Code

1. Copy the `.claude/` directory to the root of your project
2. Adjust the `deny`/`allow` lists in `settings.json` to match your project's needs
3. If you use hooks, add your hook scripts to `.claude/hooks/` and register them in `settings.json` under the `hooks` key

# 🔍 Skills in Detail

## 🛠️ Development Tools

### Code Review

This skill helps you perform comprehensive code reviews by analyzing changes between Git branches and providing detailed feedback:

- **Branch Comparison**: Analyzes code differences using `git diff` between source and target branches
- **Security Analysis**: Checks for secrets exposure, SQL injection, XSS, and other vulnerabilities
- **Performance Review**: Identifies inefficient algorithms, memory leaks, and unnecessary computations
- **Code Quality**: Looks for code smells, duplicate code, complex functions, and dead code
- **Best Practices**: Verifies proper error handling, logging, and documentation standards
- **Targeted Suggestions**: Provides specific code snippets with improvements, file paths, and line numbers

### Commit Code

This skill helps you automate your Git workflow by analyzing code changes and creating proper commits:

- **Automatic Code Analysis**: Analyzes all uncommitted changes to understand the scope and impact
- **Conventional Commits**: Determines the appropriate conventional commit type based on code changes, with no scope, based on established examples
- **Branch Management**: Creates new branches following Git best practices
- **Quality Checks**: Optionally runs pre-commit hooks and Terraform formatting before committing
- **Automated Workflow**: Handles the complete Git workflow from branch creation to pushing changes

### Create Dockerfile

This skill helps you create optimized, secure, and production-ready Dockerfiles:

- **Context Analysis**: Analyzes project files to understand dependencies and build requirements
- **Best Practices**: Follows Docker best practices including proper layer ordering and caching optimization
- **Size Optimization**: Creates minimal container images through multi-stage builds
- **Security Focus**: Implements security best practices including non-root users
- **Technology Detection**: Automatically detects your application stack from project files

### GitHub CLI

This skill helps you perform GitHub operations using the `gh` CLI:

- **Full GitHub Operations**: Issues, pull requests, pipelines, repositories, and project management
- **PR Code Review**: Fetches PR diffs and performs a full code review when given a PR URL or ID
- **Paginated Results**: Always fetches all pages to avoid missing items

### GitLab CLI

This skill helps you perform GitLab operations using the `glab` CLI:

- **Full GitLab Operations**: Issues, merge requests, pipelines, repositories, and project management
- **MR Code Review**: Fetches MR diffs and performs a full code review when given an MR URL or ID
- **Self-Hosted Support**: Works with any self-hosted GitLab instance — configure your hostname once
- **Paginated Results**: Always fetches all pages to avoid missing items

### Bitbucket CLI

This skill helps you perform Bitbucket operations using the HTTP API:

- **Full Bitbucket Operations**: Repositories, pull requests, pipelines, and issues
- **PR Code Review**: Fetches PR diffs and performs a full code review when given a PR URL or ID
- **Basic Auth via App Password**: Reads credentials from a token file — never exposes them

## 🧹 Code Quality & Linting

### Python Linting

This skill lints and formats Python code:

- **Tool Auto-Detection**: Prefers `ruff` when available, falls back to `flake8`/`black`/`isort`
- **Check-First Workflow**: Always runs in check/diff mode before applying any auto-fix
- **Optional Type Checking**: Runs `mypy` when the project uses type hints
- **Config Respecting**: Never overrides the project's existing linter/formatter configuration

### Bash Linting

This skill lints and formats Bash/shell scripts:

- **Static Analysis**: Runs `shellcheck` against all `.sh`/`.bash` files and shebang-tagged scripts
- **Formatting Checks**: Uses `shfmt` in diff mode before applying any formatting
- **Security-Aware**: Flags `eval`, unquoted globs, and command injection risks even beyond default linter output

### Go Linting

This skill lints and formats Go code:

- **Standard Toolchain**: Uses `gofmt`, `go vet`, and `golangci-lint` together
- **Non-Negotiable Formatting**: Treats `gofmt` compliance as mandatory, not a style preference
- **Config Respecting**: Honors the project's existing `.golangci.yml` ruleset

### Dockerfile Linting

This skill lints Dockerfiles for issues:

- **Static Analysis**: Runs `hadolint` against all Dockerfiles in the project
- **Security-Aware**: Always flags missing non-root `USER`, unpinned base images, and unpinned package versions
- **Config Respecting**: Honors the project's existing `.hadolint.yaml` ignore list

### Terraform Linting

This skill lints and formats Terraform code:

- **Standard Toolchain**: Uses `terraform fmt`, `terraform validate`, and `tflint` together
- **Check-First Workflow**: Always runs in check/diff mode before applying any auto-fix
- **Security-Aware**: Flags hardcoded secret-looking strings regardless of linter configuration

## 🏗️ Infrastructure & DevOps

### Create Terraform Helm Upgrade Plan

This skill helps you safely upgrade Helm releases managed by Terraform:

- **Automatic Detection**: Scans Terraform code to identify Helm release resources
- **Version Comparison**: Compares template files and default values between chart versions
- **Breaking Change Analysis**: Identifies potential breaking changes and compatibility issues
- **Detailed Planning**: Creates a comprehensive `UPGRADE_PLAN.md` with step-by-step instructions

### Maintenance Check AWS EKS

This skill checks an AWS EKS cluster for available upgrades:

- **Cluster Version**: Compares current vs. latest available Kubernetes version
- **Add-on Versions**: Checks each installed EKS add-on against the latest compatible version
- **Node Group AMIs**: Identifies outdated AMI release versions per node group
- **Structured Output**: Writes findings to a Markdown file with a ready-to-use Jira ticket

### Kubernetes CLI

This skill helps you perform Kubernetes operations via `kubectl`:

- **Context & Namespace Enforcement**: Always passes `--context` and `--namespace` explicitly to avoid targeting the wrong cluster
- **Three-Tier Command Safety**: Read-only, mutating-with-confirmation, and never-run-without-approval command tables
- **Secret-Safe**: Never prints or stores Secret values, even when inspecting resources

### Terraform CLI

This skill helps you perform Terraform operations via the `terraform` CLI:

- **Workspace Enforcement**: Always selects and confirms the correct workspace/var-file before running commands
- **Plan-Before-Apply**: Always shows a plan and gets confirmation before ever suggesting an apply
- **Three-Tier Command Safety**: Read-only, mutating-with-confirmation, and never-run-without-approval command tables

### Helm CLI

This skill helps you perform Helm operations via the `helm` CLI:

- **Context & Namespace Enforcement**: Always passes `--kube-context` and `--namespace` explicitly
- **Diff-Before-Install**: Always renders templates or shows a diff before suggesting an install/upgrade
- **Three-Tier Command Safety**: Read-only, mutating-with-confirmation, and never-run-without-approval command tables

### Ansible CLI

This skill helps you run Ansible playbooks and ad-hoc commands:

- **Inventory Enforcement**: Always passes `-i <inventory-file>` explicitly to avoid targeting the wrong hosts
- **Check-Before-Run**: Always runs with `--check --diff` first before suggesting a real playbook run
- **Vault-Safe**: Never prints or stores the contents of `ansible-vault`-encrypted files

### HashiCorp Vault CLI

This skill helps you inspect Vault structure and configuration:

- **Metadata-Only by Design**: Reads secret metadata and structure, never actual secret values
- **Address Enforcement**: Always sets `VAULT_ADDR` explicitly per command, never relying on exported environment variables
- **Destructive-Action-Safe**: Never seals/unseals the cluster, revokes tokens, or deletes secrets without explicit approval

### ArgoCD CLI

This skill helps you inspect and manage GitOps application deployments:

- **Server Enforcement**: Always passes `--server` and `--auth-token` explicitly
- **Diff-Before-Sync**: Always shows a diff before suggesting a sync
- **Three-Tier Command Safety**: Read-only, mutating-with-confirmation, and never-run-without-approval command tables

### Packer CLI

This skill helps you validate and build machine images:

- **Validate-Before-Build**: Always runs `packer validate` and `packer fmt -check -diff` before suggesting a build
- **Cost-Aware**: Flags that builds provision real, billable cloud resources
- **Credential-Safe**: Never prints or stores cloud provider credentials embedded in variable files

### Chef CLI

This skill helps you inspect Chef server state via `knife`:

- **Config Enforcement**: Always passes `--config` explicitly to avoid targeting the wrong organization
- **Dry-Run First**: Always runs `chef-client --why-run` before suggesting a real cookbook run
- **Three-Tier Command Safety**: Read-only, mutating-with-confirmation, and never-run-without-approval command tables

### Jenkins CLI

This skill helps you manage Jenkins instances via the `jenkins-cli` JAR:

- **Job Management**: List, create, update, copy, enable, and disable jobs
- **Build Operations**: Trigger, monitor, and replay pipeline builds
- **Node & View Management**: Manage agents, views, and configuration
- **Multi-Instance Support**: Configure multiple Jenkins environments (prod/staging)

## ☁️ Cloud Providers

### AWS CLI

This skill ensures AWS CLI commands always use the correct profile and region:

- **Profile & Region Enforcement**: Always passes `--profile` and `--region` explicitly to avoid using the wrong account or location
- **Environment Awareness**: Distinguishes between production, staging/non-prod, and sandbox/dev profiles
- **Configurable**: Update the profile and default region map once with your AWS profile names
- **Identity Verification**: Falls back to `aws sts get-caller-identity` when it's unclear which account a profile belongs to

### GCP CLI

This skill ensures `gcloud` commands always use the correct project, account, and region:

- **Project & Account Enforcement**: Always passes `--project` and `--account` explicitly
- **Environment Awareness**: Distinguishes between production, staging/non-prod, and sandbox/dev projects
- **Destructive-Action-Safe**: Never deletes resources or modifies IAM bindings without explicit approval

### Azure CLI

This skill ensures `az` commands always use the correct subscription and region:

- **Subscription Enforcement**: Always passes `--subscription` explicitly
- **Environment Awareness**: Distinguishes between production, staging/non-prod, and sandbox/dev subscriptions
- **Destructive-Action-Safe**: Never deletes resource groups or modifies role assignments without explicit approval

### OCI CLI

This skill ensures Oracle Cloud Infrastructure `oci` commands always use the correct profile and region:

- **Profile & Region Enforcement**: Always passes `--profile` and `--region` explicitly
- **Environment Awareness**: Distinguishes between production and staging/non-prod profiles
- **Destructive-Action-Safe**: Never terminates instances or deletes databases without explicit approval

### DigitalOcean CLI

This skill ensures `doctl` commands always use the correct context and region:

- **Context Enforcement**: Always passes `--context` explicitly
- **Environment Awareness**: Distinguishes between production and staging/non-prod contexts
- **Destructive-Action-Safe**: Never deletes droplets, clusters, or databases without explicit approval

### Alibaba Cloud CLI

This skill ensures `aliyun` commands always use the correct profile and region:

- **Profile & Region Enforcement**: Always passes `--profile` and `--region` explicitly
- **Environment Awareness**: Distinguishes between production and staging/non-prod profiles
- **Destructive-Action-Safe**: Never deletes instances or modifies RAM policies without explicit approval

### Huawei Cloud CLI

This skill ensures `hcloud` (Huawei Cloud CLI) commands always use the correct profile and region:

- **Profile & Region Enforcement**: Always passes `--cli-profile` and `--cli-region` explicitly
- **Environment Awareness**: Distinguishes between production and staging/non-prod profiles
- **Destructive-Action-Safe**: Never deletes instances or modifies IAM policies without explicit approval

### Hetzner Cloud CLI

This skill ensures Hetzner Cloud `hcloud` commands always use the correct context:

- **Context Enforcement**: Always passes `--context` explicitly
- **Environment Awareness**: Distinguishes between production and staging/non-prod contexts
- **Destructive-Action-Safe**: Never deletes servers, volumes, or firewalls without explicit approval

## 📊 Monitoring & Observability

### Grafana CLI

This skill helps you query and manage Grafana resources via the HTTP API:

- **Read-Only Queries**: Dashboards, datasources, folders, alerts, and annotations
- **Safe Mutations**: Create/update dashboards, folders, and alert rules with user confirmation
- **Token-Based Auth**: Reads credentials from token files — never exposes them

### Prometheus CLI

This skill queries Prometheus metrics via the HTTP API:

- **Instant & Range Queries**: Evaluate PromQL expressions at a point in time or over a range
- **Metadata & Exploration**: List labels, series, rules, and firing alerts
- **Token-Based Auth**: Reads credentials from token files — never exposes them

### Kibana CLI

This skill helps you query and manage Kibana resources via the HTTP API:

- **Read-Only Queries**: Saved objects, dashboards, index patterns, data views, and alerting rules
- **Safe Mutations**: Create/update saved objects and alerting rules with user confirmation
- **XSRF-Aware**: Always includes the required `kbn-xsrf` header on requests

### Elasticsearch CLI

This skill helps you query Elasticsearch via the HTTP API:

- **Read-Only Queries**: Cluster health, indices, mappings, and document search
- **Safe Mutations**: Create/update documents and index settings with user confirmation
- **Bounded Queries**: Always sets a reasonable `size` limit on searches

### Jaeger CLI

This skill helps you query Jaeger distributed tracing data via the HTTP API:

- **Trace Search**: Find traces by service, operation, duration, and time range
- **Dependency Graphs**: Fetch service dependency graphs
- **Read-Only by Design**: Jaeger's Query API exposes no mutating or destructive endpoints

### Datadog CLI

This skill helps you query and manage Datadog resources via the HTTP API:

- **Read-Only Queries**: Metrics, logs, monitors, dashboards, and events
- **Safe Mutations**: Create/update monitors and dashboards with user confirmation
- **Dual-Key Auth**: Reads both API and Application keys from token files — never exposes them

### Splunk CLI

This skill helps you query Splunk via the REST API:

- **Search Queries**: Run SPL searches and fetch results
- **Saved Search & Index Inspection**: List saved searches, indexes, and fired alerts
- **Bounded Queries**: Always sets a time range and result limit on searches

### New Relic CLI

This skill helps you query New Relic via the `newrelic` CLI:

- **NRQL Queries**: Run NRQL queries against telemetry data
- **Entity & Alert Inspection**: Search entities, applications, and alert policies/conditions
- **Profile Enforcement**: Always passes `--profile` explicitly

### AWS CloudWatch CLI

This skill helps you query AWS CloudWatch metrics, alarms, and logs:

- **Metrics & Alarms**: Query metric datapoints and alarm state/history
- **Logs Insights**: Run bounded CloudWatch Logs Insights queries
- **Builds on AWS CLI Skill**: Reuses the same profile/region configuration

### Grafana Loki CLI

This skill helps you query Grafana Loki logs via the HTTP API or `logcli`:

- **LogQL Queries**: Instant and range queries over log streams
- **Label Exploration**: List labels, values, and matching series
- **Bounded Queries**: Always sets a time range and limit on queries

### Graylog CLI

This skill helps you query and manage Graylog resources via the HTTP API:

- **Search Queries**: Run relative-time search queries over log streams
- **Stream & Dashboard Inspection**: List streams, dashboards, and triggered alerts
- **Safe Mutations**: Create/update streams and dashboards with user confirmation

### Better Stack CLI

This skill helps you query Better Stack Logs, Uptime, and Incidents via the HTTP API:

- **Monitor & Incident Inspection**: List uptime monitors, heartbeats, and incidents
- **Log Queries**: Query telemetry log data
- **Safe Mutations**: Create/update monitors and acknowledge/resolve incidents with user confirmation

### Loggly CLI

This skill helps you search Loggly logs via the HTTP API:

- **Search Queries**: Run search queries and fetch results by search job ID
- **Saved Search & Source Group Inspection**: List saved searches and source groups
- **Bounded Queries**: Always sets a time range and result size on searches

### Papertrail CLI

This skill helps you search Papertrail logs via the HTTP API:

- **Search Queries**: Run log event searches scoped by system and query
- **System & Saved Search Inspection**: List registered systems and saved searches
- **Token-Based Auth**: Reads credentials from a token file — never exposes them

### Rollbar CLI

This skill helps you query Rollbar error monitoring data via the HTTP API:

- **Item & Occurrence Inspection**: List grouped errors and their individual occurrences
- **Deploy Tracking**: List and report deploys
- **Read-Token Enforcement**: Prefers a read-only project access token over a write-scoped one

### Dynatrace CLI

This skill helps you query Dynatrace via the HTTP API:

- **Entity & Metric Queries**: Query monitored entities, metrics, and events
- **Problem Inspection**: List and inspect detected problems
- **Safe Mutations**: Create metric events/alert rules with user confirmation

### Sentry CLI

This skill helps you query Sentry error tracking data via `sentry-cli` and the HTTP API:

- **Issue & Event Inspection**: List issues, events, and releases
- **Deploy Tracking**: Create releases and register deploys with user confirmation
- **Token-Based Auth**: Reads credentials from a token file — never exposes them

### AppDynamics CLI

This skill helps you query AppDynamics via the HTTP API:

- **Application & Metric Queries**: Query applications, nodes, and metric data
- **Health Rule Inspection**: List health rule violations and events
- **OAuth-Aware**: Fetches a fresh short-lived bearer token per session rather than caching it

## 💾 Databases

### PostgreSQL CLI

This skill helps you inspect PostgreSQL databases via `psql`:

- **Schema Inspection**: List databases, tables, indexes, and roles
- **Connection-String Safety**: Always uses `service=` connection strings backed by `~/.pg_service.conf`/`.pgpass`, never raw credentials on the command line
- **Row-Count-Before-Mutate**: Always shows the equivalent `SELECT` before any `UPDATE`/`DELETE`

### MySQL CLI

This skill helps you inspect MySQL databases via the `mysql` CLI:

- **Schema Inspection**: List databases, tables, indexes, and grants
- **Connection-String Safety**: Always uses `--defaults-group-suffix` backed by `~/.my.cnf`, never raw credentials on the command line
- **Row-Count-Before-Mutate**: Always shows the equivalent `SELECT` before any `UPDATE`/`DELETE`

## 📋 Project Management

### Jira and Confluence CLI

This skill helps you manage Jira and Confluence using the Atlassian CLI (`acli`):

- **Jira Operations**: Create, update, and search issues; manage sprints and boards
- **Confluence Operations**: Create and update pages; manage spaces
- **Documented Command Structure**: Lists common `acli jira`/`acli confluence` subcommands and ready-to-use examples
- **Safe by Default**: Never deletes issues or pages without explicit user instruction

### Maintenance Create Ticket on Jira

This skill creates or updates Jira maintenance upgrade tickets:

- **File-Driven**: Reads from maintenance check output files under `./maintenance-checks/`
- **ADF Formatting**: Converts Markdown descriptions to Atlassian Document Format automatically
- **Create or Update**: Detects existing open tickets and updates them instead of duplicating

## 💼 Business Platforms

### Salesforce CLI

This skill helps you perform Salesforce operations using the `sf` CLI:

- **SOQL Queries**: Run queries and inspect records
- **Metadata Retrieval**: Retrieve and inspect Apex classes and other metadata
- **Org Enforcement**: Always passes `--target-org` explicitly to avoid targeting the wrong org

## 💬 Collaboration

### Slack CLI

This skill performs Slack operations using two tools:

- **App Management**: Install, deploy, and manage Slack apps using the `slack` CLI
- **Messaging & Channels**: Post messages, list channels, read history, and look up users via the Slack Web API
- **Safe Messaging**: Always confirms channel and message text with the user before posting

### Microsoft Teams CLI

This skill performs Microsoft Teams operations using the Microsoft Graph API:

- **Channel Messaging**: Post messages, list teams/channels, and look up users
- **Safe Messaging**: Always confirms team/channel and message text with the user before posting
- **Token-Based Auth**: Reads OAuth tokens from a token file — never exposes them

# 🤝 Contributing

Contributions are welcome! If you have a skill you'd like to share:

1. Fork this repository
2. Add your new skill under `skills/<skill-name>/SKILL.md`
3. Update this README.md to include your contribution in the appropriate category section
4. Submit a pull request

## 📖 Skill Guidelines

- Use descriptive names and clear descriptions
- Include a **Configuration** section for any values the user must customize
- Never hardcode company-specific URLs, credentials, project keys, or team names — use `<placeholder>` values
- Add a **Rules** section covering safety constraints, error handling, and edge cases
- Keep the skill focused — one skill, one workflow
- Follow the established file naming convention: `<skill-name>/SKILL.md`

# 📝 License

This project is licensed under the Apache License - see the [LICENSE](LICENSE) file for details.

---

###### Any contributions, improvements and suggestions will be highly appreciated. 😊
