---
layout: post
title: "Using Visual Studio Code Inside Docker To Edit Haskell"
date: 2020-04-10 02:39:18 +0000
published: true
github_comments_issueid: 11
tags:
---
# Intro

This blog post describes a way to use Vscode from inside of Docker to edit and debug Haskell.   The technique described will (probably) only work on a linux system because it depends on X tunneling to display the vscode window.   

# Prerequisites
* A linux desktop computer with docker installed.   

I installed docker-ce per these instructions: [https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-using-the-repository](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-using-the-repository)

# Overview
Build a haskell development Docker image, "haskell" .

The haskell image:
* is based on Ubuntu 19.10
* runs sshd to provide for logging in from the host.
* contains a user with the same name/id as you have on your host.
* has tmux installed
* has stack installed
* has vscode installed
* includes utilities and vscode extensions for Haskell development.

Start a container from the "haskell" image and then ssh into it from the host machine.  Because the user id in the container matches the host user id, shells running on the container have the same access to mounted files as on the host.  Tmux allows creation of multiple windows from inside the ssh login.  The ssh -Y option lets X windows (such as vscode) display on the host machine.

This (probably) will only work on Linux hosts.  I am using Ubuntu 16.04 with the docker community edition (installation instructions here). : [https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-using-the-repository](https://docs.docker.com/install/linux/docker-ce/ubuntu/#install-using-the-repository)

# Published Code

All of the code in this post is published on github:
* [https://github.com/dc25/haskelldev](https://github.com/dc25/haskelldev)

# Useful Bash Shell Functions
The following bash shell functions are used in this post:
* dob : wrapper around docker build
* dor : wrapper around docker run
* dosh : wrapper around ssh

All of these aliases are available in the file aliases.docker :

{% include iframecode.html 
              source-url= "https://github.com/dc25/haskelldev/blob/master/aliases.docker"
              raw-url=    "https://raw.githubusercontent.com/dc25/haskelldev/master/aliases.docker"
              height=     "560px" %}

# Building the "haskell" Image

Use the dob alias to build the "haskell" image, as follows:

{% highlight bash %}
$ git clone https://github.com/dc25/haskelldev.git
$ cd haskelldev 
$ . aliases.docker
$ dob haskell 
{% endhighlight %} 

On my computer, this took about an hour and used about 14 gigabytes of disk space.  When this is done you should have a docker image named "haskell".  

The Dockerfile in the haskelldev repository expects your userid, username, and .ssh public key as build command line arguments. The dob alias provides those arguments.

# Running the "haskell" image.

The dor bash function starts a container based on the "haskell" image as follows:

{% highlight bash %}
$ dor haskell
{% endhighlight %} 

In the container, the 'dor' function invokes a script called start.sh . That script starts a ssh daemon that enables ssh access to the running container.   

If you want to access a directory on the host machine from inside the container, a mount can be specified a run time ( when running the function, 'dor' ).  For example:

{% highlight bash %}
$ mkdir -p $HOME/workarea
$ dor haskell -v $HOME/workarea:/haskell_workarea
{% endhighlight %} 

Directories mounted in this way will have the same accessibility in the container as on the host machine.

# Logging in to the container
Use the dosh bash function to log into the running container.

{% highlight bash %}
$ dosh
{% endhighlight %} 

If all went well, your terminal should contain a shell prompt running in a tmux environment in the container.  Inside this environment, you can use tmux to create new windows as well as other useful operations.  See tmux documentation for more info.

# Haskell Development

To develop in haskell I use the following tools:

* stack
* vscode
* hie ( Haskell Ide Engine )
* ghcide (seems to be expected by other vscode plugins)
* Haskell GHCi Debug Adapter Phoityne (for interactive debugging)
* Haskell Language Server Client (vscode plugin to work with hie)
* Haskell Syntax Highlighting 

If anyone reading this know of any other tools that would complement this set, please let me know.

From inside the ssh session, use stack, vscode.

{% highlight bash %}
$ cd /workarea
$
$ # Resolver 15.2 is latest that works with entire tool set.
$ stack new sampleProject --resolver=lts-15.2  
$
$ cd sampleProject
$ 
$ # Do stack build for some setup expected by the vscode environment.
$ stack build 
$ stack build --test
$
$ code .
{% endhighlight %} 

# Notes

Interactive debugging works after creating a launch.json file for your project.   You can do this inside VScode by pressing Ctrl-Shift-d, and then following menus/prompts .   Select "haskell-debug-adapter" in the pull down menu that appears.  Initially, the test suite will be selected for debugging - to change this, replace "test/Spec.hs" in the "launch.json" file with "app/Main.hs" .

Even after going through the above process, interactive debugging did not work very well.  For example, stepping into code after hitting a breakpoint did not always work.  
