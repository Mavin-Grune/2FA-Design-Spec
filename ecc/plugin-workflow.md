# Plugin Workflow — Superpowers + ECC

## Key Concepts

- **Spec** = WHAT to build: pages, user flows, UI, business logic, API endpoints (created by `/brainstorming`)
- **Plan** = HOW to build it: TDD steps, file paths, code to write, commands to run (created by `/writing-plans`)

## Workflow Phases

See `plugin-workflow.png` for the full diagram.

### Phase 1: DB Design (Superpower, loop per domain)

Design the database schema one domain group at a time (AUTH, USER, ...).

1. Select a domain group
2. Run `/brainstorming` — domain modeling, entities, relations, seeds
3. Spec output: `docs/superpowers/specs/YYYY-MM-DD-db-<domain>.md`
4. Repeat for each domain group
5. Generate ERD from all domain specs

### Phase 2: Feature Specs (Superpower, loop per page)

Design each page one at a time, grouped by feature.

1. Select a feature group (AUTH, USER, ...)
2. Select a page in the group
3. Run `/brainstorming` — page design, user flows, UI components, business logic
4. Spec output: `docs/superpowers/specs/YYYY-MM-DD-<group>-<page>.md`
5. Repeat for each page in the group
6. Repeat for each feature group

### Phase 3: Spec Review (ECC)

Review all specs before implementation.

1. Run `/api-design` — endpoint consistency across specs
2. Run `/security-review` — auth flows, input handling
3. If issues found → go back to Phase 2 and fix specs
4. If OK → proceed to Phase 4

### Phase 4: Implementation (Superpower)

Build the code one feature group at a time.

1. Select a feature group
2. Run `/writing-plans` — TDD implementation plan per group
3. Run `/executing-plans` or `/subagent-driven-development` (built-in two-stage review)
4. Repeat for each feature group

### Phase 5: Quality Gate (ECC)

Final quality checks after all implementation is done.

1. Run `/security-review` — cross-feature security scan
2. Run `/pre-commit-review` — debug code, TODOs, unused code
3. Run `/check-quality` — format, lint, type-check

### Phase 6: Code Review & PR

1. Create PR with `gh pr create`
2. Review result:
   - Approved → Merge / Deploy
   - Issues found → Create GitHub Issue with `gh issue create`

### Issue Fix Loop

Triggered by Phase 6 (review issues) or external (bug reports, feature requests).

1. Claude Code reads issue with `gh issue view`
2. Fix code & commit
3. Push & update PR with `git push`
4. Return to Phase 6 for re-review
