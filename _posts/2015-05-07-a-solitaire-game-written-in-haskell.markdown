---
layout: post
title: "A Solitaire Game Written in Haskell"
date: 2015-05-07 21:18:00 -0700
comments: True
categories: haskell haste
---
<iframe width="720" height="650" src="https://dc25.github.io/solitaire/" frameborder="0" allowfullscreen></iframe>

As part of my efforts to learn to program in Haskell, I created the solitaire program that you see above.  Playing the game is simple - just click and drag a card.  Refresh the screen to start over. It seems to run best on a Chrome browser on a Linux machine.  It will not work well on a touchscreen device ( but, I think, that could be fixed pretty easily).

The technologies involved are:

* Haskell - using the Haste compiler.
* Typescript 
* d3.js

The project is available on github: [github.com/dc25/solitaire](https://github.com/dc25/solitaire) .  A [stand alone demo also is available](https://dc25.github.io/solitaire).  The game runs entirely in the browser - no server is involved (except for the github server that serves the html and javascript code).

Most of the code is written in Haskell.  The Typescript code is only a small step away from being Javascript but I used the typescript compiler to get error checking and with the thought that it would be easier to use it from day one than to retrofit the project with Typescript after it got larger.

The card deck is a svg file.  I used [these beautiful SVG-cards designed by David Bellot](http://svg-cards.sourceforge.net/).  Thank you David Bellot.  The code that handles the cards is all Typescript and deserves its own blog post.  I'll write that post before too long.

The d3.js library is used for manipulating the card deck and for handling events (mouse picks, drag, drop, etc).
