# /excalidraw-diagram — Create a technical diagram

Creates a diagram using the Excalidraw skill, renders it to PNG, and validates
the result visually.

## Usage

```
/excalidraw-diagram <description of what the diagram should show>
```

## Behaviour

1. **Read the skill** — Load `.claude/skills/excalidraw-diagram/SKILL.md` for design
   guidelines, color palette, and render workflow. Load
   `.claude/skills/excalidraw-diagram/references/json-schema.md` for element reference.

2. **Select agent** — Choose the agent best suited to the diagram's subject:
   - Architecture, systems, infrastructure, code flows → Developer
   - Processes, timelines, workflows, project structure → Project Manager
   - Service offerings, client-facing visuals, proposals → Marketing Strategist

3. **Design** — Plan the layout (left-to-right for flows, top-to-bottom for
   hierarchies), choose elements, and assign semantic colors per the palette in
   SKILL.md.

4. **Generate** — Write a valid `.excalidraw` JSON file. Save to
   `docs/assets/diagrams/<descriptive-name>.excalidraw`.

5. **Render** — Run the renderer:
   ```bash
   cd .claude/skills/excalidraw-diagram/references && \
     uv run python render_excalidraw.py /absolute/path/to/file.excalidraw
   ```

6. **Validate** — Read the rendered PNG. Check for: overlapping elements,
   missing labels, broken arrows, unreadable text, misaligned shapes. Fix and
   re-render until clean.

7. **Report** — Show the final PNG to the principal. State where both files are
   saved (`.excalidraw` source + `.png` render in `docs/assets/diagrams/`).

## Context

Before designing, read:
- `context/DECISIONS.md` — for relevant architectural or organisational context
- The skill's SKILL.md — for design guidelines and the validation loop

## Example

```
/excalidraw-diagram CI/CD pipeline showing GitHub Actions building the
dev container, running tests, and deploying to staging
```

This would engage the **Developer** agent to create a left-to-right flow diagram
with pipeline stages, save it to `docs/assets/diagrams/cicd-pipeline.excalidraw`,
render and validate the PNG.
