<!--
CONTRIBUTING.md layout. Gates live per section — remove gated-out sections
entirely. Headings follow the repo language; keep heading ORDER. Branch and
commit conventions are approvals-gate defaults, not pre-shipped policy.
Delete this comment when finished.
-->

# Contributing

<!-- OPENER: pointers + gate line. Real file links only, no section anchors.
Gate each link: only when the target file exists (Tier 0 docs exist per the
approved plan; ask/offer docs gate). Setup commands live in README (human
path). Workflows precede PR mechanics.
-->

See [README.md](README.md) for setup and development commands.

{{Scope declaration: one line, own scope only — this file is the source of truth for the contribution workflow.}}

Before making any code change, read [ARCHITECTURE.md](ARCHITECTURE.md) when it exists.

<!-- When ARCHITECTURE.md exists, drop "when it exists" from the shipped line;
when it does not, drop the whole line. -->

<!-- WORKFLOW: naming grammar from git-branch reality (real branch names as
examples — each resolves to a real branch or commit, otherwise mark as
`<example>`), not invented prefixes. Commit messages default to Conventional
Commits (`<type>(<scope>): <subject>`); repo evidence of a different grammar
replaces the default at the gate. Traceability anchor = the repo's tracker
key, in branch and PR title; commit policy per repo. State
each why once, then the rule. Rules stay mechanism-neutral: never name the
enforcement layer (hook, branch protection, bot). Never pad back sections
the user dropped. -->

## Branches and commits

Branch from the default branch. One concern per branch.

{{Branching rule with real examples}}

Commit messages follow [Conventional Commits](https://www.conventionalcommits.org/): `<type>(<scope>): <subject>`.

{{Traceability anchor rule + why + commit policy}}

## Pull requests

<!-- PR section: closes with a self-check list. Checklist items = verifiable
states ("CI green", "docs updated"), never advice. Include the
documentation-update obligation. An Issues section ships only when the repo
uses GitHub Issues; otherwise omit it. -->

{{Open-to-merge sequence}}
{{Review workflow}}

Before submitting:

- [ ] {{verifiable state}}
- [ ] {{verifiable state}}
- [ ] Documentation updated when behavior or interfaces changed

{{Link to CODE_OF_CONDUCT.md, SECURITY.md, SUPPORT.md, or templates when present.}}
