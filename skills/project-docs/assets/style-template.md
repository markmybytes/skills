<!-- REFERENCE SKELETON ONLY — for blank projects or as inspiration. Not the
default shape: STYLE.md is contract-defined (see SKILL.md). When scaffolding,
leave the <...> placeholders for the maintainer; delete a section only when
the project has no such layer or concern. -->

# STYLE.md - Coding Style Guide

<Scope, audience, and authority of this guide — this line is the ownership
declaration the contract requires: what is canonical here, what links to
another owner.>

## Principles

<!-- Starting defaults — not project policy until confirmed. Confirm with the
project/user before keeping any; delete unconfirmed bullets. Replace or extend
them with project-specific principles. -->

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

<!-- Context-dependent: naming, placement, API contracts, error mechanisms,
performance, E2E coverage, commit/semver, dependency, and deployment rules
require project choice. Add them as confirmed sections below, not here. -->

## Rule Priority

<!-- Safe default from common practice; confirm with the project/user before
keeping. Reorder or replace with project-specific priorities. -->

1. Security
2. Correctness
3. Architecture
4. Simplicity
5. Local consistency

## Naming Conventions

- <Language, framework, database, route, file, or component naming rule>

## Architecture and Placement

- <Where a concern belongs>
- <Ownership boundary>
- <When to split or combine files>

## Domain-Specific Rules

### <Language or framework>

- <Rule requested or confirmed by the user>

### <Language or framework>

- <Rule requested or confirmed by the user>

<!-- Add Testing Conventions or Examples and Exceptions only when needed. -->
