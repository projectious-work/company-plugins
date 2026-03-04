# Excalidraw JSON Schema Reference

Minimal reference for generating valid `.excalidraw` element JSON.

## Element Types

| Type | Purpose | Key extra properties |
|---|---|---|
| `rectangle` | Boxes, containers | — |
| `ellipse` | Circles, ovals | — |
| `diamond` | Decision nodes | — |
| `text` | Standalone labels | `text`, `fontSize`, `fontFamily`, `textAlign` |
| `arrow` | Directed connections | `points`, `startBinding`, `endBinding` |
| `line` | Undirected lines | `points` |
| `freedraw` | Freehand strokes | `points` |
| `image` | Embedded images | `fileId` (references `files` dict) |

## Common Properties (all elements)

```
id              string    Unique identifier
type            string    One of the types above
x, y            number    Position (top-left corner)
width, height   number    Bounding box size
angle           number    Rotation in radians (default 0)
strokeColor     string    Border/line color (hex)
backgroundColor string    Fill color (hex, or "transparent")
fillStyle       string    "solid" | "hachure" | "cross-hatch"
strokeWidth     number    1 (thin), 2 (normal), 4 (bold)
strokeStyle     string    "solid" | "dashed" | "dotted"
roughness       number    0 (architect) | 1 (artist) | 2 (cartoonist)
opacity         number    0–100
roundness       object    { "type": 3 } for rounded corners, null for sharp
isDeleted       boolean   Soft-delete flag
boundElements   array     [{ "id": "...", "type": "text|arrow" }]
```

## Text Properties

```
text            string    The displayed text
fontSize        number    Size in px (16–32 typical)
fontFamily      number    1 (Virgil/hand), 2 (Helvetica), 3 (Cascadia/mono)
textAlign       string    "left" | "center" | "right"
verticalAlign   string    "top" | "middle"
containerId     string    ID of parent container (for bound text)
```

When text is bound to a container:
- Text element: set `containerId` to the container's `id`
- Container element: add `{ "id": "<text-id>", "type": "text" }` to `boundElements`
- Text `width`/`height` should fit inside the container

## Arrow / Line Properties

```
points          array     [[0,0], [dx,dy]] — relative to x,y
startBinding    object    { "elementId": "id", "focus": 0, "gap": 4 }
endBinding      object    { "elementId": "id", "focus": 0, "gap": 4 }
startArrowhead  string    null | "arrow" | "dot" | "bar"
endArrowhead    string    null | "arrow" | "dot" | "bar"
```

- `focus`: -1 to 1, controls where the arrow attaches (0 = center)
- `gap`: space between arrowhead and target element border
- Target elements must list the arrow in their `boundElements`

## Grouping

Use `groupIds: ["group-1"]` on elements to visually group them. Shared
`groupIds` values link elements into a group.
