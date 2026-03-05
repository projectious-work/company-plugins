# company-plugins

Plugin marketplace for [Claude Code](https://claude.ai/claude-code) — provides agents, commands, and skills for **projectious.work**, an IT consulting company specialising in Agentic AI, Agile, and Cloud.

## Plugins

### `teamwork`

Virtual IT company agents with team orchestration and structured handoffs.

| Component | Items |
|---|---|
| Agents | Developer, Project Manager, Marketing Strategist, Coach |
| Commands | `/team` |
| Skills | Handoff format, Skill creation (meta-skill) |

### `latex`

Professional PDF document and cheatsheet creation with LuaLaTeX.

| Component | Items |
|---|---|
| Commands | `/latex`, `/latex-document`, `/latex-cheatsheet` |
| Skills | LaTeX, LaTeX cheatsheet |

### `excalidraw`

Technical diagram creation with Playwright-based PNG rendering.

| Component | Items |
|---|---|
| Commands | `/excalidraw-diagram` |
| Skills | Excalidraw diagram |

## Installation

Add the marketplace and install plugins:

```shell
/plugin marketplace add projectious-work/company-plugins
/plugin install teamwork@company-plugins
/plugin install latex@company-plugins
/plugin install excalidraw@company-plugins
```

## Setup

Some plugins require one-time workspace setup:

**Excalidraw renderer:**
```bash
cd skills/excalidraw-diagram/references && uv sync && uv run playwright install chromium
```

**LaTeX (requires LuaLaTeX + latexmk):**
```bash
cd docs/<name> && latexmk -lualatex -output-directory=out main.tex
```

## Repository Structure

```
company-plugins/
├── .claude-plugin/
│   └── marketplace.json          # marketplace catalog
├── plugins/
│   ├── latex/
│   │   ├── .claude-plugin/plugin.json
│   │   ├── skills/
│   │   │   ├── latex/SKILL.md
│   │   │   └── latex-cheatsheet/SKILL.md
│   │   └── commands/
│   │       ├── latex.md
│   │       ├── latex-document.md
│   │       └── latex-cheatsheet.md
│   ├── excalidraw/
│   │   ├── .claude-plugin/plugin.json
│   │   ├── skills/
│   │   │   └── excalidraw-diagram/
│   │   │       ├── SKILL.md
│   │   │       └── references/
│   │   └── commands/
│   │       └── excalidraw-diagram.md
│   └── teamwork/
│       ├── .claude-plugin/plugin.json
│       ├── agents/
│       ├── skills/
│       │   ├── handoff-format/SKILL.md
│       │   └── skill-creation/SKILL.md
│       └── commands/
│           └── team.md
├── README.md
├── CLAUDE.md
└── .gitignore
```

## Contributing

### Branching

All work happens on feature branches named `back-NNN/short-description` (referencing the internal backlog). Branches are merged to `main` with merge commits (no squash) after acceptance criteria are verified by the PM or Coach.

### Versioning

This repo uses [semantic versioning](https://semver.org/) (`vMAJOR.MINOR.PATCH`):

- **Major** — breaking changes to plugin structure or marketplace format
- **Minor** — new plugin, agent, skill, or command
- **Patch** — fixes, refinements, prompt improvements (at least weekly)

Releases are annotated git tags on `main`, managed by the PM.

### Testing Changes

After modifying agents, commands, or skills:
1. Verify YAML frontmatter is valid
2. Validate the marketplace: `claude plugin validate .` or `/plugin validate .`
3. Test locally: `/plugin marketplace add ./path/to/company-plugins` then install individual plugins
4. Ensure each `plugin.json` paths are correct relative to the plugin root

## License

Proprietary — projectious.work
