---
layout: post
title: "Building Jekyll site locally to be displayed on Github"
date: 2018-12-13 22:50:01 +0000
published: true
github_comments_issueid: "6"
tags: Jekyll
---

If you publish a Jekyll based website on Github they will do the Jekyll compile for you.  This is easy and quick to set up but here are some reasons you may want to build on your local machine and push the results up to Github:

* Github only supports a limited number of Jekyll plugins.  If you use any plugins outside of that set then you must build locally and push the result.
* Other web hosting sites will not support automated Jekyll builds in the same way.  If you want to compare sites you may need to do a local build anyway.
* Github does not provide automated builds for static site generators other than Jekyll.  Knowing how to push a local build lends itself to working with those other static site generators.

Here's a script that provides some help in pushing a local build to the gh-pages branch for a repository.  It's not very polished - uses will need to edit it to specify the correct baseurl for the jekyll build.  

	#! /bin/bash

	# -- Build with baseurl, '/ntest'
	# bundle exec jekyll build --baseurl '/ntest' 

	# -- Build with no baseurl
	bundle exec jekyll build 

	# -- Get rid of any existing local gh-pages
	git branch -D gh-pages

	# -- Create a new gh-pages branch not based on any existing branch
	git checkout --orphan gh-pages 

	# -- Delete the junk that git puts into this new branch 
	# -- ( see git-checkout --orphan documentation for details. )
	git rm -rf .

	# -- add, commit, and push contents from _site to gh-pages branch.
	git --work-tree _site/ add . 
	git --work-tree _site/ commit -m 'gh-pages commit' .
	git --work-tree _site/ push -f origin gh-pages

	## -- go back to working on master branch and clean up.
	git checkout -f master
	git branch -D gh-pages
	rm -rf _site
