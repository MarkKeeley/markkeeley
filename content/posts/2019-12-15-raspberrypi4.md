+++

Author = "Mark Keeley"
type = "posts"
title = "Raspberry Pi 4"
description = "A newer Pi for a better Nextcloud experience"
draft = "false"
tags = ["raspberry pi", "nextcloud", "self-host", "de-google"]
comments = "false"
slug = ""
date = 2019-12-15T17:04:25-06:00

+++

I finally broke down and got a [Raspberry Pi](https://www.raspberrypi.org/) 4 (2 Gig) a few weeks ago to replace my Raspberry Pi 3 running my [Nextcloud](https://nextcloud.com/) instance. All the marketing, info, and reviews really seem to stress the Pi 4 as a possible desktop replacement. If you do care about a desktop replacement make sure to pick up the 4 gig version. I can't say how usuable the Pi 4 is for desktop activities, I never bothered to test it out.

So what does the Pi 4 bring over the Pi 3 for those interested in self hosting their software? There's the obvious advantage that the Pi 4 comes in 1/2/4 Gig ram versions. There's improvements to the cpu. I see very noticable performance improvements running php scripts on my Nextcloud website. The pages load faster and the file synchronizing webdav features run better. Using Nextcloud there was always the sense that with the Pi 3 the performance was *almost, but not quite* there. I'm glad to see that the performance is finally there with the Pi 4.

The two best improvements to the newest Pi for my usage is that the Pi 4 is the first Pi that features USB3 and also seperates USB and networking. Previous Pis were limited it USB2 and to make matters worse, networking was done through USB2 forcing it to share precious bandwidth. This is why Pis have historically been lackluster when used as a Network Attached Storage (NAS) device. Making use of the USB3 ports with a USB3 flashdrive (tested read speeds of around 290 meg per second) has also made a noticable difference in loading files, particularly Nextcloud folders with lots of small files or folders with many pictures.

I've been scanning 20 year old family photos and sharing them with family members using Nextcloud. One thing I learned quickly when I was using a Pi 3 was that it didn't take much for it to be overwelmed when uploading photos. Often just uploading more than one photo at a time to Nextcloud was enough to make the Pi 3 freeze and need to be turned off/on to work again. Admittedly these are high quality scans (typically 1.4 Meg) but carefully uploading just one file at a time was slow. Fortunately none of this is an issue with my new Pi 4. I'm not sure if this is due to the better cpu or double the ram, but I've been able to upload multiple photos at a time without issue.

If you were watching the launch of the Raspberry Pi 4 one thing people that had them wanted to talk about was how much hotter they were running compared to previous versions. An improved firmware came out before I obtained my Pi so I can't compare them. I will say that with the new firmware my Pi 4 doesn't seem to be running noticably hotter than my Pi 3, but a webserver won't be pushing the cpu to 100% for a prolonged period of time.

<!--more-->
