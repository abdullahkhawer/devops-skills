# DevOps Skills

- Author: [Abdullah Khawer - LinkedIn](https://www.linkedin.com/in/abdullah-khawer)

# Introduction

A curated collection of **Skills** and **GitHub Copilot Chat Modes** for VS Code that enhance your development using AI to work smarter.

**Skills** are reusable, step-by-step instruction files (`SKILL.md`) that can be used across almost every major AI agent product — including [Claude Code](https://claude.ai/code), [GitHub Copilot](https://github.com/features/copilot), [OpenCode](https://opencode.ai), [Mistral AI](https://mistral.ai), [Cursor](https://www.cursor.com), and more. Each skill builds a specialized AI assistant with targeted knowledge and abilities for your DevOps workflow, significantly cutting development time by eliminating repetitive manual tasks.

**GitHub Copilot Chat Modes** are custom `chatmode.md` files specifically for Visual Studio Code that modify GitHub Copilot's behavior for specialized tasks.

# Official Documentation

- [Claude Code Skills](https://docs.anthropic.com/en/docs/claude-code/skills)
- [GitHub Copilot Chat](https://code.visualstudio.com/docs/copilot/chat/copilot-chat)
- [Custom Chat Modes](https://code.visualstudio.com/docs/copilot/chat/chat-modes#_custom-chat-modes)

# 🚀 Available Skills

| Skill | Description | Category |
| ----- | ----------- | -------- |
| [Code Review](skills/code-review/SKILL.md) | Analyze code changes between two local Git branches and perform a comprehensive code review | Development Tools |
| [Commit Code](skills/commit-code/SKILL.md) | Analyze code changes, prepare conventional commit messages, and commit to a new branch | Development Tools |
| [Create Dockerfile](skills/create-dockerfile/SKILL.md) | Create optimized, secure, production-ready Dockerfiles based on user requirements and application context | Infrastructure & DevOps |
| [Create Skill](skills/create-skill/SKILL.md) | Create a new `SKILL.md` file based on a guided conversation about a task or workflow | Development Tools |
| [Create Terraform Helm Upgrade Plan](skills/create-terraform-helm-upgrade-plan/SKILL.md) | Create a detailed upgrade plan for a Helm release managed by Terraform, comparing chart versions including breaking changes | Infrastructure & DevOps |
| [GitHub CLI](skills/github-cli/SKILL.md) | Perform GitHub operations using the `gh` CLI — issues, pull requests, pipelines, repositories, and PR code reviews | Development Tools |
| [GitLab CLI](skills/gitlab-cli/SKILL.md) | Perform GitLab operations using the `glab` CLI — issues, merge requests, pipelines, repositories, and MR code reviews | Development Tools |
| [Grafana CLI](skills/grafana-cli/SKILL.md) | Perform Grafana operations using the Grafana HTTP API via `curl` — dashboards, datasources, alerts, and annotations | Monitoring & Observability |
| [Jenkins CLI](skills/jenkins-cli/SKILL.md) | Perform Jenkins operations using the `jenkins-cli` JAR — jobs, builds, nodes, and pipeline management | CI/CD |
| [Jira and Confluence CLI](skills/jira-and-confluence-cli/SKILL.md) | Perform Jira and Confluence operations using the Atlassian CLI (`acli`) — issues, sprints, boards, pages, and spaces | Project Management |
| [Maintenance Check AWS EKS](skills/maintenance-check-aws-eks/SKILL.md) | Check available upgrades across an AWS EKS cluster and output a decision file with Jira-formatted tickets | Infrastructure & DevOps |
| [Maintenance Create Ticket on Jira](skills/maintenance-create-ticket-on-jira/SKILL.md) | Create or update a Jira ticket for a workload maintenance upgrade based on maintenance upgrade check files | Project Management |
| [Prometheus CLI](skills/prometheus-cli/SKILL.md) | Query Prometheus metrics using the HTTP API via `curl` — instant queries, range queries, metadata, and alerts | Monitoring & Observability |
| [Slack CLI](skills/slack-cli/SKILL.md) | Perform Slack operations using the Slack CLI and Slack Web API — messaging, channels, users, and app management | Collaboration |
| [AWS CLI](skills/aws-cli/SKILL.md) | Use the correct AWS CLI profile when running AWS commands based on the target environment | Infrastructure & DevOps |

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
- **Template Generation**: Automatically generates properly formatted `SKILL.md` files
- **Best Practices**: Ensures your skill follows established conventions and formatting

## Create Terraform Helm Upgrade Plan

This skill helps you safely upgrade Helm releases managed by Terraform:

- **Automatic Detection**: Scans Terraform code to identify Helm release resources
- **Version Comparison**: Compares template files and default values between chart versions
- **Breaking Change Analysis**: Identifies potential breaking changes and compatibility issues
- **Detailed Planning**: Creates a comprehensive `UPGRADE_PLAN.md` with step-by-step instructions

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

## Grafana CLI

This skill helps you query and manage Grafana resources via the HTTP API:

- **Read-Only Queries**: Dashboards, datasources, folders, alerts, and annotations
- **Safe Mutations**: Create/update dashboards, folders, and alert rules with user confirmation
- **Token-Based Auth**: Reads credentials from token files — never exposes them

## Jenkins CLI

This skill helps you manage Jenkins instances via the `jenkins-cli` JAR:

- **Job Management**: List, create, update, copy, enable, and disable jobs
- **Build Operations**: Trigger, monitor, and replay pipeline builds
- **Node & View Management**: Manage agents, views, and configuration
- **Multi-Instance Support**: Configure multiple Jenkins environments (prod/staging)

## Jira and Confluence CLI

This skill helps you manage Jira and Confluence using the Atlassian CLI (`acli`):

- **Jira Operations**: Create, update, and search issues; manage sprints and boards
- **Confluence Operations**: Create and update pages; manage spaces
- **Safe by Default**: Never deletes issues or pages without explicit user instruction

## Maintenance Check AWS EKS

This skill checks an AWS EKS cluster for available upgrades:

- **Cluster Version**: Compares current vs. latest available Kubernetes version
- **Add-on Versions**: Checks each installed EKS add-on against the latest compatible version
- **Node Group AMIs**: Identifies outdated AMI release versions per node group
- **Structured Output**: Writes findings to a Markdown file with a ready-to-use Jira ticket

## Maintenance Create Ticket on Jira

This skill creates or updates Jira maintenance upgrade tickets:

- **File-Driven**: Reads from maintenance check output files under `./maintenance-checks/`
- **ADF Formatting**: Converts Markdown descriptions to Atlassian Document Format automatically
- **Create or Update**: Detects existing open tickets and updates them instead of duplicating

## Prometheus CLI

This skill queries Prometheus metrics via the HTTP API:

- **Instant & Range Queries**: Evaluate PromQL expressions at a point in time or over a range
- **Metadata & Exploration**: List labels, series, rules, and firing alerts
- **Token-Based Auth**: Reads credentials from token files — never exposes them

## Slack CLI

This skill performs Slack operations using two tools:

- **App Management**: Install, deploy, and manage Slack apps using the `slack` CLI
- **Messaging & Channels**: Post messages, list channels, read history, and look up users via the Slack Web API
- **Safe Messaging**: Always confirms channel and message text with the user before posting

## AWS CLI

This skill ensures AWS CLI commands always use the correct profile:

- **Profile Enforcement**: Always passes `--profile` explicitly to avoid using the wrong account
- **Environment Awareness**: Distinguishes between production and staging/non-prod profiles
- **Configurable**: Update the profile map once with your AWS profile names

# 📖 How to Use Skills

## Setup for Claude Code

1. Copy the desired `SKILL.md` file from the `/skills` directory into your project at `.claude/skills/<skill-name>/SKILL.md`
2. If the skill contains `<placeholder>` values, replace them with your actual configuration
3. Start Claude Code and invoke the skill by name in your conversation

## Setup for GitHub Copilot (VS Code)

1. Copy the desired `SKILL.md` file from the `/skills` directory into your project at `.github/skills/<skill-name>/SKILL.md`
2. If the skill contains `<placeholder>` values, replace them with your actual configuration
3. Reference the skill name in your GitHub Copilot Chat conversation

## Tips for Best Results

- Review each skill's **Configuration** section and update any placeholder values before use
- Be specific about your requirements when the skill asks for clarification
- Ensure you have the necessary CLI tools installed and authenticated (each skill lists prerequisites)

**Note: For more details, refer to the chosen skill's `SKILL.md` file.**

# 🚀 Available Chat Modes

| Title | Description | Category |
| ----- | ----------- | -------- |
| [Code Reviewer](chat-modes/code-reviewer.chatmode.md) | Analyzes code changes between two branches to perform comprehensive code reviews and suggest improvements for security, performance, and code quality | Development Tools |
| [Code Commit Assistant](chat-modes/code-commit-assistant.chatmode.md) | Analyzes code changes, prepares conventional commit messages, and commits to a new branch with proper Git workflow automation | Development Tools |
| [Dockerfile Developer](chat-modes/dockerfile-developer.chatmode.md) | Develops optimized, secure, and best-practice Dockerfiles based on user requirements and application context | Infrastructure & DevOps |
| [Terraform Helm Release Upgrade Analyser](chat-modes/terraform-helm-release-upgrade-analyser.chatmode.md) | Creates a detailed upgrade plan for a Helm release created via Terraform by analysing the configuration differences between the current and desired Helm chart versions and any breaking changes | Infrastructure & DevOps |
| [Conversation to Chat Mode](chat-modes/conversation-to-chat-mode.chatmode.md) | Creates a custom chat mode file based on a conversational interface where users describe their specific task requirements and guidelines | Development Tools |

## Code Reviewer

This chat mode helps you perform comprehensive code reviews by analyzing changes between Git branches and providing detailed feedback by:

- **Branch Comparison**: Analyzes code differences between source and target branches using Git diff
- **Security Analysis**: Checks for secrets exposure, SQL injection vulnerabilities, and security best practices
- **Performance Review**: Identifies inefficient algorithms, memory leaks, and unnecessary computations
- **Code Quality**: Looks for code smells, duplicate code, complex functions, and formatting issues
- **Best Practices**: Verifies proper error handling, logging, and documentation standards
- **Targeted Suggestions**: Provides specific code snippets with improvements, file paths, and line numbers

### Demo

![Demo](demos/code-reviewer.chatmode.gif)

## Code Commit Assistant

This chat mode helps you automate your Git workflow by analyzing code changes and creating proper commits by:

- **Automatic Code Analysis**: Analyzes all uncommitted changes to understand the scope and impact of modifications
- **Conventional Commits**: Automatically determines the appropriate conventional commit type (feat, fix, docs, etc.) based on code changes
- **Branch Management**: Creates new branches for your changes following Git best practices
- **Quality Checks**: Optionally runs pre-commit hooks and Terraform formatting before committing
- **Automated Workflow**: Handles the complete Git workflow from branch creation to pushing changes

### Demo

![Demo](demos/code-commit-assistant.chatmode.gif)

## Dockerfile Developer

This chat mode helps you create optimized, secure, and production-ready Dockerfiles by:

- **Context Analysis**: Analyzes your project files to understand dependencies and build requirements
- **Best Practices**: Follows Docker best practices including proper layer ordering, caching optimization, and clean syntax
- **Size Optimization**: Creates minimal container images through multi-stage builds and efficient package management
- **Security Focus**: Implements security best practices including non-root users and vulnerability scanning
- **Technology Detection**: Automatically detects your application stack from project files (package.json, requirements.txt, etc.)

### Demo

![Demo](demos/dockerfile-developer.chatmode.gif)

## Terraform Helm Release Upgrade Analyser

This chat mode helps you safely upgrade Helm releases managed by Terraform by:

- **Automatic Detection**: Scans your Terraform code to identify Helm release resources
- **Version Comparison**: Compares template files and default values between chart versions
- **Breaking Change Analysis**: Identifies potential breaking changes and compatibility issues
- **Detailed Planning**: Creates comprehensive upgrade plans with step-by-step instructions

### Demo

![Demo](demos/terraform-helm-release-upgrade-analyser.chatmode.gif)

## Conversation to Chat Mode

This meta chat mode helps you create new custom chat modes through a guided conversation by:

- **Interactive Creation**: Walks you through a conversational interface to define your chat mode requirements
- **Template Generation**: Automatically generates properly formatted `.chatmode.md` files based on your inputs
- **Guided Questions**: Asks targeted questions about your specific task and required guidelines
- **Best Practices**: Ensures your custom chat mode follows established conventions and formatting

### Demo

![Demo](demos/conversation-to-chat-mode.chatmode.gif)

# 📖 How to Use Chat Modes

To use any of these custom chat modes effectively, follow these general steps:

## Prerequisites

- Ensure you have the necessary files, projects, or context ready for the specific chat mode you want to use
- Understand the specific requirements for your chosen chat mode by checking the chat mode file.

## Setup

1. **Install the Chat Mode**: Copy the desired `.chatmode.md` file from the `/chat-modes` directory to your workspace's `.github/chatmodes` directory
2. **Restart VS Code**: Restart VS Code to load the new chat mode
3. **Start a New Chat Session**: Open the GitHub Copilot Chat panel in VS Code
4. **Select the Chat Mode**: Click the dropdown menu at the bottom of the chat panel and select your custom chat mode
5. **Choose the Model**: Select the model to be used. For example, `Claude Sonnet 4` (recommended for best performance)
6. **Add Context**: Choose the appropriate directory or files as context for your specific task

## Prompt Example

### Simple Start Command

For most chat modes, you can simply use:

```
Start.
```

This will prompt the chat mode to guide you through any necessary questions or steps.

## Tips for Best Results

- Always select the appropriate directory or files as context for your task
- Use `Claude Sonnet 4` model for optimal performance
- Be specific about your requirements when the chat mode asks for clarification
- Ensure you have the necessary permissions and prerequisites before starting

**Note: For more details, refer to the chosen custom chat mode's `.chatmode.md` file.**

# 🛡️ Claude Code Guardrails

The `.claude/` directory contains security guardrails for [Claude Code](https://claude.ai/code) that enforce safe, read-only behaviour by default and prevent destructive or irreversible actions.

| File | Description |
| ---- | ----------- |
| [.claude/settings.json](.claude/settings.json) | Hardcoded `deny` and `allow` lists for Bash, Read, and Edit tools, plus pre/post tool-use hooks |
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
- **System-level danger**: `sudo`, `chmod 777`, `chown root`, `rm -rf` on system paths, `mkfs`, `dd`, pipe-to-shell (`curl | bash`, `wget | sh`)
- **Credential file reads**: `~/.aws/**`, `~/.kube/**`, `~/.ssh/**`, `.env`, `**/secrets/**`, `**/*credentials*`

## What is always allowed

The `allow` list pre-approves safe, read-only operations without requiring user confirmation:

- **Git read operations**: `log`, `status`, `diff`, `branch`, `show`, `rev-parse`, `remote -v`
- **AWS read-only**: `describe*`, `list*`, `get*`, `s3 ls`, `sts get-caller-identity`
- **Kubernetes read-only**: `kubectl get`, `kubectl describe`, `kubectl logs`
- **Terraform safe operations**: `plan`, `validate`, `fmt`
- **Test runners**: npm, pnpm, go, mvn, gradle, pytest
- **File operations**: `Read(./**)`, `Edit(./**)` within the project directory

## Hooks

Two hooks run automatically on every tool use:

- **`PreToolUse` → `validate-bash-command.sh`**: Validates every Bash command before execution
- **`PostToolUse` → `audit-log.sh`**: Logs every tool use for auditability

## Setup for Claude Code

1. Copy the `.claude/` directory to the root of your project
2. Ensure the hook scripts exist at `.claude/hooks/validate-bash-command.sh` and `.claude/hooks/audit-log.sh` and are executable
3. Adjust the `deny`/`allow` lists in `settings.json` to match your project's needs

# 🤝 Contributing

Contributions are welcome! If you have a skill or chat mode you'd like to share:

1. Fork this repository
2. Add your new skill under `skills/<skill-name>/SKILL.md` or your chat mode under `chat-modes/`
3. Update this README.md to include your contribution in the relevant table
4. Submit a pull request

## Skill Guidelines

- Use descriptive names and clear descriptions
- Include a **Configuration** section for any values the user must customize
- Never hardcode company-specific URLs, credentials, project keys, or team names — use `<placeholder>` values
- Add a **Rules** section covering safety constraints, error handling, and edge cases
- Keep the skill focused — one skill, one workflow
- Follow the established file naming convention: `<skill-name>/SKILL.md`

## Chat Mode Guidelines

- Use descriptive names and clear descriptions
- Include comprehensive documentation and usage examples
- Test your chat mode thoroughly before submitting
- Follow the established file naming convention: `your-mode-name.chatmode.md`

# 📝 License

This project is licensed under the Apache License - see the [LICENSE](LICENSE) file for details.

---

###### 😊 Any contributions, improvements and suggestions will be highly appreciated.

###### 🌟 Star this repository to know about awesome new skills and chat modes for your AI agent of choice.
