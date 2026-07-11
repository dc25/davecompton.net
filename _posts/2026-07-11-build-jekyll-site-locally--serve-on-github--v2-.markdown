---
layout: post
title: "Build Jekyll site locally; serve on Github (v2)"
date: 2026-07-11 13:41:17 -0700
published: true
github_comments_issueid: "29"
tags:
---

This post is a simplified version of [a post from 2018](https://davecompton.net/2018/12/13/building-jekyll-site-locally-to-be-displayed-on-github.html).

The two posts accomplish the same thing but the steps in the earlier post, worked with the repository containing the jekyll source.  The steps in this post work by creating an additional local repository in the _site directory that contains the content to be published.

-------------

If you publish a Jekyll based website on Github they will do the Jekyll compile for you.  This is easy and quick to set up but here are some reasons you may want to build on your local machine and push the results up to Github:

* Github only supports a limited number of Jekyll plugins.  If you use any plugins outside of that set then you must build locally and push the result.
* Other web hosting sites will not support automated Jekyll builds in the same way.  If you want to compare sites you may need to do a local build anyway.
* Github does not provide automated builds for static site generators other than Jekyll.  Knowing how to push a local build lends itself to working with those other static site generators.

Here's a script that provides some help in pushing a local build to the gh-pages branch for a repository.  It's not very polished - users will need to edit it to specify the correct baseurl for the jekyll build.  

```
#! /bin/bash

# -- Build with baseurl, '/ntest'
# bundle exec jekyll build --baseurl '/ntest' 

# -- Build with no baseurl
bundle exec jekyll build 

# -- Switch to the _site directory
cd _site/

# -- Set up a git repository for this directory ( if not already done )
git init 
git branch -m gh-pages
git remote add ghp git@github.com:dc25/davecompton.net # of course, use your own remote repo 

# -- add, commit, and push _site contents to gh-pages branch.

git add .
git commit -m 'adding site to be pushed to gh-pages'
git push -f ghp gh-pages
```


After pushing to gh-pages, github will unset any custom domain name you might have specified.   Just re-enter and save it if needed.
