---
layout: post
title: "Proof of Concept: image hosting"
date: 2023-02-05 14:30:31 +0000
github_comments_issueid: "22"
published: true
tags:
---
There are a number of sites that will host images for free.   Some of these are demonstrated below.   Remember these are free solutions that may or may not be maintained so it's probably a good idea to keep local copies of all your photos (i.e. don't use these for storage - just for display).   I may add to the collection if I learn about other hosting options.

---
# Dropbox:
![image from dropbox](https://www.dropbox.com/s/ffmsl8yel52wtbt/bird.jpg?raw=1)
[How to get dropbox image url.](https://www.dropboxforum.com/t5/Create-upload-and-share/How-do-I-embed-images-with-a-direct-link-from-Dropbox/td-p/245432)

---
# Flickr:
![image from flickr](https://live.staticflickr.com/65535/52670424446_5abbf3bb04_o.jpg)
[How to get flickr image url.](https://www.flickrhelp.com/hc/en-us/articles/4404078014356-Share-or-Embed-Flickr-Photos-or-Albums)

---
# github:
![image from github](https://github.com/dc25/birdphoto/raw/main/bird.jpg)
I got the image link by :
* Uploading the image to [a github repository](https://github.com/dc25/birdphoto).
* Clicking on the [image in the repository](https://github.com/dc25/birdphoto/blob/main/bird.jpg)
* Right clicking on the download link and selecting "Copy link address"

---
# imgur.com:
![image from imgur.com](https://i.imgur.com/VJtlHBD.jpg)
Just click on the uploaded image in imgur.com and use the "Direct Link"

---
# imgbb.com:
![image from imgbb.com](https://i.ibb.co/sw3Zpzq/bird.jpg)
use the "Direct links" option in the "Embed codes" menu for the uploated image in imgbb.com

---
Below is the markup for *this* page, included to show how to embed the images.

{% include iframecode.html 
              source-url= "https://github.com/dc25/davecompton.net/blob/master/_posts/2023-02-05-proof-of-concept--image-hosting.markdown"
              raw-url=    "https://raw.githubusercontent.com/dc25/davecompton.net/master/_posts/2023-02-05-proof-of-concept--image-hosting.markdown"
              height=     "800px" %}

