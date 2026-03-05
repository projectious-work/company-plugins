# company-plugins — Development Context

This is the plugin marketplace for projectious.work. It hosts multiple plugins consumed via Claude Code marketplace by project repos (including the `internal` repo).

## Structure

This repo is a **marketplace** (not a single plugin). The root `.claude-plugin/marketplace.json` catalogs three plugins:

- `plugins/teamwork/` — Agents (Developer, PM, Marketing Strategist, Coach), `/team` command, handoff format, skill creation meta-skill
- `plugins/latex/` — `/latex`, `/latex-document`, `/latex-cheatsheet` commands and corresponding skills
- `plugins/excalidraw/` — `/excalidraw-diagram` command and diagram skill with Playwright renderer

Each plugin has its own `.claude-plugin/plugin.json` declaring its components.

## Key Conventions

- Agent files use YAML frontmatter (`name`, `description`, `model`).
- Skill files use YAML frontmatter (`description`, `user-invocable`, `argument-hint`).
- Commands reference skills — they are the user-facing entry point, skills hold the implementation knowledge.
- The skill-creation meta-skill documents patterns and quality standards for new skills.
- Plugins cannot reference files outside their own directory (they are cached independently).

## Testing Changes

After modifying agents, commands, or skills:
1. Verify YAML frontmatter is valid
2. Validate the marketplace: `claude plugin validate .` or `/plugin validate .`
3. Test locally: `/plugin marketplace add ./path/to/company-plugins` then install individual plugins
4. Ensure each `plugin.json` paths are correct relative to the plugin root

## Branching & Release Strategy

### Branching

- **`main`** is the stable trunk. All work merges back to main.
- **Feature branches** per backlog item: `back-NNN/short-description`
  (e.g., `back-007/sqlite-rag`).
- Work on feature branches in **worktrees** where practical.
- **Merge commits** (no squash) to preserve full history. Merge commit message
  references the `BACK-NNN` ID.
- **PM or Coach** verifies acceptance criteria and merges to main.

### Releases & Versioning

Semantic versioning (`vMAJOR.MINOR.PATCH`), starting at `v0.1.0`.

| Level | When | Example trigger |
|---|---|---|
| **Major** | Breaking change to plugin structure or marketplace format | Incompatible plugin.json schema change |
| **Minor** | New plugin, new agent, new skill, or new command | Adding sqlite-rag skill |
| **Patch** | Fixes, refinements, prompt/skill improvements | At least weekly, PM decides timing |

- Releases are **annotated git tags** on main.
- **PM owns releases** — decides timing, writes the tag message.

## Related

- **Internal repo:** `projectious-work/internal` — consumes these plugins, hosts backlog, decisions, and project management
- **Project ID:** PROJ-004 in the internal project registry
- **Backlog items:** BACK-007 (sqlite-rag skill), BACK-010 (this repo creation), BACK-011 (marketplace consumption)
