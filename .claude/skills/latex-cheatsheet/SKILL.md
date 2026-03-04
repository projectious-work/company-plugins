---
description: Create dense reference sheets and cheatsheets using LuaLaTeX with tcbposter grid layouts
user-invocable: true
argument-hint: <description of cheatsheet>
---

# LaTeX Cheatsheet Skill

Create dense, visually structured reference sheets using `tcbposter` layouts.

**Prerequisite:** This skill extends the LaTeX skill (`.claude/skills/latex/SKILL.md`).
Read that first for compilation, validation, typography, and color rules. This
document covers only what is specific to cheatsheets.

## What Changes vs. Standard Documents

| Aspect | Standard document | Cheatsheet |
|---|---|---|
| Font size | 10–12pt | 7–9pt body, 8pt base class |
| Line spacing | 1.2–1.5x | 1.0–1.15x |
| Margins | 2.5–3cm | 2.5–5mm |
| Orientation | Portrait | Landscape (A4) |
| Layout | Linear flow | Grid (tcbposter) |
| Whitespace | 30%+ | Minimal — every pixel earns its place |

## Document Setup

Use `extarticle` with 8pt base size, landscape A4, minimal margins:

```latex
\documentclass[8pt]{extarticle}
\usepackage[USenglish]{babel}
\usepackage{fontspec}
\setmainfont{FreeSans}[Scale=0.95]
\setsansfont{FreeSans}[Scale=0.95]
\setmonofont{Source Code Pro}[Scale=0.80]
\usepackage{unicode-math}
\setmathfont{Latin Modern Math}

\usepackage[nomarginpar, a4paper, landscape,
  left=0.25cm, right=0.25cm, top=1cm, bottom=0.65cm,
  headsep=0.1cm, headheight=0.75cm, footskip=0.35cm
]{geometry}
```

## Page Layout with tcbposter

Each page is a `tcbposter` environment with a 12-column grid:

```latex
\usepackage{tcolorbox}
\tcbuselibrary{skins,raster,listings,breakable,minted,poster}

\begin{tcbposter}[
  poster = {spacing=0.6em, columns=12},
  boxes = {},
  coverage = {left=0pt, right=0pt, top=0pt, bottom=0pt},
]
  \begin{posterboxenv}[skin=cheatboxskin, adjusted title=Section Title]
    {name=uniquename, column=1, below=top, span=4}
    \input{section_file.tex}
  \end{posterboxenv}
  % ... more boxes
\end{tcbposter}
\pagebreak
```

### Grid rules
- **12 columns** provides flexible spanning: 2, 3, 4, 6 column boxes.
- **Positioning:** use `column=N`, `below=top` (or `below=boxname`), `span=N`.
- Every box gets a unique `name` for positioning references.
- Each page is a separate `tcbposter`. Use `\pagebreak` between pages.
- **Trial and error:** box heights are determined by content. Adjust column
  placement and span iteratively — compile, inspect, rearrange to fill pages
  tightly with minimal dead space.

## Box Skin Definition

Define a reusable skin in the preamble. The primary and secondary colors are
set as commands for easy theming:

```latex
\newcommand{\primarycolor}{Olive}
\newcommand{\secondarycolor}{SteelBlue}

\tcbsubskin{cheatboxskin}{standard}{
  size=title,
  left=0.2em, right=0.2em, bottom=0.2em,
  arc=0.25mm,
  colback=\primarycolor!2!white,
  colframe=\primarycolor!85!black,
  coltitle=\primarycolor!5!white,
  colbacktitle=\primarycolor!95!black,
  fonttitle=\sffamily\bfseries,
  toptitle=0mm, bottomtitle=0mm,
  fontupper=\sffamily\small,
  fontlower=\sffamily\small,
  subtitle style={
    top=0.4mm, bottom=0mm,
    boxrule=0.2pt, boxsep=0.1mm,
    colback=\primarycolor!20!white,
    colupper=\primarycolor!50!black,
    fontupper=\sffamily\bfseries\small,
    coltext=\primarycolor!10!black,
    height=1.1\baselineskip,
  },
}
```

## Content Patterns Inside Boxes

### Subtitles (sub-sections within a box)

```latex
\tcbsubtitle{Sub-Section Name}
```

### Compact tables (tabularray inside tcolorbox)

Define a table skin for consistent styling:

```latex
\tcbsubskin{cheatboxtablesimple}{empty}{
  size=minimal,
  fontupper=\sffamily\small, fontlower=\sffamily\small,
  before skip=0.2\baselineskip, after skip=0.2\baselineskip,
  colback=white, colframe=gray, frame empty,
}
```

Usage:

```latex
\begin{tcolorbox}[skin=cheatboxtablesimple,
  tabularray={
    colspec={X[1,l]X[2,l]},
    columns={valign=m, colsep=0.25em},
    rows={rowsep=0pt},
    hline{2}={solid},
  }]
  \textbf{Header 1} & \textbf{Header 2} \\
  Data & Description \\
\end{tcolorbox}
```

### Compact lists (itemize with tight spacing)

```latex
\begin{itemize}[nosep, leftmargin=1em, label=\textbullet]
  \item First item
  \item Second item
\end{itemize}
```

Always use `nosep` (from `enumitem`) to eliminate list spacing in cheatsheets.

### Two-column content inside a box (tcbitemize raster)

```latex
\begin{tcbitemize}[raster columns=2, blankest,
  raster equal height=rows, valign=top]
  \tcbitem Left column content
  \tcbitem Right column content
\end{tcbitemize}
```

### Side-by-side (diagram + text)

```latex
\begin{tcbitemize}[raster columns=2, blankest,
  raster column skip=0.5mm, raster row skip=0.5mm,
  raster every box/.style={sidebyside, sidebyside align=center,
    sidebyside gap=0.5pt, lefthand width=1.7cm,
    halign upper=center, left=0pt}]
  \tcbitem
    \begin{tikzpicture}[...]
      % diagram
    \end{tikzpicture}
  \tcblower
    \fontsize{5}{6}\selectfont
    \textbf{Title}\\
    Description text
\end{tcbitemize}
```

## Graphics in Cheatsheets

### TikZ inline diagrams
- Define shared styles in preamble with `\tikzset{}`
- Use compact bounding boxes to minimize wasted space
- Keep font sizes in diagrams at 3–5pt for annotations

### Excalidraw diagrams
- Render to PNG using the excalidraw-diagram skill
- Include with `\includegraphics[width=\linewidth]{figures/diagram.png}`

### Bitmap images and SVGs
- Store in `figures/` directory
- PNGs/JPGs: `\includegraphics[width=\linewidth]{figures/image.png}`
- SVGs: `\includesvg[width=\linewidth]{figures/image}` (requires `svg` package + inkscape)
- Set `\graphicspath{{./figures/}}` in preamble

## Typography Specifics

- Base class: `extarticle` at **8pt**
- Body text inside boxes: `\sffamily\small` (via skin definition)
- Compact annotations: `\fontsize{5}{6}\selectfont` for fine print
- Subtitles: bold, slightly larger than body
- Use `\fontsize{N}{M}\selectfont` for precise size control in tight spaces
- Minimum readable size: **5pt** for annotations, **7pt** for body content

## Header and Footer

```latex
\usepackage{fancyhdr}
\pagestyle{fancy}
\fancyhead[L]{\huge\headerfont{Title} \textcolor{\primarycolor}{Subtitle}}
\fancyhead[C]{}
\fancyhead[R]{}
\fancyfoot[L]{}
\fancyfoot[C]{}
\fancyfoot[R]{\thepage}
```

## Space Filling Strategy

Cheatsheets must fill pages as tightly as possible. The process is iterative:

1. Write all content sections as separate `.tex` files
2. Place boxes in the tcbposter grid with initial span estimates
3. Compile and inspect — note which boxes are too tall/short
4. Adjust: change `span`, reorder boxes, move content between pages
5. Repeat until pages are densely filled with minimal dead space
6. Use `column=N, below=boxname` to stack boxes vertically within columns

## File Structure

```
docs/<cheatsheet-name>/
├── main.tex              # preamble + tcbposter pages
├── topic_one.tex         # content for first box
├── topic_two.tex         # content for second box
├── ...
├── figures/              # tikz exports, excalidraw PNGs, images
├── bibliography/         # if needed
└── out/                  # build output (gitignored)
```

Split each poster box's content into its own file. This allows:
- Rearranging boxes without touching content
- Reusing content across different layouts
- Easier iteration on individual sections
