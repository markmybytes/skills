<!--
User-facing README layout. Gates live per section — remove gated-out
sections entirely. Headings follow the repo language; keep heading ORDER.
Replace every {{...}} with a user-provided or confirmed fact; never invent
badges, URLs, screenshots, or policies; delete unused examples instead.
Delete this comment when finished.
-->

<a id="readme-top"></a>

<!-- Repo-status badges: keep only badges supported by real repository
metadata. Confirm the default branch (main/master) and the real license path
before shipping the link definitions at the bottom. -->

<div align="center">

[![Tag][tag-shield]][tag-url]
[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![License][license-shield]][license-url]

</div>

<!-- LOGO: delete the whole block when no real project logo exists. -->
<br />
<!-- Title = repo name (from the remote URL), not the product name; the product
name belongs in the positioning line. Positioning leads with what this
repository is, then the product/platform in parentheses — never describe only
the product. -->
<div align="center">
  <a href="https://github.com/{{owner}}/{{repo}}">
    <img src="{{logo URL or repository path}}" alt="{{repo name}} logo" width="80" height="80">
  </a>

  <h3 align="center">{{repo name}}</h3>

  <p align="center">
    {{one-line positioning: what it is, who it is for}}
    <br />
    <!-- Keep only links backed by real issue templates or pages. -->
    <a href="{{bug-report URL}}">Report Bug</a>
    ·
    <a href="{{feature-request URL}}">Request Feature</a>
  </p>
</div>

<!-- ABOUT: a real screenshot or demo image only when one exists. -->

## About the Project

{{Project positioning in one short paragraph from user input or repo description. Never restate details other docs own.}}

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- BUILT WITH: gate — only if project-identity technologies exist.
Copy real generated markup from https://badges.pages.dev/ (Simple Badges)
with style=for-the-badge and logoColor=white. Only major technologies;
link each badge to its official site when known. Never invent badge slugs
or URLs. The badge below is an example — replace or delete. -->

### Built With

[<img src="https://img.shields.io/badge/font%20awesome-538cd7?style=for-the-badge&logo=fontawesome&logoColor=white">](https://fontawesome.com/)

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- USAGE: gate — only when a representative workflow exists.
Shortest useful flow; one example. Collapse long secondary examples with
<details>. -->

## Usage

### {{Workflow or feature}}

{{Explain the shortest useful workflow.}}

```{{language}}
{{Representative usage example}}
```

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- PREREQUISITES: gate — only prerequisites a config file cannot answer.
Shape per SKILL.md README profiles: one line per item — name plus one
qualifier or link. Never explain how a version resolves; never ship manifest
facts. A version manager covers the runtime it manages: list it, drop the
bare language bullet. Examples: VPN, version manager, accounts, services. -->

## Getting Started

### Prerequisites

- {{Required tool, service, or access not derivable from config}}

<!-- INSTALL: executable commands only. -->

### Installation / Setup

```sh
{{Executable installation commands}}
```

<!-- COMMANDS: gate — only when day-to-day commands exist.
User-relevant commands only (run, serve, build for use). When this README is
the repo's sole human-facing commands doc, include contributor basics
(development, test, lint) — evidence-backed. Purpose column = non-obvious
behavior only, e.g. "installs Playwright browsers on first run". Never
transcribe versions, flags, or formulas a script/config answers.
Agent-facing depth lives in AGENTS.md. -->

### Commands

| Command       | Purpose     |
| ------------- | ----------- |
| `{{command}}` | {{purpose}} |

<!-- DOCUMENTATION: one row per root doc that exists, purpose from its
self-description. AGENTS.md never appears here — human docs do not
reverse-reference it. Delete the section when the list would hold fewer
than three rows. -->

### Documentation

- [DEPLOYMENT.md](DEPLOYMENT.md) — Infrastructure, release process, and deployment details
- [CONTRIBUTING.md](CONTRIBUTING.md) — Branches, commits, and PRs
- [SECURITY.md](SECURITY.md) — Vulnerability reporting, security policy
- [ARCHITECTURE.md](ARCHITECTURE.md) — System design and code placement
- [{{doc}}]({{doc}}) — {{purpose}}

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- Reference-style links: replace fill-ins with real repository values;
delete unused entries. -->

[tag-url]: https://github.com/{{owner}}/{{repo}}/releases
[tag-shield]: https://img.shields.io/github/v/tag/{{owner}}/{{repo}}?style=for-the-badge&label=LATEST&color=%23B1B1B1
[contributors-shield]: https://img.shields.io/github/contributors/{{owner}}/{{repo}}.svg?style=for-the-badge
[contributors-url]: https://github.com/{{owner}}/{{repo}}/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/{{owner}}/{{repo}}.svg?style=for-the-badge
[forks-url]: https://github.com/{{owner}}/{{repo}}/network/members
[stars-shield]: https://img.shields.io/github/stars/{{owner}}/{{repo}}.svg?style=for-the-badge
[stars-url]: https://github.com/{{owner}}/{{repo}}/stargazers
[issues-shield]: https://img.shields.io/github/issues/{{owner}}/{{repo}}.svg?style=for-the-badge
[issues-url]: https://github.com/{{owner}}/{{repo}}/issues
[license-shield]: https://img.shields.io/github/license/{{owner}}/{{repo}}.svg?style=for-the-badge
[license-url]: https://github.com/{{owner}}/{{repo}}/blob/main/LICENSE
