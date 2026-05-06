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

## Rules

- NEVER target any Atlassian site other than the configured one. If a command or URL references a different site, stop and ask the user to confirm.
- Always pass `--site <your-site>.atlassian.net` explicitly on every `acli` command to avoid defaulting to a wrong or previously configured site.
- For list commands that may be paginated, always pass `--limit 100` (or use `--paginate` if supported) to ensure complete results are returned.
- NEVER run delete commands without explicit user instruction — deletions in Jira and Confluence are irreversible.
- When constructing JQL or CQL queries, prefer explicit field filters over free-text search to avoid unintended broad matches.
- If a project key, space key, board ID, or issue key looks unfamiliar, stop and ask the user to confirm before proceeding.
- NEVER read, display, or store the Atlassian API token — it is a credential and must remain secret.
