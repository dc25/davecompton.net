---
layout: post
title: "building and using Haste 0.5.0 in Docker"
date: 2015-08-27 15:48:50 +0000
published: true
tags:
---
# Update 2015-11-05
This post is obsolete.  See [this post]({{ site.baseurl}}/docker/haskell/ghcjs/haste/2015/11/05/building-and-using-ghcjs-and-haste-0.5.2-in-docker.html) for an up-to-date version.

# Introduction:
Haste is a Haskell to Javascript compiler that I have used it for several projects.  When I read that [a new Haste version (0.5.0) has been released](https://groups.google.com/forum/#!topic/haste-compiler/Hpq3VosXpYk), I wanted to try it out.  Because I am now using Docker for all of my Haskell development, the natural thing to do was to incorporate Haste into my existing Docker environment.  

[My previous post](http://dc25.github.io/myBlog/haskell/ghcjs/docker/2015/08/05/installing-and-using-ghcjs-in-a-docker-image.html) described how to create and use that environment so this post is an update of that one.

# Summary
The previous post describes in detail how to build and run a Docker based Haskell working environment.  Those steps still will work but now the result will include Haste 0.5.0 in addition to everything that was there before.

Because Haste 0.5.0 does not build under ghc 7.10.2, ghc 7.8.4 is built before Haste so that it can be used to build Haste.   This build remains in the Docker image but by default is unused after ghc 7.10.2 is built .  Once built, Haste works just fine with ghc 7.10.2 .  If you would prefer to use ghc 7.8.4 instead of 7.10.2, that is easy to specify either at container run time or at Docker build time.

The resulting Docker image will contain the following:

* Compilers
  * ghc 7.8.4  (built from source using haskell-platform, disabled by default)
  * ghc 7.10.2 (built from source using ghc 7.8.4)
  * haste 0.5.0 (built from source using ghc 7.8.4)
  * ghcjs ( the most recent as of time of image creation ; built from source using ghc 7.10.2 )  
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
To build this image, you will need to first [install docker](https://docs.docker.com/installation/) version 1.7 or later.  I have been using Docker version 1.8 .

After installing Docker, clone the git repository holding the Dockerfile and related build scripts.

    git clone https://github.com/dc25/dockerfile_for_haskell.git

If you don't already have a public/private ssh key pair, generate one now as [described here](https://help.github.com/articles/generating-ssh-keys/)

Edit the file called "personalize" in the cloned git repository.  Replace the existing public key with your own.  Also in this file, add any shell aliases and custom vim setup that you want to use in the Docker image.

From inside the repository, run Docker to build your image:    

    sudo docker build -t ghc .

On my computer this takes about 3 hours and creates a Docker image called ghc.  On a considerably older, slower machine it took about 6 hours.
    
# Running:

Enter the following bash command to run the newly created image with access to a limited collection of files on the host machine:  

    sudo docker run -v <host directory>:<container directory> ghc

For example, I organize my projects in a common directory and mount that entire folder in the Docker container.  This makes individual projects accessible in the Docker container with the same directory name that they have on the host machine:  

    sudo docker run -v /home/dave/repos:/repos ghc

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

