---
layout: post
title: "Switching from docker to podman"
date: 2020-11-28 20:34:43 +0000
published: true
tags:
---

I've been doing most of my coding inside docker based environments.  A few weeks ago I found out about podman, a docker workalike.   Podman has a couple advantages over docker : 
* No need to run as root. 
* Does not require the support of a daemon.   

There is at least one disadvantage too : 
* Not available via apt on Ubuntu 20.04 ( which I am using ).   

What follows are a few notes regarding getting podman working on Ubuntu 20.04.   These are mostly for me but there's nothing confidential here so I'm publishing it here for anyone who might run into the same (minor) issues that I ran into.

# BUILDING
I built podman from scratch using the instructions [here](https://podman.io/getting-started/installation.html) and ran into the following (minor) problems.
* btrfs-tools was not available on Ubuntu 20.04 - used btrfs-progs instead.
* libprotobuf-c0-dev was not available on Ubuntu 20.04 - used libprotobuf-c-dev instead.
* I had GOROOT already set in my environment which did not work well with setting GOPATH as suggested in instructions.  Had to unset GOROOT.

# RUNNING

* Installed slirp4netns fuse-overlayfs as needed at runtime.
* now need to explicitly EXPOSE appropriate ports for access from host.  In docker was relying on docker daemon to handle routing to docker local network.
* --userns=keep-id  was needed to get permissions right on volumes mounted from hosts.   Without this the mounted volume was only readable by root.
* --privileged  was needed for rust debugging to work in vscode.  Without this got error: "'A' packet returned an error: 8" when running a rust program from vscode.

# BASH SCRIPT
Below is a script that builds and installs podman.  It exists in [this repository](https://github.com/dc25/buildPodman) .  For the most part it just implements the steps found on the [website linked to earlier](https://podman.io/getting-started/installation.html).

{% include iframecode.html 
              source-url= "https://github.com/dc25/buildPodman/blob/main/build.sh"
              raw-url=    "https://raw.githubusercontent.com/dc25/buildPodman/main/build.sh" 
              height=     "550px" %}

