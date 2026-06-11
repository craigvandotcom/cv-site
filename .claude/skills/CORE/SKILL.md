---
name: core
description: Index to all cv-site (craigvan.com) context. Points to AGENTS.md for the summary and to docs/memory for load-on-demand site facts.
---

# cv-site — CORE (context index)

This is the **index** to cv-site context. It does not restate the summary and does
not inline deep facts — it routes to both.

- **Summary** (stack, commands, architecture, skills, rules) → **read
  `../../../AGENTS.md`** (repo root). Single source for summary-fidelity facts.
- **Exhaustive facts** → the load-on-demand pointers below.

> **Single-source rule.** Summary facts live in AGENTS.md; exhaustive facts live in
> the targets below; this SKILL.md only routes.

---

## Load on demand

| Source | When to read |
|---|---|
| `memory/auto/MEMORY.md` (repo root) | Durable site facts/rules — start here; includes copy-and-traffic principles (no revenue-first copy; "homepage is death" routing) |
| `_docs/website/PRD.md` + `SPEC.md` | Product requirements + page/feature spec for the site build |
| `_docs/website/IMPLEMENTATION.md` | How the build was executed; implementation decisions |
| `_docs/website/mockups/`, `reference-screenshots/`, `research/` | Visual direction + competitive research behind design choices |
| `docs/` + `_docs/craigvan-site/` | Earlier-era planning notes (landing-page-eat-os, website outlines) — historical; verify against the live site before relying |
| `src/pages/*.astro` | The routes themselves — file-based; neighboring files are the pattern reference |

---

## Task-type → skill routing (method skills)

Generic skills are symlinked from agent-compounds (method only, zero app facts).
**Site-specific:** `writing-guidelines` is a REAL local skill (this site's voice —
deliberately NOT neoMeta brand voice).

| Task | Load |
|---|---|
| Site copy / prose | `writing-guidelines/SKILL.md` (local, site voice) |
| Visual/layout design | `web-design-guidelines/SKILL.md` |
| CSS / visual bugs | `ui-debug/SKILL.md` |
| Verifying rendered pages / deploys | `browser-testing/SKILL.md` |
| Planning / pre-planning | `planning/SKILL.md` · `brainstorming/SKILL.md` |
| Quality verification | `audit/SKILL.md` |

## Hard rules (also in AGENTS.md — repeated because violations are costly)

- **Separate aesthetic:** never apply `@neometa/brand` (pillar colors, voice) here.
- Only quality gate: `npm run build`.
