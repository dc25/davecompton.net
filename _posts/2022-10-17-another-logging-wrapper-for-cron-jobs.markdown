---
layout: post
title: "Another Logging Wrapper For Cron Jobs"
date: 2022-10-17 20:01:12 +0000
published: true
github_comments_issueid: "20"
tags:
---
This logging wrapper, written in perl, has a few improvements on the previous one:

* Command line option: "--skip-repeats" to prevent saving results identical to previous run.
* Command line option: "--path-head" for specification of top level directory for results.  Defaults to command name.
* Command line option: "--path-resolution" for specification of resolution of directory creation for output files.   Defaults to "year"
* Pushes results to remote git repository.
* Partitions the output files by year directory.   The github web interface  complains about more than 1000 files in a directory.

{% include iframecode.html 
              source-url= "https://github.com/dc25/startup/blob/master/bin/loggedcron.pl"
              raw-url=    "https://raw.githubusercontent.com/dc25/startup/master/bin/loggedcron.pl"
              height=     "1600px" %}

So, for example, if you want a record of how much free space you have every night at 1:02, enter the following in your crontab (with appropriate changes of course):

```
02 01 * * * /home/dave/bin/loggedcron.pl df -k
```
