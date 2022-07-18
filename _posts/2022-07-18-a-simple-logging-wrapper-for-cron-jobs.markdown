---
layout: post
title: "A Simple Logging Wrapper For Cron Jobs"
date: 2022-07-18 14:06:31 -0700
published: true
tags:
---
For preserving output from cron jobs: [https://github.com/dc25/loggedcron](https://github.com/dc25/loggedcron)

In all its glory:

{% include iframecode.html 
              source-url= "https://github.com/dc25/loggedcron/blob/main/loggedcron.bash"
              raw-url=    "https://raw.githubusercontent.com/dc25/loggedcron/main/loggedcron.bash"
              height=     "240px" %}

So, for example, if you want a record of how much free space you have every night at 1:02, enter the following in your crontab (with appropriate changes of course):

```
02 01 * * * /home/dave/bin/loggedcron.bash df -k
```

