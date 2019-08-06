+++

Author = "Mark Keeley"
type = "posts"
title = "Pass and Keepass"
description = "Password management"
draft = "false"
tags = ["linux", "passwords", "command line", "pass", "keepass", "nextcloud"]
comments = "false"
slug = ""
date = 2019-08-05T02:24:57-05:00

+++

Dealing with multiple passwords is just an unpleasant fact of life for anyone using computers. The number of passwords continue to grow as computing continues to transition from applications stored on a computer to web services that all want you to create an account. Anyone that wants a more sophisticated solution to juggling passwords than relying on post it notes will eventually investigate password managers. Here I lay out my experience with two such password managers, [pass](https://www.passwordstore.org/) and [KeePass](https://keepass.info/).

<!--more-->

While looking into password management options I was struck by just how many of them revolved around a person storing passwords on computers that they didn't own and then being _allowed_ access to their info. Being reliant on internet access 100% of the time is a serious design flaw to me. When Murphy's Law strikes and you want a password to something, you don't want to be stopped by a lack of internet. I sometimes wonder if the people who never take internet access into consideration ever travel. The charitable view on this is that by having total control over your passwords companies can offer the most streamlined and convenient service possible. The cynical view realizes that this gives companies a vendor lock-in, making it inconvenient or impossible to switch your info to another password solution.

**_[pass](https://www.passwordstore.org/)_**  
In many ways pass is a command line lover's dream. It embodies the Unix philosophy of, "do one thing and do it well." For encryption it uses [GPG](https://gnupg.org/), for versioning it (optionally) uses git. It features random password generation and convenient copying to the clipboard to use unlocked passwords. The pass ecosystem has browser integration options and android/iOS apps. Unlike many password managers it's open source and YOU keep the files.

One issue that some people have is that while the passwords are encrypted, the names aren't. Everything is actually just files & folders. This means that it's possible to simply copy your pass info for backup. This also means that file names can be read. If you store your email as "Email/myname@example.com" then it's possible for a third party to know the name of your email. They won't know the password to that email account (that's encrypted) but they'll be able to learn something if they gain access to your files. This was never a concern for me, but I thought I'd mention it.

I made heavy use of pass's random password generation. I had it set up so that my computer/devices all used git and synced up to a raspberry pi. I even used an android app to have full access to my passwords on the go. I went full in on pass before realizing it wasn't for me. I don't know anyone that actually likes GPG. It's ancient by security/encryption standards, filled to the brim with antiquated 90's notions (web of trust), and has a tricky and unhelpful command line interface. Every 6 months I would upgrade to a new version of Ubuntu and when setting back up my GPG keys I would be confronted with the same realizations:

* I have a very limited understanding of GPG
* if I mess anything up I probably won't be able to fix it
* I have a bunch of complicated randomly generated passwords
* if anything goes wrong I will have locked myself out of many accounts
* GPG makes it very easy to get something wrong

The versioning/synchronized side of things wasn't any better. I am no git expert. Sure, I can sling the usual add/push/pull/commit/status basic commands with the best of them, but the moment things go off the well beaten path I'm stuck doing online searches to figure out what went wrong. When I needed to remake my pass git remote repository on a raspberry pi and ran into issues that no [duckduckgo](https://duckduckgo.com/) search could fix, I knew it was time to find a different password manager.

_While I never tried it, [QtPass](https://qtpass.org/) looks like something worth investigating for those wanting a nice graphical user interface on top of pass._

**_[KeePass](https://keepass.info/)_**  
The first thing to know about KeePass is there are actually multiple KeePass applications. There's the original KeePass, which started out as Windows only. There's [KeePassXC](https://keepassxc.org/), a multiplatform (Linux/Windows/Mac) alternative. Then there's a whole host of KeePass versions for iOS, android, and "web only." The important thing is that all these programs create a KeePass database file (.kdbx) which stores (and encrypts) your information and is compatible between the various KeePass programs. In terms of backing things up, all you need to do is make a copy of your .kdbx file.

I've been using [KeePassXC](https://keepassxc.org/) for Linux and [KeePassDroid](http://www.keepassdroid.com/) for Android. KeePassXC seems to be widely available in the various Linux distro's repositories, but for the most up to date version, you can get an [appimage](https://appimage.org/) from KeePassXC's website or use [flatpak](https://flathub.org/apps/details/org.keepassxc.KeePassXC).

There's a night and day difference with encryption between KeePass and pass. KeePass simply handles all encryption matters so I get up to date encryption easily and effortlessly. No dealing with GPG, or setting up keys, or even worrying about messing something up. As long as I remember the password to the .kdbx file, KeePass will do the rest.

When it comes to synchronizing the password file across computer and devices I use Nextcloud on my Raspberry Pi 3. I use the android Nextcloud app to synchronize the file on my phone. On my computer I directly read/write the password file through Dolphin (KDE File Manager) using webdav. Nextcloud automatically makes versions of changed files you upload so my approach gives me effortless versioning. And when my Nextcloud files are backed up my .kdbx file is also backed up.

It occurs to me that if I didn't have Nextcloud to handle this I would finally have a reason to run [syncthing](https://syncthing.net/). Synchronizing passwords stored in a .kbdx file across computers/devices is exactly syncthing's use case. When I [investigated syncthing in 2018](/2018/syncthing/) I was impressed with it (despite it being a bit fast to drain my phone battery), but I didn't have a reason to use it. A year later and that still hasn't changed. [Nextcloud](/tags/nextcloud/) continues to perfectly cover all these sorts of needs.

One last thing to mention, for those that use cloud provider services (Google Gdrive, Dropbox, iCloud, etc.) you can use that to store your password .kdbx file and have access to it across computers/devices.
