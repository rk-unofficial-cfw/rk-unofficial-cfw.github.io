---
title: "Documentation"
description: "RK-CFW documentation: SoC reference, getting started guides, and hardware compatibility tables."
body_class: page-docs
---

Welcome to the RK-CFW documentation. This wiki covers firmware installation, SoC-specific hardware details, and community contribution guidelines. All pages are open for community editing via pull requests on GitHub.

## Supported SoCs

The table below summarises support status for each Rockchip SoC.

| SoC | CPU | GPU | Max RAM | Status | LibreELEC | Armbian |
|-----|-----|-----|---------|--------|-----------|---------|
| `RK3228 / RK3229` | 4× A7 1.2 GHz | Mali-400 MP2 | 2 GB DDR3 | <span class="status-active">Supported</span> | <span class="status-active">✓ Stable</span> | — |

## Quick Links

- [Installation & flashing guide](/docs/getting-started.html) — start here if you're new
- [Flashing SD Card](/docs/flashing-sdcard.html) — boot from SD without touching eMMC
- Bootloader documentation — U-Boot and Rockchip MiniLoader
- Hardware bring-up guide — porting to a new board

## Contributing to the Docs

Documentation lives in the same GitHub repository as the firmware. To fix a typo or add a new page, fork the repo, edit the relevant file under `content/docs/`, and open a pull request.

> This project is not affiliated with Rockchip Semiconductor Co., Ltd. Use firmware images at your own risk. Always back up your device before flashing.
