# STYLE.md Structure Review — install-it

**Scope:** Structure quality of `STYLE.md` (270 lines). Findings only — no files changed.
**Verified against:** `main.go`, `pkg/storage/db.go`, `pkg/status/status.go`, `frontend/package.json`, `frontend/eslint.config.js`, `frontend/src/i18n/en.json`, `frontend/src/**/*.vue`, `AGENTS.md`, `README.md`, `HANDOFF.md`.

---

## Verdict

The structure is **good** — arguably better than most style guides. Logical flow, deterministic conflict resolution, verifiable examples, and honest scoping. The problems found are **accuracy/currency issues, not structural defects**.

---

## Strengths (verified against the repo)

1. **Clear top-down flow**: scope → how-to-use → rule priority → general principles → naming → constraints → per-stack sections → checklist → related docs. A reader is told *when* to apply rules before *what* the rules are.
2. **Rule Priority Order section is the standout structural device.** An explicit precedence list (security > correctness > architecture > simplicity > local consistency) lets contradictory rules resolve deterministically. Rare and valuable.
3. **Consistent heading hierarchy** — all sections are H2, all subsections H3. No orphaned levels.
4. **Tables for enumerable rules** — naming conventions, layout breakpoints, i18n prefix catalog. The i18n prefix table (16 rows) is a genuine reference artifact, not prose.
5. **Paired good/avoid examples** for nearly every rule (inline typing, state grouping, functions, async, error handling). Each convention is checkable, not vibes.
6. **Quick Review Checklist mirrors the body rules** section-by-section — it is a faithful condensation, not a parallel document that can drift.
7. **Honest self-scoping** (line 146): explicitly declares the layout section covers only flex/grid/show-hide/modal width and that font scaling, padding, and container widths are "tracked separately and not yet documented." This prevents false expectations.
8. **Cross-document hygiene**: AGENTS.md links to STYLE.md (line 3), STYLE.md links back to AGENTS.md + README.md. No duplication of frontend conventions across docs. HANDOFF.md (a transient working doc) is correctly *not* in Related Docs.
9. **Verifiable factual claims check out**:
   - Window sizes: `main.go` has `MinWidth: 640, MinHeight: 480` and `Width: 768, Height: 576` — matches breakpoint table.
   - `vue/padding-line-between-tags`, `vue/block-order` (script→template→style), `vue/component-api-style` (script-setup), type-based `defineProps`/`defineEmits` — all actually enforced in `frontend/eslint.config.js`.
   - `npm run lint` / `npm run format` / `npm run type-check` — all present in `frontend/package.json`.
   - i18n prefixes (`action`, `desc`, `err`, `field`, `hw`, `label`, etc.) — match actual keys in `en.json`.
   - "Prefer `ref` over `reactive`" — no `reactive(` usage anywhere in `frontend/src`.
   - Porter DB close/reopen lifecycle on import — matches `main.go` and `pkg/storage/db.go`.
   - "Do not hand-edit `frontend/wailsjs/**`" — dir exists; eslint config ignores it (line 8).

---

## Findings (issues)

### F1. Internal contradiction: Go constant naming
`Naming Conventions` table row 3 says "Constants (TS/Go) | `UPPER_SNAKE_CASE`". This collides with the very next paragraph (line 38): Go uses casing for visibility, exported = PascalCase. The repo follows the paragraph, not the table: `status.Pending`, `status.Running`, `storage.Network`, `storage.Display` — all PascalCase exported constants. The table conflates two languages with different conventions.
**Recommendation:** split the row — TS `UPPER_SNAKE_CASE`, Go PascalCase (or drop Go from the row entirely since line 38 already covers it).

### F2. Factual drift: layout breakpoints rule is not followed
Line 133 states layout uses `md:` and `xl:` only and that `sm:`/`lg:` are skipped, arguing `sm:` is moot (min window is 640). Actual code violates this *within the doc's own scope definition* (line 146 includes "show/hide of elements"):
- `frontend/src/components/DriverEditor.vue:255` and `:281` — `hidden sm:inline` (show/hide = in scope).
- `lg:` usages (`lg:max-w-2xl`, `lg:text-sm`, `lg:size-8`, `lg:mt-4`) in `DriverFormComponent.vue`, `MatchRuleFormComponent.vue`, `ProgressStepper.vue` — container widths / font sizes, which line 146 explicitly defers, so those are *not* violations of the letter, only of the headline "md: and xl: only".

Net: the rule as written is aspirational. Either the code should be fixed to drop `sm:` show/hide, or the rule softened to "prefer `md:`/`xl:`; avoid `sm:` given the 640 min width; `lg:` only for the still-undocumented width/font axis."

### F3. Async convention drift: code prefers `async/await`
Line 102 says "Prefer promise chains (`.then().catch()`) for Wails binding calls. Reserve `async/await` for cases where it genuinely simplifies logic." In practice most binding call sites use `async/await`: `CommandStatusModal.vue` (`dispatchCommand`, `handleAbort`, `show`), `pages/porter.vue` (`handleDownloadUrl`), `pages/drivers/index.vue` (`reloadGroups`), `pages/index.vue` (`handleSubmit`), `UpdateModal.vue`. Either the codebase has drifted, or "genuinely simplifies" is doing heavy lifting. Recommend either enforcing the rule or rewording it to reflect actual practice — a style guide nobody follows is documentation debt.

### F4. Minor: no table of contents / section anchors
At 270 lines with ~25 subsections, navigation is by scroll. A 5-line Contents block at the top would help; doc is just at the size threshold where it stops being optional.

### F5. Minor: no maintenance metadata
No "how to update this guide" note, no last-reviewed date/version. For a canonical conventions doc that will drift (see F2/F3), a one-line maintenance convention (e.g. "changes to this file require a PR; update the Quick Review Checklist in the same change") would keep the checklist honest.

### F6. Minor: section asymmetry — Go coverage is thin
Frontend gets 9 subsections; Go gets 3 (General, Error Handling, Database + Wails Bindings). No Go naming conventions beyond visibility, no struct/layout guidance, no concurrency notes. The `pkg/` tree is substantial (`matching`, `porter`, `update`, `sysinfo`, `cputemp`, `errcode`). This may be deliberate (Go is idiomatic, less needs saying) — but flag it as a known asymmetry.

### F7. Minor: coverage gap — no testing conventions
No section on test naming/structure (table-driven tests, `*_test.go` placement, frontend Vitest strategy). The checklist has no testing items. The repo has ~20 test files. Not an error — testing may be intentionally out of scope for a *style* guide — but it is the largest topical gap.

### F8. Cosmetic: "Constraints" mixes two kinds of rules
File-ownership rules (`.github/workflows/**`, `vendor/**`, `wailsjs/**`, secrets) sit alongside a process rule ("fix with linter/formatter first"). Functionally fine, but splitting into "Do Not Touch" vs "Workflow" would make the section scannable.

---

## Conclusion

Structure: **keep it**. The skeleton (priority order, per-stack split, mirror-checklist, explicit scope boundaries) is a model other repos could copy. Fix F1 (real contradiction), reconcile F2/F3 (rule-vs-reality drift), and the minor items are optional polish.

---

## Files copied / modified

None. Review was explicitly findings-only ("don't change anything"), so no documentation files were copied into this folder. If the STYLE.md edits implied by F1–F3 were applied, they would all be changes to `STYLE.md` only.