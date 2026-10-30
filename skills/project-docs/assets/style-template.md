<!--
REFERENCE SKELETON ONLY — for blank projects or as inspiration. Not the
default shape: STYLE.md is contract-defined. When scaffolding, leave the
{{...}} fill-ins for the maintainer; offer the Principles and Rule Priority
safe-defaults behind the approval gate; delete a section only when the project
has no such layer or concern. Delete this comment when finished.
-->

# STYLE.md - Coding Style Guide

{{Scope, audience, and authority of this guide — the ownership declaration:
what is canonical here, what links to another owner.}}

## Principles

<!-- Safe defaults, not project policy until confirmed; the user keeps or
drops each in the approval request. Replace or extend with project-specific
principles. -->

- Security and correctness first.
- Readable names and simple code over cleverness.
- Follow existing project patterns before introducing new ones.
- Explain why in comments, not what the code does.
- YAGNI: avoid speculative features and abstractions; justify an abstraction by current reuse or clarity.
- Keep responsibilities cohesive and coupling low.
- Validate untrusted input at trust boundaries (network input, files, argv, environment); prefer allowlists where applicable.
- Handle errors explicitly without leaking sensitive details.
- Tests for meaningful behavior; tests should fail when that behavior breaks.
- Update docs when behavior or public interfaces change.
- Dependencies must earn their maintenance cost; review before adding.

## Rule Priority

<!-- Safe default from common practice; confirm before keeping. Reorder or
replace with project-specific priorities. -->

1. Security
2. Correctness
3. Architecture
4. Simplicity
5. Local consistency

## Naming Conventions

- {{Language, framework, database, route, file, or component naming rule}}

## Architecture and Placement

- {{Where a concern belongs}}
- {{Ownership boundary}}
- {{When to split or combine files}}

## Commands and Tooling

<!-- STYLE.md owns style-related commands. Point at the tool config rather
than restating lint-enforceable rules. -->

- {{Lint command and scope}}
- {{Format command and scope}}
- {{Check command and scope}}

## Generated Files

- {{Generated artifact and its source}}
- {{Regeneration command}}
- {{Hand-edit policy}}

## Domain-Specific Rules

### {{Language or framework}}

- {{Rule requested or confirmed by the user}}

<!-- Add Testing Conventions or Examples and Exceptions only when needed. -->
