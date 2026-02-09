---
name: generate-codex-coding-partner
description: Generate a Codex CLI coding partner SKILL.md file.
---

# Coding Partner SKILL.md Generator (Codex CLI)

Generate a project-specific Codex CLI coding partner skill file by analyzing the current project and filling in the template.

## Steps

### 1. Read the template

Read [template.md](template.md) to understand the structure and placeholders.

### 2. Analyze the project

Investigate the project to gather context for filling in the template:

- **Tech stack**: Languages, frameworks, libraries (check package.json, requirements.txt, go.mod, Cargo.toml, etc.)
- **Architecture**: Project structure, key directories, patterns used
- **Tools & config**: Build tools, linters, test frameworks, CI/CD
- **MCP servers**: Check if the project has any MCP server configurations that could add project-specific tools
- **Existing skills**: Check `.claude/skills/` for other skills that might inform the coding partner
- **CLAUDE.md**: Read any existing CLAUDE.md files for project conventions and constraints
- **Key documents**: Look for important project documents (README, CONTRIBUTING, ADRs, design docs, API specs) that provide domain context and decisions Codex should be aware of when reviewing code

### 3. Fill in the template

Replace the placeholders with project-specific content:

- `[FILL IN PROJECT SPECIFIC WHEN TO USE HERE]` — Add rows for situations specific to this project's domain and tech stack (e.g., database migrations, API design, framework-specific patterns)
- `[FILL IN PROJECT SPECIFIC TOOLS HERE]` — Add any project-specific MCP tools or custom tool configurations that Codex should be aware of
- `[FILL IN PROJECT SPECIFIC WORKFLOWS HERE]` — Add project-specific workflows if applicable (e.g., "before running migrations, review the schema change with Codex", "review API contract changes before updating clients"). Remove the placeholder if none apply.
- `[FILL IN MORE EXAMPLES HERE]` — Add 2-3 examples using real patterns from the project, referencing realistic file paths (e.g., reviewing a component in the project's framework, debugging with project-specific tooling)

Remove any placeholder lines that don't apply. Don't leave `[FILL IN ...]` markers in the output.

### 4. Write the file

Save the generated file to `.claude/skills/codex-coding-partner/SKILL.md` in the project root.

If `.claude/skills/codex-coding-partner/` doesn't exist, create it.

## Guidelines

- Keep examples grounded in the actual project — reference real file patterns, frameworks, and conventions found during analysis
- Don't add generic filler rows; only include entries that are genuinely useful for the specific project
- If no project-specific tools are found, remove the placeholder row entirely rather than leaving the section sparse
- The generated file should be ready to use immediately with no manual editing needed
