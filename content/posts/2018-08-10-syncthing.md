+++

Author = "Mark Keeley"
type = "posts"
title = "Syncthing"
description = "Peer to Peer File Synchronization"
draft = "false"
tags = ["Syncthing", "De-Google"]
topics = []
comments = "false"
slug = ""
date = 2018-08-10T21:39:22-05:00

+++

Google Drive, Dropbox, Microsoft Onedrive, Nextcloud, Amazon Drive - there is no shortage of options to store (and often share) files over the internet. Almost all use a [client-server model](https://infogalactic.com/info/Client%E2%80%93server_model), where home computers/tablets/smart phones connect to a server that runs everything. With the large corporate services you get convenience at the expense of privacy, control of your data, and typically a subscription fee if you exceed a specified file size. Self hosted options like [Nextcloud](https://nextcloud.com/) and [Seafile](https://www.seafile.com) give you back your privacy and data, but requires more setup and server maintenance.

[Syncthing](https://syncthing.net/) is interesting because it approaches things differently. Syncthing uses a [peer to peer model](https://infogalactic.com/info/Peer-to-peer) which gives it some unique advantages and disadvantages. For the privacy minded that don't already have their own server setup, Syncthing lowers the setup effort. Just install and run Syncthing on every computer or device that you want to have synchronized files. The downside is that if you have many computers/devices you want synchronized you are probably better off with the client-server model since the amount of work Syncthing needs to do is increased for every computer/device added. 

![Syncthing](/media/syncthing.png)

The program does illustrate the power of the Go programming language quite nicely. It is a command line program that runs on many hardware platforms and operating systems and it automatically opens a web browser to the appropriate location on startup for configuration. There's been a relative paucity of cross platform graphical user interfaces for a while now. Qt continues to be the gold standard for the few languages it supports. Gtk sticks out like a sore thumb anywhere except the Gnome 3 desktop environment, and while cross platform does exist, it is a complete after thought. And then there is Electron, the bloated "just ship a browser with your app" solution that makes no effort to integrate with your desktop, but seems to have taken the world by storm because it sadly is the path of least resistance. Applications like Syncthing that make use of the web browser that is already on your computer are a great compromise - a nice cross platform gui without needing to download an additional web browser (particularly one that will rarely, if ever, get security updates.) 

While it is available on MacOS it is not available on iOS. I've also read complaints about android battery life issues. On my own small tests I did notice the battery level dropping faster than normal. It might be best to run Syncthing when you want to update your files instead of having the program run in the background. I should also point out that it does have options for updating only when charging and only when using WiFi.

An aspect of Syncthing I haven't explored is sharing files in a group. Services like Google Docs have done a great job of making online collaboration practical when dealing with text documents and spreadsheets. I can see the advantages for a work/school group to all run Syncthing and have full access to the most up to date version of files (particularly files OTHER than text/spreadsheets that are already so well supported by existing services) but I can't come up with any real world use cases.

I really don't have much more I can say about this program. It is a slick and interesting program that solves problems that I personally don't have. I have a cheap unmanaged virtual private server. I have my Raspberry Pis operating at home. I use SSH, [KDE Connect](https://play.google.com/store/apps/details?id=org.kde.kdeconnect_tp&hl=en), rsync, and git. My connectivity needs are more than covered. I find myself looking for a problem Syncthing could solve just to justify using it more.

<!--more-->
