# craigvan.com - Technical & Design Specification

**Version:** 1.0
**Last Updated:** 2026-01-19
**Status:** Definitive Specification

---

## Technical Stack

### Core

| Layer | Choice | Version |
|-------|--------|---------|
| Framework | Astro | 5.x |
| Styling | Tailwind CSS | 4.x |
| Hosting | Vercel | - |
| Package Manager | pnpm | 9.x |
| Node | Node.js | 20.x LTS |

### Integrations

| Service | Purpose | Integration |
|---------|---------|-------------|
| Substack | Newsletter | SubstackAPI.com embed |
| Umami | Analytics | Self-hosted on Vercel |
| Vercel | Hosting + CDN | GitHub auto-deploy |

### Domain Architecture

```
craigvan.com             → Astro (this project)
analytics.craigvan.com   → Umami (self-hosted)
```

---

## Project Structure

```
software/website/
├── src/
│   ├── components/
│   │   ├── Header.astro
│   │   ├── Footer.astro
│   │   ├── PillarCard.astro
│   │   ├── Badge.astro
│   │   ├── NewsletterForm.astro
│   │   ├── PosterForm.astro
│   │   └── Button.astro
│   ├── layouts/
│   │   └── BaseLayout.astro
│   ├── pages/
│   │   ├── index.astro         # Home
│   │   └── eat.astro           # EAT journey
│   └── styles/
│       └── global.css
├── public/
│   ├── fonts/
│   ├── favicon/                # From assets/favicon/
│   ├── icons/                  # From assets/icons/
│   ├── images/
│   └── logo/                   # From assets/logo/
├── assets/                     # Source assets (already populated)
│   ├── favicon/
│   ├── icons/
│   └── logo/
├── astro.config.mjs
├── tailwind.config.js
├── package.json
├── PRD.md
├── SPEC.md                     # This file
└── IMPLEMENTATION.md
```

---

## Design System

### Brand Foundation

**Aesthetic:** Blueprint Web
**Mood:** Technical precision, modern UI, authentic, functional
**Inspiration:** Architectural drawings, engineering blueprints

---

### Color Palette

#### Core Colors

| Token | Hex | RGB | Tailwind | Usage |
|-------|-----|-----|----------|-------|
| `bg` | `#1a1a1a` | 26, 26, 26 | `bg-bg` | Primary background |
| `bg-elevated` | `#242424` | 36, 36, 36 | `bg-bg-elevated` | Cards, surfaces |
| `text` | `#F5F0E6` | 245, 240, 230 | `text-text` | Primary text, borders |
| `text-muted` | `#A8A8A8` | 168, 168, 168 | `text-text-muted` | Secondary text |
| `accent` | `#01A878` | 1, 168, 120 | `text-accent` | Glow, highlights |
| `accent-dim` | `#016B4F` | 1, 107, 79 | `text-accent-dim` | Hover states |

#### Tailwind Config

```js
// tailwind.config.js
export default {
  theme: {
    extend: {
      colors: {
        'bg': '#1a1a1a',
        'bg-elevated': '#242424',
        'text': '#F5F0E6',
        'text-muted': '#A8A8A8',
        'accent': '#01A878',
        'accent-dim': '#016B4F',
      },
    }
  }
}
```

#### Accent Usage Rules

**Green (#01A878) IS used for:**
- Glow effects (box-shadow, text-shadow)
- Border accents (1-2px highlights)
- Interactive state indicators
- Badge backgrounds (LIVE status)

**Green is NEVER used for:**
- Solid background fills (large areas)
- Large text blocks
- Primary button backgrounds

---

### Typography

#### Font Stack

```css
--font-heading: 'Space Grotesk', 'Inter', system-ui, sans-serif;
--font-body: 'Inter', 'SF Pro Text', system-ui, sans-serif;
--font-mono: 'JetBrains Mono', 'Fira Code', monospace;
```

#### Tailwind Config

```js
fontFamily: {
  'heading': ['Space Grotesk', 'Inter', 'system-ui', 'sans-serif'],
  'body': ['Inter', 'SF Pro Text', 'system-ui', 'sans-serif'],
  'mono': ['JetBrains Mono', 'Fira Code', 'monospace'],
},
```

#### Type Scale

| Token | Size | Weight | Line Height | Usage |
|-------|------|--------|-------------|-------|
| `hero` | 72px / 4.5rem | 700 | 1.1 | Hero headlines |
| `h1` | 48px / 3rem | 700 | 1.2 | Page titles |
| `h2` | 36px / 2.25rem | 600 | 1.25 | Section headers |
| `h3` | 24px / 1.5rem | 600 | 1.3 | Card titles |
| `h4` | 20px / 1.25rem | 500 | 1.4 | Subsections |
| `body` | 16px / 1rem | 400 | 1.6 | Body text |
| `small` | 14px / 0.875rem | 400 | 1.5 | Captions |
| `micro` | 12px / 0.75rem | 500 | 1.4 | Badges |

#### Accent Word Glow

```css
.accent-glow {
  color: #01A878;
  text-shadow: 0 0 20px rgba(1, 168, 120, 0.6),
               0 0 40px rgba(1, 168, 120, 0.3);
}
```

---

### Spacing System

#### Base Unit: 4px

| Token | Value | Usage |
|-------|-------|-------|
| `space-1` | 4px | Tight gaps |
| `space-2` | 8px | Icon padding |
| `space-3` | 12px | Input padding |
| `space-4` | 16px | Card padding |
| `space-6` | 24px | Section padding |
| `space-8` | 32px | Large gaps |
| `space-12` | 48px | Section margins |
| `space-16` | 64px | Major breaks |
| `space-24` | 96px | Page sections |

#### Layout

| Token | Value | Usage |
|-------|-------|-------|
| `max-width` | 1200px | Content container |
| `max-width-narrow` | 800px | Text content |
| `gutter` | 24px | Side padding |

---

### Components

#### Navigation Header

```
Height: 64px
Background: bg with backdrop blur
Border-bottom: 1px solid rgba(245, 240, 230, 0.1)
Logo: Left, 32px height
CTA: Right, outlined button
```

#### Buttons

**Primary (Outlined):**
```css
.btn-primary {
  background: transparent;
  border: 1px solid var(--accent);
  color: var(--text);
  padding: 12px 24px;
  font-size: 14px;
  font-weight: 500;
  letter-spacing: 0.02em;
  transition: all 0.2s ease;
}

.btn-primary:hover {
  background: var(--accent);
  color: var(--bg);
  box-shadow: 0 0 20px rgba(1, 168, 120, 0.4);
}
```

**Secondary (Ghost):**
```css
.btn-secondary {
  background: transparent;
  border: 1px solid var(--text);
  color: var(--text);
  padding: 12px 24px;
}

.btn-secondary:hover {
  background: rgba(245, 240, 230, 0.1);
}
```

#### Pillar Cards

```css
.card {
  background: var(--bg-elevated);
  border: 1px solid rgba(245, 240, 230, 0.2);
  padding: 32px;
  position: relative;
}

/* Blueprint corner markers */
.card::before,
.card::after {
  content: '';
  position: absolute;
  width: 12px;
  height: 12px;
  border-color: rgba(245, 240, 230, 0.3);
  border-style: solid;
}

.card::before {
  top: 8px;
  left: 8px;
  border-width: 1px 0 0 1px;
}

.card::after {
  bottom: 8px;
  right: 8px;
  border-width: 0 1px 1px 0;
}

.card:hover {
  border-color: var(--accent);
  box-shadow: 0 0 30px rgba(1, 168, 120, 0.15);
}
```

**Card Structure:**
```
[Badge]         ← top right, absolute
[Icon]          ← 64x64px, centered
[Title]         ← h3, centered
[Subtitle]      ← product name, small
[Description]   ← body text, centered
```

#### Badges

```css
.badge {
  display: inline-block;
  padding: 4px 12px;
  font-size: 11px;
  font-weight: 600;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  border-radius: 2px;
}

.badge-live {
  background: var(--accent);
  color: var(--bg);
}

.badge-coming {
  background: transparent;
  border: 1px solid var(--text);
  color: var(--text);
}
```

#### Form Inputs

```css
.input {
  background: transparent;
  border: 1px solid rgba(245, 240, 230, 0.3);
  color: var(--text);
  padding: 12px 16px;
  font-size: 16px;
  width: 100%;
  transition: border-color 0.2s;
}

.input:focus {
  outline: none;
  border-color: var(--accent);
  box-shadow: 0 0 0 2px rgba(1, 168, 120, 0.2);
}

.input::placeholder {
  color: var(--text-muted);
}
```

#### Footer

```
Padding: 32px 0
Border-top: 1px solid rgba(245, 240, 230, 0.1)
Layout: Flex, space-between
- Left: Copyright
- Center: Social icons (24px, 16px gap)
- Right: Wordmark
```

---

### Effects

#### Green Glow

```css
--glow-subtle: 0 0 10px rgba(1, 168, 120, 0.3);
--glow-medium: 0 0 20px rgba(1, 168, 120, 0.4);
--glow-strong: 0 0 30px rgba(1, 168, 120, 0.5),
               0 0 60px rgba(1, 168, 120, 0.2);
```

#### Blueprint Corner Markers

```css
.corner-tl { top: 0; left: 0; border-width: 1px 0 0 1px; }
.corner-tr { top: 0; right: 0; border-width: 1px 1px 0 0; }
.corner-bl { bottom: 0; left: 0; border-width: 0 0 1px 1px; }
.corner-br { bottom: 0; right: 0; border-width: 0 1px 1px 0; }
```

---

### Responsive Breakpoints

| Name | Width | Usage |
|------|-------|-------|
| `sm` | 640px | Mobile landscape |
| `md` | 768px | Tablet |
| `lg` | 1024px | Desktop |
| `xl` | 1280px | Large desktop |

#### Mobile Adjustments

```css
@media (max-width: 768px) {
  /* Typography */
  --text-hero: 40px;
  --text-h1: 32px;
  --text-h2: 24px;
  --gutter: 16px;

  /* Stack pillar cards vertically */
  .pillars-grid {
    grid-template-columns: 1fr;
    gap: 24px;
  }

  /* Full-width buttons */
  .btn { width: 100%; }
}
```

---

### Animations

#### Principles
- Subtle, purposeful motion
- Enhance perceived performance
- No flashy distractions

#### Transitions

```css
--transition-fast: 0.15s ease;
--transition-base: 0.2s ease;
--transition-slow: 0.3s ease;
```

#### Hover Effects
- Buttons: Background fill + glow (0.2s)
- Cards: Border color + subtle glow (0.2s)
- Links: Color shift (0.15s)

---

## Page Layouts

### Home Page (`/`)

```
┌─────────────────────────────────────┐
│            HEADER                   │
│  [Logo]              [Newsletter]   │
├─────────────────────────────────────┤
│                                     │
│            HERO SECTION             │
│                                     │
│         Craig van Heerden           │
│       eat. move. meditate.          │
│                                     │
│      [Email] [Subscribe]            │
│                                     │
├─────────────────────────────────────┤
│                                     │
│         THREE PILLARS               │
│                                     │
│   ┌─────┐  ┌─────┐  ┌─────┐        │
│   │ EAT │  │MOVE │  │ MED │        │
│   │     │  │     │  │     │        │
│   │LIVE │  │SOON │  │SOON │        │
│   └─────┘  └─────┘  └─────┘        │
│                                     │
├─────────────────────────────────────┤
│            FOOTER                   │
│  © 2026     [X][YT][GH]    neoMeta │
└─────────────────────────────────────┘
```

### EAT Journey Page (`/eat`)

```
┌─────────────────────────────────────┐
│            HEADER                   │
├─────────────────────────────────────┤
│                                     │
│            HERO                     │
│         Body Compass                │
│    Nutrition sovereignty            │
│                                     │
├─────────────────────────────────────┤
│                                     │
│      POSTER DOWNLOAD SECTION        │
│                                     │
│   [Poster Preview]                  │
│                                     │
│   Get the Body Compass Poster       │
│   [Email____________] [Send]        │
│                                     │
├─────────────────────────────────────┤
│                                     │
│         APP SECTION                 │
│                                     │
│   Track with the App   [LIVE]       │
│                                     │
│   [App Screenshot]                  │
│                                     │
│   [Download] [Learn More]           │
│                                     │
├─────────────────────────────────────┤
│                                     │
│       COURSE SECTION                │
│                                     │
│   Body Compass Course  [COMING]     │
│   11 lessons...                     │
│                                     │
├─────────────────────────────────────┤
│            FOOTER                   │
└─────────────────────────────────────┘
```

---

## SEO Metadata

| Page | Title | Description |
|------|-------|-------------|
| Home | Craig van Heerden - eat. move. meditate. | Building Life Operating Systems. Start with health sovereignty. |
| EAT | Body Compass - Craig van Heerden | Nutrition sovereignty. Reclaim your intuitive relationship with food. |

---

## Performance Targets

| Metric | Target |
|--------|--------|
| Lighthouse Performance | 90+ |
| Lighthouse Accessibility | 95+ |
| Lighthouse Best Practices | 90+ |
| Lighthouse SEO | 90+ |
| First Contentful Paint | < 1.5s |
| Time to Interactive | < 3s |

---

## Assets Reference

| Asset | Path | Format |
|-------|------|--------|
| Logo (light) | `assets/logo/cv-logo-light.png` | PNG |
| Favicon | `assets/favicon/` | ICO, PNG |
| EAT icon | `assets/icons/pillar-eat-styled.png` | PNG |
| MOVE icon | `assets/icons/pillar-move-styled.png` | PNG |
| MEDITATE icon | `assets/icons/pillar-meditate-styled.png` | PNG |

---

## Visual Mockups

**Location:** `/infrastructure/tools/src/image-gen/output/website-mvp/`

| Section | Files | Decision |
|---------|-------|----------|
| Hero | `v2-hero-v1.png`, `v2-hero-v2.png` | Select before build |
| Pillars | `v2-pillars-v1.png`, `v2-pillars-v2.png` | Select before build |
| Footer | `v2-footer-v1.png`, `v2-footer-v2.png` | Select before build |
| Full page | `homepage-full-v1.png`, `homepage-full-v2.png` | Reference only |

---

## Environment Variables

### Required

```env
# Umami Analytics
PUBLIC_UMAMI_WEBSITE_ID=your-website-id
PUBLIC_UMAMI_SRC=https://analytics.craigvan.com/script.js

# Site
PUBLIC_SITE_URL=https://craigvan.com
```

### Optional (Future)

```env
# SubstackAPI (if using their JS embed)
PUBLIC_SUBSTACK_PUBLICATION=craigvandotcom
```

**Note:** All public env vars in Astro must be prefixed with `PUBLIC_`.

---

## Open Graph & Social Meta

### Home Page

```html
<meta property="og:title" content="Craig van Heerden - eat. move. meditate." />
<meta property="og:description" content="Building Life Operating Systems. Start with health sovereignty." />
<meta property="og:image" content="https://craigvan.com/og/home.png" />
<meta property="og:url" content="https://craigvan.com" />
<meta property="og:type" content="website" />
<meta name="twitter:card" content="summary_large_image" />
<meta name="twitter:site" content="@craigvandotcom" />
```

### EAT Page

```html
<meta property="og:title" content="Body Compass - Craig van Heerden" />
<meta property="og:description" content="Nutrition sovereignty. Reclaim your intuitive relationship with food." />
<meta property="og:image" content="https://craigvan.com/og/eat.png" />
<meta property="og:url" content="https://craigvan.com/eat" />
```

**OG Image Sizes:** 1200x630px recommended

---

## Social Icons

### Implementation Options

**Option A: Lucide Icons (Recommended)**
```bash
pnpm add lucide-astro
```

```astro
---
import { Twitter, Youtube, Github } from 'lucide-astro';
---
<a href="https://x.com/craigvandotcom"><Twitter size={24} /></a>
```

**Option B: Inline SVG**
Include SVG icons directly in Footer component.

**Option C: Simple Astro Icons**
```bash
pnpm add astro-icon
```

### Icon Specs
- Size: 24x24px
- Color: `text-text-muted` (hover: `text-accent`)
- Gap: 16px between icons

---

## Contact & Social

| Platform | Handle | URL |
|----------|--------|-----|
| X | @craigvandotcom | https://x.com/craigvandotcom |
| YouTube | @craigvandotcom | https://youtube.com/@craigvandotcom |
| GitHub | @craigvandotcom | https://github.com/craigvandotcom |
| Substack | craigvandotcom | https://craigvandotcom.substack.com |
| Email | - | mail@craigvan.com |
| Body Compass | - | https://bodycompass.app |

---

**This is the definitive specification. See `IMPLEMENTATION.md` for build sequence.**
