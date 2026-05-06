---
name: create-skill
description: Create a new SKILL.md file based on a guided conversation about a task or workflow.
---

# Create Skill

Run this skill when asked to create a new skill from a conversation or description of a task.

## Step 1 — Gather requirements

Ask the user only these two questions:

```
1. What specific task or problem do you want this skill to solve?
2. What are the specific instructions and guidelines for this task?
```

Wait for the user's answers before proceeding.

---

## Step 2 — Determine skill name and location

Derive a short, lowercase, hyphenated skill name from the task description (e.g. `deploy-lambda`, `check-postgres-health`).

Ask the user which AI agent they are using, then use the corresponding path:

| Agent | Skill path |
|---|---|
| Claude Code | `.claude/skills/<skill-name>/SKILL.md` |
| GitHub Copilot | `.github/skills/<skill-name>/SKILL.md` |
| Other | `skills/<skill-name>/SKILL.md` |

Create the directory if it does not exist:

```bash
mkdir -p <path-to-skill-dir>
```

---

## Step 3 — Write the SKILL.md file

Use the template below, filling in all `<...>` placeholders based on the user's answers.

```markdown
---
name: <skill-name>
description: <One-liner description of what this skill does.>
---

# <Skill Title>

Run this skill when asked to <task statement>.

## Step 1 — <First step title>

<Instructions for the first step.>

## Step 2 — <Second step title>

<Instructions for the second step.>

...

## Rules

- <Rule 1>
- <Rule 2>
```

### Guidelines for writing the skill

- Each step should have a clear title and concrete instructions or commands.
- Include bash commands in fenced code blocks where relevant.
- Rules section must cover safety constraints, error handling, and edge cases.
- Keep the skill focused — one skill, one workflow.
- Do not include secrets, credentials, or hardcoded environment-specific values.

---

## Step 4 — Confirm with the user

After writing the file, show the user the generated SKILL.md content and the path where it was saved. Ask if any adjustments are needed.

---

## Rules

- Always place the skill in the correct path for the user's AI agent (see Step 2 table).
- The skill name must be lowercase and hyphenated.
- Never include credentials, API tokens, or secrets in the generated skill.
- If the user's description is too vague to produce actionable steps, ask clarifying questions before writing the file.
