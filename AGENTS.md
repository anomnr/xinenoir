## Development

When starting the dev server, use background mode:

```
astro dev --background
```

Manage the background server with `astro dev stop`, `astro dev status`, and `astro dev logs`.

## Documentation

Full documentation: https://docs.astro.build

Consult these guides before working on related tasks:

- [Adding pages, dynamic routes, or middleware](https://docs.astro.build/en/guides/routing/)
- [Working with Astro components](https://docs.astro.build/en/basics/astro-components/)
- [Using React, Vue, Svelte, or other framework components](https://docs.astro.build/en/guides/framework-components/)
- [Adding or managing content](https://docs.astro.build/en/guides/content-collections/)
- [Adding styles or using Tailwind](https://docs.astro.build/en/guides/styling/)
- [Supporting multiple languages](https://docs.astro.build/en/guides/internationalization/)

---

# XineNoir — Agent System Instructions

## 1. Project Identity & Vision

XineNoir is a cinematic digital archive and interactive landing page for the Maxine Faye fanpage. This is not an ordinary fanpage — it is a quiet, cinematic, and emotional aesthetic space.

### 1.1 Visual Direction

| Token | Value | Notes |
|---|---|---|
| **Theme** | Monochrome, midnight blue, cyber aesthetic, sinematik | Dark-tech vibe, not generic AI purple |
| **Background** | `#0F172A` (Midnight Blue) | CSS var: `--clr-deep-navy` |
| **Accent (primary)** | `#2563EB` | CSS var: `--clr-blue-accent` |
| **Accent (light)** | `#60A5FA` | CSS var: `--clr-blue-light` |
| **Text** | `#FFFFFF` | CSS var: `--clr-white` |
| **Muted text** | `#9CA3AF` | CSS var: `--clr-silver` |
| **Border radius** | `12px` | CSS var: `--radius-global` — enforced everywhere |
| **Font** | Poppins (300–700) | Display & body. Monospace: Courier New for labels/badges |

### 1.2 Design Principles

1. **Shape Consistency Lock**: Every card, button, modal, and input uses `var(--radius-global)`. No mixed radius values.
2. **Color Consistency Lock**: One accent (`--clr-blue-accent`). No random secondary accent colors mid-page. Exception: `#10b981` (emerald) for success/event states only.
3. **Anti-Center Bias**: Hero and CTA sections use left-aligned asymmetric layouts on desktop. Centered layouts are reserved for mobile fallbacks and editorial quotes only.
4. **Tactile Feedback**: All interactive elements implement `:active { transform: scale(0.98) }` for physical push feel.
5. **Viewport Stability**: Hero sections use `min-height: 100dvh`, never `100vh`, to prevent layout jump on mobile Safari.
6. **Film Grain Overlay**: A subtle SVG noise overlay (`opacity: 0.04`) is applied globally via a `.noise-overlay` div in `Layout.astro` for cinematic texture.

---

## 2. Technology Architecture

* **Framework**: Astro (Node.js v22+) — Zero JS by default for fast page loads.
* **Styling**: Vanilla CSS scoped inside `.astro` components via `<style>` blocks. No Tailwind, no CSS frameworks.
* **Interactivity**: Vanilla JavaScript inside `<script>` blocks in `.astro` components. No React, Vue, or Svelte.
* **Data Management**: Astro Content Collections (Markdown/JSON) for gallery and archive data (future).
* **Layout Rule**: In `src/layouts/Layout.astro`, `<body>` uses `display: flex; flex-direction: column;` to prevent components from collapsing horizontally. Global CSS variables and resets are defined here.

---

## 3. Component Specifications

Build `index.astro` by importing and composing the following components in order:

### 3.1 `HomeIntro.astro` — Hero / Opening

* Asymmetric layout: text anchored left, video fills the background with a left-to-right gradient overlay.
* Logo mark "N" (italic serif), site title "XineNoir", subtitle, and `#MaxineKeren` hashtag badge.
* Background video with monochrome filter (`grayscale(100%) contrast(110%) brightness(0.5)`).
* Max 4 text elements in the hero stack. Top padding capped at `6rem`.
* Entry animation: `cinematicFadeIn` — elements slide in from the left with blur-to-sharp transition.
* Mobile: centers content and switches gradient to vertical.

### 3.2 `Gallery.astro` — Visual Archives

* **Layout**: Asymmetric bento grid (`grid-template-columns: repeat(3, 1fr)`). One highlight card spans 2 columns + 2 rows.
* Each card has a background image with monochrome filter that transitions to color on hover, with a subtle `scale(1.05)` zoom.
* Categories: JKT48, Fancam Maxine, Special Performance (expandable in future).
* Content overlay at the bottom of each card with `[01]` style badges, title, and date.
* Mobile: single-column stack.

### 3.3 `MusicCorner.astro` — Music Player

* **Reference UI**: Inspired by `https://www.noartmusic.com/label`.
* Wireframe layout with white border lines and corner brackets on dark background.
* CD/cassette-style interface with interactive Play button for audio/video covers.
* Track listing uses bracket numbering: `[01]`, `[02]`, etc.
* **CSS Isolation**: Uses `position: absolute` or strong component scoping to avoid style conflicts with `Layout.astro`.

### 3.4 `Countdown.astro` — Chronicles Dashboard

* **Layout**: CSS Grid bento layout (`grid-template-columns: repeat(3, 1fr)`) with asymmetric cell sizes.
* Primary countdown (birthday) spans 2 columns + 2 rows with a blue glow effect.
* Visual variation: some cells use solid black backgrounds (`.solid-bg`), others use tinted radial gradients (`.tinted-bg`).
* Data displayed: birthday countdown, Instagram followers, first performance timestamp, next event date, anniversary.
* Monospace `Courier New` for all numeric values. Scanline overlay for cyber aesthetic.
* Section header is left-aligned (not centered).

### 3.5 `Merch.astro` — Merchandise (Placeholder)

* Coming Soon placeholder: "Coming Soon — Maxine Merchandise".
* Placeholder for merchandise, fan projects, or pre-order functionality.

### 3.6 `MiniGame.astro` — Memory Test Trivia

* Interactive quiz with silhouette mystery box and multiple-choice answers.
* Option buttons use `var(--radius-global)`, `background: rgba(255,255,255,0.05)` for contrast, and slide right on hover.
* Correct answer: green highlight (`#10b981`), "Benar! Kamu Maxine Expert".
* Wrong answer: red highlight (`#ef4444`), "Belum tepat, coba lagi!".
* Result box animates in with `slideUp` keyframe.

### 3.7 `Quotes.astro` — Motivational Card

* 3D flip card: front shows a quote prompt ("Butuh semangat hari ini? Tap the card"), back reveals a video of Maxine.
* Uses `perspective`, `backface-visibility: hidden`, and `rotateY(180deg)` for the flip.
* Close button on the back flips the card back and resets the video.
* `z-index: 5` on the section to prevent flip animation from clipping behind adjacent sections.

### 3.8 `Community.astro` — WhatsApp CTA

* **Layout**: Left-aligned asymmetric block (not centered). Max-width container `1200px`.
* Single CTA button: "JOIN WHATSAPP ↗" — uses `var(--clr-blue-accent)` background, white text, `var(--radius-global)`.
* Button text must never wrap (`white-space: nowrap`).
* Top-left accent line (200px blue bar) as a visual anchor.

### 3.9 `Notes.astro` — Fan Message Board

* Masonry layout using CSS `column-count: 3` (responsive: 2 on tablet, 1 on mobile).
* Cards use `var(--radius-global)` with a hard shadow offset (`4px 4px 0px`).
* "Add note" card uses dashed border, triggers a modal overlay for input.
* Modal: `backdrop-filter: blur(5px)`, fade-in/out with opacity transition, content slides up.
* Submit button: full-width, `var(--clr-blue-accent)` background, `white-space: nowrap`.
* Placeholder text contrast: `rgba(255,255,255,0.4)` — passes WCAG AA against dark input backgrounds.

### 3.10 `CollectibleCards.astro` — Digital Collection

* Card dimensions: 240px × 350px. `var(--radius-global)` border radius.
* Unlocked cards: monochrome image that desaturates on hover, holographic glare sweep using `mix-blend-mode: hard-light`.
* Locked cards: dashed border, lock icon at 20% opacity, "???" status.
* Hover: `translateY(-15px) scale(1.05)` with blue box-shadow. Active: `scale(0.98)` tactile push.
* Glare animation: gradient sweeps from off-screen to visible on hover via `transform: translateX`.

### 3.11 `Footer.astro`

* Closing section with social media links for the fanpage.
* `#MaxineKeren` hashtag displayed prominently.
* Quiet, minimal design — serves as the "end of the silence".

---

## 4. Navigation Structure

Site navigation follows this hierarchy (single-page scroll targets):

| Menu Item | Target Section | Content |
|---|---|---|
| HOME | `#home` | Pengenalan Maxine |
| GALLERY | `#gallery` | Album & Fancam |
| MUSIC | `#music` | Cover / CD Player |
| COUNTDOWN | `#countdown` | Birthday & Social Media |
| GAME | `#game` | Tebak-Tebakan |
| QUOTES | `#quotes` | Quote + Video Maxine |
| NOTES | `#notes` | Pesan untuk Maxine |
| COLLECTION | `#collection` | Collectible Cards |
| MERCH | `#merch` | Merchandise / Fan Project |
| COMMUNITY | `#community` | WhatsApp Have Fun |

Navigation must render on a single line at desktop. On mobile, use a hamburger menu.

---

## 5. Implementation Rules

### 5.1 CSS Variable Usage
Always use CSS variables from `Layout.astro` instead of hardcoded values:
```css
/* Correct */
border-radius: var(--radius-global);
color: var(--clr-blue-accent);

/* Wrong */
border-radius: 12px;
color: #2563eb;
```

### 5.2 Layout Patterns
* **Bento grids**: Use explicit `grid-template-columns` with named spans. Never use `auto-fit` with `minmax` for important layouts — it produces unpredictable cell sizes.
* **Section headers**: Left-aligned by default (not centered). Exception: MiniGame and CollectibleCards which are narrower, centered-content components.
* **Max widths**: Content containers cap at `1200px` or `1400px` with `margin: 0 auto`.

### 5.3 Animation Guidelines
* Entry animations: Use `opacity: 0` → `1` with subtle `transform` (translateX, translateY, or blur). No bouncing or elastic easing on page load.
* Hover animations: Max `0.4s` duration. Use `cubic-bezier(0.175, 0.885, 0.32, 1.275)` for card lifts.
* Infinite animations: Only for ambient indicators (blinking dot, bouncing tap indicator). Never on large elements.
* Respect `prefers-reduced-motion`: Disable non-essential animations.

### 5.4 Responsive Breakpoints
* `900px`: Bento grids collapse to single column. Gallery and Countdown stack vertically.
* `768px`: Hero centers content. Masonry drops to 2 columns. Nav switches to hamburger.
* `600px`: Masonry drops to 1 column. Card containers switch to full-width vertical stack.

### 5.5 Accessibility
* All buttons must pass WCAG AA contrast ratio (4.5:1 for body text, 3:1 for large text 18px+).
* No placeholder-as-label on form inputs. Labels above inputs, errors below.
* Focus states must be visible (use `outline` or `border-color` change, not just color shift).
* Interactive elements must have `cursor: pointer`.