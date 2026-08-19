# Lune — Menu Screen Style Guide

Extracted from `lune-prototype.html` (`#screen-menu` and the shared `:root`/font
declarations it inherits). Line numbers reference `lune-prototype.html` at the
time of writing.

---

## 1. Color Palette

### CSS variables (`:root`, lines 11–22)

| Variable | Value | Usage |
|---|---|---|
| `--green-dark` | `#0a0a0a` | Primary near-black — wordmark, badges, filled CTAs |
| `--green-mid` | `#0a0a0a` | Icon color (hamburger, search, filter icons) |
| `--green-line` | `#0a0a0a` | Line/border accent |
| `--green-pale` | `#efefef` | Pale surface tint |
| `--gold` | `#0a0a0a` | Legacy accent slot |
| `--cream` | `#f2f2f2` | Light surface tint |
| `--sheet-radius` | `24px` | Bottom-sheet corner radius |
| `--rewards-bg` | `#EFEFEF` | Rewards/earn card background |
| `--rewards-text` | `var(--green-dark)` | Rewards/earn card text |

> Note: despite the semantic `green-*`/`gold` naming (a holdover from an earlier
> brand pass), every one of these currently resolves to `#0a0a0a`. The whole
> prototype today is a **monochrome black / grey / white** system.

### Hardcoded colors on the menu screen

**Text**
| Hex | Used for |
|---|---|
| `#111` | Primary text — product names, prices, selected category tab, section titles, search input |
| `#555` | Secondary text/icons — browser-bar URL text, search icon, clear button |
| `#888` | Product subtitle/meta (`.meal-sub`) |
| `#999` | Unselected category tab, calorie count (`.meal-cal`) |
| `#8a8a8a` | Search placeholder text |

**Surfaces**
| Hex | Used for |
|---|---|
| `#fff` | Screen background, filter pill, search bar background |
| `#eee` | Browser-bar URL pill background |

**Borders**
| Hex | Used for |
|---|---|
| `#ddd` | Filter pill border, search-bar-wrapper border, toolbar divider |
| `#eee` | Product row bottom border |
| `#f0f0f0` | Menu body top border |
| `#e5e5e5` | Search input bottom border |

**Badge / accent**
| Hex | Used for |
|---|---|
| `#0a0a0a` bg / `#fff` text | "Best seller" tag badge, generic meal badge |

**Placeholder art**
- `.meal-thumb` uses a warm tan `repeating-conic-gradient` (`#e8d4b8` / `#dcc4a0`) as a placeholder pattern behind product photography — the only non-grayscale color touching the menu screen.

---

## 2. Typography

**Font import**
```html
<link href="https://fonts.googleapis.com/css2?family=Archivo+Black&family=Inter:wght@400;500;600;700;800;900&display=swap" rel="stylesheet">
```

- **Display font**: `'Archivo Black', Arial, sans-serif` (`--display-font`) — used for `h1/h2/h3` and major headings elsewhere in the app (cart, checkout, sheets). **Not used on the menu screen itself** — the menu screen's headings use Inter.
- **Body font**: `'Inter', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Arial, sans-serif` — applied globally on `html, body`; every menu-screen text style below inherits this.

### Text styles

| Element | Size | Weight | Letter-spacing | Case | Color |
|---|---|---|---|---|---|
| `.lune-wordmark` (header logo) | 22px | 900 | 3px | — | `--green-dark` |
| `.filter-pill` | 12px | 700 | .06em | uppercase | `--green-mid` |
| `.cat-tab` | 13px | 700 | .04em | uppercase | `#999` (`#111` selected) |
| `.menu-section-title` | 13px | 700 | .12em | uppercase | `#111` |
| `.meal-name` | 13px | 700 | .06em | uppercase | `#111` |
| `.meal-sub` | 13px | 400 | — | — | `#888` (line-height 1.5) |
| `.meal-price` | 13px | 600 | — | — | `#111` |
| `.meal-cal` | 13px | 400 | — | — | `#999` |
| `.search-input-pill input` | 16px | — | — | — | `#111` |
| `.search-input-pill input::placeholder` | 16px | — | — | — | `#8a8a8a` |
| `.browser-bar .url-pill` | 13px | — | — | — | `#555` |
| `.tag-badge` ("Best seller") | 10px | 700 | .02em | — | `#fff` on `#0a0a0a` |
| `.meal-badge` | 9px | 700 | .06em | uppercase | `#fff` on `#0a0a0a` |

**Pattern**: labels and metadata (tabs, pills, section titles, product names) are
consistently **uppercase, bold, letter-spaced** — this is the app's dominant
typographic voice. Prices and body copy stay sentence-case and lighter weight.

---

## 3. Thumbnail / Image Style

Two distinct thumbnail conventions coexist in the app:

### A. Menu list thumbnail — `.meal-thumb` (main product list, on-screen)
```css
width: 104px; height: 104px;
border-radius: 0;               /* square corners */
background: repeating-conic-gradient(from 0deg, #e8d4b8 0deg 10deg, #dcc4a0 10deg 20deg);
overflow: hidden;
```
- Fixed **104×104px** square, `object-fit: cover` on the `<img>`.
- **No border-radius** — sharp square corners (deliberate, contrasts with the pill-heavy rest of the UI).
- Optional overlay badge (`.meal-badge`) pins flush to the top-left corner, also square (`border-radius: 0`).
- No shadow.

### B. Carousel/card thumbnail — `.extra-thumb` (cart & meal-detail upsell carousels)
```css
width: 128px; aspect-ratio: 1/1;
border-radius: 14px;            /* rounded corners */
background: #f1ede4;
overflow: hidden;
```
- **128px** card, square via `aspect-ratio: 1/1`, `object-fit: cover`.
- **14px rounded corners** — softer treatment than the menu list.
- Optional overlay badge (`.tag-badge`) is a floating pill (`border-radius: 999px`) inset 6px from the top-left, with a soft shadow (`0 1px 4px rgba(0,0,0,.25)`).

> Take-away: sharp squares = dense list contexts (menu browsing); rounded
> squares = card/carousel contexts (upsells, cart). Both crop with `cover`.

---

## 4. Component Library

### Filter pill — `.filter-pill`
Pickup-time / order-type selector. `border: 1px solid #ddd`, `background: #fff`,
`padding: 10px 14px`, `border-radius: 0`, uppercase 12px/700 text. No defined
hover/active state.

### Category tab — `.cat-tab`
Horizontal scrollable tab list. Underline-style selection (not a filled pill):
`border-bottom: 2px solid transparent` → `#111` when `.selected`, text color
`#999` → `#111`. `padding: 10px 4px; margin-right: 14px`.

### Product row — `.meal-card`
CSS grid: `grid-template-columns: 104px 1fr`, thumbnail spans 3 rows
(`grid-row: 1 / 4`) alongside name / subtitle / price stacked in the second
column. `padding: 18px 0`, `border-bottom: 1px solid #eee` (first row also
gets a `border-top`). States: `:hover { opacity: .7 }`, `:active { opacity: .5 }`.

### Section title — `.menu-section-title`
`margin: 22px 0 6px`, 13px/700 uppercase, `letter-spacing: .12em`, `#111`.

### Search bar — `.search-input-pill`
Not a pill despite the name — flat, `border-radius: 0`, bottom-border only
(`1px solid #e5e5e5`). Icon (`#555`) + borderless `<input>` (16px, `#111`,
placeholder `#8a8a8a`) + a `.search-clear-btn` (×) that only appears once text
is typed, via `input:not(:placeholder-shown) ~ .search-clear-btn`. The whole
search UI swaps in via a `.screen-menu.search-active` state, hiding the
header/filters/toolbar and revealing `.search-bar-row`.

### Icon buttons — `.icon-round-btn`, `.hamburger-btn`
Borderless, `36×36px` (icon-round-btn) or auto-sized (hamburger), transparent
background, `color: var(--green-mid)`. No hover/active state defined.

### Divider — `.toolbar-divider`
`1px × 22px` vertical rule, `background: #ddd`, separates icon buttons from
category tabs in the toolbar row.

### Badge / tag chip — `.tag-badge`, `.meal-badge`
Reusable "Best seller" / "Popular" overlay label. Two variants:
- `.tag-badge`: pill (`999px`), inset 6px, drop shadow — used on rounded
  carousel thumbnails.
- `.meal-badge`: square (`0`), flush top-left, no shadow — used on the
  square menu-list thumbnails.

---

## 5. Spacing & Radius Conventions

**Border-radius scale**
| Radius | Where |
|---|---|
| `0` | Menu screen's own controls: filter pill, cat tab, search bar, meal thumb/badge — a deliberate "sharp" treatment local to this screen |
| `999px` (pill) | Everywhere else: URL pill, tag badge, primary CTAs, apply buttons |
| `14px` | Card/carousel thumbnails (`.extra-thumb`) |
| `10px` | Filter chips (sort & filter screen) |
| `4–8px` | Small controls (checkboxes, slider handles) |

**Padding/margin**
- **20px** horizontal screen padding is the constant across the menu header, filter row, toolbar, and body.
- Pills/buttons: `10px 14px` (filter pill), `10px 4px` (cat tab).
- Product rows: `padding: 18px 0`, `column-gap: 16px` between thumbnail and text.
- Section headings: `margin: 22px 0 6px` (generous top gap, tight bottom gap).
- Small inline gaps: `6px` (pill icon/text), `10–12px` (toolbar/search rows).

---

## 6. Design Principles Observed

1. **Monochrome-first**: color does almost no signaling work — everything is
   black, white, or grey. The only warm color is the placeholder art behind
   product photos.
2. **Uppercase + letter-spacing = "label" voice**: any UI chrome (tabs, pills,
   section titles, product names, badges) is uppercase and letter-spaced;
   sentence-case is reserved for prices, subtitles, and free text.
3. **Two radius languages**: sharp squares for dense browsing (menu list),
   soft rounded pills/cards for promotional/carousel contexts (upsells,
   badges, CTAs). Radius communicates context, not just aesthetics.
4. **Interaction feedback via opacity**, not color shifts — `:hover`/`:active`
   states on rows dim rather than recolor.
