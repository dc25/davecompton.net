---
layout: post
title: "Minesweepers written using:  Elm, Reflex, Miso"
date: 2017-11-26 04:18:07 +0000
published: true
github_comments_issueid: "5"
tags: Haskell Reflex Elm Miso
---

As a learning experience, I implemented the [classic minesweeper game][minesweeperGame] in Haskell with the [Reflex toolkit][reflex], in Haskell using [the Miso toolkit][miso], and in [Elm][elm].

The source for all three is on github:

* [minesweeperReflex][reflexSource]
* [minesweeperMiso][misoSource]
* [minesweeperElm][elmSource]



Here are links to live versions of each:

* [minesweeperReflex][reflexDemo]
* [minesweeperMiso][misoDemo]
* [minesweeperElm][elmDemo]



[minesweeperGame]: https://en.wikipedia.org/wiki/Minesweeper_(video_game)

[reflex]: https://github.com/reflex-frp/reflex
[miso]: https://haskell-miso.org/
[elm]: http://elm-lang.org/

[reflexSource]: https://github.com/dc25/minesweeperReflex
[misoSource]: https://github.com/dc25/minesweeperMiso
[elmSource]: https://github.com/dc25/minesweeperElm

[reflexDemo]: https://dc25.github.io/minesweeperReflex
[misoDemo]: https://dc25.github.io/minesweeperMiso
[elmDemo]: https://dc25.github.io/minesweeperElm


The code for each version is as similar to the others as I could make it without sacrificing performance.

The Reflex version was the first implementation.  I tried a variety of approaches - the current one was the most responsive.

The Elm version was a port of the Reflex version.  It's the most responsive of the three and also the quickest to load.

The Miso version was a port of the Elm version with some code from the Reflex version brought in and then changed as needed to get it working.Programming in Miso feels very similar to programming in Elm.

Along the way I made small changes to all three of them to keep the code as similar as possible across implementations.

