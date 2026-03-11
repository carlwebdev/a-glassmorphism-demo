# CLAUDE.md — Glassmorphism UI Demo

This file provides context for AI assistants working in this repository.

## Project overview

A zero-dependency, static HTML/CSS demonstration of glassmorphism UI principles. The project showcases frosted-glass card effects, animated gradient backgrounds, and responsive layout—all using only vanilla HTML5 and CSS3 with no build step.

**Live entry point:** `index.html` (open directly in a browser or serve with any static server)

## Repository structure

```
a-glassmorphism-demo/
├── index.html            # Single-page HTML — the entire UI lives here
├── assets/
│   └── css/
│       └── styles.css    # All styling (~906 lines); zero JS
├── README.md             # Project description, references, expansion ideas
├── hello.txt             # Empty placeholder file
└── .gitattributes        # LF line-ending normalization
```

There is no `package.json`, no build tool, no JavaScript, and no test suite. Any change to `index.html` or `styles.css` is instantly visible in the browser.

## Development workflow

```bash
# Serve locally (pick any option)
python -m http.server 8000
npx http-server .
php -S localhost:8000

# Or just open the file directly
open index.html          # macOS
xdg-open index.html      # Linux
```

No compilation, no `npm install`, no environment variables.

## Tech stack

| Layer | Technology |
|---|---|
| Markup | HTML5 (semantic elements, popover API, `<dialog>`) |
| Styling | CSS3 (custom properties, Grid, Flexbox, `backdrop-filter`) |
| Typography | Google Fonts — Poppins (300, 400, 600) |
| Avatars | DiceBear API (SVG identicons, no account required) |
| JavaScript | None — mobile menu uses the native HTML `popover` attribute |

## CSS architecture

### Design tokens (`styles.css` `:root`)

All values are tokenized as CSS custom properties. Never use hard-coded magic numbers; always reference or extend the existing tokens.

**Token categories:**

| Prefix | Purpose | Examples |
|---|---|---|
| `--font-family-*` | Font stacks | `--font-family-base`, `--font-family-mono` |
| `--font-size-*` | Type scale | `--font-size-xs` → `--font-size-3xl` |
| `--font-weight-*` | Weight aliases | `--font-weight-regular` (400), `--font-weight-semibold` (600) |
| `--space-*` | Spacing scale | `--space-3xs` (0.25rem) → `--space-8xl` (3.5rem) |
| `--color-white-*` | White opacity variants | `--color-white-10` → `--color-white-95` |
| `--color-shadow-*` | Dark shadow colors | `--color-shadow-25` → `--color-shadow-40` |
| `--gradient-*` | Brand gradient stops | `--gradient-rose`, `--gradient-amber`, `--gradient-indigo` |
| `--radius-*` | Border radii | `--radius-sm` (20px), `--radius-pill` (999px) |
| `--surface-blur-*` | Backdrop filter values | `--surface-blur-strong`, `--surface-blur-soft` |

### Glassmorphism effect recipe

Every glass surface in the project uses this combination:

```css
background: var(--glass-bg);                  /* rgba(255,255,255,0.15) */
border: 1px solid var(--glass-border);        /* rgba(255,255,255,0.35) */
backdrop-filter: var(--surface-blur-strong);  /* saturate(180%) blur(24px) */
-webkit-backdrop-filter: var(--surface-blur-strong);
```

Both the `-webkit-` prefixed and unprefixed `backdrop-filter` must always be set together for Safari compatibility.

### Naming convention — BEM

Classes follow **Block__Element--Modifier** (BEM):

```
.site-header                  ← block
.site-header__brand           ← element
.site-header__nav             ← element
.button                       ← block
.button--primary              ← modifier
.button--ghost                ← modifier
.testimonial-card             ← block
.testimonial-card__header     ← element
.testimonial-card__quote      ← element
```

Stick to this convention when adding new components.

### Responsive breakpoint

One breakpoint at **720px**:
- Below 720px: desktop nav hidden, hamburger button visible, popover mobile menu active
- Grid columns collapse from multi-column to single-column

```css
@media (max-width: 720px) { … }
```

### Motion & accessibility

- Base transition: `var(--transition-base)` → `220ms ease`
- Blob animation: 12-second float loop with staggered `animation-delay`
- **Always** wrap animations in a `@media (prefers-reduced-motion: reduce)` guard that disables or simplifies them

## HTML conventions

- One `<h1>` per page (the hero heading)
- Sections use descending heading levels (`<h2>`, `<h3>`) — never skip ranks
- All interactive elements carry relevant ARIA attributes (`aria-label`, `aria-controls`, `aria-haspopup`)
- Images use `loading="lazy"` and descriptive `alt` text
- External links use `target="_blank" rel="noreferrer"`
- Section anchor IDs match nav `href` values: `#overview`, `#components`, `#testimonials`, `#principles`, `#styleguide`

## Page sections (index.html)

| Section | ID | Class | Purpose |
|---|---|---|---|
| Header | — | `.site-header` | Fixed nav with desktop links + mobile popover |
| Hero | `#overview` | `.hero` | Intro copy, CTA buttons |
| Cards | `#components` | `.cards` | Three `<article class="glass-card">` elements |
| Testimonials | `#testimonials` | `.testimonials` | DiceBear avatars + quote cards |
| Principles | `#principles` | `.principles` | Bulleted key-ingredient list |
| Styleguide | `#styleguide` | `.styleguide` | Four HTML tag reference cards |
| Footer | — | `.site-footer` | Copyright + reference links |

## Adding new components

1. Add the HTML inside `<main class="layout">` in `index.html`, following existing semantic patterns.
2. Add a matching nav anchor link in both `.site-header__nav` and `.site-header__menu` (mobile popover).
3. Write styles in `styles.css`. Use existing tokens; add new tokens to `:root` if a new value is introduced.
4. Apply the glassmorphism recipe (background + border + backdrop-filter) to any new glass surface.
5. Add a `@media (max-width: 720px)` rule for mobile layout if needed.
6. Guard any CSS animations with `prefers-reduced-motion`.

## External dependencies

| Service | Usage | Notes |
|---|---|---|
| Google Fonts | Poppins font via `<link>` in `<head>` | Requires internet access to render correctly |
| DiceBear API v7 | Identicon SVG avatars in testimonials | URL pattern: `https://api.dicebear.com/7.x/identicon/svg?seed=<Name>` |

No API keys or accounts required for either service.

## Git workflow

- **Development branch:** `claude/add-claude-documentation-Hx2o8`
- **Default remote branch:** `origin/main`
- Commits are small and descriptive (see history for style reference)
- Push with: `git push -u origin <branch-name>`

## What this project is NOT

- Not a Node/npm project — do not create `package.json` or install packages
- Not a React/Vue/Svelte app — do not introduce component frameworks
- Not a JavaScript app — do not add JS unless the feature is impossible in pure HTML/CSS
- Not tested with a test runner — visual review in-browser is the validation method
