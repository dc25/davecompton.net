---
layout: post
title: "Note to self: VM on windows11"
date: 2025-09-02 23:09:49 +0000
published: true
tags:
---


* After upgrading to windows 11, virtualbox no longer worked.
* Spent a little time trying to fix but gave up eventually.
* [Others](https://askubuntu.com/questions/1415045/ubuntu-22-04-seems-to-freeze-in-virtualbox-6-1-on-windows-11) seem to be having similar problems.
* Tried disabling hyper-v per: [disable hyper-v](https://www.xda-developers.com/disable-hyper-v-windows-11/) by running "Disable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-Hypervisor" in powershell.
* Also accessed hyper-v setting through control panel.
* This appeared to have disabled hyper-v (as seen in control panel) but didn't seem to help with virtualbox.
* Gave up on virtualbox and decided to try vmware.
* Had to create support.broadcom.com login.
* Downloaded vmware from broadcom. VMWare Workstation Pro for windows under free downloads.
* Spent some time trying to get bridged networking to work but eventually gave up and decided to fall back on NAT (which actually should be fine).
* per:    [This askubuntu answer](https://askubuntu.com/a/1399909/1176839)  was able to mount from windows host using : "sudo mount -t fuse.vmhgfs-fuse .host:/ /mnt/hgfs -o allow_other"
* also per:    [This askubuntu answer](https://askubuntu.com/a/1399909/1176839)  use ".host:/   /mnt/hgfs   fuse.vmhgfs-fuse    auto,allow_other,uid=1000,gid=1000    0   0" in /etc/fstab to get mount to persist.
* Files created with this mount are owned by root.   Still needs work.
* Left hyper-v off for now.   Is this a problem?
* Installed open-vm-tools as follows: "sudo apt install open-vm-tools open-vm-tools-desktop" per : https://knowledge.broadcom.com/external/article?legacyId=2073803


