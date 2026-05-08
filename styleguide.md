# Styleguide — The Last Protocol

## Color Tokens (CSS Custom Properties)

### Surfaces (deep space, dark mode)
| Token | Value | Usage |
|---|---|---|
| `--bg` | `#06060a` | page background |
| `--panel` | `#101015` | card / surface |
| `--panel-2` | `#16161c` | nested / secondary surface |
| `--panel-3` | `#1f1f27` | tier-3 surface (chips, tooltips) |
| `--border` | `#25252e` | 1px borders |
| `--border-strong` | `#34343d` | emphasized borders |
| `--text` | `#ededed` | primary text |
| `--text-soft` | `#c5c5cc` | secondary readable text |
| `--muted` | `#8a8a90` | de-emphasized labels |
| `--muted-2` | `#5a5a60` | very faint labels |

### Semantic Color Coding (3 axes — orange/green is the narrative spine)
| Token | Value | Meaning |
|---|---|---|
| `--c-value` | `#ff6b1a` | Bitcoin / winning / new |
| `--c-value-soft` | `#ff6b1a1f` | Bitcoin tint (~12% alpha) |
| `--c-value-glow` | `#ff6b1a66` | Bitcoin glow (~40% alpha) |
| `--c-closed` | `#00ff9d` | Fiat / losing / being absorbed |
| `--c-closed-soft` | `#00ff9d18` | Fiat tint |
| `--c-info` | `#00d4ff` | TCP/IP / Information protocol axis |
| `--c-info-soft` | `#00d4ff18` | Info tint |
| `--c-open` | `#ff6b1a` | Alias of `--c-value` (retained for back-compat) |

### Decorative tints (inline only — NOT in `:root`)
Reserved for the Section I timeline strip. Not for general use.
- Telephone tint: `#ff6090` (coral)
- Electricity tint: `#fbbf24` (amber — *also reserved for Energy Anchor lightning*)
- VoIP tint: `#a855f7` (purple)

## Typography
- **Sans:** `-apple-system, BlinkMacSystemFont, "Inter", "Segoe UI", sans-serif`
- **Mono:** `ui-monospace, SFMono-Regular, Menlo, monospace` (HUD captions, year markers, hex IDs, code)
- **Tabular numerals:** apply `font-variant-numeric: tabular-nums` on all stats and percentages

### Type Scale
| Element | Size | Weight | Letter-Spacing |
|---|---|---|---|
| H1 | 56px | 700 | -0.03em |
| Section H2 | 34px | 700 | -0.02em |
| Prologue H2 | 38px | 700 | -0.02em |
| Card / dialog H | 22–24px | 700 | -0.01em |
| Hero stat number | 120px | 700 | -0.06em |
| Smaller stat numbers | 32–44px | 700 | -0.02em |
| Body | 15–17px | 400–500 | normal |
| Pill / eyebrow | 11–12px UPPERCASE | 600–700 | 0.12–0.18em |

## Spacing Rhythm
- Section margin-bottom: **80px**
- Card internal padding: **24–32px**
- Tile/sub-card padding: **16–20px**
- Pill/badge padding: **3–12px**
- Grid gap (cards): **12–20px**
- Hero banner side bleed: **-28px** (full-width on desktop)

## Component Patterns

### Card
```css
background: var(--panel);
border: 1px solid var(--border);
border-radius: 14px;
padding: 24-32px;
```

### Pill / badge
```css
border-radius: 999px;
padding: 4px 10px;
font-size: 10–12px;
text-transform: uppercase;
letter-spacing: 0.12–0.18em;
font-weight: 700;
```

### Section header (icon chip + roman numeral)
```html
<div class="sec-head">
  <span class="sec-icon"><svg ...></svg></span>
  <div class="section-title">
    <span class="num-roman">II</span>Open Protocols Win
  </div>
</div>
```

### Tooltip
```html
<span class="tip" tabindex="0" data-tip="definition...">term</span>
```
Dotted orange underline + amber-glow card on hover/focus. Pure CSS, no JS.

### Hover lift
```css
transition: transform 0.15s ease, border-color 0.15s ease;
&:hover { transform: translateY(-2px); border-color: var(--border-strong); }
```
Applied to `.ub-card`, `.featured-article`, etc.

### Left-accent callout (closed/old)
```css
border-left: 3px solid var(--c-closed);
```

### Left-accent callout (Bitcoin/new)
```css
border-left: 3px solid var(--c-value);
```

## Iconography
- Stroke-based, `currentColor`, 1.5–2px stroke width
- 24×24 viewBox standard
- Inline SVG only (no icon font, no external sprite, no PNG/SVG file references)
- Section icons: 36×36 chip, 18×18 svg inside
- Timeline icons: 48×48 chip, 22×22 svg inside
- Unbreakable icons: 56×56 chip, 28×28 svg inside

## Visual Effects

### Tron grid + accretion (body bg)
- 56px grid lines at ~4.5% opacity orange
- Two radial glows: orange ellipse top, green ellipse bottom-right
- All `background-attachment: fixed`

### Animated starfield
- Two `body::before/after` star layers with offset shimmer cycles (7s + 11s)
- 10 absolute-positioned `.twinkle` dots pulsing on 3.5s with staggered delays
- 3 of the 10 are tinted (peach, blue) for variety
- All animation gated behind `@media (prefers-reduced-motion: reduce)` — disabled when set

### Hero accretion disk (SVG fallback for the banner)
- Multiple concentric ellipses
- Outer ring slow rotation (40s linear)
- ₿ symbol: pulsing opacity (3s)
- 3 green money streams: animated `stroke-dashoffset` for "flowing" effect

## Responsive Breakpoints
| Breakpoint | Behavior |
|---|---|
| `min-width: 580px` | `.nvs` grid: 1 → 2 col |
| `min-width: 720px` | most 2-col grids activate |
| `min-width: 760px` | timeline horizontal, stack 2-col |
| `min-width: 920px` | `.standards-list` 2 → 4 col |
| `max-width: 640px` | H1 → 40px, padding tightens, banner aspect → 2:1 |

## Naming Convention
Flat kebab-case classes. Block + element + modifier where useful.
- Block: `.tile`, `.card`, `.gauge`, `.tollbooth`, `.rail`, `.hero-banner`, `.prologue`
- Element: `.tile-thumb`, `.rail-stop`, `.gauge-svg`, `.section-h2`
- Modifier: `.rail-usd`, `.rail-btc`, `.tile.featured`, `.tl-btc`, `.compare-col.real`

## Design Decisions to Preserve
- **Three semantic colors only.** Decorative tints are reserved for the timeline strip. Don't introduce a fourth "good" or "bad" color elsewhere.
- **Orange/green is the dollar-vs-Bitcoin spine.** The image, the topology, the rails, the Glass Clock, the game theory — all use the same axis.
- **Cyan is reserved for the TCP/IP / Information axis** — a separate parallel argument, never mixed with the orange/green axis.
- **Tabular numerals everywhere money or stats appear.** Critical for credibility.
- **All animation respects `prefers-reduced-motion`.**
- **No icon fonts. No JS for tooltips. No build step. No third-party scripts.**
- **Single accent for "active":** orange. Amber `#fbbf24` is reserved for energy/lightning iconography only — never used as a UI accent.
- **Generous vertical rhythm (80px between sections).** The page is dense but never feels cramped.
