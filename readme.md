# Lambton College — Project Management Program

A modern, single‑page marketing website for Lambton College's Project Management program.
It was rebuilt from an aging Bootstrap 3 + jQuery template into a fast, dependency‑free
static site with a custom design system, scroll‑reveal animations, and a fully responsive
layout.

> **Stack:** Plain HTML5 + CSS3 + vanilla JavaScript. No framework, no build step, no
> `node_modules`. Just open the file in a browser.

---

## Table of contents

- [Highlights](#highlights)
- [Tech stack](#tech-stack)
- [Project structure](#project-structure)
- [Running locally](#running-locally)
- [How it works](#how-it-works)
  - [HTML — page sections](#html--page-sections)
  - [CSS — design system](#css--design-system)
  - [JavaScript — interactions](#javascript--interactions)
- [The animation system](#the-animation-system)
- [Customization guide](#customization-guide)
- [Accessibility & browser support](#accessibility--browser-support)
- [External resources](#external-resources)
- [Deployment](#deployment)

---

## Highlights

- **Single‑page experience** with smooth anchor navigation between sections.
- **Sticky navbar** — transparent over the hero, solid white once you scroll, with a
  scroll‑spy that highlights the active section.
- **Real mobile menu** — animated hamburger → slide‑in drawer with a dimmed backdrop,
  closing on link tap, backdrop click, or `Esc`.
- **Scroll‑reveal animations** for text and elements (fade/slide/zoom) with staggered delays.
- **Animated statistics** that count up when they enter the viewport.
- **Polish:** scroll‑progress bar, hero parallax + slow zoom, infinite logo marquee,
  hover lifts, back‑to‑top button, and a preloader.
- **Professional typography:** Space Grotesk (headings) + Inter (body).
- **Fully responsive** with dedicated breakpoints at 1024 / 860 / 600 px.
- **Accessible:** respects `prefers-reduced-motion` and uses semantic markup + ARIA labels.

## Tech stack

| Concern        | Choice                                                        |
| -------------- | ------------------------------------------------------------- |
| Markup         | Semantic HTML5                                                |
| Styling        | Hand‑written CSS with custom properties (design tokens)       |
| Behavior       | Vanilla JS (ES6, IntersectionObserver) — **no dependencies**  |
| Fonts          | Google Fonts — Space Grotesk + Inter                          |
| Icons          | Font Awesome 6 (via CDN)                                      |
| Imagery        | Unsplash (photos) + randomuser.me (avatars)                   |

## Project structure

```text
pm-mvp-forked/
├── index.html              # The entire single-page site (all sections)
├── css/
│   └── modern.css          # Design system + every component style
├── js/
│   └── modern.js           # All interactions & animations (no libraries)
├── images/
│   ├── favicon.ico         # Browser tab icon
│   └── apple-touch-icon.png# Home-screen icon (iOS)
└── readme.md               # This file
```

That's the whole project — three source files plus two icons. Everything else from the
original template (Bootstrap, jQuery, owl‑carousel, shop/blog/events pages, placeholder
images, preview videos, Flaticon fonts) was removed.

## Running locally

This is a static site, so there's no install step.

### Option A — open directly

Double‑click `index.html`, or open it in your browser. An internet connection is required
because fonts, icons, and imagery are loaded from CDNs.

### Option B — serve it (recommended)

Serving over `http://` avoids `file://` quirks and mirrors production:

```bash
# Python 3
python -m http.server 8080

# or Node
npx serve .
```

Then visit <http://localhost:8080>.

## How it works

### HTML — page sections

`index.html` is organized top to bottom into clearly commented sections. Each major
section has an `id` so the navbar and scroll‑spy can target it:

| Section      | `id`            | Notes                                            |
| ------------ | --------------- | ------------------------------------------------ |
| Navbar       | `#top`          | `header.nav` + `.nav-backdrop`                   |
| Hero         | `#home`         | Background image, headline, CTAs, animated stats |
| Logo marquee | —               | Infinite horizontal scroll of tool/brand names   |
| About        | `#about`        | Two-column intro with a floating badge card      |
| Stats band   | —               | Four count-up metrics on a dark background       |
| Courses      | `#courses`      | Responsive grid of six course cards              |
| Why us       | `#features`     | Six feature cards                                |
| Testimonials | `#testimonials` | Three student quotes with avatars                |
| News         | `#news`         | Three article cards                              |
| CTA          | —               | Conversion banner                                |
| Contact      | `#contact`      | Contact details + form                           |
| Footer       | —               | Links, socials, newsletter signup                |

The current year in the footer is filled in by a tiny inline script.

### CSS — design system

`css/modern.css` is a single, ordered stylesheet (≈21 numbered sections). It opens with a
**design‑token layer** in `:root` so the whole site can be re‑themed from one place:

```css
:root {
  --primary: #2563eb;     /* brand blue        */
  --accent:  #06b6d4;     /* brand cyan        */
  --ink:     #0b1220;     /* headings/text     */
  --navy:    #0a1530;     /* dark surfaces     */
  --grad: linear-gradient(120deg, var(--primary), var(--accent));
  --font-head: 'Space Grotesk', sans-serif;
  --font-body: 'Inter', sans-serif;
  --nav-h: 78px;          /* navbar height     */
  --container: 1180px;    /* max content width */
  /* …radii, shadows, easing… */
}
```

After the tokens, the file is grouped by concern: reset/base, layout helpers, buttons,
then one block per UI component (navbar, hero, about, stats, courses, features,
testimonials, news, CTA, contact, footer), followed by the animation utilities,
responsive overrides, and a `prefers-reduced-motion` block.

### JavaScript — interactions

`js/modern.js` is a single IIFE (no globals leaked) wiring up all behavior:

- **Preloader** — fades out on `window.load`.
- **Scroll handler** — toggles the solid navbar (`.scrolled`), grows the scroll‑progress
  bar, and shows/hides the back‑to‑top button.
- **Mobile menu** — toggles `body.menu-open`; closes on link click, backdrop click, or `Esc`.
- **Scroll reveal** — an `IntersectionObserver` adds `.in` to `[data-anim]` elements as
  they enter view (with optional per‑element delay).
- **Counters** — animates `[data-count]` numbers with an `easeOutCubic` curve and locale
  formatting when they become visible.
- **Scroll‑spy** — highlights the matching navbar link for the section in view.
- **Hero parallax** — subtle translate on the hero background while scrolling.
- **Forms** — `form[data-fake]` shows an inline success message instead of submitting
  (no backend wired up).

Everything degrades gracefully: if `IntersectionObserver` is missing or the user prefers
reduced motion, content is shown immediately and counters jump to their final value.

## The animation system

Animations are driven by **data attributes** in the HTML — no JS edits needed to animate a
new element.

**Reveal on scroll** — add `data-anim` (and optionally `data-delay` in milliseconds):

```html
<div data-anim="up">Fades up into view</div>
<div data-anim="left" data-delay="100">Slides in from the left, 100ms later</div>
```

| `data-anim` value | Effect                  |
| ----------------- | ----------------------- |
| `up` / `down`     | Fade + vertical slide   |
| `left` / `right`  | Fade + horizontal slide |
| `zoom`            | Fade + scale up         |
| `blur`            | Fade + de‑blur          |

**Count‑up numbers** — add `data-count` (and optional `data-suffix`):

```html
<span data-count="1200" data-suffix="+">0</span>   <!-- counts to 1,200+ -->
<span data-count="95"   data-suffix="%">0</span>   <!-- counts to 95%    -->
```

**Fake form submit** — add `data-fake` to a `<form>` and include a `.form-success`
element inside it to show a confirmation message on submit.

## Customization guide

- **Re‑theme the site:** change the color, font, and spacing variables in the `:root`
  block of `css/modern.css`. The gradient, buttons, accents, and dark sections all derive
  from these tokens.
- **Add a course / feature / news card:** copy an existing `<article>`/`<div>` block in the
  relevant section of `index.html` and edit its text, image, and `data-delay`.
- **Swap imagery:** replace the Unsplash URLs (hero background lives in `.hero-bg` inside
  `css/modern.css`; card images are inline `src` attributes in `index.html`). Drop in local
  files under `images/` and point to them if you prefer self‑hosted assets.
- **Change navigation:** edit the links in `header.nav` (`.nav-menu`). Each `href="#id"`
  must match a section `id` for smooth scroll and scroll‑spy to work.
- **Wire up the forms:** replace the `data-fake` handling in `js/modern.js` (and/or add a
  `action`/`method` to the `<form>`) to post to a real endpoint.

## Accessibility & browser support

- Honors `prefers-reduced-motion: reduce` — all transitions, parallax, and reveals are
  disabled and content is shown statically.
- Interactive controls have `aria-label`s; the mobile menu closes with `Esc`.
- `backdrop-filter` effects include `-webkit-` prefixes for iOS Safari.
- Targets evergreen browsers (Chrome, Edge, Firefox, Safari — desktop & mobile). The
  layout uses CSS Grid, custom properties, and `IntersectionObserver`, all broadly
  supported in current browsers.

## External resources

Loaded at runtime from CDNs (internet connection required):

- **Google Fonts** — Inter & Space Grotesk
- **Font Awesome 6.5.2** — iconography
- **Unsplash** — hero and card photography
- **randomuser.me** — testimonial avatars

To make the site fully offline‑capable, self‑host these assets under the project and update
the references in `index.html` / `css/modern.css`.

## Deployment

Because it's fully static, it can be hosted anywhere that serves files — GitHub Pages,
Netlify, Vercel, Cloudflare Pages, or any web server. Upload the project as‑is; `index.html`
is the entry point. No build command is required.
