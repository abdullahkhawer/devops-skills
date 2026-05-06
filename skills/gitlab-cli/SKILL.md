---
name: gitlab-cli
description: Perform GitLab operations using the glab CLI. Use for issues, merge requests, pipelines, repositories, and project management. ALSO use this skill when asked to review a GitLab MR or PR — when a GitLab MR URL or MR ID is provided, use glab to fetch and review the diff instead of the code-review skill.
---

# GitLab CLI Skill

Use the instance and rules below when constructing all glab commands.

## Configuration

Set your GitLab hostname below before using this skill:

| Setting | Value |
|---|---|
| GitLab Hostname | `<YOUR_GITLAB_HOSTNAME>` (e.g. `gitlab.example.com`) |

Replace `<YOUR_GITLAB_HOSTNAME>` with your GitLab instance hostname throughout this skill.

### Prerequisites (one-time setup by user)

If not already authenticated, inform the user that authentication is required and ask them to complete the following steps manually. Do not perform these steps yourself.

1. Install the `glab` CLI if not already installed:

```bash
brew install glab
```

2. Run the following command to authenticate with your GitLab instance:

```bash
glab auth login --hostname <YOUR_GITLAB_HOSTNAME>
```

## MR Code Review

When asked to review a GitLab MR or PR (URL or MR ID provided), extract the `<group/repo>` and `<MR_ID>` from the URL (ask the user if the repo is missing), then use `glab api --hostname <YOUR_GITLAB_HOSTNAME>` to fetch the MR metadata (`projects/<URL_ENCODED_REPO>/merge_requests/<MR_ID>`) and its diff (`…/diffs --paginate`), where the repo path is URL-encoded with `/` → `%2F`. Report the MR title, author, and branches, then review all changed files for: secrets/injection/XSS (security), inefficient algorithms/memory leaks (performance), code smells/dead code (quality), missing error handling, excessive or sensitive logging, and missing/outdated documentation. For each finding, include the file path, line number(s), the problematic snippet, and a suggested fix. If nothing is wrong, summarize what was reviewed and state it looks good.

## Rules

- NEVER target any GitLab instance other than `<YOUR_GITLAB_HOSTNAME>`. If a command or URL references a different host, stop and ask the user to confirm.
- Always pass `--repo <YOUR_GITLAB_HOSTNAME>/<group/repo>` explicitly for any `glab` subcommand that accepts it (e.g., `mr list`, `issue list`, `pipeline list`) — without it, the command infers the repo from the current git directory, which may silently return wrong results.
- When using `glab api`, always pass `--hostname <YOUR_GITLAB_HOSTNAME>` explicitly — omitting it may default to `gitlab.com`.
- NEVER use `glab repo list --group` — it does not work reliably against self-hosted instances; use `glab api --hostname <YOUR_GITLAB_HOSTNAME> "groups/<GROUP>/projects"` instead. For nested subgroups, URL-encode the slashes: `"groups/parent%2Fchild/projects"`.
- All `glab api` calls that return lists are paginated (default 20/page). Always pass `--paginate` to fetch all pages: `glab api --hostname <YOUR_GITLAB_HOSTNAME> --paginate "<endpoint>"`.
- For `glab mr list`, prefer explicit state flags over `--state`: use `--closed` (`-c`) for closed, `--merged` (`-M`) for merged, or `--all` (`-A`) for all states. Omitting a state flag returns open MRs by default.
