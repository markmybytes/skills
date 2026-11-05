<!--
STYLE.md layout. Exists only when a coherent coding-convention family
carries evidence-backed rules that no config or existing doc carries;
otherwise fold the rules into
ARCHITECTURE.md (placement) / AGENTS.md (generated-file bans). Created only
through the approval gate — never auto-created. Every rule bullet is the
maintainer's: fill-ins stay placeholders, never invented. Headings follow
the repo language; keep heading ORDER. Delete this comment when finished.
-->

# STYLE.md

{{Scope declaration: one line, own scope only — this file is the source of truth for coding conventions and style rules.}}

<!-- CODE CONVENTIONS: per language/domain sections, one bullet per rule.
Rule bullets state convention + why (pitfall lineage beats abstract
principles). Never restate: lint-enforceable rules (point at the config),
framework-official semantics, or what a config file can answer. Principles
and priority lists never ship here. -->

## {{Language or domain}} conventions

- {{convention}} — {{why}}

## {{Language or domain}} conventions

- {{convention}} — {{why}}

<!-- GENERATED FILES: gate — only when the repo builds named artifacts. List = artifacts + regenerate command; hand-edit policy lives in
AGENTS.md Never list. -->

## Generated files

- `{{artifact}}` — {{source and regenerate command}}
