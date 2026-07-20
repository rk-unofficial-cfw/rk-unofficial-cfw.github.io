---
title: "Welcome to RK-CFW — Project Launch"
short_title: "Project Launch"
description: "We're kicking off the RK-CFW project with initial support for RK3399 and RK3566 boards. Read about our goals, roadmap, and how to contribute."
lead: "We're launching the RK-CFW project today with initial stable support for RK3399 and RK3566 boards, and active work underway for RK3588. Here's what the project is, why we built it, and how you can get involved."
date: 2024-01-15
tags: ["Announcement", "Release"]
body_class: page-blog
---

## What is RK-CFW?

RK-CFW is a community-maintained custom firmware project for single-board computers and embedded devices powered by Rockchip SoCs. Our goal is to provide a mainline Linux-based firmware stack that replaces the vendor BSP forks typically shipped with these boards.

Rockchip produces excellent silicon, but the vendor software stack lags behind mainline Linux by several kernel generations and lacks long-term security support. We want to change that.

## Supported Hardware

At launch, stable images are available for the following SoC families:

- **RK3399** — Rock Pi 4, NanoPC-T4, Firefly-RK3399, ROC-RK3399-PC
- **RK3566** — Quartz64, Rock 3A, PineNote
- **RK3328** — Rock64, ROC-RK3328-CC
- **RK3288** — Tinkerboard, Chromebook (via depthcharge)

Work is ongoing for RK3588 (flagship) and RK3326 (gaming handhelds). RK3228 support is planned for a future release.

## What's Included

Each firmware image ships with:

- Mainline U-Boot with distro boot support
- Mainline Linux kernel (current LTS branch) with Rockchip device tree
- Minimal Debian-based root filesystem
- Hardware-accelerated video decode via `v4l2-request` and `hantro` / `rkvdec` drivers
- Panfrost open-source GPU driver (replaces binary blob)
- Thermal management and CPU frequency scaling

## Getting Involved

RK-CFW is a volunteer project and we welcome contributions of all kinds: code, documentation, testing, and hardware donations.

- **Testing:** flash an image on your board and report results in the GitHub issues tracker
- **Documentation:** add device-specific notes to the [wiki](/docs/) via pull request
- **Development:** pick up an open issue tagged `help-wanted` on GitHub

## Roadmap

Our short-term goals for the next quarter:

1. RK3588 stable image with NPU driver integration
2. RK3326 gaming handheld profile (audio, gamepad, suspend)
3. Automated CI image builds on every commit
4. OTA update mechanism via `swupdate`

> This firmware is unofficial and not supported by Rockchip Semiconductor or any board manufacturer. Flash at your own risk, and always keep a backup image and recovery method available before proceeding.
