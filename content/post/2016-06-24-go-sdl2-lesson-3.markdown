---
author: Mark Keeley
topics:
- Programming
comments: false
date: 2016-06-24T05:34:10Z
slug: go-sdl2-lesson-3
tags:
- Go / Golang
- SDL2
title: Go SDL2 Lesson 3
description: Drawing bitmap graphics
---

Today's lesson is about putting an image (png file in this case) on the screen. No more rectangles for today! Today the stick figures run amok.

![Go SDL2 Lesson 3](/media/lesson03.png)

<!--more-->

From what I recall, all the old sdl version 1.2 tutorials I read years ago would always start with using:

> SDL_LoadBMP

But I'm not going to do that. I'm diving right into SDL_Image instead. The jpg/png combo beats bitmaps in every single way. Need a photograph? Jpg. Need a lossless graphic? Png. Plus png has the very useful option of transparency (which this lesson's graphic makes use of.) And most importantly, bitmaps have just disgustingly large file sizes (the format has no compression.)

Okay fine, if you _absolutely have to use_ a bitmap for some reason, the SDL_Image library still has you covered. According to its **[website](https://www.libsdl.org/projects/SDL_image/)**, SDL_Image supports 13 file formats (BMP, GIF, JPEG, LBM, PCX, PNG, PNM, TGA, TIFF, WEBP, XCF, XPM, XV.) Personally I've never even heard of 5 of them. All right, enough chit chat, time for some code. [**Lesson 3 source code**](https://github.com/MarkKeeley/Go-SDL2-Lessons/tree/master/Lesson03)
 
> import "github.com/veandco/go-sdl2/sdl_image"

We are using a new part of the sdl library and so we need an additional import.

> img.Init(img.INIT_JPG | img.INIT_PNG)

This is an interesting call because it is entirely optional. Unlike sdl.Init you can (and should) comment out this line of code and run the program. Go ahead, I'll wait. See the program ran just like before. If you don't call img.Init it will automatically init itself when you use it to load an image. SDL_Image tries to be very efficient and so it will dynamically load the needed libraries for jpg/png/tiff/webp only when needed. Using img.Init allows you to preload all the needed image libraries at the start of a program. Presumably this is for error checking and preventing a stutter from happening when suddenly the program loads an image in a file type it hadn't previously dealt with (and thus had to dynamically load the library for it.) Of course on a desktop computer such a pause would almost certainly be imperceptible, but we are in the age of mobile phones, so there's really no telling just how low powered the device running your program might be. It's worth noting that in the source code img.Init preloads both jpg and png with the same call. The program doesn't use jpg at any point, I just wanted to demonstrate using | to use multiple flags on the same line of code.

> sdl.SetHint(sdl.HINT_RENDER_SCALE_QUALITY, "1")

This isn't _directly_ related to using images but it is handy to know. This will likely affect the quality of images you see, particularly moving images. I recommend you find time.Sleep(time.Millisecond * 10) near the end of the source code and adjust it to time.Sleep(time.Millisecond * 100) - this will slow down the rotation of the stick figure and allow you to better see the image quality differences between sdl.SetHint(sdl.HINT_RENDER_SCALE_QUALITY, "0") and sdl.SetHint(sdl.HINT_RENDER_SCALE_QUALITY, "1") or sdl.SetHint(sdl.HINT_RENDER_SCALE_QUALITY, "2") Bottom line: unless you are going for a "pixel art" graphics style you should avoid quality "0" and use at least quality "1" instead. **[The official documents on this](https://wiki.libsdl.org/SDL_HINT_RENDER_SCALE_QUALITY?highlight=%28%5CbCategoryDefine%5Cb%29%7C%28CategoryHints%29)** may be of some value.

The process of loading an image goes something like this:

 	
  * use img.Load() to load file into a surface

 	
  * CreateTextureFromSurface() takes surface and turns it into texture

 	
  * Free() the surface from memory, you don't need it anymore


I won't be going into Copy() and CopyEx() to draw the image onto the screen. It is heavily commented in the source code and definitely worth taking the time to experiment with the functions.

Eventually you will want to Destroy() any textures you have created and call img.Quit() in addition to the other sdl shutdown commands for a good program termination.

One thing worth pointing out is that the image used in the example (stick.png) has a transparent background as part of the file (one of the nice advantages of png.) At no point in the code did I need to select a color to make transparent - it is all automatic. Since this program expects a png file to load, you will need to get the stick.png file from the github link in addition to the usual Go source code file.


* * *

[**Lesson 3 source code**](https://github.com/MarkKeeley/Go-SDL2-Lessons/tree/master/Lesson03)
