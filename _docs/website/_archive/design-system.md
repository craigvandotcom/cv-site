# neoMeta Website Design System

**Version:** 1.0
**Last Updated:** 2026-01-19
**Status:** Ready for Developer Handoff

---

## Brand Foundation

**Aesthetic:** Blueprint Web - Technical precision meets modern UI
**Mood:** Authentic, functional, grounded, innovative
**Inspiration:** Architectural drawings, engineering blueprints, systems thinking

---

## Color Palette

### Core Colors (Required)

| Token | Hex | RGB | Usage |
|-------|-----|-----|-------|
| `--color-bg` | `#1a1a1a` | 26, 26, 26 | Primary background |
| `--color-bg-elevated` | `#242424` | 36, 36, 36 | Cards, elevated surfaces |
| `--color-text` | `#F5F0E6` | 245, 240, 230 | Primary text, borders |
| `--color-text-muted` | `#A8A8A8` | 168, 168, 168 | Secondary text |
| `--color-accent` | `#01A878` | 1, 168, 120 | Accent, glow, highlights |
| `--color-accent-dim` | `#016B4F` | 1, 107, 79 | Hover states, subtle accents |

### Accent Usage

**Green (#01A878) is ALWAYS used as:**
- Glow effects (box-shadow, text-shadow)
- Border accents (1-2px highlights)
- Interactive state indicators
- Badge backgrounds (LIVE status)

**Green is NEVER used as:**
- Solid background fills
- Large text blocks
- Primary button backgrounds

### Status Colors (Badges Only)

| Status | Background | Text | Border |
|--------|------------|------|--------|
| LIVE | `#01A878` | `#1a1a1a` | none |
| COMING | transparent | `#F5F0E6` | `#F5F0E6` |
| BUILDING | transparent | `#F7931A` | `#F7931A` |

---

## Typography

### Font Stack

```css
--font-heading: 'Space Grotesk', 'Inter', system-ui, sans-serif;
--font-body: 'Inter', 'SF Pro Text', system-ui, sans-serif;
--font-mono: 'JetBrains Mono', 'Fira Code', monospace;
```

**Primary Choice:** Space Grotesk (headings) + Inter (body)
**Fallback:** System fonts for performance

### Type Scale

| Token | Size | Weight | Line Height | Usage |
|-------|------|--------|-------------|-------|
| `--text-hero` | 72px / 4.5rem | 700 | 1.1 | Hero name |
| `--text-h1` | 48px / 3rem | 700 | 1.2 | Page titles |
| `--text-h2` | 36px / 2.25rem | 600 | 1.25 | Section headers |
| `--text-h3` | 24px / 1.5rem | 600 | 1.3 | Card titles |
| `--text-h4` | 20px / 1.25rem | 500 | 1.4 | Subsections |
| `--text-body` | 16px / 1rem | 400 | 1.6 | Body text |
| `--text-small` | 14px / 0.875rem | 400 | 1.5 | Captions, labels |
| `--text-micro` | 12px / 0.75rem | 500 | 1.4 | Badges, metadata |

### Text Styles

**Accent Word Glow:**
```css
.accent-glow {
  color: #01A878;
  text-shadow: 0 0 20px rgba(1, 168, 120, 0.6),
               0 0 40px rgba(1, 168, 120, 0.3);
}
```

**Uppercase Labels:**
```css
.label {
  font-size: var(--text-micro);
  font-weight: 600;
  letter-spacing: 0.1em;
  text-transform: uppercase;
}
```

---

## Spacing System

### Base Unit: 4px

| Token | Value | Usage |
|-------|-------|-------|
| `--space-1` | 4px | Tight gaps |
| `--space-2` | 8px | Icon padding, small gaps |
| `--space-3` | 12px | Input padding |
| `--space-4` | 16px | Card padding, component gaps |
| `--space-6` | 24px | Section padding |
| `--space-8` | 32px | Large gaps |
| `--space-12` | 48px | Section margins |
| `--space-16` | 64px | Major section breaks |
| `--space-24` | 96px | Page section spacing |

### Layout

| Token | Value | Usage |
|-------|-------|-------|
| `--max-width` | 1200px | Content container |
| `--max-width-narrow` | 800px | Text content |
| `--gutter` | 24px | Side padding (mobile: 16px) |

---

## Components

### Navigation

```
Height: 64px
Background: var(--color-bg) with subtle backdrop blur
Border-bottom: 1px solid rgba(245, 240, 230, 0.1)
Logo: Left aligned, 32px height
Links: Right aligned, 14px, 500 weight, 24px gap
CTA Button: Outlined style (see buttons)
```

### Buttons

**Primary (Outlined):**
```css
.btn-primary {
  background: transparent;
  border: 1px solid var(--color-accent);
  color: var(--color-text);
  padding: 12px 24px;
  font-size: 14px;
  font-weight: 500;
  letter-spacing: 0.02em;
  transition: all 0.2s ease;
}

.btn-primary:hover {
  background: var(--color-accent);
  color: var(--color-bg);
  box-shadow: 0 0 20px rgba(1, 168, 120, 0.4);
}
```

**Secondary (Ghost):**
```css
.btn-secondary {
  background: transparent;
  border: 1px solid var(--color-text);
  color: var(--color-text);
  padding: 12px 24px;
}

.btn-secondary:hover {
  background: rgba(245, 240, 230, 0.1);
}
```

### Cards (Pillar Cards)

```css
.card {
  background: var(--color-bg-elevated);
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
  border-color: var(--color-accent);
  box-shadow: 0 0 30px rgba(1, 168, 120, 0.15);
}
```

**Card Content Structure:**
```
[Badge] ← top right, absolute positioned
[Icon] ← 64x64px, line art style, centered
[Title] ← h3, centered
[Subtitle] ← product name, small caps
[Description] ← body text, centered, max 2 lines
```

### Form Inputs

```css
.input {
  background: transparent;
  border: 1px solid rgba(245, 240, 230, 0.3);
  color: var(--color-text);
  padding: 12px 16px;
  font-size: 16px;
  width: 100%;
  transition: border-color 0.2s;
}

.input:focus {
  outline: none;
  border-color: var(--color-accent);
  box-shadow: 0 0 0 2px rgba(1, 168, 120, 0.2);
}

.input::placeholder {
  color: var(--color-text-muted);
}
```

**Newsletter Form Layout:**
```
[Email Input (flex: 1)] [Submit Button (auto)]
Gap: 12px
Max-width: 480px
Centered in container
```

### Badges

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
  background: var(--color-accent);
  color: var(--color-bg);
}

.badge-coming {
  background: transparent;
  border: 1px solid var(--color-text);
  color: var(--color-text);
}
```

---

## Effects

### Green Glow

```css
/* Subtle glow for borders/accents */
--glow-subtle: 0 0 10px rgba(1, 168, 120, 0.3);

/* Medium glow for hover states */
--glow-medium: 0 0 20px rgba(1, 168, 120, 0.4);

/* Strong glow for emphasis */
--glow-strong: 0 0 30px rgba(1, 168, 120, 0.5),
               0 0 60px rgba(1, 168, 120, 0.2);
```

### Blueprint Corner Markers

Used on cards, sections, and decorative elements:
```css
/* L-shaped corner markers */
.corner-marker {
  position: absolute;
  width: 16px;
  height: 16px;
  border-color: rgba(245, 240, 230, 0.2);
  border-style: solid;
}

.corner-tl { top: 0; left: 0; border-width: 1px 0 0 1px; }
.corner-tr { top: 0; right: 0; border-width: 1px 1px 0 0; }
.corner-bl { bottom: 0; left: 0; border-width: 0 0 1px 1px; }
.corner-br { bottom: 0; right: 0; border-width: 0 1px 1px 0; }
```

### Subtle Grid Background (Optional)

```css
.blueprint-grid {
  background-image:
    linear-gradient(rgba(245, 240, 230, 0.03) 1px, transparent 1px),
    linear-gradient(90deg, rgba(245, 240, 230, 0.03) 1px, transparent 1px);
  background-size: 40px 40px;
}
```

---

## Icons

**Style:** Line art, single stroke weight (1.5-2px)
**Color:** `var(--color-text)` default, accent on hover
**Size:** 24px (nav), 48-64px (cards), 20px (inline)

**Pillar Icons (Custom):**
- EAT: Fork + knife with compass/plate
- MOVE: Dynamic human figure in motion
- MEDITATE: Seated figure with emanating circles

**Social Icons:** Standard line versions of X, YouTube, GitHub

---

## Responsive Breakpoints

| Token | Value | Description |
|-------|-------|-------------|
| `--bp-sm` | 640px | Mobile landscape |
| `--bp-md` | 768px | Tablet |
| `--bp-lg` | 1024px | Desktop |
| `--bp-xl` | 1280px | Large desktop |

### Mobile Adjustments

```css
@media (max-width: 768px) {
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

## Page Sections

### Hero Section

```
Padding: 96px 0 64px
Background: var(--color-bg)
Content: Centered, max-width 800px

Structure:
- Name (hero size, cream)
- Tagline (h2 size, "life" with accent glow)
- Subtagline (body size, muted)
- Newsletter form (centered, max 480px)
- Micro-copy below form (small, muted)
```

### Pillars Section

```
Padding: 64px 0
Background: var(--color-bg)

Structure:
- Section title (h2, centered)
- 3-column grid (gap: 24px)
- Cards with equal height
```

### Footer

```
Padding: 32px 0
Border-top: 1px solid rgba(245, 240, 230, 0.1)
Background: var(--color-bg)

Layout: Flex, space-between
- Left: Copyright
- Center: Social icons (24px, 16px gap)
- Right: neoMeta wordmark
```

---

## Animation Guidelines

**Principles:**
- Subtle, purposeful motion
- No flashy or distracting animations
- Enhance perceived performance

**Transitions:**
```css
--transition-fast: 0.15s ease;
--transition-base: 0.2s ease;
--transition-slow: 0.3s ease;
```

**Hover Effects:**
- Buttons: Background fill + glow (0.2s)
- Cards: Border color + subtle glow (0.2s)
- Links: Underline or color shift (0.15s)

**Page Load:**
- Fade in sections sequentially (stagger 100ms)
- No heavy animations that delay content visibility

---

## Do / Don't

### DO
- Use green as glow/accent only
- Maintain high contrast (cream on charcoal)
- Keep layouts clean with generous whitespace
- Use blueprint corner markers sparingly
- Ensure all text is readable

### DON'T
- Use green as solid fills
- Add decorative elements without purpose
- Use more than 2 font weights per element
- Animate everything
- Compromise readability for aesthetics

---

## Asset Checklist

**Required before build:**
- [ ] Space Grotesk font files (or Google Fonts link)
- [ ] Inter font files (or Google Fonts link)
- [ ] Custom pillar icons (EAT, MOVE, MEDITATE) - SVG
- [ ] Social icons (X, YouTube, GitHub) - SVG
- [ ] Logo/wordmark - SVG
- [ ] Favicon - ICO + PNG (16, 32, 180, 192, 512)

---

## Reference Mockups

Located at: `infrastructure/tools/src/image-gen/output/website-mvp/`

- `v2-hero-v1.png` - Full homepage reference (BEST)
- `v2-pillars-v1.png` - Pillar cards detail
- `v2-footer-v1.png` - Footer reference

---

**Next Steps:**
1. Finalize tech stack (see tech-stack.md)
2. Source/create icon assets
3. Set up project repository
4. Begin implementation
