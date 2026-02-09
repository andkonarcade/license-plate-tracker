# CLAUDE.md

## Project Overview

License Plate Tracker is a single-page web application that displays an interactive SVG map of the United States, tracking license plates spotted in the wild from the Instagram account [@andkon](https://www.instagram.com/andkon/). Clicking a collected state opens an embedded Instagram post in a modal. Hosted on [GitHub Pages](https://andkonarcade.github.io/license-plate-tracker/).

## Architecture

**This is a single-file application.** The entire app lives in `index.html` (~880 lines) with inline CSS and JavaScript. There is no build system, no bundler, no package manager, and zero external dependencies.

```
index.html          # Complete application (HTML + CSS + JS)
README.md           # Brief project description
CLAUDE.md           # This file
```

### Structure Within index.html

The file has three sections in order:

1. **`<style>`** — All CSS styles and responsive breakpoints
2. **`<body>` markup** — SVG map, small-state boxes, modal
3. **`<script>`** — Data (`plates` object), state management, UI logic

## Tech Stack

- **Vanilla HTML5, CSS3, JavaScript (ES6+)** — no frameworks, no libraries
- **SVG** — US map rendered as inline SVG with one `<path>` per state
- **Instagram Embeds** — iframes pointing to `instagram.com/p/CODE/embed`
- **GitHub Pages** — static hosting, no server or database

## Key Data Structure

The `plates` object at the top of the `<script>` block is the core data store mapping 2-letter state codes to Instagram URLs:

```javascript
const plates = {
    "al": "",                              // Not collected
    "ca": "https://www.instagram.com/p/...", // Single plate
    "tx": "url1 | url2 | url3",           // Multiple plates (pipe-separated)
    "mi": ["url1", "url2"],               // Multiple plates (array format)
};
```

Both pipe-separated strings and arrays are supported for multiple URLs per state.

## Common Tasks

### Adding a New License Plate

1. Open `index.html`
2. Find the `plates` object at the top of the `<script>` block
3. Replace the empty string for the state with the Instagram post URL
4. For additional plates on an already-collected state, append with ` | ` separator

### Modifying the UI

- **CSS**: Edit the `<style>` block in `<head>`
- **HTML structure**: Edit the body markup between `</style>` and `<script>`
- **JavaScript logic**: Edit the `<script>` block at the end of `<body>`

### Testing Changes

Open `index.html` directly in a browser. No build step or dev server required.

## Code Conventions

### Naming

- **State IDs**: 2-letter lowercase codes (`al`, `ca`, `tx`, `dc`)
- **CSS classes**: kebab-case (`.small-state-box`, `.modal-overlay`)
- **JS variables**: camelCase (`currentIndex`, `stateNames`, `plateCollected`)
- **JS functions**: camelCase (`openModal`, `closeModal`, `setupHoverSync`)

### Color Palette

| Color | Hex | Usage |
|-------|-----|-------|
| Dark background | `#0f172a` | Page background |
| Green | `#10b981` | Collected states |
| Light green | `#34d399` | Hover on collected |
| Gray | `#64748b` | Uncollected states |
| Light gray | `#94a3b8` | Hover on uncollected |

### Responsive Breakpoints

- Default: Desktop layout, small-states positioned at bottom-right of map
- `max-width: 700px` or `max-height: 600px`: Small-states move below map in horizontal row
- `max-height: 500px`: Reduced padding, smaller modal iframe

### UI Patterns

- **Hover sync**: Hovering any of (SVG path, small-state box, count badge) highlights all three
- **Modal carousel**: Arrow keys and prev/next buttons navigate multiple photos per state
- **Keyboard shortcuts**: Escape closes modal, Arrow keys navigate carousel

## Small States

Nine states are too small to click on the SVG map and are rendered as separate boxes: CT, DE, DC, MA, MD, NH, NJ, RI, VT. These are defined in the `smallStates` array in the `<script>` block.

## Development Workflow

1. Edit `index.html` directly
2. Open in browser to verify changes
3. Commit and push — GitHub Pages deploys automatically from the default branch

There is no linting, formatting, testing, or CI/CD configuration. Keep changes simple and self-contained within the single file.

## Important Notes for AI Assistants

- **Do not split the single-file architecture.** The project is intentionally a single HTML file. Do not refactor into separate CSS/JS files unless explicitly asked.
- **Preserve the SVG map paths.** The `<path>` elements for each state contain complex coordinate data. Never modify these unless specifically requested.
- **Instagram URL format matters.** URLs must follow the pattern `https://www.instagram.com/p/CODE/` — the `normalizeInstagramUrl` function handles minor variations but the base format should be correct.
- **No build step.** Changes go live by editing `index.html` and pushing. There is nothing to compile or bundle.
- **Keep it vanilla.** Do not introduce npm, frameworks, or build tools unless explicitly requested.
