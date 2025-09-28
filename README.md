# Webtools Documentation Frontend

A lightweight static documentation layout with a layered CSS architecture for easy maintenance and future scalability.

## Overview
This project separates foundational vendor styles from modern enhancements:
- `assets/vendor.css` – Minified vendor + legacy base styles (AOS animations, grid, utilities, component primitives). Do not hand-edit unless upgrading a library.
- `assets/app.css` – Modern enhancement layer (layout, design tokens, responsive sidebar, animations, dark mode (upcoming), accessibility improvements).
- `index.html` – Semantic structure with sidebar navigation, IntersectionObserver-driven active link highlighting, mobile sidebar toggle, and progressive enhancement script.

## Architecture
1. Vendor Layer (Stable)
   - Provides: resets, variables, grid, utilities, base components.
   - Loaded first so enhancement layer can override safely.
2. Enhancement Layer (Evolves)
   - Adds: design tokens, refined layout, gradients, dark-mode support, focus states, animations.
3. Behavior Layer (Inline Script)
   - Features: sidebar toggle, active section highlighting, current year injection (footer), (planned) theme persistence.

## Theming Strategy
Currently supports auto dark mode via `prefers-color-scheme`. A manual toggle is planned:
- Add a button in header (e.g. ☀ / 🌙) that toggles `data-theme="dark"` on `<html>` or a `theme-dark` class on `<body>`.
- Persist preference in `localStorage` (e.g. key: `theme-preference`).
- Enhancement layer (`app.css`) will contain `.theme-dark` overrides referencing existing tokens or adding new ones.

### Planned Tokens (example)
```
:root {
  --color-bg: #ffffff;
  --color-bg-alt: #f5f7f9;
  --color-text: #1f2429;
  --color-border: #e3e8ef;
  --color-accent: #38b2ac;
}
.theme-dark {
  --color-bg: #101316;
  --color-bg-alt: #181d21;
  --color-text: #e6ebef;
  --color-border: #2a3239;
  --color-accent: #4fd4ce;
}
```
Usage example: `background: var(--color-bg); color: var(--color-text);`

## Local Development
Because this is a static site, you can open `index.html` directly or serve it with a lightweight dev server for proper relative path handling.

### Quick Serve (PowerShell)
```
# Python 3
python -m http.server 8080
```
Navigate to: http://localhost:8080/

## Editing Guidelines
- Add new UI styles only to `app.css` (never modify `vendor.css` directly).
- If you upgrade a vendor library, replace the whole relevant section in `vendor.css` and re-test overrides.
- Keep components small, composable, and driven by CSS variables when possible.
- Prefer progressive enhancement: core content should remain accessible without JS.

## Accessibility
Implemented:
- Skip link.
- ARIA attributes for sidebar toggle.
- Proper heading hierarchy & `scroll-margin-top` for anchor offsets.
Planned:
- Higher contrast audit & keyboard navigation check.

## Performance Notes
- Single vendor bundle kept minified to reduce payload.
- Enhancement layer human-readable for iteration.
- Consider adding a build step (Prettier + cssnano + purge of unused utilities) if project grows.

## Potential Next Steps
- Manual dark mode toggle (in progress roadmap).
- Search component (client-side fuse.js or lunr.js integration).
- Active breadcrumb / table of contents generation.
- Component documentation page with code snippets + copy button.

## License
Include licenses for any third-party CSS/JS that originated in `vendor.css` (e.g., AOS, Bootstrap segments). Add them to a `LICENSES` directory if redistributed.

---
Feel free to request automation for a theme toggle or build pipeline; these tasks are straightforward to add next.
