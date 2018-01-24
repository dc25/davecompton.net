---
layout: post
title: "Haskell: ghc 7.10.2 and ghcjs in Docker"
date: 2015-08-05 21:56:33 +0000
comments:  True
published: True
categories: haskell ghcjs docker
---
# Update 2015-11-05
This post is obsolete.  See [this post]({{ site.baseurl}}/docker/haskell/ghcjs/haste/2015/11/05/building-and-using-ghcjs-and-haste-0.5.2-in-docker.html) for an up-to-date version.

# Summary:
This post explains how to quickly build a ghc 7.10.2 development development environment (including ghcjs) that runs under Docker.  To build and use this development environment you should be running a 64 bit linux version (Windows or OSX might also work using boot2docker but I have not tried either of those).

# Introduction:
Last month I started working with ghcjs, a Haskell to Javascript compiler.  At the time I was using ghc 7.6.3, which is too old to be supported by ghcjs.  Not wanting to risk messing up my working Haskell environment, I decided to try using Docker to create a isolated ghcjs workspace.

The eventual result was a Dockerfile (a type of script that can be used to create a Docker image ) that creates a Docker based Haskell working environment.

This has become my default Haskell workspace - not just for ghcjs but for all my Haskell work.

The resulting image is based on Ubuntu 15.04 and contains :   

* Languages
  * ghc 7.10.2  (built from source)
  * ghcjs ( built from source; the most recent as of time of image creation )  
  * typescript (via npm)
* Haskell development tools
  * cabal 1.22.6  (built from source)
  * ghc-mod & ghc-modi (built from source)
  * hdevtools (via cabal)
  * ghcid (via cabal)
  * hlint (via cabal)
  * stack (from fpcomplete via apt-get)
* Editors
  * vim (via apt-get; with Haskell related extensions)
    * pathogen (cloned from github)
    * syntastic (cloned from github)
    * vim-hdevtools (cloned from github)
    * vim2hs (cloned from github)
  * atom (from atom.io; with Haskell related extensions)
    * language-haskell  (via apm)
    * haskell-ghc-mod  (via apm)
    * ide-haskell  (via apm)
    * autocomplete-haskell`(via apm)
* Miscelaneous
  * sshd (via apt-get)
  * tmux (via apt-get)

# Building:
To build this image, you will need to first [install docker](https://docs.docker.com/installation/) version 1.7 or later.

After installing Docker, clone the git repository holding the Dockerfile and related build scripts.

    git clone https://github.com/dc25/dockerfile_for_haskell.git

If you don't already have a public/private ssh key pair, generate one now as [described here](https://help.github.com/articles/generating-ssh-keys/)

Edit the file called "personalize" in the cloned git repository.  Replace the existing public key with your own.  Also in this file, add any shell aliases and custom vim setup that you want to use in the Docker image.

From inside the repository, run Docker to build your image:    

    sudo docker build -t ghc_710 .

On my computer this takes about 2 hours and creates a Docker image called ghc_710.
    
# Running:

Enter the following bash command to run the newly created image with access to a limited collection of files on the host machine:  

    sudo docker run -v <host directory>:<container directory> ghc_710 

For example, I organize my projects in a common directory and mount that entire folder in the Docker container.  This makes individual projects accessible in the Docker container with the same directory name that they have on the host machine:  

    sudo docker run -v /home/dave/repos:/repos ghc_710 

You can use ssh to get shell access to the running Docker container.  Use the following command to get the ip address of the container:

    {% raw %}
    sudo docker ps -q | xargs sudo docker inspect --format '{{ .NetworkSettings.IPAddress }}'
    {% endraw %}

Once you have the ip address, ssh in to the container as shown below:

    ssh -l root <ipAddress> -X -t tmux


# Using tmux

   Once you log in, you will be running tmux, a terminal based window manager.  At first you can just disregard tmux but at some point you will want to work with multiple shells.  This is where tmux comes in handy.  

To manage multiple bash shells, I generally make the terminal that I'm using full screen and then use tmux commands to split the window into multiple panes and navigate from one pane to another.  The following tmux commands should be enough to get started:

| Keystrokes ---> | Effect |
|:-------------|:-------|
| ^b %         | split screen vertically |
| ^b "         | split screen horizontally |
| ^b o         | move to next pane |

I've found [this tmux cheatsheet](http://www.mechanicalkeys.com/files/os/notes/tm.html) to be pretty useful.


# Feedback

If you made it this far, thank you for reading my post.  If you have any feedback please don't hesitate to let me know.

\- Dave

