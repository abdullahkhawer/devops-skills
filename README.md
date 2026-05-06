# DevOps Skills

- Author: [Abdullah Khawer - LinkedIn](https://www.linkedin.com/in/abdullah-khawer)

# 💡 Introduction

`devops-skills` is a curated collection of **DevOps Skills** that enhance your development using AI to work smarter.

**Skills** are reusable, step-by-step instruction files (`SKILL.md`) that can be used across almost every major AI agent product including [Claude Code](https://claude.ai/code), [GitHub Copilot](https://github.com/features/copilot), [OpenCode](https://opencode.ai), [Mistral AI](https://mistral.ai), [Cursor](https://www.cursor.com), and more.

Each skill builds a specialized AI assistant with targeted knowledge and abilities for your DevOps workflow, significantly cutting development time by eliminating repetitive manual tasks.

Once a skill is loaded into your AI agent, it is automatically invoked whenever your prompt contains relevant context that matches the skill's purpose — no explicit command needed.

# 📚 Official Documentation

- [Agent Skills](https://agentskills.io/home)
- [Claude Code Skills](https://docs.anthropic.com/en/docs/claude-code/skills)

# 🚀 Available Skills

| Skill | Description | Category |
| ----- | ----------- | -------- |
| [Code Review](skills/code-review/SKILL.md) | Analyze code changes between two local Git branches and perform a comprehensive code review | Development Tools |
| [Commit Code](skills/commit-code/SKILL.md) | Analyze code changes, prepare conventional commit messages, and commit to a new branch | Development Tools |
| [Create Dockerfile](skills/create-dockerfile/SKILL.md) | Create optimized, secure, production-ready Dockerfiles based on user requirements and application context | Development Tools |
| [Create Skill](skills/create-skill/SKILL.md) | Create a new `SKILL.md` file based on a guided conversation about a task or workflow | Development Tools |
| [GitHub CLI](skills/github-cli/SKILL.md) | Perform GitHub operations using the `gh` CLI — issues, pull requests, pipelines, repositories, and PR code reviews | Development Tools |
| [GitLab CLI](skills/gitlab-cli/SKILL.md) | Perform GitLab operations using the `glab` CLI — issues, merge requests, pipelines, repositories, and MR code reviews | Development Tools |
| [AWS CLI](skills/aws-cli/SKILL.md) | Use the correct AWS CLI profile when running AWS commands based on the target environment | Infrastructure & DevOps |
| [Create Terraform Helm Upgrade Plan](skills/create-terraform-helm-upgrade-plan/SKILL.md) | Create a detailed upgrade plan for a Helm release managed by Terraform, comparing chart versions including breaking changes | Infrastructure & DevOps |
| [Maintenance Check AWS EKS](skills/maintenance-check-aws-eks/SKILL.md) | Check available upgrades across an AWS EKS cluster and output a decision file with Jira-formatted tickets | Infrastructure & DevOps |
| [Jenkins CLI](skills/jenkins-cli/SKILL.md) | Perform Jenkins operations using the `jenkins-cli` JAR — jobs, builds, nodes, and pipeline management | CI/CD |
| [Grafana CLI](skills/grafana-cli/SKILL.md) | Perform Grafana operations using the Grafana HTTP API via `curl` — dashboards, datasources, alerts, and annotations | Monitoring & Observability |
| [Prometheus CLI](skills/prometheus-cli/SKILL.md) | Query Prometheus metrics using the HTTP API via `curl` — instant queries, range queries, metadata, and alerts | Monitoring & Observability |
| [Jira and Confluence CLI](skills/jira-and-confluence-cli/SKILL.md) | Perform Jira and Confluence operations using the Atlassian CLI (`acli`) — issues, sprints, boards, pages, and spaces | Project Management |
| [Maintenance Create Ticket on Jira](skills/maintenance-create-ticket-on-jira/SKILL.md) | Create or update a Jira ticket for a workload maintenance upgrade based on maintenance upgrade check files | Project Management |
| [Slack CLI](skills/slack-cli/SKILL.md) | Perform Slack operations using the Slack CLI and Slack Web API — messaging, channels, users, and app management | Collaboration |

# 📖 How to Use Skills

Skills are placed in different directories depending on your AI agent:

| Agent | Skill path |
| ----- | ---------- |
| Claude Code | `.claude/skills/<skill-name>/SKILL.md` |
| GitHub Copilot | `.github/skills/<skill-name>/SKILL.md` |
| Other agents | `skills/<skill-name>/SKILL.md` |

## Setup

1. Copy the desired `SKILL.md` file from the `/skills` directory into the correct path for your AI agent (see table above)
2. If the skill contains `<placeholder>` values, replace them with your actual configuration
3. Invoke the skill by name in your AI agent conversation

## Tips for Best Results

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

## What is blocked

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

## What is always allowed

The `allow` list pre-approves safe, read-only operations without requiring user confirmation:

- **Git read operations**: `log`, `status`, `diff`, `branch`, `show`, `rev-parse`, `remote -v`
- **AWS read-only**: `describe*`, `list*`, `get*`, `s3 ls`, `sts get-caller-identity`
- **Kubernetes read-only**: `kubectl get`, `kubectl describe`, `kubectl logs`
- **Terraform safe operations**: `plan`, `validate`, `fmt`
- **Test runners**: npm, npx, pnpm, go, mvn, gradle, pytest
- **File operations**: `Read(./**)`, `Edit(./**)` within the project directory

## Setup for Claude Code

1. Copy the `.claude/` directory to the root of your project
2. Adjust the `deny`/`allow` lists in `settings.json` to match your project's needs
3. If you use hooks, add your hook scripts to `.claude/hooks/` and register them in `settings.json` under the `hooks` key

# 🔍 Skills in Detail

## Code Review

This skill helps you perform comprehensive code reviews by analyzing changes between Git branches and providing detailed feedback:

- **Branch Comparison**: Analyzes code differences using `git diff` between source and target branches
- **Security Analysis**: Checks for secrets exposure, SQL injection, XSS, and other vulnerabilities
- **Performance Review**: Identifies inefficient algorithms, memory leaks, and unnecessary computations
- **Code Quality**: Looks for code smells, duplicate code, complex functions, and dead code
- **Best Practices**: Verifies proper error handling, logging, and documentation standards
- **Targeted Suggestions**: Provides specific code snippets with improvements, file paths, and line numbers

## Commit Code

This skill helps you automate your Git workflow by analyzing code changes and creating proper commits:

- **Automatic Code Analysis**: Analyzes all uncommitted changes to understand the scope and impact
- **Conventional Commits**: Determines the appropriate conventional commit type based on code changes
- **Branch Management**: Creates new branches following Git best practices
- **Quality Checks**: Optionally runs pre-commit hooks and Terraform formatting before committing
- **Automated Workflow**: Handles the complete Git workflow from branch creation to pushing changes

## Create Dockerfile

This skill helps you create optimized, secure, and production-ready Dockerfiles:

- **Context Analysis**: Analyzes project files to understand dependencies and build requirements
- **Best Practices**: Follows Docker best practices including proper layer ordering and caching optimization
- **Size Optimization**: Creates minimal container images through multi-stage builds
- **Security Focus**: Implements security best practices including non-root users
- **Technology Detection**: Automatically detects your application stack from project files

## Create Skill

This meta-skill helps you create new skills through a guided conversation:

- **Interactive Creation**: Walks you through a two-question conversation to define your skill requirements
- **Agent-Aware**: Asks which AI agent you are using and places the file in the correct directory (`.claude/skills/`, `.github/skills/`, or `skills/`)
- **Template Generation**: Automatically generates properly formatted `SKILL.md` files
- **Best Practices**: Ensures your skill follows established conventions and formatting

## GitHub CLI

This skill helps you perform GitHub operations using the `gh` CLI:

- **Full GitHub Operations**: Issues, pull requests, pipelines, repositories, and project management
- **PR Code Review**: Fetches PR diffs and performs a full code review when given a PR URL or ID
- **Paginated Results**: Always fetches all pages to avoid missing items

## GitLab CLI

This skill helps you perform GitLab operations using the `glab` CLI:

- **Full GitLab Operations**: Issues, merge requests, pipelines, repositories, and project management
- **MR Code Review**: Fetches MR diffs and performs a full code review when given an MR URL or ID
- **Self-Hosted Support**: Works with any self-hosted GitLab instance — configure your hostname once
- **Paginated Results**: Always fetches all pages to avoid missing items

## AWS CLI

This skill ensures AWS CLI commands always use the correct profile:

- **Profile Enforcement**: Always passes `--profile` explicitly to avoid using the wrong account
- **Environment Awareness**: Distinguishes between production and staging/non-prod profiles
- **Configurable**: Update the profile map once with your AWS profile names

## Create Terraform Helm Upgrade Plan

This skill helps you safely upgrade Helm releases managed by Terraform:

- **Automatic Detection**: Scans Terraform code to identify Helm release resources
- **Version Comparison**: Compares template files and default values between chart versions
- **Breaking Change Analysis**: Identifies potential breaking changes and compatibility issues
- **Detailed Planning**: Creates a comprehensive `UPGRADE_PLAN.md` with step-by-step instructions

## Maintenance Check AWS EKS

This skill checks an AWS EKS cluster for available upgrades:

- **Cluster Version**: Compares current vs. latest available Kubernetes version
- **Add-on Versions**: Checks each installed EKS add-on against the latest compatible version
- **Node Group AMIs**: Identifies outdated AMI release versions per node group
- **Structured Output**: Writes findings to a Markdown file with a ready-to-use Jira ticket

## Jenkins CLI

This skill helps you manage Jenkins instances via the `jenkins-cli` JAR:

- **Job Management**: List, create, update, copy, enable, and disable jobs
- **Build Operations**: Trigger, monitor, and replay pipeline builds
- **Node & View Management**: Manage agents, views, and configuration
- **Multi-Instance Support**: Configure multiple Jenkins environments (prod/staging)

## Grafana CLI

This skill helps you query and manage Grafana resources via the HTTP API:

- **Read-Only Queries**: Dashboards, datasources, folders, alerts, and annotations
- **Safe Mutations**: Create/update dashboards, folders, and alert rules with user confirmation
- **Token-Based Auth**: Reads credentials from token files — never exposes them

## Prometheus CLI

This skill queries Prometheus metrics via the HTTP API:

- **Instant & Range Queries**: Evaluate PromQL expressions at a point in time or over a range
- **Metadata & Exploration**: List labels, series, rules, and firing alerts
- **Token-Based Auth**: Reads credentials from token files — never exposes them

## Jira and Confluence CLI

This skill helps you manage Jira and Confluence using the Atlassian CLI (`acli`):

- **Jira Operations**: Create, update, and search issues; manage sprints and boards
- **Confluence Operations**: Create and update pages; manage spaces
- **Safe by Default**: Never deletes issues or pages without explicit user instruction

## Maintenance Create Ticket on Jira

This skill creates or updates Jira maintenance upgrade tickets:

- **File-Driven**: Reads from maintenance check output files under `./maintenance-checks/`
- **ADF Formatting**: Converts Markdown descriptions to Atlassian Document Format automatically
- **Create or Update**: Detects existing open tickets and updates them instead of duplicating

## Slack CLI

This skill performs Slack operations using two tools:

- **App Management**: Install, deploy, and manage Slack apps using the `slack` CLI
- **Messaging & Channels**: Post messages, list channels, read history, and look up users via the Slack Web API
- **Safe Messaging**: Always confirms channel and message text with the user before posting

# 🤝 Contributing

Contributions are welcome! If you have a skill you'd like to share:

1. Fork this repository
2. Add your new skill under `skills/<skill-name>/SKILL.md`
3. Update this README.md to include your contribution in the skills table
4. Submit a pull request

## Skill Guidelines

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
