# AGENTS.md — Subagent Context

> Read by subagents spawned via agent-compounds flywheel commands (`/ac/*`, `/jef/*`).
> Tooling deployed from `../agent-compounds` (symlinked into `.claude/`).

## Project Overview

| Field       | Value |
| ----------- | ----- |
| **Name**    | cv-site |
| **Stack**   | Astro 6, Tailwind CSS 4 (`@tailwindcss/vite`). No React/Vue islands, no TypeScript source (config `.mjs`), no database. Node ≥ 22.12. |
| **Type**    | Static marketing/personal site |
| **Purpose** | craigvan.com — Craig's personal site. **Deliberately a separate aesthetic from the neoMeta brand system** — do NOT apply `@neometa/brand` pillar colors or voice here. |
| **Hosting** | Vercel (`vercel.json`). Own git repo: `github.com/craigvandotcom/cv-site`. |

## Project Commands

| Operation      | Command |
| -------------- | ------- |
| **Dev server** | `npm run dev` (Astro, `localhost:4321`) |
| **Build**      | `npm run build` (static output → `./dist/`) |
| **Preview**    | `npm run preview` (preview the production build) |
| **Astro CLI**  | `npm run astro -- <cmd>` (e.g. `astro check`, `astro add`) |

> No test or lint scripts configured — this is a small static site. Add them only if the site grows to warrant it.

## Architecture

```
cv-site/
├── src/
│   ├── layouts/        # Layout.astro (shared page shell)
│   ├── pages/          # file-based routes: index, eat-move-meditate,
│   │                   #   compass-poster, newsletter
│   └── styles/         # global.css (Tailwind entry)
├── public/             # static assets served as-is
├── astro.config.mjs    # Astro + Tailwind-via-Vite
└── vercel.json         # deploy config
```

Routing is file-based: each `.astro` file in `src/pages/` is a route by filename.

## Available Skills

> Deployed via symlink from `../agent-compounds`. Method-only (zero app facts) — load when relevant.

| Skill | Path | Load when |
| ----- | ---- | --------- |
| planning | `.claude/skills/planning/SKILL.md` | creating/refining implementation plans |
| brainstorming | `.claude/skills/brainstorming/SKILL.md` | pre-planning exploration |
| web-design-guidelines | `.claude/skills/web-design-guidelines/SKILL.md` | visual/layout design decisions |
| writing-guidelines | `.claude/skills/writing-guidelines/SKILL.md` | site copy / prose |
| ui-debug | `.claude/skills/ui-debug/SKILL.md` | CSS / visual bug investigation |
| browser-testing | `.claude/skills/browser-testing/SKILL.md` | verifying rendered pages / deploys |
| audit | `.claude/skills/audit/SKILL.md` | systematic quality verification |
| worktrees | `.claude/skills/worktrees/SKILL.md` | parallel git worktree work |

Agents available: `implementer`, `researcher`, `validator`, `code-explorer`, `browser-agent`, `browser-tester`, `device-tester`, `tester` (symlinked in `.claude/agents/`).

> Deliberately excluded (not relevant to a static Astro site): `supabase`, `capacitor`, `react-best-practices`, `testing`. Add via `../agent-compounds/deploy.sh . --skills <name>` only if the stack changes.

## Rules

- **Separate aesthetic.** This site does NOT follow the neoMeta brand system. It has its own visual identity — never import `@neometa/brand` or apply pillar colors/voice.
- Tailwind 4 is wired through the Vite plugin (`@tailwindcss/vite`), not a `tailwind.config` PostCSS pipeline. Style via utility classes + `src/styles/global.css`.
- Run `npm run build` before considering changes done — it's the only quality gate.
- Follow patterns in neighboring `.astro` files.
