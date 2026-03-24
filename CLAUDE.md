# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

Reveal.js 6.0.0 presentation decks about Taiwan submarine cable resilience (台灣海纜韌性), in zh-TW. Static HTML — no build step.

## Serving

```bash
npx serve          # serves at localhost:3000
```

No build scripts. `index.html` is the shell; slide content lives in external Markdown files under `slides/`, loaded via `data-markdown` at runtime.

## Reveal.js v6 Path Conventions

Plugin JS files are **flat** under `dist/plugin/`:
```
node_modules/reveal.js/dist/plugin/highlight.js   ✓
node_modules/reveal.js/dist/plugin/highlight/highlight.js   ✗ (does not exist)
```

Plugin CSS files are **nested** in subdirectories:
```
node_modules/reveal.js/dist/plugin/highlight/monokai.css   ✓
```

Available plugins: highlight, markdown, math, notes, search, zoom.
Available themes: beige, black, black-contrast, blood, dracula, league, moon, night, serif, simple, sky, solarized, white, white-contrast.

## Slide Authoring

Slides are written in external Markdown files under `slides/` and loaded via `data-markdown`:
```html
<section data-markdown="slides/01-phase1.md"
         data-separator="^\n---\n$"
         data-separator-notes="^Note:">
</section>
```

- Each `.md` file contains multiple slides separated by `---` (blank lines above/below)
- Use `<!-- .element: class="fragment" -->` after an element for step-by-step reveals
- Speaker notes start with `Note:` at the end of each slide block
- Inline HTML is allowed within Markdown for complex layouts (flexbox, styled divs, etc.)
- All presentation content is in zh-TW
