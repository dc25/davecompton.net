---
layout: post
title: "Rust/GTK4/ColumnView"
date: 2022-05-01 21:40:01 -0700
published: true
github_comments_issueid: 17
tags:
---

I couldn't find any Rust/GTK4 sample code showing how to use ColumnView so I wrote one.  

* Two columns
* 100000 rows
* Model is StringList containing StringObject(s) - both defined by GTK4
* Deletes row from model when user double clicks on it.
* Tested on Linux and Windows

If you read this and have any questions/comments, please let me know.


{% include iframecode.html 
              source-url= "https://github.com/dc25/gtk_columnview/blob/main/src/main.rs"
              raw-url=    "https://raw.githubusercontent.com/dc25/gtk_columnview/main/src/main.rs"
              height=     "1400px" %}

