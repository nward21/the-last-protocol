# Architecture — The Last Protocol

## Stack
- **Build step:** none. Vanilla static HTML.
- **Dependencies:** zero. No `package.json`, no `node_modules`, no bundler.
- **Runtime:** browser only. Modern evergreen browsers; uses `aspect-ratio`, CSS custom properties, `prefers-reduced-motion`.

## File Layout
```
The Last Protocol/
├── index.html          ← the entire site (~2,445 lines, ~103KB)
├── assets/
│   └── singularity-hero.jpg  (NOT YET PRESENT — onerror auto-removes <img>)
├── claude.md           ← session context + handoff
├── architecture.md     ← this file
├── styleguide.md       ← design tokens, components
├── .gitignore          ← .DS_Store .netlify/ .claude/ node_modules *.log
├── .git/
└── .netlify/state.json ← siteId only; gitignored
```

## Build & Deploy

### GitHub Pages (active)
- Source: `main` branch, root path
- URL: https://nward21.github.io/the-last-protocol/
- Trigger: any push to `main`
- Latency: ~30–60s after push
- Enabled via `gh api repos/nward21/the-last-protocol/pages -X POST` with `{"source":{"branch":"main","path":"/"}}`

### Netlify (provisioned, deploy gated)
- Account: `nickward21` slug, "Nick's Netlify" team
- Site: `the-last-protocol`
- Site ID: `01f3218c-21b8-4ae7-be60-80287cb8cd98`
- Status: site exists, deploys blocked by *"Account credit usage exceeded"*
- Resume command: `netlify deploy --prod --site 01f3218c-21b8-4ae7-be60-80287cb8cd98 --dir .` after billing fix

## Auth
- `gh` CLI authenticated as `nward21` with full `repo` scope (token in macOS keyring)
- `netlify` CLI authenticated as `NETLIFY_USER_EMAIL_ENV` — token at `~/Library/Preferences/netlify/config.json` (`users.<id>.auth.token` field, `nfc_*` format). **Never commit this file. Never echo this token to logs or context files.**

## DOM Hierarchy
```
<body>
├── .twinkle-field          (10 absolute-positioned animated stars)
└── .wrap                   (z-index: 1, max-width: 1080px, padding: 56–120px)
    ├── header              (eyebrow / H1 / subtitle / meta)
    ├── .hero-banner        (figure: SVG fallback + <img> + caption)
    ├── .prologue           (Singularity thesis)
    ├── .hero-stat          (2.8× scarcity delta callout)
    ├── 8 × .section        (Roman I–VIII, each with .sec-head icon chip)
    ├── .closing            (node vs spectator + radial decorations)
    └── footer
```

`body::before` and `body::after` are two animated starfield pseudo-element layers. The `<body>` background-image is a fixed Tron grid (56px) plus radial accretion glows.

## Color Coding Axes (semantic)
Three axes, CSS-variable-driven. The orange/green axis is the spine of the narrative.

| Axis | Token | Color | Meaning |
|---|---|---|---|
| Value | `--c-value` | `#ff6b1a` | Bitcoin / new / open / winning side |
| Fiat | `--c-closed` | `#00ff9d` | USD / old / closed / being absorbed |
| Information | `--c-info` | `#00d4ff` | TCP/IP comparison axis (parallel info-protocol) |
| (alias) | `--c-open` | `#ff6b1a` | Same as `--c-value`. Retained for back-compat after color swap. |

**Per-network timeline tints** — decorative, NOT semantic. Each network gets a unique color for differentiation only:

| Network (year) | Tint | Note |
|---|---|---|
| Telephone (1876) | `#ff6090` | coral |
| Electricity (1882) | `#fbbf24` | amber (also reserved for Energy Anchor lightning) |
| WWW (1991) | `#00d4ff` | cyan — happens to match Information axis |
| VoIP (1995) | `#a855f7` | purple |
| Bitcoin (2009) | `#ff6b1a` | orange — matches Value axis |

## Inline SVG Inventory (~30 fragments)
- Hero banner fallback: animated vortex + ₿ + 3 money streams + dollar-bill rectangles + accretion particles
- Section II topology: closed hub-and-spoke (green) + open P2P mesh (orange)
- Section II Standard Gauge: fragmented gauges (green) above, unified standard gauge (orange) below, train glyph
- Section IV Black Box clock + Glass Clock (with 21M stamp)
- Section VI Energy Anchor: single amber lightning bolt in circle → cyan block with hash lines → orange shield with checkmark
- Section VII × 4 conceptual charts: trust curve, friction-vs-innovation, network effect (n²), fat protocol
- Section header icons × 8 + timeline icons × 5 + unbreakable icons × 3 + node/spectator × 2

All SVG inline (no external references, no sprite, no icon font).

## Animations (all respect `prefers-reduced-motion: reduce`)
| Keyframe | Element | Duration | Notes |
|---|---|---|---|
| `star-shimmer-a` | `body::before` | 7s alternate | Layer-A star opacity + tiny translate |
| `star-shimmer-b` | `body::after` | 11s alternate | Layer-B opposing-phase shimmer |
| `twinkle` | `.twinkle` × 10 | 3.5s ease-in-out | Staggered delays 0–3.0s |
| `hero-spin` | `.vortex-spin` | 40s linear | Outermost accretion ellipse |
| `hero-flow` | `.money-flow` × 3 | 6 / 9 / 12s | `stroke-dashoffset` animation, last reverses |
| `hero-pulse` | `.b-symbol` | 3s ease-in-out | ₿ opacity 0.85 ↔ 1 |
| Tooltip reveal | `.tip::after` | 180ms | Opacity + translate on hover/focus |
| Hover lift | `.ub-card`, `.featured-article` | 150ms | translateY(-2px) + border tint |

## Tooltip System
Pattern: `<span class="tip" tabindex="0" data-tip="definition...">term</span>`
- Pure CSS, **no JS**
- Hover OR focus reveals `::after` card with `::before` arrow
- Dotted orange underline as affordance
- Max 260px wide, auto-positioned above the term
- `.tip-right` and `.tip-below` modifiers exist for edge-case positioning (not currently applied)
- `tabindex="0"` makes terms keyboard-focusable for accessibility

## API / External Calls
None. Page is fully static — no fetch, no analytics, no third-party scripts.

## Image Loading Strategy
The hero banner uses a **fallback-first** pattern:
1. SVG composition is rendered at `z-index: 1` (always present)
2. `<img>` is loaded at `z-index: 2` (covers SVG when present)
3. If the JPG 404s, `onerror="this.remove()"` removes the `<img>` and the SVG shows through
4. Result: the banner is never blank
