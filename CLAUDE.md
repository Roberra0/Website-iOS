# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a personal portfolio site — a collection of standalone HTML files. No build system, no framework, no package.json. Files are opened directly in a browser or served statically.

- `index.html` — interactive "dynamic island" card (the main entry point). Uses a state machine (`start → home → about/links`) with animated transitions driven by vanilla JS.
- `cv.html` — résumé/CV page with Google Analytics 4 active-time tracking.
- `card.html`, `call-card.html` — alternate card layouts.
- `photos.html` — photo gallery.

## Technical Patterns

- **Tailwind CSS via CDN** with an inline `tailwind.config` block for custom tokens (colors, spacing, border-radius, transition durations).
- **Custom CSS** for pseudo-element icons (SVG data URIs embedded in `before:bg-*` utility classes), media-query breakpoints not covered by Tailwind, and selection colors.
- **Lottie animations** via `@dotlottie/player-component` + `@lottiefiles/lottie-interactivity` loaded from jsDelivr CDN.
- **No external CSS files** — all styles are inline in each HTML file.

## Viewing / Testing

Open any file directly in a browser:
```
open index.html
```

Or serve locally (avoids CORS issues with fonts/assets):
```
npx serve .
```

## Website Design Recreation Workflow

When the user provides a reference image (screenshot) and optionally some CSS classes or style notes:

1. **Generate** a single `index.html` file using Tailwind CSS (via CDN). Include all content inline — no external files unless requested.
2. **Screenshot** the rendered page using Puppeteer (`npx puppeteer screenshot index.html --fullpage` or equivalent). If the page has distinct sections, capture those individually too.
3. **Compare** your screenshot against the reference image. Check for mismatches in:
   - Spacing and padding (measure in px)
   - Font sizes, weights, and line heights
   - Colors (exact hex values)
   - Alignment and positioning
   - Border radii, shadows, and effects
   - Responsive behavior
   - Image/icon sizing and placement
4. **Fix** every mismatch found. Edit the HTML/Tailwind code.
5. **Re-screenshot** and compare again.
6. **Repeat** steps 3–5 until the result is within ~2–3px of the reference everywhere.

Do NOT stop after one pass. Always do at least 2 comparison rounds. Only stop when the user says so or when no visible differences remain.

## Technical Defaults

- Use Tailwind CSS via CDN (`<script src="https://cdn.tailwindcss.com"></script>`)
- Use placeholder images from `https://placehold.co/` when source images aren't provided
- Mobile-first responsive design
- Single `index.html` file unless the user requests otherwise

## Rules
- Do not add features, sections, or content not present in the reference image
- Match the reference exactly — do not "improve" the design
- If the user provides CSS classes or style tokens, use them verbatim
- Keep code clean but don't over-abstract — inline Tailwind classes are fine
- When comparing screenshots, be specific about what's wrong (e.g., "heading is 32px but reference shows ~24px", "gap between cards is 16px but should be 24px")
