---
author: Mark Keeley
categories:
- Go / Golang
comments: false
date: 2016-06-18
slug: go-sdl2-lesson-1
tags:
- Go / Golang
- SDL2
title: Go SDL2 Lesson 1
description: Creating a window
---

This post is about using the [**Go**](https://golang.org/) programming language with the Simple DirectMedia Layer library ([**SDL 2**](https://www.libsdl.org/)). This post assumes you have Go installed and have a working Go Workspace as outlined [**here**](https://golang.org/doc/install). I also assume you have the sdl library (including the necessary developer files!) installed for your platform. In order for Go to use sdl you must have the needed bindings that can be found [**here**](https://github.com/veandco/go-sdl2). Follow the directions on that page exactly. Many thanks to Veandco for creating the bindings that make this possible. The source code for [**Lesson 1**](https://github.com/MarkKeeley/Go-SDL2-Lessons/blob/master/Lesson01/lesson01.go).

Okay once everything is installed and setup it is time for lesson 1 - using sdl to create a window and draw two rectangles on the screen.

![Go SDL2 Lesson 1](/media/lesson1.png)

<!--more-->

_Gaze upon it's magnificent rectangularness!_


I won't be posting the code in this post because this lesson is too simple to need it and more importantly because dealing with formatting for source code inside wordpress is a royal pain. This window is created and lasts for five seconds.



> time.Sleep(time.Second * 5)


This program has no way of handling input so clicking the close button currently does nothing. Also worth paying attention to is that the two rectangles are created in slightly different ways. With the first (blue) one a rect is created and then placed into the FillRect function. The second (green) one has a rect created inside the FillRect function.


Lastly, if the first function in the FillRect function is nil instead of a rect then sdl will fill the entire surface area with the specified color.


> screenSurface.FillRect(nil, sdl.MapRGB(screenSurface.Format, 255, 0, 0))


This will turn the entire screen red.

* * *

[**Click here to see the lesson1.go source code**](https://github.com/MarkKeeley/Go-SDL2-Lessons/blob/master/Lesson01/lesson01.go "See the lesson 1 source code")
