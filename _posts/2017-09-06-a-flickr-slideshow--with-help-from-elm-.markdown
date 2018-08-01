---
layout: post
title: "A flickr slideshow (with help from Elm)"
date: 2017-09-06 19:27:15 +0000
published: true
github_comments_issueid: "4"
tags:
---

When I saw [this ask metafilter question ("Online Photo Gallery")][1], I was inspired to write a web-app that would display photos from one of the photo hosting sites (flickr, google photos, etc) as a simple slideshow.


It turns out that [flickr has an api][2] that lends itself well to this job .   Using the flickr api I can retrieve all the information I need to display a list of photos published by a given user (optionally limited to one album) .  This includes the description of the photo saved in flickr that I can use as a photo caption.

My language of choice for this task was [Elm][3]. Using the [decoder functionality that comes with Elm][4], it's easy to work with rest api calls that return JSON data.  I used [Evan Czaplicki's url-parser library][5] to read flickr user and album names as routing parameters in the URL.

The web-app is published on github.  I created a [flickr user: "dave20477"][6] for demo purposes so clicking on this link ...

[https://dc25.github.io/askMetaGallery/#/dave20477] [7]

... will initiate a slideshow that lets you view all the public photos published by "dave20477" .

and clicking on this link ...

[https://dc25.github.io/askMetaGallery/#/dave20477/album_for_metafilter][8]

... will initiate a slideshow that lets you view all the public photos in the album "album_for_metafilter" belonging to "dave20477"


The content produced by these pages is intended to work well with html inlining so these slideshows can be embedded in a web page as shown below.

<iframe width="850" height="650" src="//dc25.github.io/askMetaGallery/#/dave20477/" frameborder="0" allowfullscreen></iframe>
[//]: # <iframe width="850" height="650" src="//172.17.0.2:8000/#/dave20477/" frameborder="0" allowfullscreen></iframe>

The source code for this web-app is [published on github][9].

[1]: http://ask.metafilter.com/313090/Online-Photo-Gallery
[2]: https://www.flickr.com/services/api/
[3]: http://elm-lang.org
[4]: http://package.elm-lang.org/packages/elm-lang/core/latest/Json-Decode
[5]: http://package.elm-lang.org/packages/evancz/url-parser/2.0.1/UrlParser
[6]: https://www.flickr.com/people/151986531@N04/
[7]: https://dc25.github.io/askMetaGallery/#/dave20477
[8]: https://dc25.github.io/askMetaGallery/#/dave20477/album_for_metafilter
[9]: https://github.com/dc25/askMetaGallery

