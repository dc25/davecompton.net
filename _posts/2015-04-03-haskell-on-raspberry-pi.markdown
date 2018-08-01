---
layout: post
title:  "Building git on a raspberry pi 2"
date:   2015-04-03 21:59:18
comments:   True
categories: jekyll update
---
I've been trying out a Raspberry Pi 2 computer for the past week or so.  Yesterday I ran into some minor problems because the version of git that apt-get gave me (1.7.10.4) did not recognize some of the configuration options in my git repositories.  To overcome these problems, I built a later version of git (1.9.5) .  This turned out to be a fairly easy thing to do.  The steps consist basically of :

* Get The Source
* Get the prerequisites
* Build git
* Install git

In more detail:

* Get The Source
    *   git clone https://github.com/git/git.git
    *   cd git
    *   git tag --list    _(to show what tags are available)_
    *   git checkout tags/v1.9.5

* Get the prerequisites
    *   sudo apt-get install zlib1g-dev
    *   sudo apt-get install libssl-dev
    *   sudo apt-get install libcurl4-openssl-dev
    *   sudo apt-get install libexpat1-dev
    *   sudo apt-get install libintl-xs-perl
    *   sudo apt-get install gettext

* Build git
    *   make

* Install git
    *   make install


The prerequisites listed here match the requirements listed in the INSTALL file in the git repository.  However, it is not always clear from the INSTALL file _exactly_ which apt-get package needs to be installed to meet the requirement.
