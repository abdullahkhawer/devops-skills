---
name: slack-cli
description: Perform Slack operations using the Slack CLI for app management and the Slack Web API (curl) for messaging, channel, and user operations.
---

# Slack CLI Skill

This skill uses **two tools** depending on the operation:

| Use case | Tool |
|---|---|
| App management (install, deploy, auth) | `slack` CLI |
| Messaging, channels, users, search | Slack Web API via `curl` |

## Configuration

Update the Slack workspace name below before using this skill:

| Setting | Value |
|---|---|
| Slack Workspace | `<your-workspace>.slack.com` |
| Slack Team Name (for CLI) | `<your-team-name>` |

---

## Tool 1 — Slack CLI (app management only)

### Prerequisites (one-time setup by user)

If not already authenticated, ask the user to complete these steps manually. Do not perform these steps yourself.

```bash
brew install slack-cli                # install if needed
slack login                           # authenticate
slack auth list                       # confirm your workspace is listed
```

### Commands

Always pass `--team <your-team-name>` on every command.

| Command | Description |
|---|---|
| `slack auth list` | List authenticated workspaces |
| `slack auth whoami` | Show current authenticated user |
| `slack app list --team <your-team-name>` | List apps in the workspace |
| `slack app install --team <your-team-name>` | Install app to workspace |
| `slack app uninstall --team <your-team-name>` | Uninstall app (requires explicit user approval) |
| `slack deploy --team <your-team-name>` | Deploy app to Slack Platform |
| `slack trigger list --team <your-team-name>` | List triggers for an app |

---

## Tool 2 — Slack Web API via `curl` (messaging, channels, users)

### Prerequisites (one-time setup by user)

To get started, create a Slack app at [api.slack.com/apps](https://api.slack.com/apps). On the **OAuth & Permissions** page, add the required scopes, then install the app to your workspace. After installation, your user token (starting with `xoxp-`) will appear on the same page. Save it to `~/.slack/token`. Never print or display the token value. Do not perform these steps yourself.

Required token scopes: `channels:read`, `channels:history`, `groups:read`, `groups:history`, `users:read`

```bash
echo "xoxp-..." > ~/.slack/token && chmod 600 ~/.slack/token
```

### Base URL

```
https://slack.com/api/<method>
```

### Common API Methods

#### List channels
```bash
curl -s -H "Authorization: Bearer $(cat ~/.slack/token)" \
  "https://slack.com/api/conversations.list?limit=200&exclude_archived=true&types=public_channel,private_channel" \
  | jq '.channels[] | {id, name}'
```

#### Get channel ID by name (replace `general` with channel name)
```bash
curl -s -H "Authorization: Bearer $(cat ~/.slack/token)" \
  "https://slack.com/api/conversations.list?limit=200&exclude_archived=true&types=public_channel,private_channel" \
  | jq -r '.channels[] | select(.name == "general") | .id'
```

#### Read latest messages from a channel
```bash
curl -s -H "Authorization: Bearer $(cat ~/.slack/token)" \
  "https://slack.com/api/conversations.history?channel=<CHANNEL_ID>&limit=10" \
  | jq '.messages[] | {ts, user, text}'
```

#### Search messages (requires `search:read` scope)
```bash
curl -s -H "Authorization: Bearer $(cat ~/.slack/token)" \
  "https://slack.com/api/search.messages?query=<search+term>&count=10" \
  | jq '.messages.matches[] | {channel: .channel.name, user: .username, text, ts}'
```

#### Post a message (confirm channel and text with user before running)
```bash
curl -s -X POST -H "Authorization: Bearer $(cat ~/.slack/token)" \
  -H "Content-Type: application/json" \
  --data '{"channel": "<CHANNEL_ID>", "text": "<MESSAGE>"}' \
  "https://slack.com/api/chat.postMessage" \
  | jq '{ok, ts, error}'
```

#### List users
```bash
curl -s -H "Authorization: Bearer $(cat ~/.slack/token)" \
  "https://slack.com/api/users.list" \
  | jq '.members[] | select(.deleted == false) | {id, name, real_name: .profile.real_name}'
```

#### Look up user by name
```bash
curl -s -H "Authorization: Bearer $(cat ~/.slack/token)" \
  "https://slack.com/api/users.list" \
  | jq -r '.members[] | select(.name == "<username>") | .id'
```

### Checking API responses

Every Slack API response includes an `ok` field. Always check it:
```bash
| jq 'if .ok then . else error(.error) end'
```

---

## Rules

- NEVER target any Slack workspace other than the one configured above.
- NEVER print, display, log, or store the contents of `~/.slack/token` or any credential value.
- NEVER post a message without first confirming the exact channel and message text with the user.
- NEVER run destructive operations (archive, delete, deactivate) without explicit user instruction.
- If a channel or user ID is unknown, always look it up first using the list commands above.
- If `~/.slack/token` does not exist, inform the user and stop. Do not attempt to find or guess the token.
