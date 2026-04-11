---
name: design-system
description: Blueprint Web design system for neoMeta website. Desktop-first responsive design, green accent glow aesthetic, technical precision meets modern UI. Use for UI components, styling, colors, spacing, typography.
---

# Design System: Blueprint Web

**Purpose:** Guide consistent, accessible, desktop-first UI development
**Domain:** UI components, styling, visual effects, responsive design
**Status:** Complete (v1.0)

---

## When to Use This Skill

**Intent Triggers:**

- Building UI components or page layouts
- Choosing colors, typography, or spacing
- Implementing visual effects (glows, corner markers)
- Working on responsive design
- Creating buttons, cards, forms, badges

**When NOT to Use:**

- Content writing (use `writing-guidelines` skill)
- Backend/API work (N/A - static site)
- Astro framework patterns (use `astro` skill)

---

## Core Principles

### 1. Desktop-First Responsive

**Primary viewport:** 1200-1920px desktop
**Responsive breakpoints:** 1024px (tablet), 768px (mobile)

```css
/* ✅ CORRECT: Desktop default, mobile adjustment */
.container {
  max-width: 1200px;
  padding: 0 24px;
}

@media (max-width: 768px) {
  .container {
    padding: 0 16px;
  }
}

/* ❌ WRONG: Mobile-first (not this project) */
.container {
  padding: 0 16px;
}

@media (min-width: 768px) {
  .container {
    padding: 0 24px;
  }
}
```

### 2. Semantic Color Tokens

**Always use Tailwind custom tokens, never hardcoded hex values:**

| Token              | Hex       | Usage                     |
| ------------------ | --------- | ------------------------- |
| `bg`               | `#1a1a1a` | Primary background        |
| `bg-elevated`      | `#242424` | Cards, elevated surfaces  |
| `text`             | `#F5F0E6` | Primary text, borders     |
| `text-muted`       | `#A8A8A8` | Secondary text            |
| `accent`           | `#01A878` | Accent, glow, highlights  |
| `accent-dim`       | `#016B4F` | Hover states, subtle accents |

```astro
<!-- ✅ CORRECT: Semantic tokens -->
<div class="bg-bg-elevated text-text border border-text/20">

<!-- ❌ WRONG: Hardcoded colors -->
<div class="bg-[#242424] text-[#F5F0E6] border border-[rgba(245,240,230,0.2)]">
```

### 3. Green Accent Usage Rules

**Green (#01A878) is ALWAYS used as:**

- Glow effects (box-shadow, text-shadow)
- Border accents (1-2px highlights)
- Interactive state indicators
- Badge backgrounds (LIVE status)

**Green is NEVER used as:**

- Solid background fills (large areas)
- Primary text color (large text blocks)
- Primary button backgrounds (use outlined style)

```astro
<!-- ✅ CORRECT: Green as glow/border -->
<h1 class="text-text">
  Build <span class="text-accent [text-shadow:0_0_20px_rgba(1,168,120,0.6)]">life</span> operating systems
</h1>

<button class="border border-accent hover:bg-accent hover:text-bg">
  Subscribe
</button>

<!-- ❌ WRONG: Green as primary background -->
<button class="bg-accent text-white">Subscribe</button>
<div class="bg-accent p-8">Content</div>
```

### 4. Blueprint Aesthetic Elements

**Corner Markers:**

```astro
<div class="relative bg-bg-elevated p-8 border border-text/20">
  <!-- Top-left corner -->
  <div class="absolute top-2 left-2 w-3 h-3 border-l border-t border-text/30"></div>
  <!-- Bottom-right corner -->
  <div class="absolute bottom-2 right-2 w-3 h-3 border-r border-b border-text/30"></div>

  <h3>Content</h3>
</div>
```

**Green Glow Effects:**

```css
/* Subtle glow for borders/accents */
box-shadow: 0 0 10px rgba(1, 168, 120, 0.3);

/* Medium glow for hover states */
box-shadow: 0 0 20px rgba(1, 168, 120, 0.4);

/* Strong glow for emphasis */
box-shadow:
  0 0 30px rgba(1, 168, 120, 0.5),
  0 0 60px rgba(1, 168, 120, 0.2);
```

---

## Quick Reference

### Typography Scale

| Token        | Size   | Weight | Usage              |
| ------------ | ------ | ------ | ------------------ |
| `text-hero`  | 72px   | 700    | Hero name          |
| `text-h1`    | 48px   | 700    | Page titles        |
| `text-h2`    | 36px   | 600    | Section headers    |
| `text-h3`    | 24px   | 600    | Card titles        |
| `text-h4`    | 20px   | 500    | Subsections        |
| `text-body`  | 16px   | 400    | Body text          |
| `text-small` | 14px   | 400    | Captions, labels   |
| `text-micro` | 12px   | 500    | Badges, metadata   |

**Font stacks:**

```css
/* Headings */
font-family: 'Space Grotesk', 'Inter', system-ui, sans-serif;

/* Body */
font-family: 'Inter', 'SF Pro Text', system-ui, sans-serif;

/* Code/mono (if needed) */
font-family: 'JetBrains Mono', 'Fira Code', monospace;
```

### Spacing Scale (4px base unit)

| Token      | Value | Usage                |
| ---------- | ----- | -------------------- |
| `space-1`  | 4px   | Tight gaps           |
| `space-2`  | 8px   | Icon padding         |
| `space-3`  | 12px  | Input padding        |
| `space-4`  | 16px  | Card padding         |
| `space-6`  | 24px  | Section padding      |
| `space-8`  | 32px  | Large gaps           |
| `space-12` | 48px  | Section margins      |
| `space-16` | 64px  | Major section breaks |
| `space-24` | 96px  | Page section spacing |

**Use Tailwind spacing classes:**

```astro
<!-- ✅ CORRECT: Scale-based spacing -->
<div class="mt-4 mb-6 px-4 py-3">

<!-- ❌ WRONG: Arbitrary values -->
<div class="mt-[17px] mb-[25px] px-[15px] py-[13px]">
```

### Common Button Patterns

```astro
<!-- Primary: Outlined with green accent -->
<button class="
  border border-accent
  text-text
  px-6 py-3
  rounded-sm
  hover:bg-accent hover:text-bg
  [box-shadow:0_0_0_rgba(1,168,120,0)]
  hover:[box-shadow:0_0_20px_rgba(1,168,120,0.4)]
  transition-all duration-200
">
  Subscribe
</button>

<!-- Secondary: Ghost/outlined -->
<button class="
  border border-text
  text-text
  px-6 py-3
  rounded-sm
  hover:bg-text/10
  transition-colors duration-200
">
  Learn More
</button>

<!-- Link button (no border) -->
<button class="
  text-accent
  hover:text-accent-dim
  transition-colors duration-150
">
  Read more →
</button>
```

### Card Patterns

```astro
<!-- Pillar Card with Blueprint corners -->
<div class="
  relative
  bg-bg-elevated
  border border-text/20
  p-8
  hover:border-accent
  hover:[box-shadow:0_0_30px_rgba(1,168,120,0.15)]
  transition-all duration-200
">
  <!-- Corner markers -->
  <div class="absolute top-2 left-2 w-3 h-3 border-l border-t border-text/30"></div>
  <div class="absolute bottom-2 right-2 w-3 h-3 border-r border-b border-text/30"></div>

  <!-- Badge (absolute positioned) -->
  <span class="absolute top-4 right-4 bg-accent text-bg text-micro uppercase tracking-wider px-3 py-1 rounded-sm">
    LIVE
  </span>

  <!-- Icon (centered, 64x64px) -->
  <img src="/icons/eat.svg" alt="EAT" class="w-16 h-16 mx-auto mb-4" />

  <!-- Title -->
  <h3 class="text-h3 text-center mb-2">EAT</h3>

  <!-- Subtitle -->
  <p class="text-small text-center text-text-muted uppercase tracking-wide mb-3">
    Body Compass
  </p>

  <!-- Description -->
  <p class="text-body text-center text-text/80 leading-relaxed">
    Eliminate reactive foods. Track meals, symptoms, and discover your unique body compass.
  </p>
</div>
```

### Badge Patterns

```astro
<!-- LIVE badge: Green solid background -->
<span class="
  inline-block
  bg-accent text-bg
  text-micro uppercase tracking-wider
  px-3 py-1
  rounded-sm
  font-semibold
">
  LIVE
</span>

<!-- COMING badge: Outlined -->
<span class="
  inline-block
  border border-text
  text-text
  text-micro uppercase tracking-wider
  px-3 py-1
  rounded-sm
  font-semibold
">
  COMING
</span>

<!-- BUILDING badge: Orange outlined (status) -->
<span class="
  inline-block
  border border-orange-500
  text-orange-500
  text-micro uppercase tracking-wider
  px-3 py-1
  rounded-sm
  font-semibold
">
  BUILDING
</span>
```

### Form Input Patterns

```astro
<!-- Email input -->
<input
  type="email"
  placeholder="Enter your email"
  class="
    bg-transparent
    border border-text/30
    text-text placeholder:text-text-muted
    px-4 py-3
    text-body
    w-full
    focus:outline-none
    focus:border-accent
    focus:[box-shadow:0_0_0_2px_rgba(1,168,120,0.2)]
    transition-all duration-200
  "
/>

<!-- Newsletter form layout -->
<form class="flex gap-3 max-w-lg mx-auto">
  <input
    type="email"
    placeholder="Enter your email"
    class="flex-1 bg-transparent border border-text/30 text-text px-4 py-3"
  />
  <button class="border border-accent text-text px-6 py-3 hover:bg-accent hover:text-bg">
    Join
  </button>
</form>
```

---

## Layout Patterns

### Container/Max-Width

```astro
<!-- Main content container -->
<div class="container mx-auto max-w-7xl px-6">
  <!-- Content -->
</div>

<!-- Narrow content (text) -->
<div class="container mx-auto max-w-3xl px-6">
  <!-- Text content -->
</div>
```

### Grid Patterns

```astro
<!-- 3-column pillar cards (desktop) -->
<div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
  <PillarCard />
  <PillarCard />
  <PillarCard />
</div>

<!-- 2-column content -->
<div class="grid grid-cols-1 lg:grid-cols-2 gap-8">
  <div>Column 1</div>
  <div>Column 2</div>
</div>
```

### Section Spacing

```astro
<!-- Page sections -->
<section class="py-24">
  <div class="container mx-auto max-w-7xl px-6">
    <h2 class="text-h2 text-center mb-12">Section Title</h2>
    <!-- Content -->
  </div>
</section>
```

---

## Responsive Breakpoints

| Token   | Value  | Usage             |
| ------- | ------ | ----------------- |
| `sm`    | 640px  | Mobile landscape  |
| `md`    | 768px  | Tablet            |
| `lg`    | 1024px | Desktop           |
| `xl`    | 1280px | Large desktop     |
| `2xl`   | 1536px | Extra large       |

**Mobile adjustments (desktop-first):**

```astro
<!-- Hero text: Large on desktop, smaller on mobile -->
<h1 class="text-hero md:text-5xl sm:text-4xl">
  Craig van Heerden
</h1>

<!-- Grid: 3 cols desktop, 1 col mobile -->
<div class="grid grid-cols-3 md:grid-cols-2 sm:grid-cols-1 gap-6">
```

---

## Animation Guidelines

**Principles:**

- Subtle, purposeful motion
- No flashy or distracting animations
- Enhance perceived performance

**Transitions:**

```css
/* Fast: Hover feedback */
transition: all 0.15s ease;

/* Base: Color changes, scale */
transition: all 0.2s ease;

/* Slow: Complex animations */
transition: all 0.3s ease;
```

**Hover effects:**

- Buttons: Background fill + glow (0.2s)
- Cards: Border color + subtle glow (0.2s)
- Links: Color shift (0.15s)

---

## Accessibility Standards

### Color Contrast

**WCAG AA Compliance:**

- Text on background: 4.5:1 minimum (#F5F0E6 on #1a1a1a = 14.6:1 ✅)
- Large text (18px+): 3:1 minimum
- UI components: 3:1 minimum

**Muted text:**

- #A8A8A8 on #1a1a1a = 4.7:1 ✅

### Semantic HTML

```astro
<!-- ✅ CORRECT: Semantic structure -->
<nav>
  <a href="/">Home</a>
  <a href="/philosophy">Philosophy</a>
</nav>

<main>
  <article>
    <h1>Page Title</h1>
    <p>Content</p>
  </article>
</main>

<!-- ❌ WRONG: Non-semantic divs -->
<div class="nav">
  <div class="link">Home</div>
</div>

<div class="main">
  <div class="article">
    <div class="title">Page Title</div>
    <div>Content</div>
  </div>
</div>
```

### ARIA Labels

```astro
<!-- Icon buttons need labels -->
<button aria-label="Subscribe to newsletter">
  <svg aria-hidden="true">...</svg>
</button>

<!-- Decorative images -->
<img src="/decorative.svg" alt="" aria-hidden="true" />

<!-- Meaningful images -->
<img src="/pillar-eat.png" alt="Body Compass EAT pillar - nutrition sovereignty" />
```

### Focus States

```astro
<!-- Visible focus indicators -->
<button class="
  focus:outline-none
  focus:ring-2 focus:ring-accent focus:ring-offset-2 focus:ring-offset-bg
">
  Button
</button>
```

---

## Anti-Patterns (Avoid)

| Don't                              | Do                                        |
| ---------------------------------- | ----------------------------------------- |
| `bg-green-500`, `text-gray-400`    | `bg-accent`, `text-text-muted`            |
| Green as solid fill backgrounds    | Green as glow/border accent only          |
| Mobile-first approach (`min-width`)| Desktop-first (`max-width`)               |
| `mt-[17px]` arbitrary values       | `mt-4` spacing scale                      |
| `hover:scale-95` (mobile pattern)  | `hover:bg-accent` (desktop pattern)       |
| Non-semantic HTML (`<div class="button">`)| Semantic HTML (`<button>`)        |

---

## Checklist Before Commit

UI/Visual:

- [ ] Colors use semantic tokens (not hex)
- [ ] Green used as accent only (not fill)
- [ ] Typography scale used (not arbitrary sizes)
- [ ] Spacing scale used (4px base unit)
- [ ] Desktop-first responsive approach
- [ ] Blueprint corner markers on cards
- [ ] Hover states with transitions

Accessibility:

- [ ] Color contrast 4.5:1+ (text)
- [ ] Semantic HTML used
- [ ] ARIA labels on icon buttons
- [ ] Alt text on meaningful images
- [ ] Focus states visible
- [ ] Heading hierarchy correct (h1 → h2 → h3)

---

## Reference Files

**Full specifications:** `/Users/craigvanheerden/Repos/software/website/SPEC.md` (Design System section)

**Mockups:** `infrastructure/tools/src/image-gen/output/website-mvp/`

---

**Remember:** Blueprint Web is about technical precision meets modern UI. Keep it clean, functional, and let the green glow provide the visual accent. Desktop-first means we optimize for the primary use case (desktop browser), then adapt down responsively.
