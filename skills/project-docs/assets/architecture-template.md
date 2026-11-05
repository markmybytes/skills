<!--
ARCHITECTURE.md layout. Gates live per section — remove gated-out sections
entirely. Headings follow the repo language; keep heading ORDER. No neighbor
pointers ("X lives in Y.md") — they live in the map holders. One
scope-declaration line is required. Delete this comment when finished.
-->

# {{project name}} — architecture

{{Scope declaration: one line, own scope only — this file is the source of truth for repo structure, code placement, and domain contracts.}}

<!-- REPO STRUCTURE: non-standard deltas only. Standard framework layout =
one pointer line ("layout follows <framework> official conventions... no
transcription"). Each delta = where it lives + who owns it at runtime. -->

## Repo structure

{{Pointer sentence for standard layout.}}

- `{{dir}}/` — {{why it exists, runtime responsibility}}

<!-- CODE PLACEMENT: where a concern belongs, ownership boundaries, when to
split or combine. Placement facts only — which directory new or relocated
code belongs to. Lifecycle-bound API usage and routing-extension rules are
domain contracts — they go in the contract sections, not here. When no
STYLE.md exists, coding conventions fold here. Each rule = convention + why.
Never restate framework-official semantics or what a lint config already
enforces — pointer to the config instead. -->

## Code placement

- {{convention}} — {{why}}
- {{lint/format target}} is decided by `{{config file}}` — not restated here.

<!-- DOMAIN CONTRACTS: gate per section — official docs do not say it, and a
change here breaks things (pitfall + reason). Group by domain with lead-in
bullets ("Data fetching:", "Hydration:", "Build:"). This is the load-bearing
content that survives every review sweep. One section per domain with
contract evidence — official docs silent, changes break; fold smaller
domains upward. Group domains with `###` subheadings — agents hesitate below
`##`-level structure otherwise — replace plain colon lead-ins. -->

## {{Domain}} contracts

### {{Domain}}

- {{contract or pitfall}} — {{reason}}
