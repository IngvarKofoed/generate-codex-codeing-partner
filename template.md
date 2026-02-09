# Codex CLI - Coding Partner

**Server**: `codex-cli`

Codex CLI is your **coding partner** and **second opinion**. Invoke it naturally and frequently.

## How Codex Reads Files

Codex does not use `@file` syntax. Instead, it reads files autonomously within its working directory. Use the `workingDirectory` parameter to point Codex at the right project root, and describe the files you want it to look at in your prompt. Codex will find and read them itself.

## When to Use Codex CLI

| Situation | Action |
|-----------|--------|
| **Creating implementation plans** | Review with Codex before presenting to user |
| **Making architectural decisions** | Ask Codex for review before committing |
| **Implementing complex logic** | Get a second opinion on the approach |
| **Debugging difficult issues** | Brainstorm solutions together |
| **Writing critical code paths** | Request implementation review |
| **Reviewing your own changes** | Use the `review` tool for structured feedback |
| **Reviewing changes before PR** | Use `review` with `base` or `uncommitted` |
| **Uncertain about best practices** | Get expert guidance |
| **User instructions seem unclear** | Validate understanding and approach |
| **Considering pushing back on user** | Validate your concerns before raising them |
| [FILL IN PROJECT SPECIFIC WHEN TO USE HERE] | |

## When NOT to Use Codex CLI

Skip Codex for:
- **Trivial changes** — typos, rename a variable, add an import
- **Tasks you're fully confident about** — no ambiguity, no risk
- **Simple file reads or searches** — use your own tools directly
- **Repeating a question you already asked** — reuse the previous answer

The round-trip has latency. Only invoke it when the second opinion adds value.

## Tools

| Tool | Purpose | Use When |
|------|---------|----------|
| `codex` | General coding questions, reviews, feedback | Default choice for coding assistance |
| `review` | Dedicated code review against branches/commits | Reviewing changes before PR or merge |
| `codex` with `fullAuto: true` | Autonomous task execution in sandbox | Need Codex to independently complete a task |
| `codex` with `sandbox: "read-only"` | Safe analysis without writes | Investigating code without risk of changes |
| `listSessions` | Check active conversation sessions | Managing multi-step workflows |
| [FILL IN PROJECT SPECIFIC TOOLS HERE] | | |

### Sandbox Modes

The `sandbox` parameter controls what Codex can do on disk:

| Mode | Behavior | Use When |
|------|----------|----------|
| `read-only` | No file writes allowed | Safe analysis, code review, investigation |
| `workspace-write` | Writes only within the working directory | Default for most tasks — safe and practical |
| `danger-full-access` | Full system access (dangerous) | Only when absolutely necessary, with user approval |

`fullAuto: true` is shorthand for `sandbox: "workspace-write"` with automatic execution (no approval prompts).

### Session Management

Codex supports multi-turn conversations via sessions:

- **`sessionId`**: Pass a session ID to continue a previous conversation. Codex retains full context from earlier turns.
- **`resetSession`**: Set to `true` to clear session history before processing the request.
- **`listSessions`**: Use the `listSessions` tool to see all active sessions with metadata.

Use sessions when a task spans multiple steps — e.g., first ask Codex to analyze a module, then in a follow-up ask it to suggest refactors based on its analysis.

**Note**: When resuming a session, `sandbox`, `fullAuto`, and `workingDirectory` parameters are not reapplied (CLI limitation). Set these on the first call.

### Reasoning Effort

The `reasoningEffort` parameter controls how deeply Codex thinks:

| Level | Use When |
|-------|----------|
| `none` / `minimal` | Quick lookups, simple questions |
| `low` / `medium` | Standard coding tasks, reviews |
| `high` / `xhigh` | Complex architecture decisions, deep debugging |

Default is unset (model decides). Use higher effort for critical decisions and lower effort for routine checks.

### `review` Tool Parameters

The `review` tool provides structured code review:

| Parameter | Type | Purpose |
|-----------|------|---------|
| `base` | string | Review changes against a specific base branch (e.g., `"main"`) |
| `commit` | string | Review a specific commit by SHA |
| `uncommitted` | boolean | Review all working tree changes (staged, unstaged, untracked) |
| `prompt` | string | Custom review instructions or focus areas (cannot combine with `uncommitted: true`) |
| `title` | string | Optional title for the review summary |
| `workingDirectory` | string | Repository to review |

## Workflows

### Plan Review

Before presenting any plan to the user:
1. Draft the plan
2. Ask Codex to review it: `codex prompt: "Review this implementation plan for [feature]. Check for gaps, risks, and better approaches: [plan text]"`
3. Incorporate feedback
4. Present the refined plan to the user

### Code Change Review

After implementing changes, before presenting to the user:
1. Use the `review` tool for structured feedback: `review uncommitted: true title: "Pre-PR review"`
2. Or for changes against a branch: `review base: "main" title: "Feature branch review"`
3. Fix any issues found before presenting to the user

### Validating Before Pushing Back

When you think user instructions are unclear or misguided:
1. Ask Codex: `codex prompt: "The user wants X. I think Y would be better because Z. Am I right to push back?"`
2. If validated, raise your concern constructively with the user
3. Propose the alternative approach

### Multi-Step Analysis with Sessions

For complex investigations that need multiple rounds:
1. Start a session: `codex prompt: "Analyze the authentication module in this project" workingDirectory: "/path/to/project" sessionId: "auth-review"`
2. Follow up: `codex prompt: "Based on your analysis, what are the top 3 security concerns?" sessionId: "auth-review"`
3. Get recommendations: `codex prompt: "Propose fixes for the issues you identified" sessionId: "auth-review"`

[FILL IN PROJECT SPECIFIC WORKFLOWS HERE]

## Examples

```
# Review a plan before presenting
codex prompt: "Review this plan for implementing real-time notifications. Am I missing any edge cases or better approaches?"

# Code review of uncommitted changes
review uncommitted: true title: "Pre-commit review"

# Review changes against main branch
review base: "main" prompt: "Focus on API contract changes and backwards compatibility"

# Review a specific commit
review commit: "abc123" title: "Hotfix review"

# Safe read-only analysis
codex prompt: "Analyze the error handling patterns in this project and identify inconsistencies" workingDirectory: "/path/to/project" sandbox: "read-only"

# Autonomous task execution
codex prompt: "Add input validation to all API endpoint handlers" workingDirectory: "/path/to/project" fullAuto: true

# Multi-turn session
codex prompt: "Examine the database query patterns in src/db/" workingDirectory: "/path/to/project" sessionId: "db-review"
codex prompt: "Which queries are missing indexes?" sessionId: "db-review"

# Quick sanity check with low reasoning effort
codex prompt: "Is this regex safe against ReDoS attacks? /^([a-z]+)+$/" reasoningEffort: "low"

# Deep architectural review with high reasoning effort
codex prompt: "Review the overall service architecture and suggest improvements for scalability" workingDirectory: "/path/to/project" reasoningEffort: "xhigh"
```

[FILL IN MORE EXAMPLES HERE]
