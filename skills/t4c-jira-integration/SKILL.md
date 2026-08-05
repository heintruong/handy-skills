---
name: t4c-jira-integration
description: "Jira workflow automation. Post concise, natural comments with Git traceability (PR/branch/commit). Use when closing issues, reporting progress, or updating Jira from code changes."
version: 1.0.0
---

# Jira Integration

Posts natural, business-language Jira comments that include Git context for traceability.

## How to use

| What | How |
|---|---|
| Done with task | Post completion comment with Git links |
| Progress update | Report what was done, what's next |
| Found blocker | Explain issue and link to PR/branch |
| Code review | Link PR and summarize findings |

## Comment Template

**Structure**: What was done (1 sentence) + any details + Git links (PR/branch/commit)

**Git line format**:
```
PR: <URL>
Branch: <branch-name>
Commit: <commit-hash>
```

**Bad**: Markdown-heavy, technical, matches PR descriptions
```
- Fixed SQL injection in auth.js
- Added email deduplication via Dictionary
- Consolidated 3 shared functions
```

**Good**: Natural, conversational, business-focused
```
Fixed the authentication bug that was causing session timeouts. Added email deduplication to prevent duplicate invite processing. Consolidated three shared functions to reduce code duplication.

PR: https://github.com/org/repo/pull/123
Branch: fix/auth-session
Commit: a1b2c3d
```

## Rules

- **One sentence summary** of what was done
- **No bullet points** — write like a memo, not a commit message
- **Always include Git links** for traceability
- **Short and direct** — 2-3 sentences max for completion comments
- **Natural language** — explain in business terms, not technical jargon
- **No dashes (—)** — use hyphens or rephrase
- **Link format**: Full GitHub URL for PRs, branch name, 7-char commit hash

## Common patterns

### Task complete
```
{What was done in one sentence}. {Any important details}

PR: {url}
Branch: {branch}
Commit: {hash}
```

### Progress update
```
{What was done}. {What's blocking or what's next}.

PR: {url} (draft) or Branch: {branch}
Commit: {hash}
```

### Blocker found
```
{What the issue is}. {Why it's a problem}. {What we need to unblock}.

Branch: {branch}
Related PR: {url}
```

### Code review findings
```
Reviewed the PR. {Summary of issues or "No issues found, ready to merge"}

PR: {url}
Findings: {1-2 key points}
```
