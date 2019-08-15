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

[**Lesson 2 source code**](https://github.com/MarkKeeley/Go-SDL2-Lessons/blob/master/Lesson02/lesson02.go)


Lesson 1 was about using sdl2 to create a window and draw something on it. Lesson 2 adds event handling so programs can be exited by user input (escape key or window close button) and adds hardware acceleration. Lesson 1 used window.GetSurface() to create a screenSurface to draw on. screenSurface wasn't hardware accelerated so it has been replaced with sdl.CreateRenderer.  A sdl.Renderer uses the hardware accelerated api available to you (DirectX, OpenGL, OpenGL ES) and gives you a cross platform way to draw graphical primitives (rectangles/lines/points) and images to the screen quickly.


Lesson 2 also introduces a bit of movement to the screen to show the importance of calling renderer.Clear() - which clears the screen to the color specified by renderer.SetDrawColor. Be sure to comment out renderer.Clear and watch what happens to the blue rectangle when you don't clear the screen each frame.


![Go SDL2 Lesson 2](/media/img/lesson02.png)

<!--more-->

{{< highlight go "linenos=table" >}}
package main

import "fmt"
import "os"
import "time"
import "github.com/veandco/go-sdl2/sdl"

const screenWidth = 640
const screenHeight = 480

func main() {

	err := sdl.Init(sdl.INIT_EVERYTHING)
	if err != nil {
		fmt.Fprintf(os.Stderr, "Failed to initialize sdl: %s\n", err)
		os.Exit(1)
	}

	window, err := sdl.CreateWindow("Go + SDL2 Lesson 2", sdl.WINDOWPOS_UNDEFINED, sdl.WINDOWPOS_UNDEFINED,
		screenWidth, screenHeight, sdl.WINDOW_SHOWN)
	if err != nil {
		fmt.Fprint(os.Stderr, "Failed to create renderer: %s\n", err)
		os.Exit(2)
	}

	renderer, err := sdl.CreateRenderer(window, -1, sdl.RENDERER_ACCELERATED)
	if err != nil {
		fmt.Fprint(os.Stderr, "Failed to create renderer: %s\n", err)
		os.Exit(2)
	}
	renderer.Clear()

	var event sdl.Event
	isRunning := true
	var xpos int32 = 0
	var movement int32 = 1

	// main loop
	for isRunning {
		// handle events, in this case escape key and close window
		for event = sdl.PollEvent(); event != nil; event = sdl.PollEvent() {
			switch t := event.(type) {
			case *sdl.QuitEvent:
				isRunning = false
			case *sdl.KeyboardEvent:
				if t.Keysym.Sym == sdl.K_ESCAPE {
					isRunning = false
				}
			}
		}
		// set color black
		// red, green, blue, alpha (alpha determines opaque-ness - usually 255)
		renderer.SetDrawColor(0, 0, 0, 255)
		// clear the window with specified color - in this case black.
		// renderer.Clear() 's purpose might not be clear when things are stationary,
		// but comment out renderer.Clear() and watch what happens to the moving blue
		// rect without it
		renderer.Clear()
		renderer.SetDrawColor(0, 255, 0, 255)
		renderer.FillRect(&sdl.Rect{0, 0, 200, 200})
		renderer.SetDrawColor(0, 0, 255, 255)

		// quick and dirty method to keep the blue rect bouncing inside the window
		xpos = xpos + movement
		// explicitly type cast screenWidth into a type int32 so it can be used
		// for positioning code
		if (xpos + 100) >= int32(screenWidth) {
			movement = -1
		}
		if xpos < 0 {
			movement = 1
			xpos = 0
		}
		renderer.DrawRect(&sdl.Rect{xpos, 300, 100, 80})

		// The rects have been drawn, now it is time to tell the renderer to show
		// what has been draw to the screen
		renderer.Present()

		// quick and dirty way to do animation without taking into account how much
		// time has passed or that different computers will run at different speeds
		// and therefore the blue rect might be moving too fast on some computers to
		// see it, ruining the demonstration. Change the time value to experiment with
		// different blue rect speeds
		time.Sleep(time.Millisecond * 10)

	}

	renderer.Destroy()
	window.Destroy()

	sdl.Quit()
}
{{< / highlight >}}

[**Lesson 2 source code**](https://github.com/MarkKeeley/Go-SDL2-Lessons/blob/master/Lesson02/lesson02.go) is also available on Github.

**July 31, 2018 Update:** I have updated the code to address the breaking changes that [version 0.3](https://github.com/veandco/go-sdl2/releases) of Go-SDL2 brought. 
