---
name: aws-cloudwatch-cli
description: Perform AWS CloudWatch operations using the AWS CLI. Use for querying metrics, logs, alarms, and dashboards. Requires the aws-cli skill's profile/region configuration.
---

# AWS CloudWatch CLI Skill

This skill builds on the `aws-cli` skill's profile and region configuration — always pass `--profile` and `--region` explicitly on every command, using the same environment table.

## Command Format

```bash
aws cloudwatch <command> --profile <profile> --region <region> [options]
aws logs <command> --profile <profile> --region <region> [options]
```

## Available Commands

### Read-only (safe, no confirmation needed)

| Command | Description |
|---|---|
| `aws cloudwatch list-metrics` | List available metrics |
| `aws cloudwatch get-metric-data` / `get-metric-statistics` | Query metric datapoints |
| `aws cloudwatch describe-alarms` | List alarms and their state |
| `aws cloudwatch describe-alarm-history` | Show alarm state change history |
| `aws cloudwatch list-dashboards` / `get-dashboard` | List or view dashboards |
| `aws logs describe-log-groups` | List log groups |
| `aws logs describe-log-streams` | List log streams in a group |
| `aws logs filter-log-events` / `get-log-events` | Query log events |
| `aws logs start-query` + `get-query-results` | Run and fetch a CloudWatch Logs Insights query |

### Example — Logs Insights query

```bash
QUERY_ID=$(aws logs start-query \
  --profile <profile> --region <region> \
  --log-group-name <log-group> \
  --start-time $(( $(date -u +%s) - 3600 )) \
  --end-time $(date -u +%s) \
  --query-string 'fields @timestamp, @message | filter @message like /ERROR/ | limit 50' \
  --query "queryId" --output text)

aws logs get-query-results --profile <profile> --region <region> --query-id "$QUERY_ID"
```

### Mutating operations (confirm with user before running)

| Command | Description |
|---|---|
| `aws cloudwatch put-metric-alarm` | Create or update an alarm |
| `aws cloudwatch put-dashboard` | Create or update a dashboard |
| `aws cloudwatch put-metric-data` | Publish a custom metric datapoint |
| `aws logs create-log-group` / `create-log-stream` | Create a new log group/stream |
| `aws logs put-retention-policy` | Change a log group's retention period |

### Never run — requires explicit human approval

| Command | Reason |
|---|---|
| `aws cloudwatch delete-alarms` | Irreversible alarm deletion |
| `aws cloudwatch delete-dashboards` | Irreversible dashboard deletion |
| `aws logs delete-log-group` | Irreversibly deletes a log group and all its data |
| `aws logs delete-log-stream` | Irreversibly deletes log stream data |

## Rules

- Always specify `--profile` and `--region` explicitly, per the `aws-cli` skill's environment table — never rely on defaults.
- If the target environment is unclear, ask the user before running the command.
- NEVER run `delete-*` commands without explicit user instruction — these are irreversible.
- For `filter-log-events`/`start-query`, always bound the time range (`--start-time`/`--end-time`) — an unbounded query on a busy log group can be slow and return excessive data.
- If unsure which log group or alarm name is correct, list them first with a read-only call before querying or mutating.
- NEVER read or output any log content that looks like it contains secrets, tokens, or credentials — flag it to the user instead of displaying it verbatim.