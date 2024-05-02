---
layout: post
title: "Minimal Vscode Typescript Setup"
date: 2020-04-23 04:07:18 +0000
published: true
github_comments_issueid: 10
tags:
---
Just some notes so I don't have to figure this out once again.


{% highlight bash %}
mkdir hw
cd hw
tsc --init
vim tsconfig.json   # edit to uncomment sourceMap line
vim hw.ts
code .
{% endhighlight %} 

Now 
* Open typescript file inside vscode editor
* Under Terminal menu, run "watch" build task.
* Set desired breakpoints
* Under Run menu, "Start Debugging"
