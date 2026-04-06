# smokeping-webadmin

<div align="center">

**Web-based administration interface for SmokePing network latency monitoring.**

Configure targets, alerts, probes, and visualizations — without editing config files by hand.

[![License](https://img.shields.io/badge/license-Apache%202.0-green.svg)](LICENSE)
[![Platform](https://img.shields.io/badge/platform-Linux-lightgrey.svg)]()
[![Packages](https://img.shields.io/badge/packages-RPM%20%2B%20DEB-blue.svg)]()

[Features](#features) · [Install](#install) · [Screenshots](#screenshots) · [Contributing](#contributing)

</div>

---

## What is smokeping-webadmin?

[SmokePing](https://oss.oetiker.ch/smokeping/) is the gold standard for network latency monitoring — but configuring it means editing Perl-syntax config files by hand. Adding a target, changing an alert threshold, or reorganizing your monitoring hierarchy means vi, syntax errors, and `systemctl restart`.

**smokeping-webadmin** gives SmokePing a proper web admin interface:

- **Add, edit, and remove monitoring targets** from a web form — no config file editing
- **Manage alert thresholds and notifications** visually
- **Organize targets into hierarchies** with drag-and-drop (curated targets)
- **Draft mode** — stage changes and review them before applying to the live config
- **Catalog view** — browse all targets with search and filtering
- **Basic Auth** — shared authentication realm with SmokePing itself
- **Safe** — generates valid SmokePing config, validates before applying

## Features

- **Web CGI admin interface** — runs alongside your existing SmokePing installation
- **Draft manager** — make changes in a staging area, review, then apply
- **Curated targets** — organize monitoring targets into meaningful groups
- **Catalog + search** — find any target across your entire SmokePing config
- **Definition editor** — edit probe definitions, alert rules, and global settings
- **Automatic backup** — config is backed up before every apply
- **CLI tool** — `smokeping-admin` command for scripted operations
- **RPM + DEB packages** — install in one command on Rocky/RHEL/Debian/Ubuntu

## Install

### From RPM (Rocky Linux, RHEL, Fedora)

```bash
# Download the bundle
# Then:
sudo ./install-bundle.sh
```

The installer:
1. Installs the `decllc-smokeping-webadmin` RPM
2. Resolves dependencies from your distro repos
3. Runs `setup-smokeping-admin-system.sh` which configures Apache with Basic Auth
4. Sets up the admin CGI alongside your existing SmokePing CGI

### From DEB (Debian, Ubuntu)

```bash
# Download the bundle
# Then:
sudo ./install-bundle.sh
```

Same workflow — the DEB bundle includes the same setup tooling.

### Requirements

- SmokePing (must already be installed and running — any standard installation works)
- Apache 2.4+ with mod_cgi enabled
- Perl 5.26+

smokeping-webadmin works with **any SmokePing installation** — upstream packages from your distro repos, manual builds, or existing deployments. No special base package required.

### After install

Access the admin at:

```
https://your-server/smokeping/admin.cgi
```

Credentials are shared with your SmokePing Basic Auth realm — same username and password.

## How it works

```
┌──────────────────────────────────────────────┐
│  Browser                                      │
│  https://server/smokeping/admin.cgi           │
└──────────────────┬───────────────────────────┘
                   │
          ┌────────▼─────────┐
          │  smokeping-admin  │
          │  CGI (Perl)       │
          ├──────────────────┤
          │  Draft Manager   │  ← stage changes before applying
          │  Catalog         │  ← browse + search targets
          │  Curated Targets │  ← organized groups
          │  Definitions     │  ← probes, alerts, globals
          └────────┬─────────┘
                   │ generates valid config
          ┌────────▼─────────┐
          │  SmokePing       │
          │  /etc/smokeping/ │
          │  config files    │
          └──────────────────┘
```

The admin CGI reads and writes SmokePing's config files through a Perl library (`DECLLC::SmokePing::Admin`) that understands SmokePing's config syntax. Changes go through the draft manager first — you review before applying.

## Perl Library

The admin is built on a modular Perl library:

| Module | Purpose |
|---|---|
| `DECLLC::SmokePing::Admin` | Core admin logic |
| `DECLLC::SmokePing::Admin::Catalog` | Target browsing + search |
| `DECLLC::SmokePing::Admin::CuratedTargets` | Organized target groups |
| `DECLLC::SmokePing::Admin::Definitions` | Probe/alert/global editing |
| `DECLLC::SmokePing::Admin::DraftManager` | Staging + review before apply |
| `DECLLC::SmokePing::Admin::Runtime` | CGI runtime + auth integration |

## CLI

```bash
# List all targets
smokeping-admin list

# Add a target
smokeping-admin add --host 8.8.8.8 --menu "Google DNS" --hierarchy /Network/DNS

# Apply pending drafts
smokeping-admin apply

# Show draft diff
smokeping-admin diff
```

## Also available as an OpenUTM plugin

smokeping-webadmin is available as a standalone tool AND as an integrated plugin for [OpenUTM](https://dec-llc.biz/products/openutm.html) — DEC-LLC's unified threat management platform. When running inside OpenUTM, latency monitoring data feeds into the UTM's network intelligence layer.

## Screenshots

*Coming soon — the web interface is functional; screenshots will be added to this README.*

## Contributing

Contributions welcome. Please open an issue first to discuss what you'd like to change.

## License

Apache License 2.0 — see [LICENSE](LICENSE) for details.

## Author

**DEC-LLC** (Diwan Enterprise Consulting LLC)
[dec-llc.biz](https://dec-llc.biz) · [info@decllc.biz](mailto:info@decllc.biz)
