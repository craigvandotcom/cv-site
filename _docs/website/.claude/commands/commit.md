**MANDATORY: Use TodoWrite to create a task list BEFORE starting:**

- Check/create branch
- Run pre-commit checks (format, lint, type-check, test, build)
- Stage all changes (git add -A)
- Commit and push

Mark each as `in_progress` when starting, `completed` when done.
**This ensures no steps are forgotten.**

---

- check if we are on main
  - if yes, create new branch
  - if no, continue
- **SAFETY CHECK**: Show current changes and verify before proceeding
  - Run `git status --short` and display output
  - **If you see ANY deletions (D)**: STOP and ask user "You're about to delete X files. Is this intentional?"
  - Wait for user confirmation before proceeding if deletions are present
- run all of our pre-commit checks (in order):
  1. pnpm format
  2. pnpm lint
  3. pnpm astro check
  4. # No separate test step for static site - covered by build
  5. pnpm build
- if any check fails:
  - fix issues automatically where possible
  - report remaining issues
  - do not commit until all checks pass
- **CRITICAL**: Stage ALL changes with `git add -A` before committing
  - NEVER use selective staging (e.g., `git add features/`)
  - The `pnpm format` command may modify files - these MUST be included
  - This has caused lost work in the past - ALWAYS stage everything
- **VERIFY**: Run `git status --short` again after staging
  - All files should show as staged (M/A, not " M")
  - Double-check for unexpected deletions
- commit all changes to the branch
- push the branch to the remote repository
