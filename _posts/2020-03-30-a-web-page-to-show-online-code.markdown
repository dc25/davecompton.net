---
layout: post
title: "A Web Page to Show Online Code"
date: 2020-03-30 18:58:32 +0000
published: true
github_comments_issueid: 9
tags:
---

**NOTE: This project is in flux.   As of now (2020-04-07) it works but I may be making changes in the near future.  If that happens, I'll update this page accordingly.**

**In particular, the helper site, "https://dc25.github.io/showCode" may also change and, if you use that link directly, in the same way that I have here, your site may stop working.   For this reason, I suggest making a your own copy of that helper site and using your own copy.**

***

A few months back I found myself wanting to discuss (in this blog) some code hosted on github .   How could I display the code in the blog?   I could have used the jekyll highlight tag and inlined the code but then if the actual code changed, the inlined version would be out of date.   
At first I tried to inline the raw github version of the code of interest but that fails because xframe options are set to deny such usage.

To make this work, I created a webpage that would retrieve the content of the url hosting the code and then display it.  The web page is currently hosted at "https://dc25.github.io/showCode" and it expects an encoded url as argument with the id "codeURL".   This is a static site that does not depend on a backend server.

The code displayed will be highlighted using [code prettify by google](https://github.com/google/code-prettify).

{% capture display-code %}
https://dc25.github.io/showCode?codeURL={{ "https://raw.githubusercontent.com/dc25/showCode/master/show.ts" | url_encode }}
{% endcapture %}

So, for example, clicking on this link ...  [{{display-code}}]({{display-code}}) will bring up a page showing typescript code that the showCode site uses to fetch and display online code.

Or (as shown below) the same web page can be embedded in another page using an iframe.   I am using some html and css to decorate the iframe.
{% include iframecode.html 
              title=      "show.ts"
              source-url= "https://github.com/dc25/showCode/blob/master/show.ts"
              raw-url=    "https://raw.githubusercontent.com/dc25/showCode/master/show.ts"
              height=     "460px" %}

Below is the markdown used to create this page (the prettifier seems to have some trouble with markdown).  This includes the javascript that helps set up the content on this page.
{% include iframecode.html 
              title=      "2020-03-30-a-web-page-to-show-online-code.markdown"
              source-url= "https://github.com/dc25/davecompton.net/blob/master/_posts/2020-03-30-a-web-page-to-show-online-code.markdown"
              raw-url=    "https://raw.githubusercontent.com/dc25/davecompton.net/master/_posts/2020-03-30-a-web-page-to-show-online-code.markdown"
              height=     "860px" %}

Here is the html included to decorate the displayed code.
{% include iframecode.html 
              title=      "iframecode.html" 
              source-url= "https://github.com/dc25/davecompton.net/blob/master/_includes/iframecode.html"
              raw-url=    "https://raw.githubusercontent.com/dc25/davecompton.net/master/_includes/iframecode.html" 
              height=     "250px" %}

And here is the sass for the css used to style the included html.
{% include iframecode.html 
              title=      "_iframecode.scss" 
              source-url= "https://github.com/dc25/davecompton.net/blob/master/_sass/_iframecode.scss" 
              raw-url=    "https://raw.githubusercontent.com/dc25/davecompton.net/master/_sass/_iframecode.scss" 
              height=     "500px" %}

