# cv-site

## MANDATORY FIRST ACTION

Read `AGENTS.md` for project context, stack, commands, and conventions.

## Project-Specific Context

- **craigvan.com — Craig's personal site.** Astro 6 + Tailwind 4, static, deployed on Vercel. Own git repo.
- **Deliberately separate aesthetic from neoMeta.** Do NOT apply the `@neometa/brand` system (pillar colors, voice rules) here — this site has its own identity.
- **Minimal by design:** no database, no React islands, no test/lint scripts. The only quality gate is `npm run build`.
- **Agent tooling** is deployed from `../agent-compounds` via symlinks in `.claude/`. To add/remove tools: `../agent-compounds/deploy.sh . --list` then re-run with the parts you want. Do not copy tool files in — keep them symlinked so the canonical version stays the source of truth.
