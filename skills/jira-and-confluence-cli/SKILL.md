---
name: jira-and-confluence-cli
description: Perform Jira and Confluence operations using the Atlassian CLI (acli). Use for issues, sprints, boards, pages, spaces, and project management.
---

# Jira and Confluence CLI Skill

Use the instance and rules below when constructing all `acli` commands.

## Configuration

Set your Atlassian site URL before using this skill:

| Setting | Value |
|---|---|
| Atlassian Site | `<your-site>.atlassian.net` |

Replace `<your-site>.atlassian.net` with your actual Atlassian site domain throughout this skill.

### Prerequisites (one-time setup by user)

If not already set up, inform the user that setup is required and ask them to complete the following steps manually. Do not perform these steps yourself.

1. Install the Atlassian CLI if not already installed:

```bash
brew tap atlassian/homebrew-acli
brew install acli
```

2. Authenticate with your Atlassian instance:

```bash
acli auth login
```

This opens a browser window at `https://api.atlassian.com/oauth2/authorize/...`. Click **Accept** to grant the Atlassian CLI the required OAuth scopes. Once authorized, return to the terminal and select your site from the list when prompted.

## Command Structure

```
acli jira workitem   # create, get, list, update, transition
acli jira board      # list, get
acli jira sprint     # list, get
acli confluence page # create, get, list, update
```

## Common Commands

### Create a work item
```bash
acli jira workitem create \
  --project "PROJECT_KEY" \
  --type "Story" \
  --summary "Summary here" \
  --description-file /path/to/description.txt
```

For multi-line descriptions always use `--description-file` pointing to a temp file — inline `--description` with newlines is error-prone.

### List work items (JQL)
```bash
acli jira workitem list --jql "project = ABC AND status != Done" --limit 100
```

### Get a work item
```bash
acli jira workitem get --id ABC-1234
```

### Transition a work item
```bash
acli jira workitem transition --id ABC-1234 --status "In Progress"
```

### Create a Confluence page
```bash
acli confluence page create \
  --space "SPACE_KEY" \
  --title "Page Title" \
  --content-file /path/to/content.txt
```

## Rules

- For list commands, always pass `--limit 100` to ensure complete results are returned (`--paginate` is not supported by `acli`).
- NEVER run delete commands without explicit user instruction — deletions in Jira and Confluence are irreversible.
- When constructing JQL or CQL queries, prefer explicit field filters over free-text search to avoid unintended broad matches.
- If a project key, space key, board ID, or issue key looks unfamiliar, stop and ask the user to confirm before proceeding.
- NEVER read, display, or store the Atlassian API token — it is a credential and must remain secret.
