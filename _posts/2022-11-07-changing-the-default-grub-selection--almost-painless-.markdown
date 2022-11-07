---
layout: post
title: "Changing the default grub selection (almost painless)"
date: 2022-11-07 15:05:06 +0000
github_comments_issueid: "21"
published: true
tags:
---

A couple months ago I added Linux Mint as a boot option on my work computer.   This morning after booting into Mint for the first time in months, updating with apt, and rebooting I found that my grub boot menu had been reordered with Mint as default.  Because I rarely boot into Mint, I wanted to change the default boot selection.

Later on, working in Ubuntu now, I searched the web and found [these instructions](https://askubuntu.com/a/110738) that say to 
* change /etc/default/grub to specify a default 
* run update-grub.   

I did this but nothing changed.

The reason that nothing changed is that Mint had taken ownership of the boot process.   After booting into Mint and making the same changes, the default boot option was updated as expected.

Looking back, this should have been obvious but it cost me a few minutes.

At some point, I expect that an Ubuntu update will again rewrite the grub configuration and I'll have to go through the process again on the Ubuntu side.
