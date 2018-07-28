---
author: Mark Keeley
topics:
- Programming
comments: false
date: 2016-06-20T20:47:06Z
slug: go-sdl2-lesson-2
tags:
- Go / Golang
- SDL2
title: Go SDL2 Lesson 2
description: Moving rectangles
---

Okay, so a change of plans. It is 2016 and anyone who has ever done a "Hello World" in any language has a [Github](https://github.com/) account so I might as well make use of [**mine**](https://github.com/MarkKeeley). This means I can simply point to the source code on my github account and people can enjoy proper formatting and nice color keywords without me trying to fight wordpress or find a decent wordpress code plugin and praying no update suddenly breaks it or worrying about trying to stuff formatted code in a responsive wordpress theme. Instead I'll heavily comment code and use this page for a higher level overview.

<!--more-->

[**Lesson 2 source code**](https://github.com/MarkKeeley/Go-SDL2-Lessons/blob/master/Lesson02/lesson02.go)


 Lesson 1 was about using sdl2 to create a window and draw something on it. Lesson 2 adds event handling so programs can be exited by user input (escape key or window close button) and adds hardware acceleration. Lesson 1 used window.GetSurface() to create a screenSurface to draw on. screenSurface wasn't hardware accelerated so it has been replaced with sdl.CreateRenderer.  A sdl.Renderer uses the hardware accelerated api available to you (DirectX, OpenGL, OpenGL ES) and gives you a cross platform way to draw graphical primitives (rectangles/lines/points) and images to the screen quickly.


Lesson 2 also introduces a bit of movement to the screen to show the importance of calling renderer.Clear() - which clears the screen to the color specified by renderer.SetDrawColor. Be sure to comment out renderer.Clear and watch what happens to the blue rectangle when you don't clear the screen each frame.


![Go SDL2 Lesson 2](/media/lesson02.png)


* * *


[**Lesson 2 source code**](https://github.com/MarkKeeley/Go-SDL2-Lessons/blob/master/Lesson02/lesson02.go)
