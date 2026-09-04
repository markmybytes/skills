# STYLE.md — Coding Style Guide

Canonical source for Go coding conventions and architecture rules in this
repository. `README.md` owns human onboarding; `AGENTS.md` owns agent
workflow. Rules here are pending maintainer confirmation.

## Principles

<!-- Starting defaults — not project policy until confirmed. Keep or delete
each; replace or extend with project-specific principles. -->

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

<!-- Safe default from common practice; reorder or replace with
project-specific priorities. -->

1. Security
2. Correctness
3. Architecture
4. Simplicity
5. Local consistency

## Naming Conventions

- <Go identifier, file, or package naming rule>

## Architecture and Placement

- <Where a concern belongs>
- <Ownership boundary>
- <When to split or combine files>

## Domain-Specific Rules

### Go

- <Rule requested or confirmed by the maintainer>

<!-- Add Testing Conventions or Examples and Exceptions only when needed. -->