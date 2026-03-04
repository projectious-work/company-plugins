# company-plugin

Virtual IT company plugin for [Claude Code](https://claude.ai/claude-code) — provides agents, commands, and skills for **projectious.work**, an IT consulting company specialising in Agentic AI, Agile, and Cloud.

## What's Included

### Agents

| Agent | Role |
|---|---|
| Developer | Coding, architecture, infrastructure, code review |
| Project Manager | Task tracking, scheduling, milestone management, backlog ownership |
| Marketing Strategist | Content creation, outreach, positioning, client-facing communication |
| Coach | Agile coaching, backlog health audits, impediment resolution, standups |

### Commands

| Command | Description |
|---|---|
| `/team` | Orchestrate a task across the virtual company |
| `/excalidraw-diagram` | Create and render technical diagrams |
| `/latex-document` | Create a LaTeX document |
| `/latex` | Create a LaTeX document (direct) |
| `/latex-cheatsheet` | Create a LaTeX cheatsheet |

### Skills

- **Handoff format** — structured inter-agent communication
- **Excalidraw diagrams** — create and render technical diagrams as Excalidraw JSON + PNG
- **LaTeX** — professional PDF documents with LuaLaTeX
- **LaTeX cheatsheet** — dense reference sheets using tcbposter grid layouts
- **Skill creation** — meta-skill for building new skills (not user-invocable)

## Installation

Install via Claude Code marketplace:

```
/plugin marketplace add projectious-work/company-plugin
```

## Setup

Some skills require one-time workspace setup:

**Excalidraw renderer:**
```bash
cd .claude/skills/excalidraw-diagram/references && uv sync && uv run playwright install chromium
```

**LaTeX (requires LuaLaTeX + latexmk):**
```bash
cd docs/<name> && latexmk -lualatex -output-directory=out main.tex
```

## License

Proprietary — projectious.work
