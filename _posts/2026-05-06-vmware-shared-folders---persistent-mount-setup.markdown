---
layout: post
title: "VMware Shared Folders — Persistent Mount Setup"
date: 2026-05-06 10:38:13 -0700
published: true
github_comments_issueid: "28"
tags:
---

Enables automatic mounting of VMware shared folders at `/mnt/hgfs` on an Ubuntu guest.

This post and the technique described here are thanks to claude.ai .

## Prerequisites

```bash
sudo apt install open-vm-tools
```

## Steps

**1. Create the unit file**

```bash
sudo tee /etc/systemd/system/mnt-hgfs.mount << 'EOF'
[Unit]
Description=VMware Shared Folders
DefaultDependencies=no
Requires=open-vm-tools.service
After=open-vm-tools.service

[Mount]
What=.host:/
Where=/mnt/hgfs
Type=fuse.vmhgfs-fuse
Options=defaults,allow_other,auto_unmount,entry_timeout=0,attr_timeout=0

[Install]
WantedBy=multi-user.target
EOF
```

**2. Enable and start it**

```bash
sudo systemctl daemon-reload
sudo systemctl enable --now mnt-hgfs.mount
```

**3. Verify**

```bash
systemctl status mnt-hgfs.mount
ls /mnt/hgfs/
```

## Notes

- `DefaultDependencies=no` is required. Without it, systemd creates an ordering cycle: `.mount` units are automatically added to `local-fs.target`, but `open-vm-tools` starts *after* `local-fs.target`, so the mount can never satisfy its own dependency. `DefaultDependencies=no` removes the unit from `local-fs.target` ordering, breaking the cycle.
- An `/etc/fstab` entry is not a viable alternative — fstab-generated units have `DefaultDependencies=yes` and there is no fstab option to override it.
- The mount point `/mnt/hgfs` does not need to be created manually; the fuse driver creates it on first mount.
- Shared folders appear as subdirectories under `/mnt/hgfs/` (e.g. `/mnt/hgfs/shared`).
