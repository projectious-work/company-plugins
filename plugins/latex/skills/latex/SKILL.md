---
description: Create, compile, and visually validate professional PDF documents using LuaLaTeX
user-invocable: true
argument-hint: <description of document>
---

# LaTeX Document Skill

Create, compile, and visually validate LaTeX documents using LuaLaTeX.

## Workflow

1. **Design** — decide document type, structure, and layout.
2. **Write** — create `.tex` files following the rules below.
3. **Compile** — run `latexmk` with LuaLaTeX to produce PDF.
4. **Validate** — extract page images with `pdftoppm`, inspect visually, fix, recompile.

## Compilation

Always compile from the document's directory with a separate output directory:

```bash
cd <doc-directory> && latexmk -lualatex -output-directory=out main.tex
```

For bibliography support (biber), `latexmk` handles multiple passes automatically.

## Visual Validation Loop

After compilation, extract pages as images for visual inspection:

```bash
pdftoppm -png -r 200 out/main.pdf out/preview
```

This produces `out/preview-1.png`, `out/preview-2.png`, etc. Read each PNG with
the Read tool. Check for:
- Overfull/underfull boxes (text protruding into margins)
- Broken cross-references (`??`)
- Misaligned elements, orphaned headings, widow/orphan lines
- Color contrast issues, unreadable small text
- Missing figures or broken image paths

Fix the `.tex` source, recompile, and re-inspect until clean.

## Document Structure Rules

### Engine and encoding
- **Always use LuaLaTeX.** Never pdflatex or xelatex.
- LuaLaTeX provides native UTF-8. Do NOT load `inputenc` or `fontenc`.
- Use `fontspec` for all font configuration.

### Document class
- Use `extarticle` (with `extsizes`) when small base sizes are needed (8pt, 9pt).
- Use KOMA-Script classes (`scrartcl`, `scrreprt`, `scrbook`) for standard documents — they provide superior defaults for typography, margins, and headings.
- Use `fancyhdr` only with standard/ext classes; use `scrlayer-scrpage` with KOMA.

### Package load order
Load in this order to avoid conflicts:
1. `babel` (language/hyphenation)
2. `fontspec`, `unicode-math` (fonts)
3. `geometry` (page layout)
4. `xcolor` (colors — before tikz/tcolorbox)
5. `graphicx`, `tikz`, `tcolorbox` (graphics)
6. `tabularray` (tables)
7. `amsmath`, `siunitx` (math/units)
8. `biblatex` + `csquotes` (bibliography)
9. `hyperref` (links — always near last)
10. `cleveref` (cross-refs — always last)

### Modern packages only

| Purpose | Use | Avoid |
|---|---|---|
| Bibliography | `biblatex` + `biber` | `bibtex` + `natbib` |
| Cross-refs | `cleveref` (`\cref{}`) | manual `\ref{}` |
| Units | `siunitx` | manual formatting |
| Colored boxes | `tcolorbox` | `mdframed` / `framed` |
| Tables | `tabularray` or `booktabs` | `tabular` + `\hline` |

## Typography Rules

### Fonts
- Maximum **2 font families + 1 monospace**. Declare all in preamble.
- Recommended pairings (all available in our TeX Live install):
  - Body: `FreeSans` (sans) or `TeX Gyre Pagella` (serif)
  - Mono: `Source Code Pro`
  - Math: `Latin Modern Math` or `TeX Gyre Pagella Math`

### Line length and spacing
- Target **60–70 characters per line** (66 optimal). Adjust `\textwidth` accordingly.
- For A4 portrait: margins of 2.5–3cm at 11pt produce ~65 chars/line.
- Paragraph separation: **indentation XOR vertical spacing**, never both.
- Heading spacing: space above = 1.5–2x space below.

### Microtypography
- Always load `microtype` for documents longer than one page:
  ```latex
  \usepackage[activate={true,nocompatibility},tracking=true]{microtype}
  ```

## Color Rules

Define a **5-color semantic palette** in the preamble. Follow the 60/30/10 rule:
- 60% neutral/white (body, backgrounds)
- 30% primary/secondary (headings, rules, structure)
- 10% accent (callouts, highlights)

```latex
\definecolor{primary}{HTML}{2C3E50}     % headings, rules
\definecolor{secondary}{HTML}{2980B9}   % subheadings, links
\definecolor{accent}{HTML}{E74C3C}      % callouts, warnings
\definecolor{neutral}{HTML}{7F8C8D}     % captions, secondary text
\definecolor{background}{HTML}{F8F9FA}  % code blocks, boxes
```

- Minimum contrast ratio: **4.5:1** for normal text, **3:1** for 18pt+.
- Document must remain comprehensible in grayscale.
- Use semantic color names (`primary`, `warning`), not descriptive (`red1`, `blue2`).

## Graphics and Diagrams

### TikZ diagrams
- Load `tikz` with needed libraries: `\usetikzlibrary{...}`
- Define shared styles in the preamble with `\tikzset{}`
- Keep diagrams self-contained — each diagram in its own file via `\input{}`

### TikZ coordinate system rule
- **Always place a virtual origin `(0,0)` at the lower-left corner** of the
  diagram. Define it as a named coordinate:
  ```latex
  \coordinate (origin) at (0,0);
  ```
- Position all shapes, nodes, and other coordinates **relative to this origin**
  using absolute offsets or relative placements (`right=2cm of origin`,
  `above right=1cm and 3cm of origin`, etc.).
- This ensures consistent, predictable positioning and makes diagrams easier to
  resize, align, and compose. Never scatter shapes using unanchored absolute
  coordinates — always relate back to `(0,0)`.

### External images
- Store in a dedicated `figures/` directory (or `assets/`)
- Set `\graphicspath{{./figures/}}` in preamble
- Supported formats: PNG, JPG, PDF (vector), SVG (via `\usepackage{svg}` + inkscape)
- For Excalidraw diagrams: render to PNG first (see excalidraw-diagram skill),
  then include with `\includegraphics`

### Image inclusion
```latex
\includegraphics[width=0.8\textwidth]{filename}  % no extension needed
```

## File Organisation

For documents that are part of a larger project (e.g., alongside source code):

```
project/
├── src/                    # source code
├── docs/
│   ├── <doc-name>/
│   │   ├── main.tex        # entry point
│   │   ├── chapter1.tex    # split by section/chapter
│   │   ├── figures/        # images and diagrams
│   │   ├── out/            # build output (gitignored)
│   │   └── bibliography/   # or shared at project level
│   └── assets/             # shared assets across documents
├── bibliography/           # shared .bib files
└── .gitignore              # include LaTeX artifacts + out/
```

For standalone document projects:
```
docs/
├── <doc-name>/
│   ├── main.tex
│   ├── sections/           # split content files
│   ├── figures/
│   ├── out/                # gitignored
│   └── bibliography/
```

Key rules:
- One `main.tex` entry point per document
- Split content into files via `\input{}` — one file per logical section
- Build output goes to `out/` directory (always gitignored)
- Multiple documents in a project each get their own subdirectory under `docs/`

## .gitignore for LaTeX

Ensure these patterns are in `.gitignore`:

```
out/
*.aux *.bbl *.blg *.log *.out *.toc *.fls *.fdb_latexmk
*.synctex.gz *.run.xml *.bcf *tcbtemp _minted-* svg-inkscape/
```

## Design Principles (CRAP)

- **Contrast** — headings must differ from body in size, weight, AND color.
- **Repetition** — every heading level looks identical throughout.
- **Alignment** — left-align body text. Center only titles and single-line items.
- **Proximity** — related items grouped together; heading binds to content below.

## Information Hierarchy

Establish exactly 4–5 levels:

| Level | Element | Treatment |
|---|---|---|
| 1 | Document title | Largest, bold, primary color |
| 2 | Section heading | Large, bold, primary color |
| 3 | Subsection | Medium, semibold, secondary color |
| 4 | Body text | Normal size, regular weight |
| 5 | Captions/footnotes | Smaller, lighter/italic |

Never skip a level.
