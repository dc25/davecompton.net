---
layout: post
title: "A Gnome extension to show IP address on desktop"
date: 2022-05-31 11:07:27 -0700
published: true
github_comments_issueid: "18"
tags:
---

**NOTE:  As of [GNOME Shell 45](https://gjs.guide/extensions/upgrading/gnome-shell-45.html#esm) , this no longer works.  I spent a few hours trying to update it but decided it was not worth the effort.   My guess is that it *could* be fixed but it's not that important to me and I don't think anyone else is using it.   If someone reading this figures out how to get it working again, please let me know what it takes.**


Here's a way to configure Gnome to display some useful information on the Ubuntu Gnome desktop.  I use a variety of virtual machines and it's nice to be able to easily tell which machine I'm dealing with at any one time as well as having the ip address for ssh access to that machine.  I use this extension to display the machine name and ip addresses.   I also show some other information but that's easy to configure so if you use this extension, you can decide.

The code resides [on github](https://github.com/dc25/cmd_wallpaper) .   It consists of less than 50 lines of javascript and a few lines of json.  Most of the code came from the answer to [this stackoverflow question](https://stackoverflow.com/questions/29816027/a-simple-way-to-show-my-hostname-ip-in-gnome-panel-or-on-my-desktop-background) .   I made a couple minor modifications.   

Usage instructions are in the README for the project. They amount to copying the project to a path in your local machine and specifying a program for the extension to run.  I use the following shell script:

{% include iframecode.html 
              source-url= "https://github.com/dc25/cmd_wallpaper/blob/main/desktop_notes.sh"
              raw-url=    "https://raw.githubusercontent.com/dc25/cmd_wallpaper/main/desktop_notes.sh"
              height=     "400px" %}

This results in a desktop that looks like this:

![Desktop Image]({{ site.baseurl }}/assets/annotated.jpg)
