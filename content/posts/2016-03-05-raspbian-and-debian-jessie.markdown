---
author: Mark Keeley
topics:
- Technology
comments: true
date: 2016-03-05T02:02:15Z
slug: raspbian-and-debian-jessie
tags:
- Raspberry Pi
title: Raspbian and Debian Jessie
---

Since I seem to be on a Raspberry Pi kick lately...

While I was repurposing an older Pi into a home file server I ran into a problem. No problem right? Just check out the numerous Raspberry Pi tutorials all over the internet and be on my way - except those weren't working either. Turns out the official raspbian installs (and maybe upgrades from older versions) are now based off of Debian Jessie instead of the previous Debian Squeeze. Why does that matter? SystemD. The operating system now uses SystemD for low level things. This means any and all tutorials dealing with low level things like services are out of date. Ouch. So many tutorials for Raspberry Pi's that were created since 2012 (and books/ebooks for that matter) won't work for beginners.

The good news is the newer SystemD way isn't any more difficult, just different. I've taken to adding the word "jessie" to many of my Raspberry Pi google searches to find more current guides or just debian jessie guides (since that is what raspbian mostly is now.)

<!--more-->
