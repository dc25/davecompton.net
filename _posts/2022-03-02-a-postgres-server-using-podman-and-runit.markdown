---
layout: post
title: "A postgres server using podman and runit"
date: 2022-03-02 16:10:10 -0800
published: true
tags:
---

Here's a way to quickly set up a postgres server on a ubuntu 21.10 desktop machine.

Install podman and runit.
Configure runit to run postgres in a podman container.

That's all.  The server should start immediately and on reboot.

Here's a script that does this.

{% include iframecode.html 
              source-url= "https://github.com/dc25/podmanPostgresRunit/blob/main/setup.bash"
              raw-url=    "https://raw.githubusercontent.com/dc25/podmanPostgresRunit/main/setup.bash"
              height=     "400px" %}

* Takes a couple of minutes to start the first time due to image download time.
* Use podman ps to query podman
* To disable the server, remove the run script.
* Use sv to monitor/start/stop/restart the service:

{% include iframecode.html 
              source-url= "https://github.com/dc25/podmanPostgresRunit/blob/main/sampleUsage.sh"
              raw-url=    "https://raw.githubusercontent.com/dc25/podmanPostgresRunit/main/sampleUsage.sh"
              height=     "560px" %}


As configured the database served is not stored anywhere permanent.   You can fix this by changing the run script to suit your needs.

