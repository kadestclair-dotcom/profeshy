# Profeshy design assets: Pixel Ledger

Design direction approved by Kade on 2026-11-18. The full canvas (every screen in dark and light, tablet layouts, brand and store artboards, pixel proofs) lives in Claude Design: https://claude.ai/artifact/A897bZd3qkqN6M432oBafv (private to the owner).

**These are design assets, not app code.** Nothing here is wired into an app. The project is still spike-first and contract-gated (see `docs/`); building screens waits for the gates.

## Rules the art follows

1. **Pixels in the frame, never in the numbers.** Prices, times and warnings are set in Inter with tabular figures. Pixel art and Pixelify Sans are for icons, illustration and headings 20 px and up.
2. **Every number has a source and an age.** Each price shows its fetch time and a source badge (Blizzard API, ForeverDB or Collected). Prices are hourly snapshots; nothing is called real-time.
3. **Absence looks like absence.** Missing prices, unnamed items (icon plus item ID) and history gaps each have their own state. Nothing is guessed, zeroed or smoothed.
4. **One palette.** Every pixel uses the 28-colour master palette in `palette/`. Every exported image was checked: no off-palette colours (the white action and state masks are tinted at runtime).
5. **Fixed grids, whole pixels.** Icons are 16 × 16 or 24 × 24, illustrations 64 × 48, all with a 1-pixel ink-950 outline. One art pixel per screen pixel per density step; never mix densities in one image.

## Folders

| Folder | What | Files |
|---|---|---|
| `palette/` | `profeshy.gpl` (GIMP/Aseprite) and `palette.json`, the 28 colours | 2 |
| `tokens/` | `tokens.json`: dark and light UI tokens mapped to palette names; `palette_colors_reference.xml` (reference only) | 2 |
| `app-icon/` | Adaptive icon foreground and monochrome layers (108 dp, coin inside the 66 dp safe zone) per density; background is solid blue-800; `play_store_512.png`; notification icon | 13 entries |
| `professions/` | All 12 profession tools, 16 × 16 | 12 assets |
| `coins/` | Gold, silver, copper, 16 × 16 | 3 assets |
| `rarity/` | Six quality tiers, each its own shape | 6 assets |
| `kade-icons/` | Kade's own icons (v2–v4) as drawn: tab icons 24 × 24, small icons 16 × 16 | 17 assets |
| `items-placeholder/` | Stand-in item art for mockups only; real item icons come from the game's API at runtime | 4 assets |
| `action-masks/` | One-colour action icons (back, close, filter…), white masks tinted with the text colour | 12 assets |
| `state-masks/` | Fresh, stale, missing, estimate markers, white masks | 4 assets |
| `illustrations/` | 12 scenes: 4 onboarding, empty states, offline, error, history starts today, celebration, credits | 12 assets |

Each asset folder holds an `.svg` (one unit per art pixel, `shape-rendering="crispEdges"`) and a PNG per Android density: `mdpi` 1×, `hdpi` 1.5×, `xhdpi` 2×, `xxhdpi` 3×, `xxxhdpi` 4×, all nearest-neighbour.

**About hdpi:** 1.5× can't split a pixel evenly, so hdpi PNGs alternate 1- and 2-pixel columns. They are still sharper than letting Android shrink the xhdpi file (which blurs). The app icon is the exception: at 108 dp its coin is 2 dp per art pixel, so hdpi lands on a clean 3 px.

## Type

- **Pixelify Sans** (SIL OFL): wordmark and headings 20 px and up.
- **Inter** (SIL OFL), tabular figures: every price, label and body line.

## Colour contrast (WCAG 2.x, measured from `tokens.json`)

Every text pair passes 4.5:1. Up and down also differ in lightness and carry ▲ ▼, so they never rely on hue alone.

| Text token | on | Dark | Light |
|---|---|---|---|
| t1 | bg | 14.9:1 | 14.9:1 |
| t1 | s1 | 11.0:1 | 17.2:1 |
| t2 | s1 | 8.2:1 | 12.7:1 |
| t3 | bg | 7.0:1 | 5.8:1 |
| t3 | s1 | 5.2:1 | 6.7:1 |
| gold | bg | 11.9:1 | 7.9:1 |
| link | s1 | 8.8:1 | 6.7:1 |
| up | s1 | 7.8:1 | 5.8:1 |
| down | s1 | 5.3:1 | 8.1:1 |
| stale | s1 | 4.8:1 | 9.1:1 |
| est | s1 | 5.9:1 | 9.2:1 |
| miss | s1 | 5.2:1 | 6.7:1 |
| onfill | fill | 11.9:1 | 11.9:1 |
| uc | s1 | 4.8:1 | 8.1:1 |

## Credits and notices

Item names from ForeverDB.net (credit link required in the app). Profeshy is an independent fan project, not affiliated with or endorsed by Blizzard Entertainment. World of Warcraft is a trademark of Blizzard Entertainment.

Store graphics (Play feature graphic 1024 × 500, README banner 1280 × 320, social preview 1280 × 640) and Play screenshots are exported from the canvas, not stored here yet.
