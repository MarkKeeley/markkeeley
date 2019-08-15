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

![Go SDL2 Lesson 3](/media/img/lesson03.png)

<!--more-->

From what I recall, all the old sdl version 1.2 tutorials I read years ago would always start with using:

> SDL_LoadBMP

But I'm not going to do that. I'm diving right into SDL_Image instead. The jpg/png combo beats bitmaps in every single way. Need a photograph? Jpg. Need a lossless graphic? Png. Plus png has the very useful option of transparency (which this lesson's graphic makes use of.) And most importantly, bitmaps have just disgustingly large file sizes (the format has no compression.)

Okay fine, if you _absolutely have to use_ a bitmap for some reason, the SDL_Image library still has you covered. According to its **[website](https://www.libsdl.org/projects/SDL_image/)**, SDL_Image supports 13 file formats (BMP, GIF, JPEG, LBM, PCX, PNG, PNM, TGA, TIFF, WEBP, XCF, XPM, XV.) Personally I've never even heard of 5 of them. The source code for [**Lesson 3 source code**](https://github.com/MarkKeeley/Go-SDL2-Lessons/tree/master/Lesson03) is also available on Github.

{{< highlight go "linenos=table" >}}
package main

import (
	"fmt"
	"os"
	"time"

	"github.com/veandco/go-sdl2/img"
	"github.com/veandco/go-sdl2/sdl"
)

const screenWidth = 640
const screenHeight = 480

func main() {

	err := sdl.Init(sdl.INIT_EVERYTHING)
	if err != nil {
		fmt.Fprintf(os.Stderr, "Failed to initialize sdl: %s\n", err)
		os.Exit(1)
	}

	window, err := sdl.CreateWindow("Go + SDL2 Lesson 3", sdl.WINDOWPOS_UNDEFINED, sdl.WINDOWPOS_UNDEFINED,
		screenWidth, screenHeight, sdl.WINDOW_SHOWN)
	if err != nil {
		fmt.Fprint(os.Stderr, "Failed to create renderer: %s\n", err)
		os.Exit(2)
	}

	renderer, err := sdl.CreateRenderer(window, -1, sdl.RENDERER_ACCELERATED)
	if err != nil {
		fmt.Fprint(os.Stderr, "Failed to create renderer: %s\n", err)
		os.Exit(3)
	}

	renderer.Clear()

	// Unnecessary preloading of jpg and png libraries. Can be commented out and program will automatically load
	// the correct library when you use "img.Load()"
	img.Init(img.INIT_JPG | img.INIT_PNG)

	// SUGGEST to sdl that it use a certain scaling quality for images. Default is "0" a.k.a. nearest pixel sampling
	// try out settings 0, 1, 2 to see the differences with the rotating stick figure. Change the
	// time.Sleep(time.Millisecond * 10) into time.Sleep(time.Millisecond * 100) to slow down the speed of the rotating
	// stick figure and get a good look at how blocky the stick figure is at RENDER_SCALE_QUALITY 0 versus 1 or 2
	sdl.SetHint(sdl.HINT_RENDER_SCALE_QUALITY, "1")

	// Load the glorious programmer art stick figure into memory
	surfaceImg, err := img.Load("stick.png")
	if err != nil {
		fmt.Fprintf(os.Stderr, "Failed to load PNG: %s\n", err)
		os.Exit(4)
	}

	// This is for getting the Width and Height of surfaceImg. Once surfaceImg.Free() is called we lose the
	// ability to get information about the image we loaded into ram
	imageWidth := surfaceImg.W
	imageHeight := surfaceImg.H

	// Take the surfaceImg and use it to create a hardware accelerated textureImg. Or in other words take the image
	// sitting in ram and put it onto the graphics card.
	textureImg, err := renderer.CreateTextureFromSurface(surfaceImg)
	if err != nil {
		fmt.Fprintf(os.Stderr, "Failed to create texture: %s\n", err)
		os.Exit(5)
	}
	// We have the image now as a texture so we no longer have need for surface. Time to let it go
	surfaceImg.Free()

	var event sdl.Event
	isRunning := true
	// used for rotating stick figure
	var angle float64 = 0.0

	for isRunning {
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

		renderer.SetDrawColor(255, 255, 55, 255)
		renderer.Clear()

		// Draw the first stick figure using the simpler Copy() function. First parameter is the image we want to draw on
		// screen. Second parameter is the source sdl.Rect of what we want to draw. In this case we instead pass nil, a shortcut
		// telling sdl to draw the entire image. You could use a sdl.Rect to specify drawing only a part of the image - especially
		// useful for animation.
		//
		// The third parameter speficies where on the screen the image will go (X & Y) and how large/small it will be. Alter the
		// 50's to grow or shrink the width and height as desired - or use imageWidth and imageHeight instead to use the normal
		// size of the image.
		renderer.Copy(textureImg, nil, &sdl.Rect{0, 0, 50, 50})

		// Make the second stick figure rotate
		angle += 1.0
		if angle > 360.0 {
			angle = 0
		}

		// A different way of drawing onto the screen with more options. The first 3 parameters are the same. The fourth
		// parameter is angle of degrees - use 0 if you don't want the image angled.
		//
		// The fifth parameter is to specify a point that the image rotates around. We use nil to use the default
		// Width / 2 and Height / 2 (vertical and horizontal center of image)
		//
		// The Last parameter is the RenderFlip setting. Do you want your image looking normal? Use sdl.FLIP_NONE
		// Do you want your image looking the other way? sdl.FLIP_HORIZONTAL
		// Do you want your image upside down? sdl.SDL_FLIP_VERTICAL
		// Do you want your image upside down AND looking the other way? sdl.FLIP_HORIZONTAL | sdl.SDL_FLIP_VERTICAL
		renderer.CopyEx(textureImg, nil, &sdl.Rect{200, 200, imageWidth, imageHeight}, angle, nil, sdl.FLIP_HORIZONTAL)

		renderer.Present()

		time.Sleep(time.Millisecond * 10)

	}

	// free the texture memory
	textureImg.Destroy()
	// we may or may not use img.Init(), but it's good form to properly shut down the sdl_image library
	img.Quit()
	renderer.Destroy()
	window.Destroy()

	sdl.Quit()
}
{{< / highlight >}}

> img.Init(img.INIT_JPG | img.INIT_PNG)

This is an interesting call because it is entirely optional. Unlike sdl.Init you can (and should) comment out this line of code and run the program. Go ahead, I'll wait. See the program ran just like before. If you don't call img.Init it will automatically init itself when you use it to load an image. SDL_Image tries to be very efficient and so it will dynamically load the needed libraries for jpg/png/tiff/webp only when needed. Using img.Init allows you to preload all the needed image libraries at the start of a program. Presumably this is for error checking and preventing a stutter from happening when suddenly the program loads an image in a file type it hadn't previously dealt with (and thus had to dynamically load the library for it.) It's worth noting that in the source code img.Init preloads both jpg and png with the same call. The program doesn't use jpg at any point, I just wanted to demonstrate using | to use multiple flags on the same line of code.

> sdl.SetHint(sdl.HINT_RENDER_SCALE_QUALITY, "1")

This isn't _directly_ related to using images but it is handy to know. This will likely affect the quality of images you see, particularly moving images. I recommend you find time.Sleep(time.Millisecond * 10) near the end of the source code and adjust it to time.Sleep(time.Millisecond * 100) - this will slow down the rotation of the stick figure and allow you to better see the image quality differences between sdl.SetHint(sdl.HINT_RENDER_SCALE_QUALITY, "0") and sdl.SetHint(sdl.HINT_RENDER_SCALE_QUALITY, "1") or sdl.SetHint(sdl.HINT_RENDER_SCALE_QUALITY, "2") Bottom line: unless you are going for a "pixel art" graphics style you should avoid quality "0" and use at least quality "1" instead. **[The official documents on this](https://wiki.libsdl.org/SDL_HINT_RENDER_SCALE_QUALITY?highlight=%28%5CbCategoryDefine%5Cb%29%7C%28CategoryHints%29)** may be of some value.

The process of loading an image goes something like this:

 	
  * use img.Load() to load file into a surface

 	
  * CreateTextureFromSurface() takes surface and turns it into texture

 	
  * Free() the surface from memory, you don't need it anymore

Eventually you will want to Destroy() any textures you have created and call img.Quit() in addition to the other sdl shutdown commands for a good program termination.

One thing worth pointing out is that the image used in the example (stick.png) has a transparent background as part of the file (one of the nice advantages of png.) At no point in the code did I need to select a color to make transparent - it is all automatic. For this lesson you will need a png file. You can use my stick figure (found on [Github](https://github.com/MarkKeeley/Go-SDL2-Lessons/tree/master/Lesson03), or use a png file of your own (ideally one with transparency.)

**July 31, 2018 Update:** Updated code to deal with breaking changes introduced to Go-SDL2 by [version 0.3](https://github.com/veandco/go-sdl2/releases).
