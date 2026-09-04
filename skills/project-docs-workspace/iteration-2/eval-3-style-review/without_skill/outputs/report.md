# STYLE.md Structure Review

**Repo:** `C:\Users\User\AppData\Local\Temp\opencode\project-docs-evals\fixtures\desktop-style-bl`
**File reviewed:** `STYLE.md` (270 lines)

## Verdict

The structure is **good** — above average for a repo style guide. It is well-scoped, logically ordered, verifiable, and actionable. Findings below; no changes made.

## Strengths

1. **Clear scope statement up front** (line 3): "Applies to all code in the repository unless a language-specific section says otherwise." This is the single most important structural element — it tells the reader exactly what the document governs and where exceptions live.

2. **Rule priority order** (lines 11–17): Security → correctness → architecture → simplicity → local consistency. Most style guides omit this; it gives readers a deterministic tie-break when rules conflict. The "How To Use This Guide" section (lines 5–9) correctly frames rules as guidance, not law.

3. **Language split by top-level sections**: Frontend (Vue/TS) and Backend (Go) are cleanly separated with `---` dividers. Scoping is obvious.

4. **Decision tables over prose**: Naming conventions table (lines 29–36) and the 17-row i18n prefix table (lines 165–184) are scannable and unambiguous. This is the right format for reference material.

5. **Concrete code examples for every convention**: Inline typing, state grouping, functions, async patterns, Wails bindings, Pinia stores. Good examples are paired with "Good/Avoid" contrast.

6. **Verifiable claims, not vibes**: Window size claims cite actual values with source (`main.go: Width: 768, Height: 576`). I verified these against `main.go` lines 118–121 — accurate. The "why" for skipping `sm:`/`lg:` breakpoints is reasoned (lines 141–144), not asserted.

7. **Quick Review Checklist** (lines 244–265): A condensed, triaged (All/Frontend/Backend) checklist mirrors the body sections. This gives a fast path for reviewers without duplicating the detail.

8. **Related Docs section** (lines 267–270) closes the loop: `AGENTS.md` links back to `STYLE.md`, creating a healthy cross-doc cycle.

9. **Right size**: ~270 lines for a Go + Vue/TS codebase is proportionate. Not bloated, not skeletal.

## Weaknesses / Structural Gaps

1. **No table of contents.** At 270 lines with 5 top-level sections, navigation is easy today — but the doc will grow (see gap #2) and has no anchors/TOC. Cheap to add when sections exceed ~8.

2. **Explicitly admitted content gaps, no stubs.** Line 146: "Font-size scaling, padding density, and container widths are tracked separately and not yet documented." The structure has no placeholder section for typography/spacing — the info is "tracked separately" nowhere in this repo that I could find. Either add a stub section or drop the sentence; today it dangles.

3. **Uneven depth between language sections.** Frontend has 10 subsections (Basics → Pinia Stores); Backend has 4 (General, Error Handling, Database, Wails Bindings). This mirrors reality (frontend is where style actually varies) so it's not a defect — but the asymmetry means a reader looking for Go conventions finds a thin section. Consider a one-line note explaining the frontend bias.

4. **Normative vs. advisory is implicit, not structural.** "How To Use" says *apply with judgment*, but the Constraints block (lines 42–46) is hard rules ("Do not modify .github/workflows/**", "Do not hand-edit frontend/wailsjs/**"). The soft/hard split works in practice but is never labeled. Marking Constraints as *hard* rules vs. the rest as *guidance* would formalize the existing intent.

5. **Duplication with AGENTS.md — drift risk.** Linter commands (`npm run lint`/`go fmt`/`go vet`), the `Bind`/`EnumBind` rule, and the Porter DB lifecycle appear in both STYLE.md (lines 46, 204, 223–224, 226–240) and AGENTS.md (lines 24–32, 75–76). Cross-document duplication is the main structural risk here; a single source of truth (one doc points at the other) would prevent divergence.

6. **Scope creep in Backend/Database** (lines 222–224): the Porter close/reopen lifecycle is architecture, not style. It belongs in AGENTS.md (where it already exists, per #5). Minor.

7. **Missing sections** — gaps a style guide of this completeness could own:
   - **Testing conventions**: the repo has extensive tests (`pkg/*/*_test.go`, fuzz tests), yet no test-naming/structure guidance. Biggest content gap.
   - **Naming for Go files/identifiers beyond visibility** is one sentence (line 38); the TS half of the naming table is far richer.

8. **"Fixed with linter/formatter first"** (line 46) is a process rule buried inside a constraints list of path-protection rules. It's arguably the most important day-to-day rule in the doc — it deserves its own subsection.

## Non-issues (verified accurate)

- App name "install-it" matches `wails.json` and `main.go` Title.
- Window min `640x480`, default `768x576` match `main.go` (lines 118–121).
- `Bind`/`EnumBind` location claim matches `main.go` (lines 151–215).
- `HANDOFF.md` is a transient refactor checklist — correctly *not* listed in Related Docs.

## Suggested priority order (if acted on)

1. Resolve duplication with AGENTS.md (#5) — highest drift risk.
2. Fix the dangling "tracked separately" sentence (#2).
3. Add a testing-conventions section (#7) — most valuable new content.
4. Reclassify Constraint #8 and label hard vs. soft rules (#4).

## Changes

None. Review-only task; no files modified, nothing copied.