+++
description = "Connect to Server in Mate Desktop"
title = "Convenient file transfer between Linux computers"
Author = "Mark Keeley"
date = "2016-10-12T04:33:45-05:00"
tags = ["Mate Desktop", "Linux"]
topics = ["Technology"]
type = "posts"
slug = ""
draft = false
comments = "false"

+++

I've been installing the pre-release version of [Ubuntu Mate 16.10](https://ubuntu-mate.org) and I was reminded of a useful feature I found out about a few months ago called "Connect to Server..." I do a lot of file transfers between computers. Sometimes I transfer from my Raspberry Pi network file server and sometimes from my virtual private server. In the past I used [Filezilla](https://filezilla-project.org/) for file transfers, but for whatever reason the user interface barely worked for me in Ubuntu Mate 16.04.

Fortunately for those using the Mate desktop there is great built in option. <!--more-->Mate's file browser Caja can connect to other computers using various protocols (SSH/FTP/samba/ect.) This way you can deal with files on other computers the same way you handle files locally. 

So how do you do this? First open the Caja file browser. Next go to the file menu and select "Connect to Server..."

![Connect to Server](/media/connectserver.gif)

Then select the connection type and fill out the needed info.

![Connect to Server 2](/media/connectserver.png)

These days you always want to be using SSH over FTP whenever possible. FTP is unsecure and vulnerable to 'man in the middle' attacks. Remember to select "Add bookmark" and give a name to have a handy shortcut. 
