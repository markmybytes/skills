# HANDOFF — project-docs skill review

State after first review pass against user's reference repos
(`h730-toolbox`, `install-it`). Next session picks up from here.

## Reference repos (user-approved docs style)

- `C:\Users\User\Documents\Repositories\h730-toolbox` — developer-facing
  README, formal AGENTS.md doc-map table, CONTRIBUTING + DEPLOYMENT,
  1-line `CLAUDE.md` (`@AGENTS.md`) alias, no emoji.
- `C:\Users\User\Documents\Repositories\install-it` — user-facing README
  with shields.io `for-the-badge`, zh_Hant translation, no formal doc map.

Shared conventions across both: identical Rule Priority order
(Security → Correctness → Architecture → Simplicity → Local consistency),
explicit YAGNI, STYLE.md as canonical SSoT, AGENTS.md summarizes + links.

## Applied (this iteration)

1. SKILL.md description: pushier triggering — review/improve/stale/duplicated docs.
2. Intro run-on paragraph → 4 bullets (evidence, never invent, don't infer, Markdown-only).
3. VERIFY flow → 7-item checklist.
4. Doc-map rule: AGENTS.md always (table); README map in dev-onboarding flow
   for dev profile, Getting Started bullets only when several docs exist for user profile.
5. Generic agent-harness alias rule (after optional docs table): offer one-line
   redirect file (`CLAUDE.md` → `@AGENTS.md`), ask user first, follow tool syntax.
6. style-template.md: Rule Priority prefilled with the 5-item default, behind approval comment.
7. agents-template.md: emoji removed from Always/Ask First/Never.
8. readme-user-template.md: Documentation map comment — include when several docs exist.

## Deferred

- **STYLE.md handling** (next iteration): user considers it non-standard —
  format/structure/content vary hugely between repos; only common principles
  transfer. Currently under-triggered ("when conventions are substantial").
  Redesign its scaffold/assess flow later.
- **README translations** (zh_Hant pattern): no instruction added; add only if
  user translates READMEs regularly.
- **Test evals skipped** this round per user. Suggested when resuming:
  scaffold mode on a bare repo + audit mode on h730-toolbox, with-skill vs
  baseline, then `eval-viewer/generate_review.py`.

## Philosophy to preserve

SSoT everywhere except `README.md`/`AGENTS.md` (same purpose, different
audiences — audience-specific summaries allowed). One canonical owner per
operational fact; everyone else links.
