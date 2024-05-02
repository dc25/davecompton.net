---
layout: post
title:  "Reinstalling haskell/cabal/haste on ubuntu."
date:   2015-04-18 22:42:00
comments:   True
categories: jekyll update
---
Over the last couple of months I have been using the haste (haskell -> javascript ) compiler.  Because the haste-compiler does not work well with cabal sandboxes, I've been doing "cabal install" commands that go into the ~/.cabal directory that is shared by all projects.

Every now and then I suspect that my haskell configuration has fallen into some bad state that is causing me problems.  When this happens, I like to uninstall all haskell related software, delete all my personal haskell configuration, and start over. 

What follows is a summary of how I go about doing that uninstall/delete/reinstall process.  If you don't care about haste, disregard that part.  Everything up to there is haste-indepedent.

This is just "what works for me".  I don't claim that what follows is efficient, complete, or even necessary.  I do, however, find it to be an occasionally useful part of my own work flow.

### FIRST, START FROM SCRATCH
* Clean out existing installations of ghc, cabal, haste :

> rm -rf ~/.ghc ~/.cabal ~/.haste   
> sudo apt-get remove haskell-platform          
> sudo apt-get remove cabal-install             
> sudo apt-get remove ghc    
> sudo apt-get autoremove    

* (get rid of any other ghc/cabal installations you may have too)

> which ghc 

( should report nothing )

> which cabal 

( should report nothing )

* reboot to make a clean start

> sudo reboot

###  INSTALL HASKELL
* Install platform using apt-get

> sudo apt-get update  
> sudo apt-get install haskell-platform            

###  STEP UP TO A NEWER CABAL
> cabal update  

* Install a usable version of cabal.  I use 1.20.0.3 because it is the latest that works well with the haskell-related vim extensions that I use (ghc-mod, hdevtools, hlint).  [More details here]( http://stackoverflow.com/questions/27298268/why-does-cabal-init-break-ghc-mod-check  ).

> git clone git://github.com/haskell/cabal.git   
> cd cabal  
> git checkout tags/Cabal-v1.20.0.3  
> cabal install Cabal/ cabal-install/  

* Now make sure PATH environment variable includes ~/.cabal/bin  
* Start a new shell.
* Verify that the correct version of cabal is running.

> which cabal  

(should report $HOME/.cabal/bin/cabal )

> cabal --version

Should report as follows:  

>cabal --version  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;cabal-install version 1.20.0.4  
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;using version 1.20.0.3 of the Cabal library   

(I don't know why install version differs from library version)

At this point you should be able to use haskell (ghc) and cabal.  The cabal version you have installed includes the "cabal sandbox" feature.

[This introduction to cabal sandboxes](http://coldwa.st/e/blog/2013-08-20-Cabal-sandbox.html) is my original reference for this cabal-related information.

###  INSTALL HASTE
* get haste:

> git clone https://github.com/valderman/haste-compiler.git

* Install a stable version of haste.

> cd haste-compiler  
> git checkout tags/0.4.4.3  
> cabal install  
> haste-boot --local  

At this point you should be able to use haste (hastec) in addition to haskell and cabal.  Note that haste does not work well with cabal sandboxes.


[This page from the haste documentation](http://haste-lang.org/downloads/) is my original reference for this haste-related information.
 
###  VERIFY THAT HASTE WORKS
Haste comes with a test suite that you should now be able to run.  From within the haste-compiler directory execute "./runtest.sh" .  When I do this, I get 58 out of 59 tests passing and 1 failure.

