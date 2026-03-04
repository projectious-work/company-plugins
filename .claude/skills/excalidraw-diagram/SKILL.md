---
description: Create and render technical diagrams as Excalidraw JSON, then validate visually by rendering to PNG
user-invocable: true
argument-hint: <description of diagram>
---

# Excalidraw Diagram Skill

Create and render technical diagrams as `.excalidraw` JSON files, then validate
them visually by rendering to PNG.

## Workflow

1. **Design** — decide layout, elements, and connections based on the diagram's
   purpose. Think spatially: left-to-right or top-to-bottom flow.
2. **Generate** — write a valid `.excalidraw` JSON file. See
   `references/json-schema.md` for element types and properties.
3. **Render & validate** — run the renderer, view the PNG, fix issues, re-render.

## Design Guidelines

- **One idea per diagram.** If it needs scrolling, split it.
- **Left-to-right** for flows/pipelines. **Top-to-bottom** for hierarchies.
- **Group related elements** with consistent spacing (40–60px gaps).
- **Use containers** (rectangles with `type: "rectangle"`) to group related items.
  Place child elements inside and bind arrows to the container, not the children.
- **Label everything.** Every shape gets a `text` property or a bound text element.
- **Arrows need bindings.** Set `startBinding` and `endBinding` with the target
  element's `id`. Use `points: [[0,0], [dx,dy]]` for direction.
- **Font sizes:** titles 28–32, labels 20–24, annotations 16–18.
- **Consistent sizing:** similar elements should have the same width/height.
- **Keep it readable at 1x zoom.** Minimum text size 16px.
- **Align elements** on a rough grid. Eyeball alignment looks sloppy.

## Color Palette

Use these semantic colors consistently within a diagram:

| Role | Fill | Stroke |
|---|---|---|
| Primary / action | `#a5d8ff` | `#1971c2` |
| Secondary / support | `#d0bfff` | `#6741d9` |
| Success / positive | `#b2f2bb` | `#2f9e44` |
| Warning / caution | `#ffec99` | `#e8590c` |
| Danger / error | `#ffc9c9` | `#e03131` |
| Neutral / container | `#e9ecef` | `#495057` |

Set `backgroundColor` to the fill and `strokeColor` to the stroke. Use
`"fillStyle": "solid"` for filled shapes.

## Rendering

From the workspace root:

```bash
cd .claude/skills/excalidraw-diagram/references && \
  uv run python render_excalidraw.py <path-to-file.excalidraw>
```

Outputs a PNG in the same directory as the input file (or use `--output path.png`).

Options: `--scale 2` (default), `--width 1920` (max viewport width).

### First-time setup

```bash
cd .claude/skills/excalidraw-diagram/references
uv sync
uv run playwright install chromium
```

## Validation Loop

1. Render the `.excalidraw` file to PNG.
2. Read the PNG with the Read tool to visually inspect it.
3. Check for: overlapping elements, missing labels, broken arrows, unreadable
   text, misaligned shapes, color inconsistency.
4. Fix issues in the `.excalidraw` JSON.
5. Re-render and re-inspect. Repeat until clean.

## File Format Essentials

Every `.excalidraw` file must have:

```json
{
  "type": "excalidraw",
  "version": 2,
  "elements": [ ... ],
  "appState": { "viewBackgroundColor": "#ffffff" }
}
```

Element `id` values must be unique strings. Use descriptive IDs like
`"server-box"`, `"db-arrow"` for easier debugging.
