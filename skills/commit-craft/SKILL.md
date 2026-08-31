---
name: commit-craft
description: >
  Write well-crafted git commit messages using Conventional Commits types combined with
  Chris Beams' seven rules. Use whenever the user asks to write, generate, or improve a
  commit message, says "commit message", "write a commit", "/commit", "commit this",
  asks what to put in a commit, or has staged or uncommitted changes that need a message —
  even if they never say "commit message" (e.g. "summarize my staged changes",
  "what should I name this commit?", "help me describe this change").
---

Diff says *what*. Message exists for *why* — reader is `git log` six months later.
Git tools (`log --oneline`, `shortlog`) treat text before first blank line as title.

## Process

1. Read change first: `git diff --cached` (or `git diff`, `git status`). Message from guessed diff = wrong message.
2. Skim `git log --oneline -15`. Repo uses different style consistently (lowercase, no prefixes)? Match repo — consistency beats external standard.
3. Write per rules below. Output message in fenced code block. No `git commit` unless asked.

## Format

```
<type>(<optional scope>): <imperative summary, <=50 chars>

<optional body, wrapped 72, explains why>
```

## Subject

- Type: `feat` `fix` `refactor` `perf` `docs` `test` `chore` `build` `ci` `style` `revert`. Scope optional.
- Imperative — Git itself does (`Merge branch 'x'`, `Revert "y"`). Test: subject completes *"If applied, this commit will ___"*. ✓ "fix login crash" ✗ "fixed bug" ✗ "changing behavior".
- <=50 chars, hard limit 72. Can't summarize in 50? Too many changes in one commit — say so, suggest splitting.
- No trailing period.
- Blank line before body — always when body exists. Without it `log`, `shortlog`, `rebase` misparse. Most-broken rule.

## Body — why, not what

Diff shows what. Code shows how. Body answers:

- How did it work before, what was wrong?
- Why this approach? Side effects, unintuitive consequences?
- What would future debugger wish they knew?

No body when subject fully self-explanatory (typos, version bumps, obvious renames). Bloated message = missing message, both bad.

Wrap 72. Issues at bottom: `Closes #42`, `Refs #17`. Breaking: `!` after type/scope + `BREAKING CHANGE:` paragraph.

## Never

- "This commit …", "I", "we"
- Restating diff ("modified 3 files, added foo") — diff's job
- Implementation details — belong in code comments

## Examples

Trivial — no body:
```
docs: fix typo in installation guide
```

Bug fix — why not obvious from diff:
```
fix(auth): lock session store during token refresh

refresh_session reads the session and writes the rotated token in two
separate steps, so two concurrent refreshes could interleave and hand
out tokens from a revoked session. A single store-level lock around all
session reads and writes closes the window.

Closes #342
```

Breaking:
```
refactor(api)!: rename /v1/orders to /v1/checkout

BREAKING CHANGE: clients on /v1/orders get 410 after 2026-06-01;
migrate to /v1/checkout before then.
```

Bad — vague subject, past tense, no why, mixed changes:
```
fixed bug

fixed two build-breaking issues: reverted the visitor class and
eliminated the failing tests, also some tweaks to package files.
```

---

Source: Chris Beams, "How to Write a Git Commit Message" — https://cbea.ms/git-commit/.
