---
name: ms-teams-cli
description: Perform Microsoft Teams operations using the Microsoft Graph API via curl. Use for posting channel messages, listing teams/channels, and looking up users.
---

# Microsoft Teams CLI Skill

Microsoft Teams has no official first-party CLI — use the Microsoft Graph API via `curl` for all operations.

## Configuration

Update the tenant details below before using this skill:

| Setting | Value |
|---|---|
| Azure AD Tenant ID | `<your-tenant-id>` |
| App Registration Client ID | `<your-client-id>` |
| Token File | `~/.msteams/token` |

### Prerequisites (one-time setup by user)

Register an Azure AD application with `ChannelMessage.Send`, `Team.ReadBasic.All`, and `Channel.ReadBasic.All` Graph API permissions (admin consent required for most Teams scopes), then obtain an OAuth token and save it to the token file. Never print or display the token value. Do not perform these steps yourself.

```bash
mkdir -p ~/.msteams && echo "<your-access-token>" > ~/.msteams/token && chmod 600 ~/.msteams/token
```

## Command Format

All API calls use `curl` with the token read from the token file:

```bash
curl -s -H "Authorization: Bearer $(cat ~/.msteams/token)" \
  "https://graph.microsoft.com/v1.0/<endpoint>" | jq .
```

## Available API Endpoints

### Read-only (safe, no confirmation needed)

| Endpoint | Method | Description |
|---|---|---|
| `/v1.0/me/joinedTeams` | GET | List teams the authenticated user has joined |
| `/v1.0/teams/<team-id>/channels` | GET | List channels in a team |
| `/v1.0/teams/<team-id>/channels/<channel-id>/messages` | GET | List messages in a channel |
| `/v1.0/teams/<team-id>/channels/<channel-id>/messages/<message-id>` | GET | Get a specific message |
| `/v1.0/users?$filter=<query>` | GET | Look up users |
| `/v1.0/users/<user-id>` | GET | Get a specific user |

### Mutating operations (confirm with user before running)

| Endpoint | Method | Description |
|---|---|---|
| `/v1.0/teams/<team-id>/channels/<channel-id>/messages` | POST | Post a message to a channel (confirm channel and text with user first) |
| `/v1.0/teams/<team-id>/channels` | POST | Create a new channel |
| `/v1.0/chats/<chat-id>/messages` | POST | Send a 1:1 or group chat message |

### Never run — requires explicit human approval

| Endpoint | Method | Reason |
|---|---|---|
| `/v1.0/teams/<team-id>/channels/<channel-id>` | DELETE | Irreversibly deletes a channel and its messages |
| `/v1.0/teams/<team-id>/channels/<channel-id>/messages/<message-id>` | DELETE | Irreversible message deletion |
| `/v1.0/groups/<team-id>` | DELETE | Irreversibly deletes the entire team |
| `/v1.0/teams/<team-id>/members/<membership-id>` | DELETE | Removes a member from the team |

### Example — post a message

```bash
curl -s -X POST -H "Authorization: Bearer $(cat ~/.msteams/token)" \
  -H "Content-Type: application/json" \
  --data '{"body": {"content": "<MESSAGE>"}}' \
  "https://graph.microsoft.com/v1.0/teams/<team-id>/channels/<channel-id>/messages" | jq .
```

## Rules

- NEVER target any Azure AD tenant other than `<your-tenant-id>` configured above.
- NEVER print, display, log, or store the contents of `~/.msteams/token` or any credential value.
- Always read the token inline with `$(cat ~/.msteams/token)` — never hardcode a token value in a command.
- NEVER post a channel or chat message without first confirming the exact team/channel and message text with the user.
- NEVER run DELETE operations without explicit user instruction — these are irreversible.
- If a team ID, channel ID, or user is unknown, always look it up first using the list/lookup endpoints above.
- Access tokens for Graph API typically expire in ~1 hour — if a call fails with `401 Unauthorized`, inform the user that the token likely needs refreshing rather than retrying repeatedly.
- If `~/.msteams/token` does not exist, inform the user and stop. Do not attempt to find or guess the token.