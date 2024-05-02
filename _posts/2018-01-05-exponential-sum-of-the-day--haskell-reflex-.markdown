---
layout: post
title: "Exponential Sum Of the Day (Haskell/Reflex)"
date: 2018-01-05 21:16:26 +0000
published: true
github_comments_issueid: "6"
tags: Haskell Reflex Javascript
tags:
---

[This post][johnCookPost] describes a way to associate a pretty graph with every date as follows:  A date is represented by three integers (month, day, year) which are used as input into an equation to get a series of points which are then summed to get another series of points and finally connected by line segments to get the graph.  

As an exercise, I implemented a [web app that draws these graphs][dailySumApp].  It initially draws the graph that cooresponds to today's date.  Forward and back buttons let you view the graphs for other dates.

The web app is written in [Haskell][haskellLanguage], and compiled to [Javascript][javascriptLanguage] using the [ghcjs compiler][ghcjsCompiler].  The [reflex library][reflexLibrary] is used for event handling and display.


[johnCookPost]: https://www.johndcook.com/blog/2017/11/02/recent-exponential-sums/

[dailySumApp]: https://dc25.github.io/dailySum/

[haskellLanguage]: https://www.haskell.org/
[javascriptLanguage]: https://en.wikipedia.org/wiki/JavaScript
[ghcjsCompiler]: https://github.com/ghcjs/ghcjs
[reflexLibrary]: https://hackage.haskell.org/package/reflex
