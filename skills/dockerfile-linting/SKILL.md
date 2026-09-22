---
name: dockerfile-linting
description: Lint Dockerfiles using hadolint according to standard best practices. Use when asked to lint or check style/security issues in Dockerfiles.
---

# Dockerfile Linting Skill

Run this skill when asked to lint or check a Dockerfile for issues.

## Step 1 — Detect available tooling

```bash
which hadolint 2>/dev/null
ls .hadolint.yaml .hadolint.yml 2>/dev/null
```

If `hadolint` is not installed, inform the user and ask before installing:

```bash
brew install hadolint
```

---

## Step 2 — Find Dockerfiles

```bash
find . -type f \( -iname "Dockerfile" -o -iname "Dockerfile.*" -o -iname "*.dockerfile" \) -not -path "*/node_modules/*" -not -path "*/.git/*"
```

---

## Step 3 — Run linting (read-only check)

```bash
hadolint <Dockerfile>
```

Respect the project's `.hadolint.yaml` if present (e.g. ignored rules) — do not override it.

Report all findings grouped by file, with line numbers, rule codes (e.g. `DL3008`), and severity (error/warning/info/style).

---

## Step 4 — Explain and suggest fixes

`hadolint` does not auto-fix — for each finding, show the offending line and a suggested replacement. Common fixes:

- `DL3008`/`DL3018`/`DL3028`: pin package versions (e.g. `apt-get install -y package=1.2.3`).
- `DL3009`: clean up package manager caches in the same `RUN` layer.
- `DL3025`: use JSON/exec form for `CMD`/`ENTRYPOINT`.
- `DL3002`: avoid running as root — add a `USER` instruction.
- `DL3006`: pin the base image tag (avoid `latest`).
- `DL4006`: set `SHELL ["/bin/bash", "-o", "pipefail", "-c"]` before any piped `RUN` command.

---

## Rules

- NEVER install `hadolint` without asking the user first.
- Respect the project's existing `.hadolint.yaml` ignore list — do not re-flag rules the project has intentionally silenced, but you may mention them if they look unsafe (e.g. ignoring `DL3002`, running as root).
- Report the exact command run and its full output — do not summarize away findings.
- Always flag missing non-root `USER`, unpinned base image tags, and unpinned package versions even if `hadolint`'s default severity for them is only "info" or "style" — these are security-relevant in production images.
- Do not suggest disabling a rule via inline `# hadolint ignore=DLxxxx` comments unless the user explicitly asks for an exception and confirms the reason.
