# Development Context Protocol

**Read `.claude/skills/CORE/SKILL.md` for full project context.**

---

## ⛔ STOP - MEMORY FIRST ⛔

**DO NOT respond until you complete this:**

```bash
cm context "<task>" --workspace . --json
```

□ Run command (task = 2-6 word summary)
□ Parse: `relevantBullets` + `historySnippets`
□ Integrate into working context
□ If task relates to prior work: `cass search "<keywords>" --json --limit 5`
□ Review past session context for relevant solutions/decisions

**cm context = playbook rules. cass search = raw past conversation matches. Both matter.**

**Skip = context blindness. No exceptions.**

---

## Development Workflow

**Cycle:** `/backlog` → `/plan-init` → `/plan-review` → `/work` → `/work-review` → `/merge`
