+++

Author = "Mark Keeley"
type = "posts"
title = "Video Capture with Dazzle DVC100 using Linux"
description = ""
draft = "false"
tags = ["linux", "guide"]
comments = "false"
slug = ""
date = 2019-12-19T00:44:54-06:00

+++

Over the years I have occasionally had to search the internet for a solution to a problem that almost no one else seemed to be encountering. Eventually I would stumble upon someone's blog where they encountered the same issue, but they also had the solution to the problem. In that grand tradition of helping others I present this post: "**Capturing video with Dazzle DVC100 Rev. 1.1 using Linux in late 2019**."

My Dad has been video recording family events since 1988. This means he has a huge collection of old [VHS-C](https://infogalactic.com/info/VHS-C) tapes - thankfully well labeled. Aware of the issue with tapes fading/degrading over time, I gathered up every vhs-c tape I could find and digitized them all. Now years later, I recently discovered an additional 37 tapes.

<!--more-->

Last time I used a computer running Windows 7 to record, but everything I have now is Linux or Windows 10. The [driver download website](http://cdn.pinnaclesys.com/SupportFiles/Hardware_Installer/readmeHW10.htm) hasn't been updated since 2012 and doesn't support Windows 10. I did try to use the Windows 7 drivers but it didn't work. Fortunately the Linux side of things has improved greatly from what it was years ago. With [Video 4 Linux 2](https://infogalactic.com/info/Video4Linux) my video capture device is recognized and I've been able to start recording again - with a few gotchas. I haven't been able to get [VLC](https://www.videolan.org/vlc/) working with Dazzle but [OBS Studio](https://obsproject.com/) easily worked.

Due to the nature of the videos I want to digitize I record with ffmpeg in an uncompressed format to edit and [deinterlace](https://infogalactic.com/info/Deinterlacing) later. A 30 minute long recording is 53+ gigabytes.

First you need to figure out which audio capture device belongs to Dazzle.

>arecord -l

For my computer this is the result:

>**** List of CAPTURE Hardware Devices ****
card 1: Generic [HD-Audio Generic], device 0: ALC892 Analog [ALC892 Analog]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 1: Generic [HD-Audio Generic], device 2: ALC892 Alt Analog [ALC892 Alt Analog]
  Subdevices: 1/1
  Subdevice #0: subdevice #0
card 2: DVC100 [DVC100], device 0: USB Audio [USB Audio]
  Subdevices: 1/1
  Subdevice #0: subdevice #0

Next we need to figure out which video device belongs to the Dazzle. If the dazzle is the only video device it will probably be /dev/video0.

>ls -l /dev/video*

Lastly I make use of the qv4l2 application to configure Video4Linux. Use this program to switch between composite and s-video input. On my system the default is PAL (european) and not NTSC (north america) so every time I restart my computer I need to use this program to switch over to NTSC before I start recording. On a Debian/Ubuntu system the command to install it is

>sudo apt install qv4l2

This will also install the v4l2 utilities if your computer doesn't have them.

And *finally* the command to start recording with ffmpeg:

>ffmpeg -thread_queue_size 1024 -f video4linux2 -input_format yuyv422 -i /dev/video0 -thread_queue_size 1024 -f alsa -i hw:2,0 -c:v copy -c:a pcm_s16le -ac 1 output.avi

Make sure you are in the location you want to record the file. Press q to stop recording. So what does that command all do? It tells ffmpeg to start recording with the dazzle's native uncompressed video format (yuyv422) and that the video device is video0. If your dazzle is on a different video device number you will need to change the command. It also tells ffmpeg to record audio uncompressed (pcm_s16le) and that the audio is found at card 2 (hw:2,0). If your dazzle's audio is a different number you will need to make the adjustment.

When I first tried recording the audio would occasionally drop out and ffmpeg would regularly warn about the default thread queue size being too small. I added -thread_queue_size 1024 to increase the amount of ram made available so that wouldn't be an issue. Yes, -thread_queue_size being listed twice is intentional; once for video, second for audio.

While this hasn't been an issue for me using ffmpeg, using other programs sometimes made the dazzle's audio stop working. [This helpful website](https://forums.linuxmint.com/viewtopic.php?t=123022) has a useful command to reset audio if it stops working:

>v4l2-ctl --set-standard=ntsc --set-input=0 --set-ctrl=mute=0

For my simple editing needs I use [Avidemux](https://flathub.org/apps/details/org.avidemux.Avidemux) to work on my recordings. There are multiple options to deinterlace recordings found in the video filter section.
