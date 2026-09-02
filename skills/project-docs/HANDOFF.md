# HANDOFF — project-docs skill

Pickup doc for a fresh session. Read SKILL.md + assets first, then this.
Worked via the **skill-creator** skill (`C:\Users\User\.agents\skills\skill-creator`)
— its loop: capture intent → draft/edit skill → run test prompts (with-skill vs
baseline subagents) → generate eval viewer (`eval-viewer/generate_review.py`) for
human review → read `feedback.json` → improve → repeat → description optimization
(`scripts.run_loop`) → optional packaging (`scripts.package_skill`).
Follow that loop for everything below marked TODO.

## Skill under development

`skills/project-docs/` in this repo (C:\Users\User\Documents\Repositories\skills).

Purpose: scaffold / audit / refine / consolidate project Markdown docs —
baseline `README.md`, `AGENTS.md`, `CONTRIBUTING.md`; optional `STYLE.md`,
`DEPLOYMENT.md`, `SECURITY.md`, etc. Flow: CLASSIFY → GATHER → PLAN → ASK →
EXECUTE → VERIFY, one approval gate (`lgtm`/`go`/`approve`), audit mode
read-only. 5 templates in `assets/`.

## Design decisions (user-confirmed — do not revert)

1. **SSoT philosophy**: one canonical owner per operational fact;
   `README.md`/`AGENTS.md` are the ONLY docs allowed audience-specific
   duplicated summaries (same purpose, different audiences). Everyone else
   links to the owner. Adjacent docs (status/migration) reference-only.
2. **STYLE.md = exactly two jobs**: scaffold an EMPTY skeleton (never author
   rules) and review existing STYLE.md against a 5-point opinionated structure
   (read-only findings). No content editing, no rule mining from code — content
   edits only on explicit user request. Rationale: STYLE.md encodes maintainer
   taste; human/AI code is unreliable ground truth. Principles defaults in the
   template are the only pre-written content, behind the approval gate.
3. **Evidence rules**: never invent facts/policy/commands; don't infer
   undocumented policy; user-provided facts or repo evidence only.
4. **Templates mirror the user's reference repos** (below): user-README with
   shields `for-the-badge` badges, dev-README with doc map in onboarding flow,
   AGENTS.md with doc-map table, no emoji anywhere.
5. **STYLE.md handling**: always recommended when code conventions exist,
   created only on user approval; its review rides audit mode.

## Reference repos (user-approved style — compare against these)

- `C:\Users\User\Documents\Repositories\h730-toolbox` — developer-facing
  README, formal AGENTS.md doc-map table, CONTRIBUTING + DEPLOYMENT, 1-line
  `CLAUDE.md` (`@AGENTS.md`) alias.
- `C:\Users\User\Documents\Repositories\install-it` — user-facing README with
  shields badges, zh_Hant README translation, no formal doc map.
- Shared: identical Rule Priority (Security → Correctness → Architecture →
  Simplicity → Local consistency), explicit YAGNI, STYLE.md canonical for
  conventions, AGENTS.md summarizes + links.

## Git state

- Branch `project-docs-review` (pushed; PR not opened).
- Commit `59cfdaa` = first-iteration fixes + first HANDOFF.
- **Uncommitted**: all second-pass (STYLE.md redesign), third-pass (full
  re-review via independent reviewer), and HANDOFF updates. Commit + push
  these first.
- Repo commit style: conventional commits, lowercase, body explains why
  (commit-craft skill). Last message used
  `refactor(project-docs): apply review fixes and handoff`.

## Done (summary — details in git history / prior handoff versions)

- Pass 1: description pushier; intro + VERIFY as checklists; README doc-map
  rule (AGENTS always, dev README in onboarding flow); agent-harness alias
  rule (`CLAUDE.md` → `@AGENTS.md`, ask first, non-Markdown aliases allowed);
  agents-template emoji removed; readme-user doc-map comment.
- Pass 2: STYLE.md redesigned to two-job contract; mining pipeline removed;
  style-template Principles merged/generalized (YAGNI named, trust boundaries,
  follow-existing-patterns, dependency rule sharpened, scope line = ownership
  declaration).
- Pass 3 (independent reviewer: 4 breaking + 8 polish, all but 1 nit fixed):
  contract reword (project-specific rules vs Principles defaults); LICENSE/
  alias carve-out from "Markdown only"; STYLE.md review wired into audit mode;
  review rubric vs scaffold placeholders reconciled; stale ASK-gate item
  removed; contributing-template conventions gated behind approval; dev-README
  doc map moved into onboarding flow (duplicate removed); frontmatter
  over-promise trimmed.
- Declined nit: EXECUTE order lists STYLE.md before optional docs ("if in
  scope" reading accepted).

## TODO (in order)

1. **Commit + push** the uncommitted second/third-pass work on
   `project-docs-review`.
2. **Test evals** (user skipped so far — next milestone). Suggested eval set
   for `evals/evals.json`:
   - Scaffold on a bare/small repo (README + AGENTS + CONTRIBUTING baseline).
   - Audit mode on h730-toolbox (should find little — it's user-approved).
   - STYLE.md review on install-it (5-point checklist, read-only findings).
   - Mono-stack project (single-language CLI/library) — exercises concern-based
     STYLE.md sections and the single-stack path.
   - Single-doc scope request (e.g. "just fix my README") — scope override.
   Run per skill-creator: spawn with-skill AND baseline (no skill) subagents
   per prompt in the same turn, workspace `<skill-name>-workspace/iteration-N/`,
   capture `timing.json` from task notifications, grade assertions, run
   `python -m scripts.aggregate_benchmark`, then ALWAYS generate the viewer
   (`eval-viewer/generate_review.py`, `--static` if no display) BEFORE
   evaluating outputs yourself.
3. **Description optimization** (`scripts.run_loop`) after content is stable —
   use the model ID powering the session; 20 trigger queries, mix of
   should/should-not, near-miss negatives.
4. **Packaging** via `scripts.package_skill` (only if `present_files` tool
   available, else just point at the directory).

## Deferred / declined

- README translations (zh_Hant pattern): no instruction added; revisit only if
  user translates READMEs regularly.
- EXECUTE-order nit above.
- Blind comparison mode: not needed so far.

## User preferences

- Terse communication; no emoji; no fluff.
- Wants docs matching the two reference repos; SSoT is the core philosophy.
- Prefers deciding via quick multiple-choice questions; defers subjective
  things (STYLE.md content) to themselves, not the agent.
