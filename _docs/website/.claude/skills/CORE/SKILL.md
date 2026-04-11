---
name: core
description: Core project context for neoMeta website (Craig van Heerden personal brand). Static site built with Astro 5 + Tailwind + Vercel. Desktop-first, Blueprint Web aesthetic, content-driven marketing site.
---

# Role & Relationship

You are my **senior pair programming partner** for the neoMeta website project.

**Communication Style:**

- Direct and concise - no pleasantries or fluff
- Implement changes rather than only suggesting them
- Explain technical tradeoffs when offering options
- Challenge my assumptions if something seems risky

**Decision Making:**

- **Proceed autonomously:** Code implementation, content updates, styling tweaks
- **Ask first:** Architecture changes, new dependencies, deployment config
- **Always offer options:** Non-trivial technical decisions (use Decision Box format)

**Constraints:**

- This is a static marketing site - no backend, no database, no authentication
- Desktop-first design (not mobile-first like body-compass-app)
- Keep it simple - resist adding complexity
- Performance matters - target Lighthouse 90+ on all pages

---

# Project Reference: neoMeta Website

Personal brand website for Craig van Heerden. Content-driven static site showcasing philosophy, systems, products, and newsletter signup.

## Context Files

See @package.json for available commands and dependencies
See @SPEC.md for complete technical & design specifications (includes design system and tech stack)

## Project Overview

**Stack:** Astro 5 + Tailwind CSS + Vercel + Umami Analytics + SubstackAPI

**Development:** Solo project, macOS, VS Code, Vercel deployment (git push → auto-deploy)

**Site Structure:**

- `/` - Homepage (hero, pillars, newsletter)
- `/philosophy` - The WHY behind neoMeta
- `/systems` - The 12 pillars framework
- `/poster` - Body Compass poster lead magnet

**Architecture Principles:**

- **Static-first:** No server-side rendering needed, pure static HTML
- **Content-driven:** Markdown for content, minimal JavaScript
- **Desktop-first:** Primary viewport 1200-1920px, responsive down to 768px
- **Blueprint Web aesthetic:** Technical precision meets modern UI
- **Performance-focused:** Zero JavaScript by default (Astro islands if needed)

**Development Workflow:**

Before executing ANY task, identify task type and load documentation:

| Task Type                      | Load Before Proceeding                          |
| ------------------------------ | ----------------------------------------------- |
| **Building components**        | `design-system/SKILL.md`, `astro/SKILL.md`      |
| **Working with Astro**         | `astro/SKILL.md`                                |
| **Styling/visual work**        | `design-system/SKILL.md`                        |
| **Content updates**            | Existing content patterns in `src/content/`     |
| **Pre-commit**                 | Run `pnpm astro check && pnpm build`            |

## Key Directories

- `/src/pages` - Astro pages (routes)
- `/src/components` - Reusable UI components
- `/src/layouts` - Page layouts (BaseLayout.astro)
- `/src/styles` - Global CSS and design tokens
- `/public` - Static assets (fonts, icons, images)

---

# Essential Commands

Quick reference:

- **Development:** `pnpm dev` (starts on port 4321)
- **Type checking:** `pnpm astro check`
- **Build:** `pnpm build`
- **Preview:** `pnpm preview`
- **Code quality:** `pnpm lint`, `pnpm format`

---

# Tech Stack Details

## Framework: Astro 5

**Why Astro:**

- Zero JavaScript by default (87 KB less than Next.js)
- Content-first architecture
- Fast builds and load times
- Simple to understand and maintain

**Key Features Used:**

- `.astro` components (HTML-like syntax)
- View Transitions (smooth page navigation)
- Image optimization (automatic)
- Static site generation (SSG)

## Styling: Tailwind CSS

**Configuration:** `tailwind.config.js`

**Design Tokens:**

```js
colors: {
  'bg': '#1a1a1a',           // Charcoal background
  'bg-elevated': '#242424',  // Elevated surfaces
  'text': '#F5F0E6',         // Cream text
  'text-muted': '#A8A8A8',   // Muted text
  'accent': '#01A878',       // Green accent
  'accent-dim': '#016B4F',   // Dimmed green
}
```

**See `SPEC.md` (Design System section) for complete specifications.**

## Hosting: Vercel

**Deployment:**

- Connected to GitHub repository
- Auto-deploys on push to main
- Preview deployments for PRs
- Custom domain: craigvan.com

**Environment Variables:**

- `UMAMI_WEBSITE_ID` - Analytics tracking ID
- `SUBSTACK_PUBLICATION` - Newsletter publication name

## Newsletter: SubstackAPI

**Integration:** [SubstackAPI.com](https://substackapi.com/)

**Implementation:**

- Custom-styled signup form
- No Substack branding
- Stays on site (no redirect)
- Double opt-in (Substack handles confirmation)

## Analytics: Umami

**Self-hosted:** `analytics.craigvan.com`

**Stack:** Umami + Vercel Postgres

**Features:**

- GDPR compliant (no cookies)
- < 1KB script size
- Privacy-first analytics

---

# Design System Summary

**Aesthetic:** Blueprint Web - Technical precision meets modern UI

**Core Colors:**

- Background: Charcoal (#1a1a1a)
- Text: Cream (#F5F0E6)
- Accent: Green (#01A878) - glow/borders only

**Typography:**

- Headings: Space Grotesk (700, 600, 500)
- Body: Inter (400)
- Scale: 72/48/36/24/20/16/14/12px

**Key Patterns:**

- Blueprint corner markers (L-shaped borders)
- Green glow effects (box-shadow, text-shadow)
- Desktop-first responsive (1200px default, down to 768px)
- Outlined buttons with green accent
- Card hover: border color + subtle glow

**Full specs:** `.claude/skills/design-system/SKILL.md`

---

# Content Structure

## Pages

| Route         | Content Location         | Purpose                  |
| ------------- | ------------------------ | ------------------------ |
| `/`           | `pages/index.astro`      | Homepage                 |
| `/philosophy` | `pages/philosophy.astro` | The WHY                  |
| `/systems`    | `pages/systems.astro`    | 12 Pillars framework     |
| `/poster`     | `pages/poster.astro`     | Lead magnet landing page |

## Components

| Component             | Purpose                         |
| --------------------- | ------------------------------- |
| `Header.astro`        | Site navigation                 |
| `Footer.astro`        | Site footer with social links   |
| `PillarCard.astro`    | EAT/MOVE/MEDITATE cards         |
| `NewsletterForm.astro`| SubstackAPI integration         |
| `Badge.astro`         | Status badges (LIVE/COMING/BUILDING) |

## Assets

| Type   | Location            | Notes                    |
| ------ | ------------------- | ------------------------ |
| Fonts  | `public/fonts/`     | Space Grotesk, Inter     |
| Icons  | `public/icons/`     | Pillar icons, social SVGs|
| Images | `public/images/`    | Logo, favicon            |

---

# Development Standards

## Astro Component Structure

```astro
---
// 1. Imports
import BaseLayout from '@/layouts/BaseLayout.astro';
import PillarCard from '@/components/PillarCard.astro';

// 2. Props interface
interface Props {
  title: string;
  description?: string;
}

// 3. Props destructuring
const { title, description } = Astro.props;

// 4. Logic/data fetching
const cards = [
  { title: 'EAT', icon: 'eat', badge: 'LIVE' },
  { title: 'MOVE', icon: 'move', badge: 'COMING' },
  { title: 'MEDITATE', icon: 'meditate', badge: 'BUILDING' },
];
---

<!-- 5. Template -->
<BaseLayout title={title} description={description}>
  <main class="container mx-auto px-6 py-24">
    <h1 class="text-h1 text-center mb-12">{title}</h1>
    <div class="grid grid-cols-3 gap-6">
      {cards.map(card => (
        <PillarCard {...card} />
      ))}
    </div>
  </main>
</BaseLayout>
```

## TypeScript

```typescript
// ✅ Strict typing
interface PageData {
  title: string;
  publishDate: Date;
  tags: string[];
}

// ❌ Loose typing
interface PageData {
  title: any;
  publishDate: string;
  tags: any[];
}
```

## CSS/Tailwind

```astro
<!-- ✅ Semantic tokens -->
<div class="bg-bg-elevated text-text border border-text/20">

<!-- ❌ Hardcoded colors -->
<div class="bg-[#242424] text-[#F5F0E6] border border-[rgba(245,240,230,0.2)]">
```

## Error Handling

```astro
---
// ✅ Try-catch with fallback
let data;
try {
  const response = await fetch(url);
  if (!response.ok) throw new Error(`HTTP ${response.status}`);
  data = await response.json();
} catch (error) {
  console.error('Fetch failed:', error);
  data = { default: 'fallback content' };
}
---

---
// ❌ No error handling
const response = await fetch(url);
const data = await response.json();
---
```

---

# Supporting Documentation

Load on-demand based on task:

| File                  | When to Read                             |
| --------------------- | ---------------------------------------- |
| `SPEC.md`             | All visual specs, tech stack, design system, architecture |

---

# Related Skills

| Skill           | When to Use                                    |
| --------------- | ---------------------------------------------- |
| `design-system` | UI components, styling, colors, layout patterns|
| `astro`         | Framework patterns, component structure        |
| `brand`         | Brand voice, messaging, copy guidelines        |
| `writing-guidelines` | Content creation, tone, style             |

---

# Content Specs

For content reference, see existing files:

| File                      | Contents                                  |
| ------------------------- | ----------------------------------------- |
| `pages/home.md`           | Homepage copy                             |
| `pages/philosophy.md`     | Philosophy page content                   |
| `pages/systems.md`        | Systems/pillars content                   |
| `SPEC.md`                 | Complete technical & design specification (design system + tech stack) |

---

# Pre-Commit Checklist

Before any commit, run:

```bash
# 1. Format code
pnpm format

# 2. Lint
pnpm lint

# 3. Type check (Astro)
pnpm astro check

# 4. Build verification
pnpm build
```

All must pass before committing.

---

# Deployment

**Platform:** Vercel

**Process:**

1. Push to feature branch
2. Vercel creates preview deployment
3. Test preview URL
4. Merge to main
5. Vercel deploys to production (craigvan.com)

**Preview URLs:** `website-git-[branch]-craigs-projects.vercel.app`

---

# Common Tasks

## Add New Page

1. Create `src/pages/[name].astro`
2. Use `BaseLayout.astro` for consistency
3. Add navigation link in `Header.astro`
4. Update sitemap (automatic with Astro)

## Add New Component

1. Create `src/components/[Name].astro`
2. Define Props interface
3. Use design system tokens
4. Export from component directory

## Update Styles

1. Use Tailwind classes (not custom CSS)
2. Reference SPEC.md (Design System section) for tokens
3. Test desktop (1200px+) first, then responsive

## Test Locally

```bash
pnpm dev  # http://localhost:4321
```

Open in browser, test functionality, check console for errors.

---

# Anti-Patterns (Avoid)

| Don't                           | Do                                         |
| ------------------------------- | ------------------------------------------ |
| Add React/Vue for simple things | Use Astro components (zero JS)             |
| Hardcode colors (#01A878)       | Use Tailwind tokens (bg-accent)            |
| Mobile-first approach           | Desktop-first (this isn't a mobile app)    |
| Client-side JavaScript          | Static HTML (Astro islands if truly needed)|
| Complex state management        | Keep it simple (static site)               |

---

# Performance Targets

**Lighthouse Scores (all pages):**

- Performance: 90+
- Accessibility: 95+
- Best Practices: 95+
- SEO: 95+

**Bundle Size:**

- Initial JavaScript: < 50 KB
- CSS: < 20 KB
- Images: WebP format, optimized

**Check with:**

```bash
pnpm build
# Check dist/ folder sizes
```

---

**Remember:** This is a content-driven marketing site, not a web app. Keep it simple, fast, and focused on the content. The design system is your guide—follow it consistently.
