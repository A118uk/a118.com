# A118 UI Style Guide

A reusable design system for building pages that match the look, feel, and interaction patterns established across this project. This guide documents the visual language, CSS patterns, and component conventions so they can be applied consistently whether a page is a monitoring dashboard, a tool, or a marketing / content site.

---

## Visual Character

The design is a **dark glassmorphic interface** — restrained, data-forward, mission-control rather than consumer app. Key qualities:

- Very dark background (near-black) with a fixed parallax image visible through a semi-transparent overlay.
- Frosted-glass cards that float above the background.
- A five-colour "stripe" palette used consistently for hierarchy, state, and identity.
- Minimal chrome — no gradients on interactive elements, no rounded-pill buttons, minimal shadows except on modals.
- Monospace type for all data values (timestamps, counters, codes).
- **Montserrat** for marketing display type (the logo title block, hero headings); a neutral system stack for UI chrome and copy.
- Restrained motion — mostly opacity, background-colour, and small transform transitions.

---

## Colour Palette

The palette is derived from a five-stop gradient called "the stripe". Each colour has a defined semantic role and must not be used interchangeably.

| Name | Value | Semantic role |
|---|---|---|
| **Orange** | `rgb(238, 115, 0)` | Primary action, active state, accent badge |
| **Amber** | `rgb(247, 165, 27)` | Data values, live readouts, active chips, primary button hover |
| **Olive** | `rgb(146, 151, 76)` | Secondary actions, status indicators, open states |
| **Dark sage** | `rgb(100, 115, 97)` | Structural / neutral interactive (close buttons, borders, hamburger) |
| **Mauve** | `rgb(93, 85, 90)` | Muted / tertiary actions, footer text, disabled-adjacent |

### Surface and text tokens

```css
:root {
  --c-orange:     rgb(238, 115, 0);
  --c-amber:      rgb(247, 165, 27);
  --c-olive:      rgb(146, 151, 76);
  --c-sage:       rgb(100, 115, 97);
  --c-mauve:      rgb(93,  85,  90);

  --c-surface-0:  rgb(4, 4, 6);             /* image backgrounds */
  --c-surface-1:  rgb(18, 18, 20);          /* flat card back */
  --c-surface-2:  rgba(28, 28, 31, 0.72);   /* glass card */
  --c-surface-3:  rgba(22, 22, 24, 0.82);   /* dark overlay */

  --c-text-hi:    rgb(210, 210, 215);       /* headings */
  --c-text-mid:   rgb(190, 190, 195);       /* body */
  --c-text-low:   rgb(90, 90, 100);         /* subdued labels */
  --c-text-dim:   rgb(55, 55, 65);          /* keyboard hints, counters */
}
```

### Using the palette

- **Solid orange** — only for the highest-priority primary buttons, pill badges on lightboxes, the active-page underline dash before section headings.
- **Amber on hover** — the primary (orange) button shifts to amber on hover. Amber is also the canonical data colour.
- **Tinted variants** — all non-primary button backgrounds use a low-opacity tint of the relevant colour (e.g. `rgba(100, 115, 97, 0.18)` for dark-sage secondaries). The surface stays dark; the colour shows clearly on hover.
- **Amber for data** — any displayed value that changes at runtime (timestamps, counts, countdowns, metric values) uses amber in monospace.
- **Never pure white or pure black** — the darkest surface is `rgb(4, 4, 6)`; the lightest text is `rgb(210, 210, 215)`.

---

## Background & Page Structure

```css
/* Base body fill — visible if background image fails */
body { background-color: rgb(39, 39, 41); }

/* Fixed parallax image */
.bg {
  position: fixed;
  inset: 0;
  z-index: -2;
  background: transparent url('n67.webp') no-repeat center center;
  background-attachment: fixed;
  background-size: cover;
}

/* Dark overlay for legibility */
.overlay {
  position: relative;
  background: rgba(22, 22, 24, 0.82);
  min-height: 100vh;
  display: flex;
  flex-direction: column;
}
```

Every page has three layers: `body` fill → `.bg` image → `.overlay` darkness. Content sits inside `.overlay`. The fixed background attachment produces a subtle parallax as the page scrolls.

Background image file: `n67.webp`.

---

## The Stripe

The stripe is the five-colour band that sits at the heart of the brand. It appears in three different sizes, each with a specific purpose.

### Logo stripe — identity element

A wide band (15 × 75 px) that sits between the logo mark and the title block in every header, and above the copyright line in every footer. It may be oriented vertically (headers, most cards) or horizontally (footers).

```css
.stripe-logo {
  width: 15px;
  height: 75px;
  background-image: linear-gradient(to bottom,
    var(--c-orange) 20%,
    var(--c-amber)  20% 40%,
    var(--c-olive)  40% 60%,
    var(--c-sage)   60% 80%,
    var(--c-mauve)  80%);
  border-radius: 0.25rem;
  flex-shrink: 0;
}
.stripe-logo.horizontal {
  width: 75px;
  height: 15px;
  background-image: linear-gradient(to right,
    var(--c-orange) 20%,
    var(--c-amber)  20% 40%,
    var(--c-olive)  40% 60%,
    var(--c-sage)   60% 80%,
    var(--c-mauve)  80%);
}
```

### Card accent stripe — 4px on any edge

Used as a decorative accent on cards and content boxes. Can be placed on the left, right, top, or bottom edge.

```css
.stripe-v {
  width: 4px;
  border-radius: 2px;
  background-image: linear-gradient(to bottom,
    var(--c-orange) 20%,
    var(--c-amber)  20% 40%,
    var(--c-olive)  40% 60%,
    var(--c-sage)   60% 80%,
    var(--c-mauve)  80%);
  flex-shrink: 0;
  align-self: stretch;
}
.stripe-h {
  height: 4px;
  border-radius: 2px;
  background-image: linear-gradient(to right,
    var(--c-orange) 20%,
    var(--c-amber)  20% 40%,
    var(--c-olive)  40% 60%,
    var(--c-sage)   60% 80%,
    var(--c-mauve)  80%);
  flex-shrink: 0;
}
```

On the horizontal card-accent variant, **orange is at the left** end; on the vertical variant, **orange is at the top**.

### Bullet stripe — bullet for feature lists

See the [Feature Lists](#feature-lists--bullet-system) section below.

---

## Typography

Two font stacks, used for specific roles:

```css
/* Montserrat — display: logo title block, hero headings, nav, big marketing type */
--font-display: 'Montserrat', -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Helvetica Neue', sans-serif;

/* System — UI chrome, body copy, buttons, small labels */
--font-ui: -apple-system, BlinkMacSystemFont, 'Segoe UI', 'Helvetica Neue', sans-serif;

/* Monospace — data values, timestamps, counters, code */
--font-mono: 'SF Mono', 'Fira Code', 'Cascadia Code', monospace;
```

Montserrat is loaded from Google Fonts at weights 300, 400, 600, 700:

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@300;400;600;700&display=swap" rel="stylesheet">
```

### Scale

| Use | Font | Size | Weight | Transform | Colour |
|---|---|---|---|---|---|
| Header title (logo block) | display | `1.75rem` | 600 | — | `var(--c-text-hi)` |
| Header subtitle (logo block) | display | `0.85rem` | 300 | uppercase, `letter-spacing: 0.18em` | `var(--c-text-low)` |
| Hero H1 | display | `2.4rem` | 600 | — (letter-spacing `-0.01em`) | `var(--c-text-hi)` |
| Content-card title | display | `1.05rem` | 600 | — | `var(--c-text-hi)` |
| Section heading | ui | `0.92rem` | 400 | uppercase, `letter-spacing: 0.10em` | `rgb(200, 200, 208)` |
| Monitoring card title | ui | `1rem` | 400 | uppercase, `letter-spacing: 0.14em` | `var(--c-text-hi)` |
| Body / hero copy | ui | `0.82–0.95rem` | 400 | — | `var(--c-text-mid)` |
| Button text | ui | `0.73rem` | 500 | uppercase, `letter-spacing: 0.06em` | varies |
| Nav link | display | `0.78rem` | 500 | — (`letter-spacing: 0.04em`) | `var(--c-text-mid)` |
| Data values | mono | `0.82–0.92rem` | 400 | — | `var(--c-amber)` |
| Keyboard hints | ui | `0.60rem` | 400 | uppercase, `letter-spacing: 0.06em` | `var(--c-text-dim)` |

Broadly: **Montserrat for the things that feel like "brand" or "content"** (logo block, hero, site nav, content-card titles). **System font for anything that feels like "UI"** (section labels, monitoring cards, chips, summary bars, buttons).

---

## Header

The header always follows the same left-to-right structure:

> **[ logo mark ] [ logo stripe ] [ title block (title + subtitle) ] [ right-side content ]**

The right-side content uses `margin-left: auto` and is one of:

- A status block (live-updating time, indicator dot) for monitoring pages.
- A back / navigation link for detail pages.
- A site nav + CTA button for multi-page marketing sites.

### Structure

```css
header {
  position: sticky;
  top: 0;
  z-index: 100;
  background: rgba(16, 16, 18, 0.88);
  backdrop-filter: blur(10px);
  border-bottom: 1px solid rgba(255, 255, 255, 0.06);
}
.header-inner {
  display: flex;
  align-items: center;
  gap: 1.25rem;
  max-width: 1400px;
  margin: 0 auto;
  padding: 1rem 2rem;
}

.logo-mark { height: 72px; width: auto; flex-shrink: 0; display: block; }

.title-block {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 0.1rem;
}
.title-block .title {
  font-family: var(--font-display);
  font-weight: 600;
  font-size: 1.75rem;
  color: var(--c-text-hi);
  letter-spacing: 0.01em;
  line-height: 1.1;
}
.title-block .subtitle {
  font-family: var(--font-display);
  font-weight: 300;
  font-size: 0.85rem;
  color: var(--c-text-low);
  letter-spacing: 0.18em;
  text-transform: uppercase;
  line-height: 1.2;
}

.header-right { margin-left: auto; display: flex; align-items: center; gap: 1rem; }
```

### HTML

```html
<header>
  <div class="header-inner">
    <img src="w92.svg" alt="A118" class="logo-mark">
    <div class="stripe-logo"></div>
    <div class="title-block">
      <div class="title">Project A118</div>
      <div class="subtitle">Journey Beyond</div>
    </div>
    <div class="header-right">
      <!-- status block / back link / nav + CTA -->
    </div>
  </div>
</header>
```

The title/subtitle pattern repeats across every page — `Project A118 / JOURNEY BEYOND`, `CCTV Dashboard / Friday, 17 April 2026`, `Pen Click Dose Calculator / Dial-based injection pen planning`, etc.

Logo file: `w92.svg`.

### Back / navigation link style

```css
.header-back {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  font-size: 0.7rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--c-sage);
  text-decoration: none;
  padding: 0.4rem 0.9rem;
  border: 1px solid rgba(100, 115, 97, 0.3);
  border-radius: 0.25rem;
  transition: background 0.15s, color 0.15s, border-color 0.15s;
}
.header-back:hover {
  background: rgba(100, 115, 97, 0.15);
  color: var(--c-olive);
  border-color: rgba(146, 151, 76, 0.4);
}
```

### Status block (right side, monitoring pages)

```css
.header-status {
  display: flex;
  flex-direction: column;
  align-items: flex-end;
  gap: 0.22rem;
}
.status-line {
  display: flex;
  align-items: center;
  gap: 0.4rem;
  font-size: 0.62rem;
  letter-spacing: 0.07em;
  text-transform: uppercase;
  color: var(--c-sage);
  white-space: nowrap;
}
.status-line.dim { color: rgb(75, 75, 85); }

.status-dot {
  width: 6px; height: 6px;
  border-radius: 50%;
  background: var(--c-olive);
  flex-shrink: 0;
  animation: blink 2.4s ease-in-out infinite;
}
@keyframes blink {
  0%, 100% { opacity: 1; }
  50%      { opacity: 0.3; }
}
```

### Multi-page site nav (right side, content sites)

When a site has several pages, the header carries inline nav between the title block and the CTA. Below 960 px the links collapse into a [hamburger dropdown](#dropdown-menu).

```css
.site-nav { display: flex; align-items: center; gap: 0.25rem; }
.site-nav a {
  font-family: var(--font-display);
  font-size: 0.78rem;
  font-weight: 500;
  color: var(--c-text-mid);
  text-decoration: none;
  padding: 0.5rem 0.9rem;
  border-radius: 0.25rem;
  letter-spacing: 0.04em;
  transition: background 0.15s, color 0.15s;
}
.site-nav a:hover { background: rgba(100, 115, 97, 0.15); color: var(--c-olive); }
.site-nav a.is-active {
  color: var(--c-amber);
  background: rgba(247, 165, 27, 0.1);
}
```

```html
<div class="header-right">
  <nav class="site-nav">
    <a href="#" class="is-active">Destinations</a>
    <a href="#">Experiences</a>
    <a href="#">Starships</a>
    <a href="#">InfiniDrive</a>
  </nav>

  <div class="menu nav-menu" data-menu>
    <button class="menu-trigger" type="button" aria-label="Menu" aria-haspopup="true" aria-expanded="false">
      <span></span><span></span><span></span><span></span><span></span>
    </button>
    <div class="menu-panel menu-panel-right" role="menu">
      <a href="#" class="is-active" role="menuitem">Destinations</a>
      <a href="#" role="menuitem">Experiences</a>
      <a href="#" role="menuitem">Starships</a>
      <a href="#" role="menuitem">InfiniDrive</a>
      <hr>
      <a href="#" role="menuitem">Book Now</a>
    </div>
  </div>

  <a href="#" class="btn btn-play">Book Now</a>
</div>
```

The CTA stays visible at all viewport widths. Only `.site-nav` collapses into the hamburger.

---

## Dropdown Menu

The dropdown uses the five-dashes stripe as its trigger (so the menu icon reads as both "menu" and "A118"). The same component powers the collapsed site nav and any standalone dropdown.

### Trigger (hamburger)

The trigger is an explicit 34 × 34 px button with **flex column + `justify-content: space-between`** — this pins the first bar to the top padding and the last bar to the bottom padding, with the middle three spaced evenly between, at whole-pixel offsets. Do not use CSS grid here; fractional row heights cause sub-pixel antialiasing that makes the bars look uneven.

```css
.menu { position: relative; }

.menu-trigger {
  display: inline-flex;
  flex-direction: column;
  justify-content: space-between;
  align-items: stretch;
  width: 34px;
  height: 34px;
  padding: 8px;
  background: rgba(100, 115, 97, 0.12);
  border: 1px solid rgba(100, 115, 97, 0.25);
  border-radius: 0.25rem;
  cursor: pointer;
  transition: background 0.15s, border-color 0.15s;
  box-sizing: border-box;
  vertical-align: middle;
}
.menu-trigger:hover {
  background: rgba(100, 115, 97, 0.22);
  border-color: rgba(146, 151, 76, 0.4);
}
.menu-trigger span {
  display: block;
  width: 100%;
  height: 2px;
  border-radius: 1px;
  flex: 0 0 2px;
}
.menu-trigger span:nth-child(1) { background: var(--c-orange); }
.menu-trigger span:nth-child(2) { background: var(--c-amber); }
.menu-trigger span:nth-child(3) { background: var(--c-olive); }
.menu-trigger span:nth-child(4) { background: var(--c-sage); }
.menu-trigger span:nth-child(5) { background: var(--c-mauve); }
```

### Panel

The panel defaults to anchoring to the trigger's **left** edge. When the menu sits on the right of a container (as in the site-nav case), add `menu-panel-right` to anchor to the right edge instead.

```css
.menu-panel {
  position: absolute;
  top: calc(100% + 0.5rem);
  left: 0;
  min-width: 220px;
  background: rgba(20, 20, 22, 0.96);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 0.4rem;
  box-shadow: 0 16px 40px rgba(0, 0, 0, 0.6);
  backdrop-filter: blur(12px);
  padding: 0.4rem;
  opacity: 0;
  transform: translateY(-4px);
  pointer-events: none;
  transition: opacity 0.15s, transform 0.15s;
  z-index: 200;
}
.menu-panel-right { left: auto; right: 0; }

.menu.open .menu-panel {
  opacity: 1;
  transform: translateY(0);
  pointer-events: auto;
}
.menu-panel a {
  display: block;
  padding: 0.55rem 0.8rem;
  font-family: var(--font-display);
  font-size: 0.8rem;
  font-weight: 500;
  color: var(--c-text-mid);
  text-decoration: none;
  letter-spacing: 0.04em;
  border-radius: 0.25rem;
  transition: background 0.12s, color 0.12s;
}
.menu-panel a:hover { background: rgba(100, 115, 97, 0.18); color: var(--c-olive); }
.menu-panel a.is-active { background: rgba(247, 165, 27, 0.12); color: var(--c-amber); }
.menu-panel hr { border: none; border-top: 1px solid rgba(255, 255, 255, 0.08); margin: 0.35rem 0.2rem; }
```

> **Don't clip the dropdown.** The ancestor header must not have `overflow: hidden`, otherwise the panel is cropped the moment it extends below the header.

### Behaviour

```js
document.querySelectorAll('[data-menu]').forEach(menu => {
  const trigger = menu.querySelector('.menu-trigger');
  const setOpen = (open) => {
    menu.classList.toggle('open', open);
    trigger.setAttribute('aria-expanded', open ? 'true' : 'false');
  };
  trigger.addEventListener('click', e => {
    e.stopPropagation();
    document.querySelectorAll('[data-menu].open').forEach(m => { if (m !== menu) m.classList.remove('open'); });
    setOpen(!menu.classList.contains('open'));
  });
});
document.addEventListener('click', e => {
  document.querySelectorAll('[data-menu].open').forEach(menu => {
    if (!menu.contains(e.target)) menu.classList.remove('open');
  });
});
document.addEventListener('keydown', e => {
  if (e.key === 'Escape') document.querySelectorAll('[data-menu].open').forEach(m => m.classList.remove('open'));
});
```

---

## Hero

Used as the top-of-page block on content and marketing pages.

```css
.hero {
  position: relative;
  border-radius: 0.5rem;
  overflow: hidden;
  border: 1px solid rgba(255, 255, 255, 0.06);
  background: linear-gradient(120deg, rgba(12, 12, 16, 0.55), rgba(12, 12, 16, 0.25));
  backdrop-filter: blur(2px);
  padding: 4rem 3rem;
  min-height: 320px;
  display: flex;
  align-items: center;
  gap: 2rem;
}
.hero-copy { max-width: 640px; }
.hero h1 {
  font-family: var(--font-display);
  font-weight: 600;
  font-size: 2.4rem;
  line-height: 1.15;
  color: var(--c-text-hi);
  margin-bottom: 0.9rem;
  letter-spacing: -0.01em;
}
.hero p {
  font-size: 0.95rem;
  color: var(--c-text-mid);
  margin-bottom: 1.8rem;
  line-height: 1.6;
}
.hero-actions { display: flex; gap: 0.75rem; flex-wrap: wrap; }
```

The hero lets the fixed parallax background show through its semi-transparent fill — **no orange glow, no separate feature image, and no stripe inside the hero card**. Keep it quiet; the page already has strong identity from the header.

---

## Section Heading

A lightweight heading used between major page sections (above a card grid, a feature list, a tech panel, etc.). The leading orange dash ties the heading to the stripe palette.

```css
.section-heading {
  font-family: var(--font-ui);
  font-size: 0.92rem;
  font-weight: 400;
  color: rgb(200, 200, 208);
  letter-spacing: 0.10em;
  text-transform: uppercase;
  margin-bottom: 1.25rem;
  display: flex;
  align-items: center;
  gap: 0.7rem;
}
.section-heading::before {
  content: "";
  display: block;
  width: 24px;
  height: 2px;
  background: var(--c-orange);
  border-radius: 1px;
}
```

---

## Cards

Cards are the primary content unit. Two common shapes:

- **Monitoring card** — stripe accent + card body, used for dashboards and tool pages.
- **Media card** — image above, content-card title + paragraph below, used on marketing / destination pages.

Both sit on the same glass surface and share hover state.

### Base styling

```css
.card {
  background: var(--c-surface-2);
  border: 1px solid rgba(255, 255, 255, 0.07);
  border-radius: 0.4rem;
  overflow: hidden;
  display: flex;
  backdrop-filter: blur(10px);
  transition: border-color 0.25s ease, box-shadow 0.25s ease, transform 0.2s ease;
}
.card:hover {
  border-color: rgba(238, 115, 0, 0.4);
  box-shadow: 0 4px 24px rgba(238, 115, 0, 0.08);
  transform: translateY(-2px);
}
.card-body { padding: 1.1rem 1.2rem; flex: 1; display: flex; flex-direction: column; gap: 0.45rem; }
```

### Monitoring-card title (system font, uppercase)

```css
.card-title {
  font-family: var(--font-ui);
  font-size: 1rem;
  font-weight: 400;
  text-transform: uppercase;
  letter-spacing: 0.14em;
  color: var(--c-text-hi);
}
.card-body p { font-size: 0.82rem; color: var(--c-text-mid); line-height: 1.5; }
```

### Media card (marketing pages)

```css
.card-media { flex-direction: column; }
.card-media .img-wrap {
  aspect-ratio: 16 / 9;
  border-bottom: 1px solid rgba(255, 255, 255, 0.06);
  background: var(--c-surface-1);
  overflow: hidden;
}
.card-media .img-wrap img {
  width: 100%; height: 100%; object-fit: cover; display: block;
  transition: opacity 0.15s;
  cursor: zoom-in;
}
.card-media .img-wrap img:hover { opacity: 0.88; }
.card-media .card-body { padding: 1.1rem 1.2rem 1.3rem; }
.card-media .card-title {
  font-family: var(--font-display);
  font-size: 1.05rem;
  font-weight: 600;
  letter-spacing: 0.02em;
  text-transform: none;
  color: var(--c-text-hi);
}
```

### Card grid

```css
.card-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 1.2rem;
}
```

---

## Summary / Info Bar

A card-like horizontal strip used above content lists to display key statistics:

```css
.summary-bar {
  background: var(--c-surface-2);
  border: 1px solid rgba(255, 255, 255, 0.07);
  border-radius: 0.4rem;
  padding: 0.9rem 1.4rem;
  display: flex;
  align-items: center;
  gap: 1.8rem;
  backdrop-filter: blur(10px);
  flex-wrap: wrap;
}
.summary-divider { width: 1px; height: 34px; background: rgba(255, 255, 255, 0.07); flex-shrink: 0; }
.summary-stat { display: flex; flex-direction: column; gap: 0.1rem; }
.stat-label {
  font-size: 0.62rem; letter-spacing: 0.09em; text-transform: uppercase;
  color: rgb(85, 85, 95);
}
.stat-value { font-family: var(--font-mono); font-size: 0.92rem; color: var(--c-amber); }
```

Place action buttons (e.g. "Play Latest") at the far right using `margin-left: auto` on the first such element.

---

## Feature Lists & Bullet System

Replaces the legacy `✧` icon with bullets drawn from the stripe. Two variants, picked by list length.

### Rule: which bullet to use

| List length | Bullet | Why |
|---|---|---|
| Exactly **5** items | **Cycling square** (`.cycle-bullet`) | Each item gets one colour of the stripe in order — the list itself becomes the stripe. |
| **Any other** count (4, 6, 10, …) | **Exploded dashes** (`.dashes-bullet`) | Each bullet is a miniature vertical stripe, so the pattern still reads as "A118" regardless of item count. |

Never mix the two variants in a single list.

### Exploded-dashes bullet

```css
.dashes-bullet {
  display: flex;
  flex-direction: column;
  gap: 2px;
  flex-shrink: 0;
  margin-top: 0.3rem;
}
.dashes-bullet span {
  display: block;
  width: 0.8rem;
  height: 2px;
  border-radius: 1px;
}
.dashes-bullet span:nth-child(1) { background: var(--c-orange); }
.dashes-bullet span:nth-child(2) { background: var(--c-amber);  }
.dashes-bullet span:nth-child(3) { background: var(--c-olive);  }
.dashes-bullet span:nth-child(4) { background: var(--c-sage);   }
.dashes-bullet span:nth-child(5) { background: var(--c-mauve);  }
```

```html
<div class="dashes-bullet" aria-hidden="true">
  <span></span><span></span><span></span><span></span><span></span>
</div>
```

### Cycling-colour bullet (5-item lists only)

The parent wrapper carries `.cycle-list`; each bullet is `.cycle-bullet`. The cycle uses `:nth-child(5n+N)` so longer lists technically still cycle correctly — but restrict this style to exactly-5-item lists for maximum clarity.

```css
.cycle-bullet {
  width: 0.82rem;
  height: 0.82rem;
  border-radius: 3px;
  flex-shrink: 0;
  margin-top: 0.3rem;
}
.cycle-list > *:nth-child(5n+1) .cycle-bullet { background: var(--c-orange); }
.cycle-list > *:nth-child(5n+2) .cycle-bullet { background: var(--c-amber);  }
.cycle-list > *:nth-child(5n+3) .cycle-bullet { background: var(--c-olive);  }
.cycle-list > *:nth-child(5n+4) .cycle-bullet { background: var(--c-sage);   }
.cycle-list > *:nth-child(5n+5) .cycle-bullet { background: var(--c-mauve);  }
```

### Feature-item card

Bullets sit inside a glass "feature item" — a mini card containing bullet + heading + copy.

```css
.feature-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 1rem;
}
.feature-item {
  display: flex;
  gap: 0.9rem;
  padding: 1.1rem 1.2rem;
  background: var(--c-surface-2);
  border: 1px solid rgba(255, 255, 255, 0.07);
  border-radius: 0.4rem;
  backdrop-filter: blur(10px);
}
.feature-item h3 {
  font-family: var(--font-ui);
  font-size: 0.82rem;
  font-weight: 500;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--c-text-hi);
  margin-bottom: 0.35rem;
}
.feature-item p { font-size: 0.8rem; color: var(--c-text-mid); line-height: 1.5; }
```

```html
<div class="cycle-list feature-grid">
  <div class="feature-item">
    <div class="cycle-bullet" aria-hidden="true"></div>
    <div><h3>Low-Gravity Flying</h3><p>…</p></div>
  </div>
  <!-- … 4 more items … -->
</div>
```

---

## Two-Column Layout

Image-plus-text content panel, e.g. a product feature explained next to a diagram.

```css
.two-col {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2rem;
  align-items: start;
}
.two-col .col-panel {
  background: var(--c-surface-2);
  border: 1px solid rgba(255, 255, 255, 0.07);
  border-radius: 0.4rem;
  padding: 1.5rem;
  backdrop-filter: blur(10px);
}
.two-col .col-panel h3 {
  font-family: var(--font-ui);
  font-size: 0.92rem;
  font-weight: 400;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: var(--c-text-hi);
  margin-bottom: 0.9rem;
}
.two-col .col-panel p { font-size: 0.85rem; color: var(--c-text-mid); margin-bottom: 0.9rem; }
.two-col .col-panel p:last-child { margin-bottom: 0; }
.two-col .col-media {
  border-radius: 0.4rem;
  overflow: hidden;
  border: 1px solid rgba(255, 255, 255, 0.06);
  aspect-ratio: 4 / 3;
  background: var(--c-surface-1);
}
```

Collapses to a single column at 900 px.

---

## Long-Form Content / Tech Panel

Used for technical detail pages and other long-form prose-plus-data content. Distinguishing feature: amber-tinted table headers that echo the data colour elsewhere in the system.

```css
.tech-panel {
  background: var(--c-surface-2);
  border: 1px solid rgba(255, 255, 255, 0.07);
  border-radius: 0.4rem;
  padding: 2rem;
  backdrop-filter: blur(10px);
}
.tech-panel h2 {
  font-family: var(--font-ui);
  font-size: 0.92rem;
  font-weight: 400;
  text-transform: uppercase;
  letter-spacing: 0.14em;
  color: var(--c-text-hi);
  margin-bottom: 1rem;
  padding-bottom: 0.6rem;
  border-bottom: 1px solid rgba(255, 255, 255, 0.08);
}
.tech-panel h3 {
  font-family: var(--font-ui);
  font-size: 0.78rem;
  font-weight: 500;
  text-transform: uppercase;
  letter-spacing: 0.12em;
  color: var(--c-amber);
  margin: 1.5rem 0 0.7rem;
}
.tech-panel p { font-size: 0.88rem; color: var(--c-text-mid); margin-bottom: 0.9rem; line-height: 1.65; }

.tech-panel table { width: 100%; border-collapse: collapse; margin: 1.2rem 0; font-size: 0.82rem; }
.tech-panel th, .tech-panel td {
  padding: 0.65rem 0.9rem;
  text-align: left;
  border-bottom: 1px solid rgba(255, 255, 255, 0.06);
}
.tech-panel th {
  background: rgba(247, 165, 27, 0.08);
  color: var(--c-amber);
  font-weight: 500;
  text-transform: uppercase;
  letter-spacing: 0.08em;
  font-size: 0.68rem;
  border-bottom: 1px solid rgba(247, 165, 27, 0.2);
}
.tech-panel tbody tr:hover { background: rgba(255, 255, 255, 0.02); }
.tech-panel td:first-child { color: var(--c-text-hi); }
.tech-panel td + td { font-family: var(--font-mono); color: var(--c-amber); }
```

---

## Buttons

All buttons share a base class that resets browser defaults:

```css
.btn {
  display: inline-flex;
  align-items: center;
  gap: 0.4rem;
  padding: 0.45rem 1rem;
  border-radius: 0.25rem;
  border: none;                 /* variants that need a border declare it themselves */
  font-family: var(--font-ui);
  font-size: 0.73rem;
  font-weight: 500;
  letter-spacing: 0.06em;
  text-decoration: none;
  text-transform: uppercase;
  transition: background 0.15s, color 0.15s, border-color 0.15s, opacity 0.15s;
  white-space: nowrap;
  cursor: pointer;
}
.btn-lg { padding: 0.65rem 1.3rem; font-size: 0.78rem; }
```

> **Important:** `border: none` must be on the base class. When `<button>` elements are used (rather than `<a>`), browsers add a default grey border that overrides any transparent/missing border. Only variants that explicitly need a visible border (browse, yesterday, image) declare one.

### Variants

```css
/* Primary — solid orange, highest-priority actions. Hover shifts to amber. */
.btn-play { background: var(--c-orange); color: rgb(15, 15, 18); }
.btn-play:hover { background: var(--c-amber); }

/* Secondary — olive tint — navigation / browse */
.btn-browse {
  background: rgba(100, 115, 97, 0.18);
  color: var(--c-olive);
  border: 1px solid rgba(100, 115, 97, 0.3);
}
.btn-browse:hover { background: rgba(100, 115, 97, 0.32); }

/* Muted — mauve tint — low-priority / "yesterday"-style actions */
.btn-yesterday {
  background: rgba(93, 85, 90, 0.22);
  color: rgb(160, 148, 155);
  border: 1px solid rgba(93, 85, 90, 0.4);
}
.btn-yesterday:hover { background: rgba(93, 85, 90, 0.38); }

/* Amber — image / timelapse / data-context actions */
.btn-image {
  background: rgba(247, 165, 27, 0.14);
  color: var(--c-amber);
  border: 1px solid rgba(247, 165, 27, 0.28);
}
.btn-image:hover { background: rgba(247, 165, 27, 0.26); }

.btn.disabled, .btn:disabled { opacity: 0.35; pointer-events: none; cursor: default; }
```

### Button icons

Small inline SVG icons (10 × 10 px) sit before the label text:

```html
<!-- Play triangle -->
<svg width="10" height="10" viewBox="0 0 10 10" fill="currentColor">
  <path d="M2 1.5l6 3.5-6 3.5V1.5Z"/>
</svg>

<!-- Download arrow -->
&#8595;
```

---

## Close Buttons

Close buttons appear in all modal panels. They are **rounded squares** (not circles) and use the dark-sage family, signalling "neutral control" rather than a primary action.

```css
.lb-close {
  background: rgba(100, 115, 97, 0.15);
  border: 1px solid rgba(100, 115, 97, 0.28);
  border-radius: 6px;
  width: 30px;
  height: 30px;
  display: flex;
  align-items: center;
  justify-content: center;
  color: var(--c-sage);
  cursor: pointer;
  font-size: 1rem;
  line-height: 0;            /* removes line box — optical centering of × glyph */
  transition: background 0.15s, color 0.15s, border-color 0.15s;
  flex-shrink: 0;
}
.lb-close:hover {
  background: rgba(238, 115, 0, 0.22);
  color: var(--c-amber);
  border-color: rgba(238, 115, 0, 0.35);
}
```

**Why `line-height: 0`:** The `×` (`&times;`) glyph sits visually below the typographic centre in most system fonts. Setting `line-height: 0` collapses the line box so flexbox centres on the glyph's actual rendered bounds rather than the line box.

---

## Modal / Lightbox Panels

All pop-up viewers share the same panel structure and backdrop.

### Backdrop

```css
.lb-overlay {
  display: none;
  position: fixed;
  inset: 0;
  z-index: 9000;
  background: rgba(6, 6, 8, 0.96);
  backdrop-filter: blur(8px);
  align-items: center;
  justify-content: center;
}
.lb-overlay.open { display: flex; }
```

### Panel

```css
.lb-panel {
  width: 100%;
  max-width: min(96vw, 1400px);
  background: rgba(20, 20, 22, 0.95);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 0.4rem;
  overflow: hidden;
  box-shadow: 0 32px 80px rgba(0, 0, 0, 0.85);
  display: flex;
  flex-direction: column;
}
```

### Header bar

```css
.lb-header {
  display: flex;
  align-items: center;
  gap: 1rem;
  padding: 0.75rem 1.2rem;
  border-bottom: 1px solid rgba(255, 255, 255, 0.06);
  background: rgba(14, 14, 16, 0.8);
  flex-shrink: 0;
}

/* Orange pill badge — identifies the subject (camera name, category label) */
.lb-cam-badge {
  font-size: 0.65rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: rgb(15, 15, 18);
  background: var(--c-orange);
  padding: 0.18rem 0.55rem;
  border-radius: 0.2rem;
  font-weight: 600;
  flex-shrink: 0;
}

/* Title — takes remaining space */
.lb-title {
  flex: 1;
  font-size: 0.82rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: rgb(180, 180, 188);
}

/* Highlighted value within a title (monospace, amber) */
.lb-title em {
  font-family: var(--font-mono);
  font-style: normal;
  color: var(--c-amber);
}
```

### Footer bar

```css
.lb-footer {
  display: flex;
  align-items: center;
  gap: 0.5rem;
  padding: 0.65rem 1.2rem;
  border-top: 1px solid rgba(255, 255, 255, 0.06);
  background: rgba(14, 14, 16, 0.8);
  flex-shrink: 0;
}
```

Footer contains navigation buttons, a counter, a download link, and/or a keyboard hint.

### Footer buttons

```css
.lb-btn {
  display: inline-flex;
  align-items: center;
  gap: 0.3rem;
  padding: 0.32rem 0.75rem;
  border: 1px solid rgba(255, 255, 255, 0.1);
  border-radius: 0.25rem;
  background: rgba(40, 40, 44, 0.9);
  color: rgb(150, 150, 160);
  cursor: pointer;
  font-size: 0.7rem;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  transition: background 0.14s, color 0.14s, border-color 0.14s;
  text-decoration: none;
  white-space: nowrap;
}
.lb-btn:hover:not(:disabled) {
  background: rgba(100, 115, 97, 0.22);
  color: var(--c-olive);
  border-color: rgba(100, 115, 97, 0.35);
}
.lb-btn:disabled { opacity: 0.28; cursor: default; }

.lb-btn.download {
  background: rgba(238, 115, 0, 0.12);
  border-color: rgba(238, 115, 0, 0.3);
  color: var(--c-orange);
}
.lb-btn.download:hover {
  background: rgba(238, 115, 0, 0.24);
  border-color: rgba(238, 115, 0, 0.5);
  color: var(--c-amber);
}

.lb-kbd-hint {
  font-size: 0.6rem;
  letter-spacing: 0.06em;
  text-transform: uppercase;
  color: var(--c-text-dim);
  margin-left: auto;
  white-space: nowrap;
}

.lb-counter {
  flex: 1;
  text-align: center;
  font-family: var(--font-mono);
  font-size: 0.64rem;
  letter-spacing: 0.07em;
  color: rgb(72, 72, 82);
}
```

### HTML structure

```html
<div id="my-lightbox" class="lb-overlay" role="dialog" aria-modal="true" aria-label="Viewer">
  <div class="lb-panel">
    <div class="lb-header">
      <span class="lb-cam-badge" id="lb-badge">Label</span>
      <div class="lb-title" id="lb-title">…</div>
      <button class="lb-close" id="lb-close" aria-label="Close">&times;</button>
    </div>
    <!-- content: <img> or <video controls> -->
    <div class="lb-footer">
      <button class="lb-btn" id="lb-prev" disabled>&#8592; Newer</button>
      <span class="lb-counter" id="lb-counter"></span>
      <button class="lb-btn" id="lb-next" disabled>Older &#8594;</button>
      <a class="lb-btn download" id="lb-download" href="#" download>&#8595; Download</a>
      <span class="lb-kbd-hint">&#8592;&#8594; navigate &middot; esc close</span>
    </div>
  </div>
</div>
```

### Closing behaviour

- Close button click → call close function
- Backdrop click (target === overlay element) → call close function
- `Escape` key → call close function
- For video lightboxes: `Space` toggles pause/play

---

## Countdown Badge (Live Image Overlays)

Used on any live-refreshing image to show time until next refresh. Positioned absolutely over the image's top-right corner.

```css
.countdown-badge {
  position: absolute;
  top: 0.5rem;
  right: 0.5rem;
  width: 3rem;                         /* fixed — never resizes as digits change */
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(22, 22, 24, 0.82);
  border: 1px solid rgba(255, 255, 255, 0.10);
  border-radius: 0.3rem;
  backdrop-filter: blur(8px);
  padding: 0.22rem 0;
  pointer-events: none;
  user-select: none;
}
.countdown-badge-value {
  font-family: var(--font-mono);
  font-size: 0.82rem;
  letter-spacing: 0.04em;
  color: var(--c-amber);
  line-height: 1;
}
```

The image wrapper must be `position: relative`. Display format: `Ns` (e.g. `5s`, `4s`). Use a fixed pixel/rem `width` to prevent the badge from changing size as digits change.

```html
<div class="image-wrap" style="position: relative;">
  <img id="live-img" src="…" alt="…"/>
  <div class="countdown-badge">
    <span class="countdown-badge-value" id="countdown-value">5s</span>
  </div>
</div>
```

```js
const REFRESH_MS = 5000;
let remaining = REFRESH_MS / 1000;
const valueEl = document.getElementById('countdown-value');

function updateCountdown() { valueEl.textContent = `${remaining}s`; }

setInterval(() => {
  remaining -= 1;
  if (remaining <= 0) {
    remaining = REFRESH_MS / 1000;
    document.getElementById('live-img').src = liveUrl + '?_t=' + Date.now();
  }
  updateCountdown();
}, 1000);
```

---

## Accordion Components

Used for date/time hierarchical archives. Two-level: outer (Day) → inner (Hour).

```css
.day-section {
  background: rgba(28, 28, 31, 0.65);
  border: 1px solid rgba(255, 255, 255, 0.07);
  border-radius: 0.4rem;
  overflow: hidden;
  margin-bottom: 0.65rem;
  backdrop-filter: blur(8px);
  transition: border-color 0.2s;
}
.day-section.open { border-color: rgba(255, 255, 255, 0.10); }

.day-header {
  display: flex;
  align-items: center;
  gap: 0.8rem;
  padding: 0.85rem 1.2rem;
  cursor: pointer;
  user-select: none;
  transition: background 0.15s;
}
.day-header:hover { background: rgba(238, 115, 0, 0.04); }
.day-section.open .day-header { border-bottom: 1px solid rgba(255, 255, 255, 0.06); }

.chevron {
  color: rgb(80, 80, 90);
  font-size: 0.65rem;
  transition: transform 0.2s ease;
  flex-shrink: 0;
  line-height: 1;
}
.day-section.open > .day-header > .chevron { transform: rotate(90deg); }

.day-label {
  font-size: 0.74rem;
  letter-spacing: 0.08em;
  text-transform: uppercase;
  color: var(--c-text-hi);
}
.day-count {
  margin-left: auto;
  font-family: var(--font-mono);
  font-size: 0.75rem;
  color: var(--c-amber);
}
.day-content { display: none; padding: 0.9rem 1.2rem 1rem; flex-wrap: wrap; gap: 0.4rem; }
.day-section.open .day-content { display: flex; }
```

**Lazy rendering** is strongly recommended for large datasets: render the content panel's children only when the section is first opened.

```js
let rendered = false;
function toggle() {
  section.classList.toggle('open');
  if (section.classList.contains('open') && !rendered) {
    rendered = true;
    // …build and append child elements here…
  }
}
header.addEventListener('click', toggle);
header.addEventListener('keydown', e => {
  if (e.key === 'Enter' || e.key === ' ') { e.preventDefault(); toggle(); }
});
```

### Time chips

Individual selectable items within an accordion. Used for recordings, image frames, etc.

```css
.time-chip {
  padding: 0.24rem 0.5rem;
  font-family: var(--font-mono);
  font-size: 0.74rem;
  background: rgba(28, 28, 31, 0.7);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 0.2rem;
  cursor: pointer;
  color: rgb(150, 150, 162);
  transition: background 0.1s, border-color 0.1s, color 0.1s;
  white-space: nowrap;
  line-height: 1.4;
}
.time-chip:hover {
  background: rgba(238, 115, 0, 0.13);
  border-color: rgba(238, 115, 0, 0.38);
  color: var(--c-orange);
}
.time-chip.is-current {
  background: rgba(247, 165, 27, 0.18);
  border-color: rgba(247, 165, 27, 0.45);
  color: var(--c-amber);
}
```

---

## Loading States

### Animated bar

Used while data is being fetched:

```css
.loading-state {
  text-align: center;
  padding: 2.5rem 2rem;
  color: rgb(85, 85, 95);
  font-size: 0.78rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
}
.loading-bar {
  width: 180px;
  height: 2px;
  background: rgba(255, 255, 255, 0.06);
  border-radius: 1px;
  margin: 1.1rem auto 0;
  overflow: hidden;
}
.loading-bar-fill {
  height: 100%;
  background: var(--c-orange);
  border-radius: 1px;
  animation: bar-pulse 1.4s ease-in-out infinite;
}
@keyframes bar-pulse {
  0%   { width: 0%;  margin-left: 0; }
  50%  { width: 60%; margin-left: 20%; }
  100% { width: 0%;  margin-left: 100%; }
}
```

### Inline data states

```css
.data-value.loading { color: rgb(65, 65, 75); font-style: italic; }
.data-value.error   { color: rgb(140, 60, 60); font-size: 0.74rem; }
```

---

## Image Preview Cards (Thumbnails)

Aspect-ratio-constrained image containers used inside content cards:

```css
.img-wrap {
  margin-bottom: 1rem;
  border-radius: 0.25rem;
  overflow: hidden;
  border: 1px solid rgba(255, 255, 255, 0.06);
  background: var(--c-surface-1);
  aspect-ratio: 16 / 9;
  display: flex;
  align-items: center;
  justify-content: center;
}
.img-wrap img {
  width: 100%;
  height: 100%;
  object-fit: cover;
  display: block;
  cursor: zoom-in;
  transition: opacity 0.15s;
}
.img-wrap img:hover { opacity: 0.88; }
```

---

## Cache-Busting Pattern

When polling images that should not be cached by the browser, append a timestamp query parameter. The backend can strip the query string before proxying if needed — the important thing is the browser sees a new URL each time.

```js
const REFRESH_MS = 5000;

function liveUrl(baseUrl) {
  return `${baseUrl}?_t=${Date.now()}`;
}

// On refresh:
img.src = liveUrl('/path/to/image');
```

For lightbox images, preload before swapping to avoid flash:

```js
function refreshLightbox(targetId) {
  const capturedId = targetId;          // guard against stale callback
  const preload = new Image();
  preload.onload = () => {
    if (currentId === capturedId) {     // still showing the same subject
      img.src = preload.src;
    }
  };
  preload.src = liveUrl('/path/to/full-res');
}
```

---

## Footer

The footer mirrors the header identity — horizontal logo stripe above copyright / site-name text — with optional link row.

```css
footer {
  margin-top: auto;
  padding: 2.5rem 2rem 2rem;
  text-align: center;
  border-top: 1px solid rgba(255, 255, 255, 0.05);
}
footer .stripe-logo { margin: 0 auto 1.1rem; }

.footer-links {
  display: flex; justify-content: center; gap: 1.4rem;
  margin-bottom: 1rem;
  flex-wrap: wrap;
}
.footer-links a {
  font-family: var(--font-ui);
  font-size: 0.68rem;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  color: var(--c-sage);
  text-decoration: none;
  transition: color 0.15s;
}
.footer-links a:hover { color: var(--c-amber); }

.footer-text {
  font-size: 0.68rem;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: rgb(60, 60, 70);
}
```

Structure: `.stripe-logo.horizontal` → footer links → copyright line.

---

## Responsive Breakpoints

| Breakpoint | Behaviour |
|---|---|
| ≤ 960 px | `.site-nav` hides; `.menu.nav-menu` (dashes-hamburger) shows in its place. |
| ≤ 900 px | `.two-col` collapses to a single column. |
| ≤ 760 px | Single-column layout. Header status block hidden. Reduced padding. Hero stacks vertically. |

```css
@media (max-width: 900px) {
  .two-col { grid-template-columns: 1fr; }
}
@media (max-width: 960px) {
  .site-nav { display: none; }
  .menu.nav-menu { display: block; }
}
@media (max-width: 760px) {
  .header-inner { padding: 1rem; gap: 0.8rem; }
  .header-status { display: none; }
  .main { padding: 1.5rem 1rem; gap: 2rem; }
  .hero { padding: 2.2rem 1.5rem; flex-direction: column; align-items: flex-start; }
  .hero h1 { font-size: 1.8rem; }
  .list-row { grid-template-columns: 5rem 1fr; }
  .list-row > *:nth-child(n+3) { display: none; }
  footer { padding: 1.8rem 1rem; }
}
```

---

## List Row Alignment

This is a frequent source of layout bugs. Follow these rules exactly.

### Rule: Use CSS Grid for multi-column list rows

Any list or log where rows contain multiple columns (badges, names, filenames, values) **must use CSS Grid with explicit column widths**, not flexbox. With flexbox, a badge or label that is one character longer than expected will push all subsequent columns to the right, causing misalignment between rows.

```css
/* CORRECT — grid gives every row identical column positions */
.list-row {
  display: grid;
  grid-template-columns: 6.5rem 6rem 1fr 8rem 6rem;
  align-items: center;
  gap: 0.7rem;
  padding: 0.55rem 0.9rem;
  background: rgba(28, 28, 31, 0.55);
  border: 1px solid rgba(255, 255, 255, 0.05);
  border-radius: 0.3rem;
  font-size: 0.78rem;
}

/* WRONG — flex-grow on content column shifts subsequent columns */
.list-row {
  display: flex;
  align-items: center;
  gap: 0.6rem;
}
.list-row .filename { flex: 1; }  /* everything after this drifts */
```

### Rule: Badges must have a fixed-width grid column

Badge columns (status badges, camera name badges, decision badges) **must be sized by the grid column**, not by their content. Never rely on `min-content` for a badge that can have variable-length text.

```css
/* Status badge column: 6.5rem fixed */
/* Camera badge column: 6rem fixed   */
grid-template-columns: 6.5rem 6rem 1fr;
```

The badge element itself should use `overflow: hidden; text-overflow: ellipsis; white-space: nowrap` to clip long text within the fixed column, never expand it.

### Rule: Filename/content column uses 1fr

The filename or primary content column always takes all remaining space using `1fr`. It should also have `overflow: hidden; text-overflow: ellipsis; white-space: nowrap` so long filenames clip rather than wrap.

### Rule: Never mix row types in the same container

If a container shows two types of rows (e.g. "queued" vs "processing" with different badge lengths), both row types must use the same `grid-template-columns` definition so columns align between them.

### Badge styles

```css
.list-badge {
  font-size: 0.62rem; letter-spacing: 0.1em; text-transform: uppercase;
  padding: 0.2rem 0.45rem; border-radius: 0.2rem;
  text-align: center;
  overflow: hidden; text-overflow: ellipsis; white-space: nowrap;
  font-weight: 600;
}
.list-badge.ok    { background: rgba(146, 151, 76, 0.2); color: var(--c-olive); }
.list-badge.live  { background: var(--c-orange); color: rgb(15, 15, 18); }
.list-badge.mute  { background: rgba(93, 85, 90, 0.25); color: rgb(160, 148, 155); }
```

### Summary

| Column type | Sizing rule |
|---|---|
| Status/decision badge | Fixed `rem` grid column |
| Camera/source badge | Fixed `rem` grid column |
| Filename / primary content | `1fr` |
| Metadata (classes, confidence, time) | Fixed `rem` grid column |

---

## Accessibility Notes

- All interactive elements have `cursor: pointer`.
- Accordion headers use `role="button"` and `tabindex="0"` and respond to Enter and Space key events.
- Modal overlays use `role="dialog"` and `aria-modal="true"` with a descriptive `aria-label`.
- Close buttons have `aria-label="Close"`.
- `document.body.style.overflow = 'hidden'` is set while a modal is open and restored on close.
- Decorative SVG icons, bullet elements, and the hamburger bars use `aria-hidden="true"`.
- The hamburger trigger is a real `<button>` with `aria-haspopup="true"` and `aria-expanded` kept in sync by the menu script.
- Menu panels use `role="menu"`; links use `role="menuitem"`.
- Live image elements have meaningful `alt` text even when the image is being refreshed.

---

## Design Principles

1. **Dark, never black** — no pure black surfaces. Darkest is `rgb(4, 4, 6)` for image backgrounds.
2. **Data is amber** — anything that updates at runtime is rendered in amber monospace to signal liveness.
3. **Primary is orange, hover is amber** — one primary action per view. Supporting actions are olive (neutral), mauve (muted), or amber (data-context).
4. **Stripe = identity** — the five-colour gradient appears on every page: as the logo-stripe in headers and footers, as card-accent stripes, and as the bullet for every feature list.
5. **Montserrat speaks, system font works** — Montserrat for the voiced parts (logo block, hero, nav, media titles); system font for the working parts (labels, buttons, monitoring chrome).
6. **Lazy rendering** — never build more DOM than is visible. Expand on demand.
7. **No credential leakage** — authentication for backend services is handled server-side (e.g. reverse proxy). URLs in client JS are relative paths with no credentials.
8. **Preload before swap** — live images are preloaded into a throwaway `Image()` object and only applied to the visible element on successful load. This eliminates flicker.
