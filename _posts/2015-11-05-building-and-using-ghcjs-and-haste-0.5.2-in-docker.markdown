---
layout: post
title: "Building and using ghc, ghcjs and Haste in Docker"
date: 2015-11-05 13:37:20 +0000
published: True
categories: docker haskell ghcjs haste
comments: True
---

* **Dec 5, 2015 :  Now building with Haste 0.5.3 ( previously was 0.5.2 )**
* **Dec 12, 2015 :  Now building with ghc 7.10.3 ( previously was 7.10.2 )**
* **Dec 26, 2015 :  No longer using sshd for shell access.**
* **March 14, 2016 :  Allowing for optional sshd at runtime.**

---

[This earlier post described how to create and use a docker environment for ghcjs development.]({{ site.baseurl}}/haskell/ghcjs/docker/2015/08/05/installing-and-using-ghcjs-in-a-docker-image.html)

[This earlier post extends the original ghcjs post to include Haste.]({{ site.baseurl}}/2015/08/27/building-and-using-haste-0.5.0-in-docker.html)

This post supersedes both of those earlier posts.

# Summary:
This post explains how to quickly build a Haskell development development environment (including ghcjs and Haste) that runs under Docker.  To build and use this development environment you should be running a 64 bit Linux version (Windows or OSX might also work using boot2docker but I have not tried either of those).

# Introduction:
Earlier this year I started working with ghcjs, a Haskell to Javascript compiler.  At the time I was using ghc 7.6.3, which is too old to be supported by ghcjs.  Not wanting to risk messing up my working Haskell environment, I decided to try using Docker to create a isolated ghcjs workspace.  When a new version of Haste was released, I added it to the configuration.

The eventual result was a Dockerfile (a type of script that can be used to create a Docker image ) that creates a Docker based Haskell working environment.

This has become my default Haskell workspace - not just for ghcjs but for all my Haskell work.

The resulting image is based on Ubuntu 15.10 (aka "Wily Werewolf") and contains :   

* Compilers
  * **ghc 7.10.3** (built from source using ghc 7.8.4)
  * **haste 0.5.3** (built from source using ghc 7.10.3)
  * **ghcjs** ( the most recent as of time of image creation ; built from source using ghc 7.10.3 )  
* Haskell development tools
  * **cabal 1.22.6**  (built from source)
  * **ghc-mod** & **ghc-modi** (via cabal)
  * **hdevtools** (via cabal)
  * **ghcid** (via cabal)
  * **hlint** (via cabal)
  * **stack** (from fpcomplete via apt-get)
* Editors
  * **vim** (via apt-get; with Haskell related extensions)
    * **pathogen** (cloned from github)
    * **syntastic** (cloned from github)
    * **vim-hdevtools** (cloned from github)
    * **vim2hs** (cloned from github)
    * **vimproc** (cloned from github)
    * **ghcmod-vim** (cloned from github)
* Miscelaneous
  * **tmux** (via apt-get)

# Building:
To build this image, you will need to first [install docker](https://docs.docker.com/installation/) version 1.7 or later.

After installing Docker, clone the git repository holding the Dockerfile and related build scripts.

    git clone https://github.com/dc25/dockerfile_for_haskell.git

From inside the repository, run Docker to build your image:    

    sudo docker build -t <imageName> .

On my computer this takes about three hours.

# Running:

Run the new image as follows:

    {% raw %}
    sudo docker  run -it --entrypoint /start.sh <imageName> `id -u -n` `id -u`
    {% endraw %}

This will start a container and create a user inside that container with same name and id as you have on your host machine.  This allows you to work on files and directories mounted from the host machine without permission problems.  

If you want to mount a directory from the host machine, use the -v docker option:

    {% raw %}
    sudo docker  run -it -v /hostpath:/containerpath --entrypoint /start.sh <imageName> `id -u -n` `id -u`
    {% endraw %}

If you want to run a sshd daemon in the docker environment, add your ssh public key (the contents of ~/.ssh/id_dsa.pub or ~/.ssh/id_rsa.pub) as an additional argument to the command line:

    {% raw %}
    sudo docker  run -it -v /hostpath:/containerpath --entrypoint /start.sh <imageName> `id -u -n` `id -u` "`cat ~/.ssh/id_dsa.pub`"
    {% endraw %}

If you start the docker environment in this way, you can access it using a passwordless ssh login from the host machine.  The advantage of doing this is that tmux seems to behave better in an ssh shell than in a bash shell.  I don't know why.

I use the following two bash functions to start my docker/haskell environment.  Each takes one argument: the name of the docker image to be run.  

    function doba
    {
        sudo docker run -it -v /hostpath:/containerpath --entrypoint /start.sh $1 `id -u -n` `id -u`
    }

    function dosh
    {
        sudo docker run -it -v /hostpath:/containerpath --entrypoint /start.sh $1 `id -u -n` `id -u` "`cat ~/.ssh/id_dsa.pub`"
    }

You will need the ip addresses of the running docker container to ssh into it.  The following bash function will display that address.

    {% raw %}
    function doip
    {
        sudo docker ps -q | xargs sudo docker inspect --format '{{ .NetworkSettings.IPAddress }}'
    }
    {% endraw %}

The directory **build_scripts** contains the following files intended for customization

* **personalize.sh**: Invoked once when the container starts.
* **myBashrc**: Invoked from inside every bash shell that runs inside the container.
* **myVimrc**: This will be sourced by vim when it starts.

# Using tmux

   Inside the docker container, you will be running tmux, a terminal based window manager.  At first you can just disregard tmux but at some point you will want to work with multiple shells.  This is where tmux comes in handy.  

To manage multiple bash shells, I generally make the terminal that I'm using full screen and then use tmux commands to split the window into multiple panes and navigate from one pane to another.  The following tmux commands should be enough to get started:

| Keystrokes ---> | Effect |
|:-------------|:-------|
| ^b %         | split screen vertically |
| ^b "         | split screen horizontally |
| ^b o         | move to next pane |

I've found [this tmux cheatsheet](http://www.mechanicalkeys.com/files/os/notes/tm.html) to be useful.

# Versions

From within a container you should be able to verify the following versions:

    dave@99c4c23f4476:~$ cabal --version
    cabal-install version 1.22.6.0
    using version 1.22.4.0 of the Cabal library 
    dave@99c4c23f4476:~$ ghc --version
    The Glorious Glasgow Haskell Compilation System, version 7.10.3
    dave@99c4c23f4476:~$ ghcjs --version
    The Glorious Glasgow Haskell Compilation System for JavaScript, version 0.2.0 (GHC 7.10.3)
    dave@99c4c23f4476:~$ hastec --version
    0.5.3


# Feedback

If you made it this far, thank you for reading my post.  If you have any feedback please don't hesitate to let me know.

\- Dave
