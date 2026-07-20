---
title: "Flashing SD Card"
description: "How to write RK-CFW firmware images to an SD card on Linux, macOS, and Windows."
body_class: page-docs
---

Booting from an SD card is the safest way to try RK-CFW. On most Rockchip boards a valid SPL/U-Boot on SD takes priority over eMMC at power-on, so you can test the firmware without touching your existing eMMC installation. If something goes wrong, remove the card and the board returns to its original state.

## Prerequisites

- An SD card of at least 8 GB — Class 10 / UHS-I or faster recommended
- An SD card reader (USB adapter is fine; avoid cheap no-name readers)
- The firmware image (`.img.xz` or `.img`) from the [releases page](https://github.com/rk-unofficial-cfw/releases)
- One of the following write tools:
  - `dd` + `xzcat` — Linux and macOS
  - `bmaptool` — Linux only; faster than `dd` for sparse images
  - [balenaEtcher](https://etcher.balena.io) — GUI, Windows / macOS / Linux
- **A complete backup of any data on the SD card** — it will be overwritten

## 1. Download and verify the image

```bash
# Linux / macOS
sha256sum -c rkcfw-rk322x-stable.img.xz.sha256

# Windows (PowerShell)
Get-FileHash .\rkcfw-rk322x-stable.img.xz -Algorithm SHA256
```

## 2. Identify your SD card device

Insert the SD card and find its device path. **Choosing the wrong device will permanently overwrite that disk.**

```bash
# Linux
lsblk

# macOS
diskutil list
```

Unmount any auto-mounted partitions before writing:

```bash
# Linux (replace sdX with your device, e.g. sdb)
sudo umount /dev/sdX*

# macOS (replace N with your disk number)
diskutil unmountDisk /dev/diskN
```

## 3. Write to SD card

### Linux — dd

```bash
xzcat rkcfw-rk322x-stable.img.xz \
  | sudo dd of=/dev/sdX bs=4M status=progress
sudo sync
```

### Linux — bmaptool

```bash
sudo apt install bmap-tools
sudo bmaptool copy rkcfw-rk322x-stable.img.xz /dev/sdX
```

### macOS — dd

```bash
xzcat rkcfw-rk322x-stable.img.xz \
  | sudo dd of=/dev/rdiskN bs=4m
sudo sync
```

### Windows — balenaEtcher

1. Download and install [balenaEtcher](https://etcher.balena.io).
2. Click **Flash from file** and select the `.img.xz` image (Etcher decompresses automatically).
3. Click **Select target** and choose your SD card. Etcher hides system drives for safety.
4. Click **Flash!** and wait for the validation pass to complete.

## 4. Boot from SD card

| Board type | How to trigger SD boot |
|------------|------------------------|
| Most SBCs | Insert SD card before applying power. Board boots SD automatically if a valid SPL is found. |
| Boards with a physical Maskrom / boot button | Hold the button while connecting power, then release. |
| TV boxes without exposed button or pads | Erase the eMMC bootloader first via `rkdeveloptool` (see [eMMC flashing guide](/docs/getting-started.html)), then SD boot becomes the only option. |
| Boards with Maskrom pads (no button) | Momentarily short the Maskrom pads while applying power, then release. |

## 5. First boot

1. Wait 30–60 seconds for the first-boot filesystem expansion to complete.
2. Connect via serial console (115200 8N1) or HDMI to observe boot output.
3. Default SSH credentials: `root / rkcfw` — change immediately after login.
4. Verify you are running from SD: `lsblk` should show root on `mmcblk0`, not `mmcblk1` (eMMC).

## Troubleshooting

| Symptom | Cause | Fix |
|---------|-------|-----|
| Board ignores SD card and boots eMMC | SD card has no valid SPL or write was incomplete | Re-flash with `dd bs=4M` and confirm `sync` completes; try `bmaptool` which verifies blocks |
| Board hangs at U-Boot prompt | Image built for a different board variant | Check exact board model and revision; download the matching image |
| Write takes over 30 minutes | Slow card reader or SD card below Class 10 | Use a USB 3 card reader and a UHS-I card; switch to `bmaptool` |
| Filesystem errors or kernel panic on first boot | Write buffer not flushed before card removal | Always wait for `sync` to return before removing the card; re-flash |

> **Warning:** Writing to the wrong device will permanently overwrite that disk. Always double-check the target path with `lsblk` or `diskutil list` before running `dd` or `bmaptool`.
