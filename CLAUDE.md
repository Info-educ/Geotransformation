# CLAUDE.md — AI Assistant Guide for Geotransformation

## Project Overview

This is a **French educational geometry web application** for teaching geometric transformations to students. It is a single, self-contained HTML5 file with no build process, no package manager, and no external runtime dependencies.

- **File**: `GeoTransformation1.html` (2141 lines, ~105 KB)
- **Language**: French (all UI text)
- **Stack**: HTML5 + CSS3 + Vanilla JavaScript (ES6 classes)
- **Target users**: French secondary school students and teachers

---

## How to Run

Open `GeoTransformation1.html` directly in any modern web browser. No build steps, no `npm install`, no server required.

```bash
# On Linux
xdg-open GeoTransformation1.html

# On macOS
open GeoTransformation1.html
```

---

## Repository Structure

```
Geotransformation/
├── GeoTransformation1.html   # Entire application (HTML + CSS + JS)
├── README.md                 # Minimal project description
└── CLAUDE.md                 # This file
```

There is no `src/` directory, no `node_modules`, no build output. Everything lives in the single HTML file.

---

## Application Architecture

### Tab Modules

The UI has four tabs, each representing a geometric transformation type:

| Tab ID | Class | Feature |
|--------|-------|---------|
| `pane-axiale` | `Axiale` | Symétrie Axiale (mirror symmetry) |
| `pane-centrale` | `Centrale` | Symétrie Centrale (point symmetry) |
| `pane-translation` | `Translation` | Translation (vector translation) |
| `pane-rotation` | `Rotation` | Rotation (angular rotation) |

### JavaScript Class Hierarchy

```
Geo (base class, ~500+ lines)
├── Axiale     extends Geo
├── Centrale   extends Geo
├── Translation extends Geo
└── Rotation   extends Geo
```

Each class is instantiated once on page load and manages its own canvas and controls.

### Key Base Class (`Geo`) Methods

| Method | Purpose |
|--------|---------|
| `draw()` | Main render loop entry point (overridden per subclass) |
| `drawGrid()` | Renders coordinate grid |
| `drawPoly()` | Renders polygons |
| `drawDot()` | Renders labeled points |
| `s2w(x, y)` | Screen → world coordinate conversion |
| `w2s(x, y)` | World → screen coordinate conversion |
| `_dn(e)` | Mouse/touch down handler |
| `_mv(e)` | Mouse/touch move handler |
| `_up(e)` | Mouse/touch up handler |
| `startAnim()` | Launches `requestAnimationFrame` loop |
| `_codeMilieu()` | Draws equal-length tick marks |
| `_codeAngleDroit()` | Draws right-angle indicators |

---

## Code Conventions

### JavaScript

- **ES6 classes** with `extends` for all four transformation modules
- **Private methods** prefixed with `_` (e.g., `_dn`, `_mv`, `_up`, `_codeMilieu`)
- **Points** are `[x, y]` arrays throughout
- **Polygons** are arrays of `[x, y]` points
- Short variable names (`x`, `y`, `cx`, `cy`) inside render/math loops
- Descriptive names elsewhere

### HTML Element IDs

Element IDs follow a `prefix-name` pattern scoped to each tab:

| Prefix | Tab |
|--------|-----|
| `ax-` | Axiale |
| `ce-` | Centrale |
| `tr-` | Translation |
| `ro-` | Rotation |

Example: `ax-shape`, `tr-vx`, `ro-ang`

### CSS

- **CSS custom properties** (`--bg`, `--surface`, `--text`, etc.) for theming
- **Dark/light mode** via `data-theme` attribute on `<html>` element
- **Breakpoint**: `680px` for mobile layout adaptation
- **Color semantics**:
  - Blue `#4f8ef7` — original shapes
  - Pink `#f74f8e` — transformed shapes
  - Green `#4ff7b0` — axes / construction lines
  - Orange `#f7c94f` — rotation / vectors
  - Cyan `#4ff7f7` — translation

---

## Interactive Features

- **Drag**: points, shapes, and transformation parameters (axis, center, vector, angle)
- **Zoom**: mouse wheel or pinch gesture
- **Pan**: `Ctrl` + drag
- **Animation**: play/pause button per tab
- **Image import**: drag-and-drop or click-to-upload for applying transformations to images
- **Theme toggle**: light/dark mode switcher in the header
- **Shape selector**: shared across tabs (selection state syncs)
- **Real-time display**: coordinate readout, slider values update as you drag

---

## Making Changes

Since everything is in one HTML file, all edits go to `GeoTransformation1.html`.

### Adding a new feature or fixing a bug

1. Identify whether the change is:
   - **Visual/layout** → edit the `<style>` block near the top
   - **Geometry math / rendering** → edit the `<script>` block at the bottom
   - **UI controls** → edit the relevant `pane-*` HTML section
2. Test by opening the file in a browser
3. No build or lint step needed

### Adding a new transformation tab

1. Add a `<button>` in the tab navigation
2. Add a `<div class="pane" id="pane-yourname">` section with controls + canvas
3. Create a new class `YourName extends Geo` in the `<script>` block
4. Instantiate it in the tab-switch handler

---

## No Build / No Tests

This project has **no automated tests** and **no build pipeline**. Verification is manual: open the file in a browser and interact with it.

There is no linter, formatter, or CI configuration. Keep code style consistent with the existing conventions described above.

---

## Localization

All user-facing text is in **French**. Do not translate or add English UI strings. Variable names and code comments may be in either French or English — be consistent with the surrounding code.

---

## Git Workflow

- Default branch: `master`
- Feature branches follow the pattern: `claude/<description>-<id>`
- Commit messages should be in English and describe the change clearly
- Push with: `git push -u origin <branch-name>`
