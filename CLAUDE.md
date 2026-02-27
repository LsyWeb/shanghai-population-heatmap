# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a **zero-dependency, single-file** interactive heatmap visualization of Shanghai's population distribution by district, based on the 2024 Shanghai Statistical Yearbook. Deployed as a static site on Vercel.

Live demo: https://shanghai-population-heatmap.vercel.app

## Running Locally

No build step, no package manager, no dependencies:

```bash
# macOS
open index.html

# Or serve with any static file server
python3 -m http.server 8080
```

## Architecture

Everything lives in `index.html` — a single self-contained file with three sections:

1. **CSS** (`:root` variables → component styles) — Dark theme with a 7-stop heat color scale (`--heat1` through `--heat7`), three-column grid layout (left panel 260px | map | right panel 220px)

2. **HTML** — Three-panel layout:
   - Left: `#districtList` — ranked district list (rendered by JS)
   - Center: `#map` SVG (600×580 viewBox) — polygon-based map (rendered by JS)
   - Right: mode toggle, color legend, summary stats, TOP 3 list

3. **JavaScript** — Key data structures and functions:
   - `districts[]` — array of 16 district objects with `{ id, name, pop (万人), area (km²), cx, cy }` for label placement
   - `polygons{}` — SVG polygon point strings for each district (keyed by `id`), simplified but geographically faithful shapes scaled to the 600×580 viewBox
   - `getHeatColor(normalized)` — interpolates through 7 color stops (deep purple → bright yellow)
   - `getNormalized(mode)` — min-max normalizes values for the current mode
   - `renderMap(mode)` / `renderList(mode)` — full re-render on mode switch
   - `highlightDistrict(id)` — syncs highlight between map clicks and list clicks
   - `setMode(mode)` — switches between `'population'` and `'density'` views

## Key Conventions

- **Zero dependencies**: No npm, no bundler, no external JS libraries. Google Fonts is the only external resource.
- **CSS variables**: All colors and theme values defined in `:root`. Reuse existing variables; add new ones there.
- **JS naming**: camelCase for functions and variables.
- **Data updates**: To update district population data, edit the `districts` array in `index.html`. Use official Shanghai Bureau of Statistics data.
  ```js
  { id: 'pudong', name: '浦东新区', pop: 568.2, area: 1210, cx: 420, cy: 310, paths: true }
  // pop: 常住人口（万人）, area: 辖区面积（km²）, cx/cy: SVG label center coordinates
  ```
- **Map geometry**: District shapes are in `polygons{}` as SVG polygon point strings. Coordinates are in the 600×580 viewBox space.

## Commit Style

Follow conventional commits: `feat:`, `fix:`, `docs:`, `chore:`, `refactor:`
