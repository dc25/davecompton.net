---
layout: post
title: "Notes about tteck dashboards for proxmox"
date: 2024-03-02 09:03:56 -0800
published: true
github_comments_issueid: "25"
tags:
---

I'm trying out some of the "dashboards" for proxmox listed under the tteck scripts collection.   I'll update this page as I go.

---
# Mafl LXC

Installation went smoothly.   Access was in browser via port 3000. This results in a page that looks like this:

![image from github](https://github.com/dc25/davecompton.net_images/raw/main/mafl_home_page.jpg)


As far as I can tell, each of these icons is a link that by default is not enabled.   I looked briefly at the documentation to find out what it takes to get them working but got impatient and moved on.

---
# Heimdall Dashboard LXC

Again, install went smoothly.   Access through port 7990, Used the builtin gui to add a couple of "apps" resulting in this:


![image from github](https://github.com/dc25/davecompton.net_images/raw/main/heimdall_dashboard.jpg)

These are clickable and I configured them to link to the plex and pi-hole web interfaces.

I'm starting to get the impression that a "dashboard" is just a fancy collection of bookmarks.

---
# Homepage LXC

Again, install went without a hitch.   Access using browser at port# 3000.   Configure using scripts at /opt/homepage/config . I added plex and LMS to get a setup like this:

![image from github](https://github.com/dc25/davecompton.net_images/raw/main/homepage_dashboard.jpg)

---
# Thoughts

These seem pretty easy to install and configure but I don't think they buy you all that much.   I'll probably stick with plain old bookmarks.


