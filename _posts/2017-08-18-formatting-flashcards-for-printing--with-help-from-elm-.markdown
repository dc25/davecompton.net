---
layout: post
title: "Formatting flashcards for printing (with help from Elm)"
date: 2017-08-18 20:35:50 +0000
published: true
github_comments_issueid: "3"
tags:
---
As part of an effort to improve my Spanish vocabulary I wanted to print some English/Spanish flashcards.  My hope was to create a comma separated value file containing the vocabulary and then (somehow) transform that data into a printable format that I would then send to my printer to print multiple cards onto cardstock (front and back).  Once printed I would cut the cardstock up into individual cards.

After searching online and then [asking online][1]  for an easy way to do this ( neither having results I was completely happy with ) I decided to do it myself using Elm.

Using [Michael Bernstein's elm-csv-file-upload repo][2] as a reference it was straightforward to implement the functionality that I wanted.

[Here's the repo][3] and [here's the working app][4].  More technical details are available in the README file for the repo.

Please let me know if you have any comments or suggestions about this tool.

[1]: http://ask.metafilter.com/312359/how-to-print-flashcards-at-home
[2]: https://github.com/mrb/elm-csv-file-upload
[3]: https://github.com/dc25/printFlashcards
[4]: https://dc25.github.io/printFlashcards

