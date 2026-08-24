---
layout: post
title: "linux screen blank"
date: 2026-08-24 11:06:00 -0700
github_comments_issueid: "30"
published: true
tags:
---

# Blank Screen / Unresponsive Keyboard Investigation

## diagnosis and summary thanks to Claude

## Symptom
On this Dell 14 Plus 2-in-1 (Intel Lunar Lake, Arc 130V/140V graphics), the screen
occasionally goes completely black and the keyboard stops responding — except F5
(keyboard backlight toggle) still works. Happens after long periods of inactivity
with the laptop lid **open** (not closed), on battery power. Connecting/disconnecting
the charger while stuck made no difference.

## Diagnosis
Checked `journalctl`/`dmesg` across ~10 recent boots. No kernel panics, GPU
hangs/resets, or unclean shutdowns were logged — but a clear pattern emerged:

- **Aug 23, 19:17:06** — system entered suspend (`s2idle`) after the 15-minute
  battery idle timeout
- **~20:34–20:35** (≈78 min later) — kernel and networking resumed successfully
  (confirmed via Tailscale reconnect logs scrambling back online at that exact time)
- **Display and input never came back** — nothing further logged until a hard
  power-cycle forced a new boot

**Root cause:** the kernel resumes from suspend correctly, but the `xe` gra
driver / Wayland compositor fails to reinitialize the display pipeline afterward —
a known class of bug on this very new Lunar Lake + `xe` driver combination.
Backlight toggling  works because backlight toggling is handled by the EC/ACPI layer directly, not
through the wedged compositor. BIOS was already current (1.11.0), so this is a
driver-side issue, not a firmware gap.

## Fix Applied
Disabled auto-suspend on battery so the machine can't hit the broken resume.
It will still blank/lock the screen on idle, just won't suspend:

gsettings set org.gnome.settings-daemon.plugins.power sleep-inactive-battery-type 'nothing'

Verified current settings:

| Setting | Value |
|---|---|
| `sleep-inactive-battery-type` | `nothing` |
| `sleep-inactive-battery-timeout` | 900s |
| `sleep-inactive-ac-type` | `nothing` |
| `sleep-inactive-ac-timeout` | 3600s |

## Safe Recovery (if it happens again before/without this fix)
Instead of holding the power button (risks filesystem corruption from unflushed
writes), use the Magic SysRq key — handled directly by the kernel's keyboar
interrupt handler, bypassing the frozen compositor entirely.

This system's SysRq is enabled at level `176` (Ubuntu default), which permits only:

- **S** — sync disks (flush dirty pages)
- **U** — remount filesystems read-only
- **B** — reboot

(`R`/`E`/`I` — raw keyboard toggle and process terminate/kill — are disabled by
default on Debian/Ubuntu and won't do anything here.)

**To use:** hold **Alt+SysRq** (often shares the PrtScn key, sometimes need
tap **S**, release, tap **U**, release, tap **B** — with a brief pause between each.
