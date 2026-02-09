# Generate Coding Partner

A Claude Code skill that generates a project-specific **Codex CLI coding partner** skill file.

When invoked, it analyzes your project — tech stack, architecture, conventions, and tooling — then produces a tailored `SKILL.md` that teaches Claude Code to use Codex CLI as a second opinion and review partner throughout your workflow.

## What It Does

1. **Reads** the included [template](template.md) — a structured skill definition for using Codex CLI as a coding partner
2. **Analyzes** your project (dependencies, structure, CLAUDE.md, existing skills, MCP servers, docs)
3. **Fills in** project-specific placeholders with real examples, tools, and workflows
4. **Writes** the result to `.claude/skills/codex-coding-partner/SKILL.md` in your project

The generated skill tells Claude Code *when* and *how* to consult Codex CLI — plan reviews, architectural decisions, code reviews, debugging, and more — grounded in your actual codebase.

## Installation

Clone this repo into your `~/.claude/skills/` directory:

```bash
git clone git@github.com:IngvarKofoed/generate-codeing-partner.git \
  ~/.claude/skills/generate-codex-coding-partner
```

That's it. The skill is now available in Claude Code.

## Usage

From Claude Code, run:

```
/generate-codex-coding-partner
```

Or just ask naturally:

```
Generate a Codex coding partner for this project
```

Claude Code will analyze your project and write the result to `.claude/skills/codex-coding-partner/SKILL.md` in your current project.

## Examples

### React + TypeScript project

The generator might produce entries like:

```markdown
| **Reviewing component patterns** | Ask Codex to review component composition and hook usage |
| **API contract changes** | Validate API types and client/server alignment with Codex |
```

```
# Review a component for React best practices
codex prompt: "Look at src/components/Dashboard.tsx. Review this component for performance issues, unnecessary re-renders, and hook usage." workingDirectory: "/path/to/project"

# Validate API types
codex prompt: "Look at src/types/api.ts and src/api/client.ts. Check that the client types match the API contract." workingDirectory: "/path/to/project"
```

### Python + FastAPI project

```markdown
| **Database migrations** | Review schema changes with Codex before running migrations |
| **Endpoint design** | Get feedback on REST API design and Pydantic models |
```

```
# Review a migration
codex prompt: "Look at alembic/versions/003_add_users.py. Review this migration for safety. Can it be rolled back?" workingDirectory: "/path/to/project"

# Review endpoint design
codex prompt: "Look at app/routers/users.py. Review the endpoint design, error handling, and response models." workingDirectory: "/path/to/project"
```

### Go microservice

```markdown
| **Concurrency patterns** | Validate goroutine and channel usage with Codex |
| **Interface design** | Get feedback on interface boundaries between packages |
```

```
# Review concurrency code
codex prompt: "Look at internal/worker/pool.go. Review this worker pool for race conditions and proper shutdown handling." workingDirectory: "/path/to/project"

# Validate interface design
codex prompt: "Look at internal/storage/store.go. Is this interface well-scoped or should it be split?" workingDirectory: "/path/to/project"
```

## Files

| File | Purpose |
|------|---------|
| `SKILL.md` | Skill definition — steps and guidelines for the generator |
| `template.md` | Template for the generated coding partner skill |

## Why?

Getting a second opinion on plans, architecture, and code catches mistakes early. This skill automates the setup so every project gets a coding partner tuned to its specific stack and conventions — no manual configuration needed.
