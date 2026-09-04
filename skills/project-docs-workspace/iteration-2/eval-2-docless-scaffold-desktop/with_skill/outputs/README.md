<a id="readme-top"></a>

<!-- PROJECT SHIELDS -->
<div align="center">

[![Tag][tag-shield]][tag-url]
[![Contributors][contributors-shield]][contributors-url]
[![Forks][forks-shield]][forks-url]
[![Stargazers][stars-shield]][stars-url]
[![Issues][issues-shield]][issues-url]
[![License][license-shield]][license-url]

</div>

<br />
<div align="center">

  <h3 align="center">install-it</h3>

  <p align="center">
    A Windows driver and software installer with automatic hardware matching.
  </p>
</div>

<!-- ABOUT THE PROJECT -->

## About the Project

install-it is a Windows desktop application that helps you install drivers and
software after a fresh system setup. You organize installers and commands into
named options, optionally attach match rules so the right options are selected
automatically based on your hardware, then run them one by one or in parallel.

Built for people who reinstall Windows often, and for anyone who wants a
repeatable, hardware-aware way to bring a machine back up to speed.

Key features:

- **Install options** — group installers or commands into options; entries run
  in sequence, or one at a time for mutually exclusive options.
- **Match rules** — automatically select options based on device names for the
  CPU, motherboard, GPU, memory, network, and storage, using contains, equal,
  and regex patterns against live hardware queried through WMI.
- **Execution control** — track per-entry status, abort running commands, and
  treat early completion or allowed exit codes as success.
- **Setup tools** — one-click access to Windows management consoles, settings,
  and power actions (shutdown, reboot, BIOS/UEFI).
- **Import / Export** — back up your options and app settings to a ZIP and
  restore them on another machine.
- **Automatic updates** — checks GitHub Releases for new versions and updates
  in place with SHA-256 verification.
- **CPU temperature** — optional live CPU temperature display (uses the PawnIO
  driver, installed automatically when the feature is enabled).

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- BUILT WITH -->

### Built With

- [Go](https://go.dev/) — backend and system integration
- [Wails v2](https://wails.io/) — desktop application framework
- [Vue 3](https://vuejs.org/) — frontend
- [SQLite](https://www.sqlite.org/) — local data storage

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- USAGE EXAMPLES -->

## Usage Examples

### Create an install option

On the main page, create an option in the Network, Display, or Miscellaneous
category and add entries pointing at the installers or commands you want to
run. When the option is selected, all its entries execute in order.

### Auto-select options with match rules

Match rules describe your hardware with patterns — for example, a GPU name
matching `RTX 30` with the *Contain* operator. When the app detects your
hardware, options whose rules match are selected automatically, so you do not
have to remember which option belongs to which machine.

### Run and monitor

Select the options you want and execute. The status view shows progress and
results per entry; running tasks can be aborted, and you can choose what
happens on completion (nothing, reboot, shutdown, or boot to BIOS/UEFI).

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- GETTING STARTED -->

## Getting Started

### Prerequisites

- Windows 10 or 11
- Microsoft Edge WebView2 Runtime — use a **bundled** release to skip a
  system-wide install, or let the app download the runtime itself.

### Installation / Setup

1. Download the latest release ZIP from the
   [Releases](https://github.com/markmybytes/install-it/releases) page.
   - `install-it.windows-x64.zip` / `install-it.windows-x86.zip` — includes
     data files; uses the system WebView2 Runtime.
   - `install-it.windows-x64-bundled.zip` / `install-it.windows-x86-bundled.zip`
     — also includes the WebView2 Runtime files.
2. Extract the ZIP to a folder (for example `C:\tools\install-it`).
3. Run `install-it.exe`.

The app keeps its configuration in a `conf` folder (SQLite database and
settings file) and its installers in a `drivers` folder next to the
executable. The optional CPU temperature feature installs the PawnIO driver on
your system; the driver stays installed when the feature is disabled.

### Development

```sh
# Install the Wails v2 CLI
go install github.com/wailsapp/wails/v2/cmd/wails@v2.12.0

# Build the frontend and run the app in dev mode
wails dev
```

### Documentation

- [CONTRIBUTING.md](CONTRIBUTING.md) — Development workflow, testing, pull requests
- [AGENTS.md](AGENTS.md) — Repository instructions for coding agents

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- LICENSE -->

## License

Distributed under the GNU General Public License version 2. See
[LICENSE](LICENSE).

<p align="right">(<a href="#readme-top">back to top</a>)</p>

<!-- MARKDOWN LINKS & IMAGES -->
<!-- https://www.markdownguide.org/basic-syntax/#reference-style-links -->

[tag-url]: https://github.com/markmybytes/install-it/releases
[tag-shield]: https://img.shields.io/github/v/tag/markmybytes/install-it?style=for-the-badge&label=LATEST&color=%23B1B1B1
[contributors-shield]: https://img.shields.io/github/contributors/markmybytes/install-it.svg?style=for-the-badge
[contributors-url]: https://github.com/markmybytes/install-it/graphs/contributors
[forks-shield]: https://img.shields.io/github/forks/markmybytes/install-it.svg?style=for-the-badge
[forks-url]: https://github.com/markmybytes/install-it/network/members
[stars-shield]: https://img.shields.io/github/stars/markmybytes/install-it.svg?style=for-the-badge
[stars-url]: https://github.com/markmybytes/install-it/stargazers
[issues-shield]: https://img.shields.io/github/issues/markmybytes/install-it.svg?style=for-the-badge
[issues-url]: https://github.com/markmybytes/install-it/issues
[license-shield]: https://img.shields.io/github/license/markmybytes/install-it.svg?style=for-the-badge
[license-url]: https://github.com/markmybytes/install-it/blob/main/LICENSE