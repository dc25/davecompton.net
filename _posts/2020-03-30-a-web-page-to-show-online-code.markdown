---
layout: post
title: "A Web Page to Show Online Code"
date: 2020-03-30 18:58:32 +0000
published: true
tags:
---
A few months back I found myself wanting to discuss (in this blog) some code hosted on github .   How to display the code in the blog?   I could have used the jekyll highlight tag and inlined the code but then if the actual code changed, the inlined version would be out of date.   

At first I tried to inline the raw github version of the code of interest but, as you can see below, that didn not work:

<iframe width="300" height="200" src="https://raw.githubusercontent.com/dc25/showCode/master/show.ts" frameborder="0" allowfullscreen></iframe>

To make this work, I created a webpage that would retrieve the content of the url hosting the code and then display it.  The web page is currently hosted at "https://dc25.github.io/showCode" and it expects an encoded url as argument with the id "codeURL".   

So, for example, clicking on this link ...  <a id="urllink" href=""></a> ... will bring up the page showing the code of interest.

Or (more usefully) the web page can be embedded in another page using an iframe:

<iframe id="showCode" style="width:100%; height:390px" src='' ></iframe>

Up to this point, the example code that I have been using is the typescript code used to fetch and display online code.

Below is the markdown used to create this page:

<iframe id="showPage" style="width:100%; height:750px" src='' ></iframe>

<script> 
    function showCode(t)
    {
        return "https://dc25.github.io/showCode?codeURL=" + encodeURIComponent(t);
    }

    let code = "https://raw.githubusercontent.com/dc25/showCode/master/show.ts";

    let page = "https://raw.githubusercontent.com/dc25/davecompton.net/master/_posts/2020-03-30-a-web-page-to-show-online-code.markdown";
    document.getElementById("urllink").innerHTML=showCode(code);
    document.getElementById("urllink").href=showCode(code);
    document.getElementById("showCode").src=showCode(code);
    document.getElementById("showPage").src=showCode(page);
</script>
