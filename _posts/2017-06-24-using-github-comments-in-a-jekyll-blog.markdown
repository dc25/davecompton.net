---
layout: post
title: "Using Github comments in a Jekyll blog"
date: 2017-06-24 18:00:37 +0000
published: true
github_comments_issueid: "1"
tags:
---

### Background

A couple of months ago, [Don Williamson][1] demonstrated a clever technique for displaying Github issue comments on a static blog post.  He also provided a [nice write up][2] of the technique.

[1]: http://donw.io "Check out Don's blog!"
[2]: http://donw.io/post/github-comments/ "motivation and technical details"


### Limitations

A limitation of the implementaion was that it was specific to the [Hugo static site generator][hugo].  It turns out though that with minor changes, it can be adapted to work with a site based on the [Jekyll static site generator][jekyll].

[hugo]: https://gohugo.io "A Fast & Modern Static Website Engine"
[jekyll]: https://jekyllrb.com "The Jekyll site!"

### Jekyll

[This blog][myBlog] is based on Jekyll and includes the changes needed to support use of Github comments.  This post has Github comments enabled so that the comments shown below are from a Github issue.  To add to those comments, click the "Post a comment on Github" button below.  This will take you to a Github page at which anyone logged in to Github can add a comment to the discussion that shows up at the bottom of this page.  

[myBlog]: https://dc25.github.io/myBlog "Why bother?"

### Modifications

The code consists of four new files and two modifications:

* New Files
    * `_includes/github-comments.html`
    * `_sass/_gh-code.scss`
    * `_sass/_gh-comments.scss`
    * `js/github-comments.js`

* Modified Files
    * `_layouts/default.html`
    * `_sass/minima/initialize.scss`

### Reference Implementation
The repository [dc25/minimaWithGithubComments][minimaCommented] is a clone of the [jekyll/minima][minima] repository modified to allow Github comments.  In addition to the above Github comments changes, [the blog][minimaBlog] contains a [single post][minimaBlogPost] that shares the same comments as this post.   

[minimaCommented]: http://github.com/dc25/minimaWithGithubComments "github repository for minimaWithGithubComments"
[minima]: https://github.com/jekyll/minima "github repository for minima"
[minimaBlog]: https://dc25.github.io/minimaWithGithubComments "Click to see the blog"
[minimaBlogPost]: https://dc25.github.io/minimaWithGithubComments/2017/06/25/example-of-blog-post-with-github-comments.html "Click to see the post"

### Configuration

* Set, `github_comments_repository` in `_config.yml` to the name of the repository to use for comments.  You can either create a repository for this purpose or use an already existing one.

* Create a new issue in the repository specified in `_config.yml` to hold the comments for your blog post.  Github will assign it an id.  

* Set `github_comments_issueid` in front matter for a blog post to the id just assigned by Github. 

