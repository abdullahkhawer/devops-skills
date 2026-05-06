---
name: maintenance-create-ticket-on-jira
description: Create or update a Jira ticket for a workload maintenance upgrade, based on maintenance upgrade check files.
---

# Create Maintenance Upgrade Ticket on Jira Skill

Run this skill when asked to create or update a Jira maintenance upgrade ticket based on maintenance upgrade check file(s) under `maintenance-checks` directory at the root of the working directory.

This skill reads maintenance upgrade check files, extracts the details related to Jira ticket title and description, then creates or updates the corresponding issue in the configured Jira project.

## Configuration

Update the values below with your Jira project details before using this skill:

| Setting | Value |
|---|---|
| Atlassian Site | `<your-site>.atlassian.net` |
| Jira Project Key | `<YOUR_PROJECT_KEY>` (e.g. `OPS`, `INFRA`) |

---

## Step 1 — Locate maintenance upgrade check file(s)

Find all maintenance upgrade check files in the `./maintenance-checks/` directory:

```bash
ls ./maintenance-checks/maintenance-check-*.md
```

If the user specified a particular file, use that. If multiple files are found and none was specified, list them and ask the user which one(s) to process. Process one file at a time — after completing all steps for a file, return to this step and process the next selected file.

---

## Step 2 — Parse the maintenance upgrade file

Read the maintenance upgrade check file and extract:

- **Maintenance upgrade required** — from the `## Decision` → `**Maintenance upgrade required:**` field
- **Ticket title** — from the `## Jira Ticket` → `**Title:**` field
- **Parent** — from the `## Jira Ticket` → `**Parent:**` field
- **Label** — from the `## Jira Ticket` → `**Label:**` field
- **Story Points** — from the `## Jira Ticket` → `**Story Points:**` field
- **Refined** — from the `## Jira Ticket` → `**Refined:**` field
- **Ticket description** — from the `## Jira Ticket` → `**Description:**` block (everything after `**Description:**` to the end of the file)

If any extracted field or block is missing or blank, report: `"Cannot proceed: '<field-name>' is missing in <filename>"` and stop.

If `Maintenance upgrade required: No`, inform the user that no maintenance upgrade ticket is needed for this file and stop.

---

## Step 3 — Build the JSON payload(s)

Jira requires **Atlassian Document Format (ADF)** for rich text, and custom fields must be set at creation time via `additionalAttributes` (the `edit` command does not support custom fields).

First, ensure the `./tmp/` directory exists:

```bash
mkdir -p ./tmp
```

Parse the markdown description and produce an ADF document following these rules:

- Each blank-line-separated paragraph → `{"type":"paragraph","content":[...]}`
- `**bold text**` → `{"type":"text","text":"bold text","marks":[{"type":"strong"}]}`
- `` `code` `` → `{"type":"text","text":"code","marks":[{"type":"code"}]}`
- `**N. Heading text**` when the entire paragraph consists solely of this pattern (i.e. `**<digit(s)>. <text>**` with nothing else on the line) → `{"type":"heading","attrs":{"level":3},"content":[{"type":"text","text":"N. Heading text"}]}` — this takes priority over the bold text rule; do NOT apply it when the bold span appears alongside other inline content
- `## Heading text` → `{"type":"heading","attrs":{"level":2},"content":[{"type":"text","text":"Heading text"}]}`
- `### Heading text` → `{"type":"heading","attrs":{"level":3},"content":[{"type":"text","text":"Heading text"}]}`
- Bullet list items (`- item`) → `{"type":"bulletList","content":[{"type":"listItem","content":[{"type":"paragraph","content":[...]}]},...]}` — group consecutive items into one `bulletList` node; indented sub-items (`  - subitem`, two-space indent) → nest a second `bulletList` inside the parent `listItem`'s content array
- Markdown tables → `{"type":"table","attrs":{"isNumberColumnEnabled":false,"layout":"default"},"content":[<rows>]}` where each row is `{"type":"tableRow","content":[<cells>]}` and each cell is either `{"type":"tableHeader","content":[...]}` (header row) or `{"type":"tableCell","content":[...]}` (data rows); each cell wraps its text in a paragraph node
- Plain URLs (starting with `http`) inside text → `{"type":"text","text":"<url>","marks":[{"type":"link","attrs":{"href":"<url>"}}]}`
- Markdown links (`[label](url)`) → `{"type":"text","text":"label","marks":[{"type":"link","attrs":{"href":"url"}}]}`
- Inline `- label: url` reference lines → render as a paragraph with the label as plain text followed by the URL as a link mark
- Numbered list items (`1. item`, `2. item`) → `{"type":"orderedList","content":[{"type":"listItem","content":[{"type":"paragraph","content":[...]}]},...]}` — group consecutive items into one `orderedList` node
- Fenced code blocks (` ``` ... ``` `) → `{"type":"codeBlock","attrs":{"language":""},"content":[{"type":"text","text":"<code content>"}]}` — use the language hint if specified (e.g. ` ```bash `), otherwise leave `language` as an empty string
- Horizontal rules (`---` on its own line) → `{"type":"rule"}`

Always write the ADF description object alone to `./tmp/jira-description.json` (used by the update path in Step 6):

```json
{
  "version": 1,
  "type": "doc",
  "content": [ ... ]
}
```

For the **create** path, also write the complete payload to `./tmp/jira-create.json`:

```json
{
  "projectKey": "<YOUR_PROJECT_KEY>",
  "type": "Story",
  "summary": "<ticket-title>",
  "parentIssueId": "<parent>",
  "labels": ["<label>"],
  "description": {
    "version": 1,
    "type": "doc",
    "content": [ ... ]
  },
  "additionalAttributes": {
    "customfield_10121": <story-points>,
    "customfield_10732": {"value": "Yes"}
  }
}
```

Only include `customfield_10121` if Story Points is present. Only include `customfield_10732` if Refined is `Yes`.  
Omit `additionalAttributes` entirely if neither field is applicable.

After writing the files, validate each one is well-formed JSON using `jq`:

```bash
jq empty ./tmp/jira-description.json && echo "jira-description.json is valid"
jq empty ./tmp/jira-create.json && echo "jira-create.json is valid"
```

If either command exits with an error, fix the malformed JSON and re-validate before proceeding to Step 4. Do not continue until both files pass validation.

---

## Step 4 — Check for an existing ticket

Search the configured project for an open ticket whose summary matches the extracted title:

```bash
acli jira workitem search \
  --jql "project = <YOUR_PROJECT_KEY> AND summary ~ \"<ticket-title>\" AND statusCategory != Done" \
  --json \
  --limit 100
```

- If **one match** is found: proceed to Step 6 (update).
- If **multiple matches** are found: list them and ask the user which issue to update, then proceed to Step 6.
- If **no match** is found: proceed to Step 5 (create) and then proceed to Step 7.

---

## Step 5 — Create a new Jira ticket

Create a new issue from the payload file:

```bash
acli jira workitem create \
  --from-json ./tmp/jira-create.json \
  --json
```

---

## Step 6 — Update an existing Jira ticket

Update the summary, description, and label of the existing issue using the ADF description file:

```bash
acli jira workitem edit \
  --key <issue-key> \
  --summary "<ticket-title>" \
  --description-file ./tmp/jira-description.json \
  --labels "<label>" \
  --json \
  --yes
```

Note: Do not attempt to set Story Points or Refined when updating — the `edit` command does not support custom fields. These fields must be set manually on the ticket if needed.

---

## Step 7 — Report outcome

After creating or updating the ticket, output a short summary:

```
Action        : Created / Updated
Issue         : <YOUR_PROJECT_KEY>-<number>
Title         : <ticket-title>
Parent        : <parent>
Label         : <label>
Story Points  : <story-points or N/A>
Refined       : <Yes / No / N/A>
URL           : https://<your-site>.atlassian.net/browse/<YOUR_PROJECT_KEY>-<number>
```

If the action was **Updated**, append a note:

```
Note: Story Points and Refined cannot be updated automatically. Please set them manually on the ticket if needed.
```

Then clean up the temporary files that are no longer needed:

```bash
rm -f ./tmp/jira-create.json ./tmp/jira-description.json
```

---

## Rules

- Use `acli jira workitem` subcommands — not `acli jira issue` or `acli jira --site`.
- Only target the configured project key. If the user asks for a different project, confirm before proceeding.
- Only create or update tickets when `Maintenance upgrade required: Yes` in the maintenance upgrade file.
- NEVER delete or transition (close/resolve) issues — only create and update.
- NEVER read, display, or store the Atlassian API token.
- If `acli` is not installed or not authenticated, follow the setup instructions in the `jira-and-confluence` skill and ask the user to complete them before proceeding.
- If any `acli` command fails, report the full error to the user and stop — do not retry silently.
