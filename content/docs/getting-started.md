---
title: "Getting Started"
description: "How to install and flash RK-CFW on your Rockchip device."
body_class: page-docs
---

This guide walks you through downloading, verifying, and flashing RK-CFW onto a supported Rockchip device. The process takes about 15 minutes on a board you have already tested with the stock firmware.

## Prerequisites

- A supported Rockchip device (see [compatibility table](/docs/))
- A USB-A to USB-C (or Micro-USB) data cable — charge-only cables will not work
- A Linux or macOS host with `rkdeveloptool` installed
- At least 4 GB free disk space for image extraction
- **A complete backup of your current eMMC/SD card**

## 1. Clone the repository

```bash
git clone https://github.com/rk-unofficial-cfw/firmware.git
cd firmware
git checkout stable/rk3228
```

## 2. Install rkdeveloptool

```bash
# Debian / Ubuntu
sudo apt install rkdeveloptool

# Arch Linux
yay -S rkdeveloptool

# Build from source (any distro)
git clone https://github.com/rockchip-linux/rkdeveloptool
cd rkdeveloptool
autoreconf -i
./configure
make && sudo make install
```

## 3. Enter Maskrom mode

Power off the board. Hold the **Maskrom button** (or short the Maskrom pads on boards without a button), then connect the USB cable to your host. The board will enumerate as a Rockchip USB device without booting.

```bash
sudo rkdeveloptool ld
# Expected output:
# DevNo=1 Vid=0x2207,Pid=0x330c,LocationID=1 Maskrom
```

## 4. Flash the firmware

```bash
# Write the bootloader first
sudo rkdeveloptool db bootloader/rk322x_loader.bin

# Write the full system image
sudo rkdeveloptool wl 0 images/rk322x-rkcfw-stable.img

# Reset the device
sudo rkdeveloptool rd
```

## 5. First boot

1. Remove the USB cable after the reset command completes.
2. Wait 30–60 seconds for the first-boot filesystem expansion.
3. Connect via serial console (115200 8N1) or HDMI to observe boot output.
4. Default SSH credentials: `root / rkcfw` — change immediately.

## Troubleshooting

| Symptom | Cause | Fix |
|---------|-------|-----|
| Device not detected in `rkdeveloptool ld` | Charge-only cable or USB hub | Use a direct data-capable cable; try a different port |
| Board loops in bootloader | Wrong loader binary for SoC revision | Check the SoC stepping and use the matching `_v*` loader |
| Kernel panic on first boot | Image written to wrong offset | Re-flash using `wl 0` (not `wl 0x40`) |
| No HDMI output after boot | Missing device tree overlay | Add `OVERLAY=rk-hdmi` to `/boot/uEnv.txt` and reboot |

> **Warning:** Flashing firmware carries a risk of bricking your device. This project is experimental. Always ensure you have a recovery method (eMMC writer, JTAG, or a known-good SD card) before proceeding.
