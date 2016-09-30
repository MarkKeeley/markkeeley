---
author: Mark Keeley
categories:
- Technology
comments: true
date: 2015-06-10T12:29:49Z
slug: experimenting-with-the-kore-remote-control-app
tags:
- Kodi
- Raspberry Pi
title: Experimenting with the Kore remote control app
---

Recently I’ve been trying out [Kore](https://play.google.com/store/apps/details?id=org.xbmc.kore&hl=en) – a remote control android app for [Kodi](http://kodi.tv/).

I’ve had my [Raspberry Pi](https://www.raspberrypi.org/) version 1 model B for almost a year now. With [OpenElec](http://openelec.tv/) installed it turns the Pi into a fantastic media center. Small, cheap, and quiet it has worked out well. The one downside is while the Pi can play mp4 video with no trouble, it cannot do much else while playing. Case in point, trying to use a mouse to pause/unpause a video currently playing is an exercise in frustration. I imagine that Raspberry Pi 2’s with their much improved cpu’s are able to handle such situations.

The Kore remote control app is slick and very functional. It has all the desired functions and easily interfaced with my Pi. The only issues I have encountered are almost certainly due to the Pi’s ability to play video – but with no cpu horsepower left to spare. Once a video starts playing it constantly connects to the Pi, only to lose the connection and reconnect over and over again.

The other issue I had with Kore was when my Pi switched to a new IP address. When I started Kore it kept trying to find the Pi at the previous location. It didn’t allow me to say, “Hey, look for the Pi over at this new location.” I had to delete the Kore local app data on my android phone to fix this problem. I then changed my Pi to a static IP to prevent this problem from happening again, but this seems like something to improve in a later version of the Kore app.

<!--more-->
