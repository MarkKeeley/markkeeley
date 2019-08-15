---
author: Mark Keeley
topics:
- Programming
comments: false
date: 2016-06-18
slug: go-sdl2-lesson-1
tags:
- Go / Golang
- SDL2
title: Go SDL2 Lesson 1
description: Creating a window
---

This post is about using the [**Go**](https://golang.org/) programming language with the Simple DirectMedia Layer library ([**SDL 2**](https://www.libsdl.org/)). This post assumes you have Go installed and have a working Go Workspace as outlined [**here**](https://golang.org/doc/install). I also assume you have the sdl library (including the necessary developer files!) installed for your platform. In order for Go to use sdl you must have the needed bindings that can be found [**here**](https://github.com/veandco/go-sdl2). Follow the directions on that page exactly. Many thanks to Veandco for creating the bindings that make this possible. The source code for Lesson 1 can also be found [**on Github**](https://github.com/MarkKeeley/Go-SDL2-Lessons/blob/master/Lesson01/lesson01.go).

Okay once everything is installed and setup it is time for Lesson 1 - using SDL to create a window and draw two rectangles on the screen.

![Go SDL2 Lesson 1](/media/img/lesson1.png)

<!--more-->

_Gaze upon it's magnificent rectangularness!_

{{< highlight go "linenos=table" >}}
package main

//imported for logging if something went wrong
import "fmt"

//imported for logg
//imported to keep the windoing if something went wrong and to exit program if there was a problem
import "os"
import "time"

//imported to use sdl2
import "github.com/veandco/go-sdl2/sdl"

func main() {
	// try to initialize everything
	err := sdl.Init(sdl.INIT_EVERYTHING)
	if err != nil {
		fmt.Fprintf(os.Stderr, "Failed to initialize sdl: %s\n", err)
		os.Exit(1)
	}

	// try to create a window
	window, err := sdl.CreateWindow("Go + SDL2 Lesson 1", sdl.WINDOWPOS_UNDEFINED, sdl.WINDOWPOS_UNDEFINED,
		640, 480, sdl.WINDOW_SHOWN)
	if err != nil {
		fmt.Fprint(os.Stderr, "Failed to create renderer: %s\n", err)
		os.Exit(2)
	}

	// window has been created, now need to get the window surface to draw on window
	screenSurface, err := window.GetSurface()
	if err != nil {
		fmt.Fprint(os.Stderr, "Failed to create surface: %s\n", err)
		os.Exit(2)
	}

	// create the first rectangle (x position, y position, width, height)
	rect := sdl.Rect{300, 300, 100, 80}
	// draw the rect on the window surface and choose color based on r,g,b - in this case the color blue
	screenSurface.FillRect(&rect, sdl.MapRGB(screenSurface.Format, 0, 0, 255))

	// draw second rectangle (this one green) - demonstrates the fact your can create rects as needed inside function calls
	//
	// the first argument in FillRect wants a pointer and neither rect we have used was a pointer so the & was used in both cases
	// to provide the needed pointer
	screenSurface.FillRect(&sdl.Rect{0, 0, 200, 200}, sdl.MapRGB(screenSurface.Format, 0, 255, 0))

	// if nil is used as the first argument instead of a rect that tells sdl to draw the rect on the entire window surface area
	// uncomment the next line to see the entire window in red
	//screenSurface.FillRect(nil, sdl.MapRGB(screenSurface.Format, 255, 0, 0))

	// it is not enough to draw on the window surface, you must tell sdl to show what you've done
	window.UpdateSurface()

	// used to keep window open for five seconds
	time.Sleep(time.Second * 5)

	// program is over, time to start shutting down. Keep in mind that sdl is written in C and does not have convenient
	// garbage collection like Go does
	window.Destroy()

	sdl.Quit()
}
{{< / highlight >}}

This window is created and lasts for five seconds.

> time.Sleep(time.Second * 5)


This program has no way of handling input so clicking the close button currently does nothing. Also worth paying attention to is that the two rectangles are created in slightly different ways. With the first (blue) one a rect is created and then placed into the FillRect function. The second (green) one has a rect created inside the FillRect function.


Lastly, if the first function in the FillRect function is nil instead of a rect then sdl will fill the entire surface area with the specified color.


> screenSurface.FillRect(nil, sdl.MapRGB(screenSurface.Format, 255, 0, 0))


This will turn the entire screen red.
