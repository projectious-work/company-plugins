# company-plugin — Development Context

This is the reusable virtual company plugin for projectious.work. It is consumed via Claude Code marketplace by project repos (including the `internal` repo).

## Structure

- `.claude/agents/` — Agent definitions (Developer, PM, Marketing Strategist, Coach)
- `.claude/commands/` — User-invocable commands (`/team`, `/excalidraw-diagram`, `/latex-document`, `/latex`, `/latex-cheatsheet`)
- `.claude/skills/` — Skills (handoff-format, excalidraw-diagram, latex, latex-cheatsheet, skill-creation)
- `.claude-plugin/` — Plugin and marketplace manifests

## Key Conventions

- Agent files use YAML frontmatter (`name`, `description`, `model`).
- Skill files use YAML frontmatter (`description`, `user-invocable`, `argument-hint`).
- Commands reference skills — they are the user-facing entry point, skills hold the implementation knowledge.
- The skill-creation meta-skill documents patterns and quality standards for new skills.

## Testing Changes

After modifying agents, commands, or skills:
1. Verify YAML frontmatter is valid
2. Test locally by running commands in a workspace that has this plugin
3. Ensure `plugin.json` paths are correct (`./.claude/agents/`, etc.)

## Related

- **Internal repo:** `projectious-work/internal` — consumes this plugin, hosts backlog, decisions, and project management
- **Project ID:** PROJ-004 in the internal project registry
- **Backlog items:** BACK-007 (sqlite-rag skill), BACK-010 (this repo creation), BACK-011 (marketplace consumption)
