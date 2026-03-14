# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Personal portfolio site — standalone HTML files, no build system, no framework, no package.json.

**Current files at root:**
- `index.html` — iOS-style incoming call card animation (main entry point)
- `cv.html` — résumé/CV page with Google Analytics 4 active-time tracking
- `photos.html` — photo gallery, iOS Photos app aesthetic (dark, `#000` bg)

**Reference materials (`_reference/`):**
- `index.html` — reference design for a "dynamic island" card (Tailwind-based)
- `notes.md` — Claude Code project notes
- `resume.txt`, PDF — CV content source
- `icons/`, `books/` — asset sources

**Assets at root:**
- `icons/` — SVG and PNG icons used by pages
- `photos/` — photo assets for `photos.html`
- `headshot.png`, `icons/memoji.png` — profile images
- `screenshots/` — reference screenshots for design comparison

## Technical Patterns

### Existing pages (`index.html`, `cv.html`, `photos.html`)
- Pure vanilla CSS — no Tailwind. Custom CSS properties (CSS vars) for theming.
- Font: Inter loaded from `https://kons.fyi/fonts/inter.woff2`
- GA4 tracking on all pages (`G-BCCGP9K8G5`). `cv.html` and `photos.html` also fire a `time_on_page` event with active milliseconds on unload.
- All styles inline in each HTML file — no external CSS.

### `index.html` states
- **Closed** (`screenshots/closed state.png`) — pill-shaped card (370px wide) with memoji left, name + email center, green call button right.
- **Blur** (`screenshots/blur state.png`) — mid-transition frame: memoji and call button cross horizontally with a directional (horizontal-only) motion blur applied via an SVG `feGaussianBlur` filter. Card stretches during travel.
- **Open** (`screenshots/open_state.png`) — expanded card (595px wide) with back arrow left, app icon row center, memoji flipped right. Speech bubble appears above memoji.
- **Message** (`screenshots/imessage.png`, `screenshots/open_state_message_expanded.png`) — iMessage overlay fades in over the card; conversation with send phases.

### Reference design (`_reference/index.html`)
- **Tailwind CSS via CDN** with an inline `tailwind.config` block for custom tokens (colors, spacing, border-radius, transition durations)
- Custom CSS pseudo-element icons using SVG data URIs in `before:bg-*` utility classes
- State machine: `start → home → about/links` with animated transitions (vanilla JS)
- Lottie animations via `@dotlottie/player-component` + `@lottiefiles/lottie-interactivity` from jsDelivr CDN
- Assets reference `https://kons.fyi/` (font, background image)

## Viewing / Testing

```
open cv.html          # open directly
npx serve .           # serve locally (avoids CORS for fonts/assets)
```

## Website Design Recreation Workflow

When building/iterating on a page from a reference image or `_reference/` file:

1. **Generate** HTML inline — all styles in `<style>`, no external CSS files.
2. **Screenshot** with Puppeteer (`npx puppeteer screenshot <file> --fullpage`).
3. **Compare** against the reference. Check: spacing/padding (px), font sizes/weights, colors (exact hex), alignment, border radii, shadows, image sizing.
4. **Fix** every mismatch found.
5. **Re-screenshot** and repeat until no visible differences remain (target: within ~2–3px).

Always do at least 2 comparison rounds before stopping.

## Rules
- Match the reference exactly — do not add features or "improve" the design
- If the user provides CSS classes or style tokens, use them verbatim
- When comparing screenshots, be specific (e.g. "heading is 32px but reference shows ~24px")
- Keep all styles inline in each HTML file
