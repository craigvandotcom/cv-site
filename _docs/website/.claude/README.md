# neoMeta Website - Claude Developer Environment

Complete `.claude/` setup for the neoMeta website project (Astro 5 + Tailwind + Vercel).

**Created:** 2026-01-19
**Adapted from:** body-compass-app/.claude/
**Total documentation:** ~7,300 lines

---

## Directory Structure

```
.claude/
├── agents/               # Specialized subagents (4 total)
│   ├── engineer.md       # Implements code following plans
│   ├── tester.md         # Validates builds, accessibility, quality
│   ├── code-explorer.md  # Researches patterns before building
│   └── reviewer.md       # Audits code quality, standards, accessibility
│
├── commands/             # Orchestrated workflows (6 total)
│   ├── build.md          # Build new features (research → implement → QA)
│   ├── work.md           # Execute approved plans
│   ├── plan.md           # Create implementation plans
│   ├── review.md         # Multi-agent code review
│   ├── merge.md          # Create PR, implement feedback, merge
│   └── commit.md         # Pre-commit checks and commit
│
├── skills/               # Domain knowledge (5 total: 3 local + 2 symlinked)
│   ├── CORE/             # Project context, tech stack, standards
│   │   └── SKILL.md      # Astro 5, Tailwind, Vercel, static site
│   ├── design-system/    # Blueprint Web aesthetic
│   │   └── SKILL.md      # Colors, typography, components, desktop-first
│   ├── astro/            # Framework patterns
│   │   └── SKILL.md      # Component structure, routing, optimization
│   ├── brand/            # Brand voice (symlink to root)
│   └── writing-guidelines/ # Content style (symlink to root)
│
└── README.md             # This file
```

---

## Key Adaptations from body-compass-app

### Framework Changes

| body-compass-app          | website                   |
| ------------------------- | ------------------------- |
| Next.js 15 (React)        | Astro 5 (zero JS)         |
| `pnpm test` (Jest)        | `pnpm astro check`        |
| `pnpm build:check`        | `pnpm build`              |
| Mobile-first PWA          | Desktop-first static site |
| Supabase database         | No database (static)      |
| Clerk auth                | No auth                   |

### Design System Changes

| body-compass-app          | website                   |
| ------------------------- | ------------------------- |
| Mobile-first (320-428px)  | Desktop-first (1200-1920px)|
| Touch interactions        | Hover states              |
| Zone colors (health app)  | Blueprint Web aesthetic   |
| 44x44px touch targets     | Standard desktop targets  |
| `active:scale-95`         | `hover:bg-accent`         |

### Commands Workflow

All 6 commands preserved with Astro-specific adaptations:

- `/build` - Same multi-agent workflow, adapted for static site validation
- `/work` - Same TDD approach, adapted verification (no Jest)
- `/plan` - Same exploration → spec → approval flow
- `/review` - Same parallel reviewers, adapted for accessibility/performance
- `/merge` - Same PR → feedback → merge flow
- `/commit` - Adapted checks: `astro check` + `build` (no separate tests)

---

## Agents Overview

### engineer.md

**Model:** Sonnet
**Tools:** Read, Write, Edit, Bash, Grep, Glob

**Skills loaded:**

- CORE (always)
- design-system (when building UI)
- astro (when working with framework)

**Verification:**

- `pnpm astro check` (type checking)
- `pnpm build` (build verification)

**Key adaptations:**

- No Jest tests
- Astro component patterns
- Desktop-first CSS patterns
- Static site constraints

### tester.md

**Model:** Sonnet
**Tools:** Read, Write, Edit, Bash, Grep, Glob

**Validation focus:**

- Build success (no TypeScript errors)
- Lighthouse scores (95+ accessibility, 90+ performance)
- Visual/design system compliance
- Accessibility (WCAG AA)
- Newsletter form functionality

**Key adaptations:**

- No Jest/Playwright E2E tests
- Lighthouse audits primary validation
- agent-browser for visual verification
- Desktop responsiveness (not mobile-first)

### code-explorer.md

**Model:** Haiku (cost-effective)
**Tools:** Read, Grep, Glob

**Research areas:**

- Astro page/component patterns
- Tailwind usage patterns
- Content structure (Markdown, MDX)
- Design system token usage

**Output:** `.claude/plans/research/YYYY-MM-DD-HHMM-exploration-*.md`

### reviewer.md

**Model:** Sonnet
**Tools:** Read, Grep, Glob, Bash

**Review dimensions:**

- Correctness (logic, types)
- Accessibility (WCAG AA, semantic HTML)
- Performance (bundle size, image optimization)
- Design system compliance (semantic tokens)
- SEO & meta tags

**Output:** `.claude/plans/review/YYYY-MM-DD-HHMM-review-*.md`

---

## Skills Overview

### CORE/SKILL.md (586 lines)

**Project context for neoMeta website**

**Contents:**

- Tech stack (Astro 5, Tailwind, Vercel)
- Project structure (pages, components, layouts)
- Design system summary
- Development standards (Astro components, TypeScript, CSS)
- Pre-commit checklist
- Performance targets (Lighthouse 90+)
- Common tasks and anti-patterns

**Key sections:**

- Role & Relationship (communication style)
- Project Overview (architecture principles)
- Essential Commands (dev, build, check)
- Tech Stack Details (Astro, Tailwind, Vercel, Umami, SubstackAPI)
- Content Structure (pages, components, assets)
- Development Standards (component structure, TypeScript, error handling)

### design-system/SKILL.md (746 lines)

**Blueprint Web design system**

**Contents:**

- Core principles (desktop-first, semantic tokens, green accent rules)
- Typography scale (Space Grotesk + Inter)
- Spacing scale (4px base unit)
- Component patterns (buttons, cards, badges, forms)
- Layout patterns (containers, grids, sections)
- Responsive breakpoints (desktop down to tablet/mobile)
- Accessibility standards (WCAG AA, semantic HTML, ARIA)
- Anti-patterns to avoid

**Key sections:**

- Desktop-First Responsive (primary 1200-1920px)
- Semantic Color Tokens (bg, text, accent)
- Green Accent Usage Rules (glow/borders only, never solid fills)
- Blueprint Aesthetic Elements (corner markers, glow effects)
- Quick Reference (typography, spacing, buttons, cards, badges)

### astro/SKILL.md (612 lines)

**Astro 5 framework patterns**

**Contents:**

- Component anatomy (frontmatter, template, styles)
- Zero JavaScript by default principle
- Routing (file-based, static, dynamic)
- Layouts (BaseLayout pattern with meta tags)
- Image optimization (Astro Image component)
- View Transitions (page animations)
- Environment variables (public vs private)
- Data fetching (build-time only)
- TypeScript (Props interfaces)
- Common patterns (newsletter form, conditionals, lists)
- Performance best practices

**Key sections:**

- Component Anatomy (props, logic, template structure)
- Zero JavaScript by Default (when to avoid client directives)
- Routing (file-based pages, dynamic routes)
- Image Optimization (src/assets vs public)
- Common Mistakes (avoiding client JS, Props interfaces)

### brand/ (symlinked)

**Shared brand voice and messaging**

Located at: `/Repos/.claude/skills/brand/`

Used by:

- body-compass-app
- website (via symlink)
- Future projects needing consistent brand voice

### writing-guidelines/ (symlinked)

**Shared content style and tone**

Located at: `/Repos/.claude/skills/writing-guidelines/`

Used by:

- newsletter (content creation)
- website (via symlink)
- Future content-driven projects

---

## Command Workflows

### /build - Build New Features

**Phases:**

1. Initialize task list (TodoWrite)
2. Parallel code exploration (3x code-explorer)
3. Synthesize findings & create plan
4. Ask questions if needed
5. Present plan for approval (WAIT)
6. Parallel implementation (engineer agents)
7. Quality assurance (reviewer + tester)
8. Auto-run /commit

**Adapted for website:**

- No Jest tests in QA phase
- Lighthouse audits for validation
- Build checks instead of test suite
- Desktop responsiveness validation

### /work - Execute Approved Plans

**Phases:**

1. Create task list (TodoWrite)
2. Load approved plan (discover and validate)
3. Spawn Engineer #1 (build tests from specs, verify RED)
4. Review tests (quality check)
5. Spawn Engineer #2 (implement until GREEN)
6. Commit + push (on success)
7. Hand-off to /review

**Adapted for website:**

- "Tests" are build checks + type checks
- RED state = build fails / type errors
- GREEN state = build succeeds, types pass
- Verification via `pnpm astro check && pnpm build`

### /plan - Create Implementation Plans

**Phases:**

1. Initialize & classify (BUILD | IMPROVE | FIX)
2. Parallel code exploration (3x code-explorer)
3. Validation baseline ("Taste the Tools")
4. Synthesize findings & create plan
5. Present for approval (WAIT)
6. Finalize & commit plan

**Adapted for website:**

- Validation baseline includes Lighthouse
- Build verification (not test running)
- Visual/accessibility baseline capture
- Static site constraints documented

### /review - Multi-Agent Code Review

**Phases:**

1. Create task list (TodoWrite)
2. Gather context (plan, diff, build status)
3. Spawn 4 reviewers in parallel
4. Aggregate & categorize findings
5. Auto-fix clear issues (spawn engineer)
6. Commit report & fixes
7. Validate preview deployment (Vercel)
8. Present decisions to user

**Adapted for website:**

- Accessibility reviewer (WCAG AA focus)
- Performance reviewer (Lighthouse, bundle size)
- SEO/meta reviewer (titles, descriptions, OG tags)
- Design system compliance (semantic tokens, Blueprint aesthetic)

### /merge - Ship Code

**Phases:**

1. Preparation & validation (checks, staging)
2. PR creation (or verify existing)
3. Wait for checks & Claude review (intelligent polling)
4. Implement feedback
5. Document deferred items & lessons
6. Final verification & confirmation
7. Merge & cleanup

**No adaptations needed** - works identically for static site.

### /commit - Pre-Commit Checks

**Phases:**

1. Branch check (create if on main)
2. Safety check (warn on deletions)
3. Pre-commit checks (format, lint, astro check, build)
4. Stage all changes (git add -A)
5. Verify staging
6. Commit and push

**Adapted for website:**

- `pnpm astro check` instead of `pnpm type-check`
- `pnpm build` instead of `pnpm test` + `pnpm build:check`
- Same format/lint/staging workflow

---

## Usage Examples

### Building a New Component

```bash
# 1. Plan the component
/plan "Create PillarCard component with Blueprint aesthetic"

# 2. Implement the plan
/work

# 3. Review the implementation
/review

# 4. Merge to main
/merge
```

### Quick Commit (No Planning)

```bash
# For small changes (content updates, style tweaks)
/commit
```

### Creating a New Page

```bash
# 1. Plan the page
/plan "Create /systems page with 12 pillars content"

# 2. Execute
/work

# 3. Review
/review

# 4. Ship
/merge
```

---

## Verification Checklist

After creating this environment, verify:

### Structure Created

- [ ] `.claude/agents/` with 4 agents (engineer, tester, code-explorer, reviewer)
- [ ] `.claude/commands/` with 6 commands (build, work, plan, review, merge, commit)
- [ ] `.claude/skills/CORE/SKILL.md` (project context)
- [ ] `.claude/skills/design-system/SKILL.md` (Blueprint Web)
- [ ] `.claude/skills/astro/SKILL.md` (framework patterns)
- [ ] `.claude/skills/brand/` (symlink to root)
- [ ] `.claude/skills/writing-guidelines/` (symlink to root)
- [ ] `CLAUDE.md` (project entry point)
- [ ] `.claude/README.md` (this file)

### Adaptations Applied

- [ ] Commands use `pnpm astro check` instead of `pnpm type-check`
- [ ] Commands use `pnpm build` instead of `pnpm test` + `pnpm build:check`
- [ ] Agents validate with build checks (not Jest tests)
- [ ] Design system reflects desktop-first (not mobile-first)
- [ ] Skills reference Astro 5 patterns (not Next.js)
- [ ] Skills reference Blueprint Web aesthetic (not Zone colors)

### References Correct

- [ ] All file paths absolute (no relative paths)
- [ ] Skills reference SPEC.md (design system section)
- [ ] Skills reference SPEC.md (tech stack section)
- [ ] Commands reference correct verification commands
- [ ] Agents load correct skills for tasks

---

## Maintenance Notes

### When to Update

**CORE skill:**

- Tech stack changes (new dependencies, framework updates)
- New pages added to site structure
- New components created
- Performance targets adjusted
- Deployment config changes

**design-system skill:**

- Visual spec changes (colors, typography, spacing)
- New component patterns
- Accessibility standards updates
- Responsive breakpoint changes

**astro skill:**

- Framework version updates (Astro 5 → 6)
- New Astro features used
- Build configuration changes
- Image optimization patterns

**Agents:**

- Model changes (Sonnet → Opus)
- Tool availability changes
- Verification steps updated
- Output format changes

**Commands:**

- Workflow improvements
- New quality gates
- Deployment process changes

### Consistency with body-compass-app

**Keep aligned:**

- Command workflow structure (/build, /work, /plan, /review, /merge, /commit)
- Agent specialization roles (engineer, tester, code-explorer, reviewer)
- Multi-agent orchestration patterns
- TodoWrite usage for phase tracking
- Plan file formats and locations

**Allow divergence:**

- Technology-specific verification (Astro vs Next.js)
- Design patterns (desktop-first vs mobile-first)
- Testing strategies (build checks vs Jest)
- Validation tools (Lighthouse vs Playwright)

---

## Related Documentation

**Root PAI system:**

- `/Repos/.claude/skills/pai/SKILL.md` - PAI system documentation
- `/Repos/.claude/skills/skill-builder/SKILL.md` - Skill creation guide
- `/Repos/.claude/skills/CORE/SKILL.md` - Root orchestrator context

**Project-specific:**

- `software/website/SPEC.md` - Complete technical & design specification (includes design system and tech stack)
- `software/website/CLAUDE.md` - Project entry point

**Shared skills:**

- `/Repos/.claude/skills/brand/` - Brand voice (symlinked)
- `/Repos/.claude/skills/writing-guidelines/` - Content style (symlinked)

---

## Success Criteria

This environment is successful when:

1. ✅ All 4 agents created and adapted for static site
2. ✅ All 6 commands copied and adapted for Astro
3. ✅ 3 local skills created (CORE, design-system, astro)
4. ✅ 2 shared skills symlinked (brand, writing-guidelines)
5. ✅ CLAUDE.md entry point created
6. ✅ Commands use correct verification (astro check, build)
7. ✅ Design system reflects Blueprint Web (not Zone colors)
8. ✅ Skills reference correct tech stack (Astro, not Next.js)
9. ✅ ~7,300 lines of documentation created
10. ✅ Ready for website development to begin

---

**Status:** Complete ✅

**Next steps:**

1. Initialize Astro project with Tailwind
2. Configure design tokens in tailwind.config.js
3. Create BaseLayout.astro with meta tags
4. Build core components (Header, Footer, PillarCard)
5. Implement homepage with newsletter form
6. Test /build workflow with first feature

---

**Created by:** architect subagent
**Date:** 2026-01-19
**Source:** Adapted from body-compass-app/.claude/
