+++

Author = "Mark Keeley"
type = "posts"
title = "Nextcloud"
description = ""
draft = "false"
tags = ["Nextcloud", "Nextcloudpi", "De-Google", "self host"]
topics = []
comments = "false"
slug = ""
date = 2018-08-20T22:54:08-05:00

+++

[Nextcloud](https://nextcloud.com/) is undoubtedly the biggest name in open source cloud software right now. The list of features is substantial. Sure, it has the standard file sharing options and android/iOS clients, but it goes further with:

* end to end encryption
* optional server side storage encryption
* contacts
* calendar
* optional RSS feed reader
* video conferencing

![Nextcloud](/media/img/nextcloud.png)

I haven't tested out the video chat capabilities and I doubt the Raspberry Pi 3 I run Nextcloud on is up to the challenge anyways. I'm generally a big fan of compiled binaries (like from Go) but there's no getting around the fact that PHP does give Nextcloud a lot of add-on potential. The add-on I've been using the most is the RSS feed reader. I hadn't even planned on using it, but when KDE's Personal Information Management system bugged out (again!) I thought I'd try something new. Desktop or smartphone, the RSS feed reader works great.

With an open source project as popular as Nextcloud it should come as no surprise that there are multiple ways to install it. Yes, there's always manual installation, but [docker](https://hub.docker.com/_/nextcloud/) and [snap](https://snapcraft.io/nextcloud) options exist. The method I'm using is called [NextcloudPi](https://ownyourbits.com/nextcloudpi/). NextcloudPi is an all in one download that has everything needed to run Nextcloud and even auto updates itself. I put it on my Raspberry Pi 3 and it has worked really well. I've been surprised at how well it has run. It isn't the speediest thing but it is more than enough to do what I ask of it. 

I see why Nextcloud is thought of as the first choice for personal cloud software. Over the month that I've been using it, Nextcloud has been a consistently great experience. Anyone looking into getting control of their own personal data should give it a try.

<!--more-->
