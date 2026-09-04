<!-- Scaffolded skeleton, not project policy yet. The Principles bullets and
Rule Priority are unconfirmed defaults: keep or replace only the ones the
maintainer confirms. Leave every rule bullet for the maintainer to write. -->

# STYLE.md - Coding Style Guide

This guide is the single source of truth for styling and coding practices in
this repository. It is read by humans and coding agents alike; `AGENTS.md`
links here instead of duplicating these rules.

## Principles

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

### Python

- <Rule requested or confirmed by the user>