## MANDATORY FIRST ACTION

**Read `.claude/skills/CORE/SKILL.md` for full context.**

---

# neoMeta Website Project

Personal brand website for Craig van Heerden. Content-driven static site built with Astro 5 + Tailwind CSS + Vercel.

## Quick Start

```bash
# Development
pnpm dev  # http://localhost:4321

# Type checking
pnpm astro check

# Build
pnpm build

# Preview build
pnpm preview
```

## Project Structure

```
software/website/
├── src/
│   ├── pages/           # Routes (index, philosophy, systems, poster)
│   ├── components/      # UI components (Header, Footer, PillarCard, etc.)
│   ├── layouts/         # Page layouts (BaseLayout.astro)
│   ├── styles/          # Global CSS
│   └── assets/          # Images for optimization
├── public/              # Static assets (fonts, icons, logo)
├── .claude/
│   ├── agents/          # Subagents (engineer, tester, code-explorer, reviewer)
│   ├── commands/        # Workflows (build, work, plan, review, merge, commit)
│   └── skills/          # Skills (CORE, design-system, astro, brand, writing-guidelines)
├── SPEC.md              # Design system + technical specifications
├── PRD.md               # Product requirements
└── package.json         # Dependencies and scripts
```

## Design System

**Aesthetic:** Blueprint Web - Technical precision meets modern UI

**Colors:**

- Background: #1a1a1a (charcoal)
- Text: #F5F0E6 (cream)
- Accent: #01A878 (green - glow/borders only)

**Typography:**

- Headings: Space Grotesk
- Body: Inter
- Scale: 72/48/36/24/20/16/14/12px

**Full specs:** `SPEC.md` (Design System section) + `.claude/skills/design-system/SKILL.md`

## Tech Stack

**Framework:** Astro 5 (zero JavaScript by default)
**Styling:** Tailwind CSS (semantic tokens)
**Hosting:** Vercel (auto-deploy on push)
**Analytics:** Umami (self-hosted)
**Newsletter:** SubstackAPI (custom embed)

**Full details:** `SPEC.md` (Technical Stack section) + `.claude/skills/CORE/SKILL.md`

## Developer Environment

### Skills

- **CORE** - Project context, tech stack, standards
- **design-system** - Blueprint Web aesthetic, colors, typography, components
- **astro** - Framework patterns, component structure, optimization
- **brand** - Brand voice, messaging (symlinked from root)
- **writing-guidelines** - Content style, tone (symlinked from root)

### Agents

- **engineer** - Implements code following plans
- **tester** - Validates builds, accessibility, visual quality
- **code-explorer** - Researches patterns before building
- **reviewer** - Audits code for quality, standards, accessibility

### Commands

- **/build** - Build new features (research → implement → QA)
- **/work** - Execute approved plans
- **/plan** - Create implementation plans
- **/review** - Multi-agent code review
- **/merge** - Create PR, implement feedback, merge
- **/commit** - Pre-commit checks and commit

## Key Differences from body-compass-app

| Aspect            | body-compass-app         | website                  |
| ----------------- | ------------------------ | ------------------------ |
| Framework         | Next.js 15               | Astro 5                  |
| JavaScript        | React (client-side)      | Zero JS by default       |
| Design            | Mobile-first, PWA        | Desktop-first, static    |
| Tests             | Jest + Playwright        | Build checks only        |
| Styling           | Tailwind (mobile patterns)| Tailwind (desktop patterns)|
| Database          | Supabase                 | None (static site)       |
| Auth              | Clerk                    | None                     |
| Type check        | `pnpm type-check`        | `pnpm astro check`       |
| Build check       | `pnpm build:check`       | `pnpm build`             |

## Pre-Commit Workflow

```bash
# 1. Format
pnpm format

# 2. Lint
pnpm lint

# 3. Type check (Astro)
pnpm astro check

# 4. Build verification
pnpm build
```

All must pass before committing.

## Deployment

**Platform:** Vercel

**Process:**

1. Push to feature branch → Vercel creates preview
2. Test preview URL
3. Merge to main → Vercel deploys to craigvan.com

**Preview URL format:** `website-git-[branch]-craigs-projects.vercel.app`

## MVP Scope

### Pages (v1)

- `/` - Homepage (hero, three pillar cards, newsletter, footer)
- `/eat` - EAT Journey page (poster download, app section, course teaser)

### Deferred to v2

- `/move` - MOVE pillar journey
- `/meditate` - MEDITATE pillar journey
- Philosophy page
- Systems page (12 pillars)
- About page
- Blog/Essays section

## Performance Targets

**Lighthouse scores (all pages):**

- Performance: 90+
- Accessibility: 95+
- Best Practices: 95+
- SEO: 95+

## References

- **Specifications:** `SPEC.md` (design system + technical stack)
- **Requirements:** `PRD.md` (product requirements)
- **Implementation:** `IMPLEMENTATION.md` (5-phase build plan)
- **Mockups:** `infrastructure/tools/src/image-gen/output/website-mvp/`
- **Astro docs:** https://docs.astro.build/

---

**Next steps:** Initialize Astro project with Tailwind, configure design tokens, build components following design system.
