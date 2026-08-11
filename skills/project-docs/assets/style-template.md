<!-- Adaptive STYLE.md template. Keep only user-requested or user-confirmed sections; delete empty ones. -->

# STYLE.md - Coding Style Guide

<Scope, audience, and authority of this guide.>

## Common Principles

<!-- Optional before approval. Present these as suggestions; keep only if the
project/user approves them. After approval they become project policy — remove
this label and the note below. Replace or remove them with project-specific
rules. -->

> Optional defaults — safe baseline, not project policy. Confirm with the
> project owner before keeping any. After approval, remove this note and the
> label above.

- Security and correctness first.
- Readable names and simple code over cleverness.
- Explain why in comments, not what the code does.
- Avoid speculative features and abstractions; justify an abstraction by current reuse or clarity.
- Keep responsibilities cohesive and coupling low.
- Validate untrusted input at server boundaries; prefer allowlists where applicable.
- Handle errors explicitly without leaking sensitive details.
- Tests for meaningful behavior; tests should fail when that behavior breaks.
- Update docs when behavior or public interfaces change.
- Pin or review dependencies where relevant.

<!-- Context-dependent: naming, placement, API contracts, error mechanisms,
performance, E2E coverage, commit/semver, dependency, and deployment rules
require project choice. Add them as confirmed sections below, not here. -->

## Rule Priority

<Include only confirmed project priorities, such as security, correctness,
architecture, readability, and consistency.>

## Core Principles

- <Requested or user-confirmed project principle>

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
