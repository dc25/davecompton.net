---
layout: post
title: "A Web Page to Show Online Code"
date: 2020-03-30 18:58:32 +0000
published: true
github_comments_issueid: 9
tags:
---
A few months back I found myself wanting to discuss (in this blog) some code hosted on github .   How to display the code in the blog?   I could have used the jekyll highlight tag and inlined the code but then if the actual code changed, the inlined version would be out of date.   

At first I tried to inline the raw github version of the code of interest but, as you can see below, that did not work:

<iframe width="300" height="200" src="https://raw.githubusercontent.com/dc25/showCode/master/show.ts" frameborder="0" allowfullscreen></iframe>

To make this work, I created a webpage that would retrieve the content of the url hosting the code and then display it.  The web page is currently hosted at "https://dc25.github.io/showCode" and it expects an encoded url as argument with the id "codeURL".   

The code displayed will be highlighted using [code prettify by google](https://github.com/google/code-prettify).

So, for example, clicking on this link ...  <a id="urllink" href=""></a> ... will bring up the page showing the code at the encoded URL.


Or (more usefully) the web page can be embedded in another page using an iframe:

<div id="showCode" class="code-container">
  <iframe class="code-text" src='' ></iframe>
  <hr>
  <div class="code-bar">
    <a href="" class="code-link"></a>
    <a href="" class="code-raw-link">view raw</a>
  </div>
</div>

So far, the example code that I have been using is the typescript code used to fetch and display online code.

Below is the markdown used to create this page (the prettifier seems to have some trouble with markdown):

<div id="showPage" class="code-container" style="height:800px">
  <iframe class="code-text" src='' ></iframe>
  <hr>
  <div class="code-bar">
    <a href="" class="code-link"></a>
    <a href="" class="code-raw-link">view raw</a>
  </div>
</div>

<script> 

    // Return the URL that will display the code found at the URL argument
    function showCodeURL(codeUrl)
    {
        return "https://dc25.github.io/showCode?codeURL=" + encodeURIComponent(codeUrl);
    }

    // Embed (in this page) code hosted elsewhere on the web 
    // Expects to find a bit of html that is tagged by "id"
    // Other than the "id" tag, the html is boilerplate.
    function embedCode(id, name, rawURL, sourceURL)
    {
        let codeContainer = document.getElementById(id);
        let codeText = codeContainer.getElementsByClassName("code-text")[0];
        codeText.src = showCodeURL(rawURL);

        let codeBar = codeContainer.getElementsByClassName("code-bar")[0];
        let codeRawLink = codeBar.getElementsByClassName("code-raw-link")[0];
        codeRawLink.href = rawURL;

        let codeLink = codeBar.getElementsByClassName("code-link")[0];
        codeLink.innerHTML = name;
        codeLink.href = sourceURL;
    }

    let code = "https://raw.githubusercontent.com/dc25/showCode/master/show.ts";

    document.getElementById("urllink").innerHTML=showCodeURL(code);
    document.getElementById("urllink").href=showCodeURL(code);

    embedCode("showCode"
             ,"show.ts"
             ,"https://raw.githubusercontent.com/dc25/showCode/master/show.ts"
             ,"https://github.com/dc25/showCode/blob/master/show.ts"
             );

    embedCode("showPage"
             ,"2020-03-30-a-web-page-to-show-online-code.markdown"
             ,"https://raw.githubusercontent.com/dc25/davecompton.net/master/_posts/2020-03-30-a-web-page-to-show-online-code.markdown"
             ,"https://github.com/dc25/davecompton.net/blob/master/_posts/2020-03-30-a-web-page-to-show-online-code.markdown"  
             );
</script>
