---
author: Mark Keeley
topics:
- Programming
comments: false
date: 2016-07-01T03:13:17Z
slug: go-sdl2-lesson-4
tags:
- Go / Golang
- SDL2
title: Go SDL2 Lesson 4
description: Working with fonts
---

While I was working on lesson 4 I noticed that the **[SDL2 bindings](https://github.com/veandco/go-sdl2)** had been updated since the last time I checked, and the update added a new **[example](https://github.com/veandco/go-sdl2-examples/blob/382efbbb8a866db862770c6b5a28f778c0749ce3/examples/text/text.go)** on font rendering. Fortunately for me, the example doesn't deal with hardware acceleration and only handles 1 of the 3 types of fonts so I'm not trashing this lesson. 

![Go SDL2 Lesson 4](/media/img/lesson04.png)

<!--more-->

It is worth remembering that the default for "go get" when pointed at a package is to add any files the local repository is missing and leave everything else alone. Use "go get -u" to also update local files. [**Lesson 4 source code (and font)**](https://github.com/MarkKeeley/Go-SDL2-Lessons/tree/master/Lesson04) is available on Github. The code is a bit different from the previous lessons, stuffing everything into a main() was getting to be a mess.

{{< highlight go "linenos=table" >}}
package main

import (
	"fmt"
	"os"
	"time"

	"github.com/veandco/go-sdl2/sdl"
	"github.com/veandco/go-sdl2/ttf"
)

const (
	screenWidth  = 640
	screenHeight = 480
)

// Globals - terrible for serious programs, great for short examples
var (
	window         *sdl.Window
	renderer       *sdl.Renderer
	event          sdl.Event
	quit           bool
	err            error
	solidTexture   *sdl.Texture
	blendedTexture *sdl.Texture
	shadedTexture  *sdl.Texture
)

func Setup() (successful bool) {
	err = sdl.Init(sdl.INIT_VIDEO)
	if err != nil {
		fmt.Printf("Failed to initialize sdl: %s\n", err)
		return false
	}
	// Using the SDL_ttf library so need to initialize it before using it
	if err = ttf.Init(); err != nil {
		fmt.Printf("Failed to initialize TTF: %s\n", err)
		return false
	}

	window, err = sdl.CreateWindow("Go + SDL2 Lesson 4", sdl.WINDOWPOS_UNDEFINED,
		sdl.WINDOWPOS_UNDEFINED, screenWidth, screenHeight, sdl.WINDOW_SHOWN)
	if err != nil {
		fmt.Printf("Failed to create renderer: %s\n", err)
		return false
	}

	renderer, err = sdl.CreateRenderer(window, -1, sdl.RENDERER_ACCELERATED)
	if err != nil {
		fmt.Printf("Failed to create renderer: %s\n", err)
		return false
	}

	sdl.SetHint(sdl.HINT_RENDER_SCALE_QUALITY, "1")

	return true
}

func Shutdown() {
	solidTexture.Destroy()
	shadedTexture.Destroy()
	blendedTexture.Destroy()
	ttf.Quit()
	renderer.Destroy()
	window.Destroy()
	sdl.Quit()
}

func HandleEvents() {
	for event = sdl.PollEvent(); event != nil; event = sdl.PollEvent() {
		switch t := event.(type) {
		case *sdl.QuitEvent:
			quit = true
		case *sdl.KeyboardEvent:
			if t.Keysym.Sym == sdl.K_ESCAPE {
				quit = true
			}
		}
	}
}

func Draw() {
	renderer.SetDrawColor(255, 255, 55, 255)
	renderer.Clear()

	// Draw the 3 text textures
	renderer.Copy(solidTexture, nil, &sdl.Rect{(screenWidth / 2) - 89, 60, 190, 53})
	renderer.Copy(shadedTexture, nil, &sdl.Rect{(screenWidth / 2) - 112, 130, 244, 53})
	renderer.Copy(blendedTexture, nil, &sdl.Rect{(screenWidth / 2) - 117, 200, 244, 53})

	renderer.Present()
}

func CreateFonts() (successful bool) {
	var font *ttf.Font

	if font, err = ttf.OpenFont("Roboto-Regular.ttf", 40); err != nil {
		fmt.Printf("Failed to open font: %s\n", err)
		return false
	}

	var solidSurface *sdl.Surface
	if solidSurface, err = font.RenderUTF8Solid("Solid Text", sdl.Color{255, 0, 0, 255}); err != nil {
		fmt.Printf("Failed to render text: %s\n", err)
		return false
	}

	if solidTexture, err = renderer.CreateTextureFromSurface(solidSurface); err != nil {
		fmt.Printf("Failed to create texture: %s\n", err)
		return false
	}

	solidSurface.Free()

	var shadedSurface *sdl.Surface
	if shadedSurface, err = font.RenderUTF8Shaded("Shaded Text",
		sdl.Color{0, 255, 0, 255}, sdl.Color{255, 0, 255, 255}); err != nil {
		fmt.Printf("Failed to render text: %s\n", err)
		return false
	}

	if shadedTexture, err = renderer.CreateTextureFromSurface(shadedSurface); err != nil {
		fmt.Printf("Failed to create texture: %s\n", err)
		return false
	}

	shadedSurface.Free()

	var blendedSurface *sdl.Surface
	if blendedSurface, err = font.RenderUTF8Blended("Blended Text", sdl.Color{0, 0, 255, 255}); err != nil {
		fmt.Printf("Failed to render text: %s\n", err)
		return false
	}

	if blendedTexture, err = renderer.CreateTextureFromSurface(blendedSurface); err != nil {
		fmt.Printf("Failed to create texture: %s\n", err)
		return false
	}

	blendedSurface.Free()
	font.Close()

	return true
}

func main() {
	// initialize sdl and create the window & renderer
	// if there's an error terminate the program
	if !Setup() {
		os.Exit(1)
	}

	// isolate most of the new code into a seperate func for reading
	//terminate program is there is an error
	if !CreateFonts() {
		os.Exit(2)
	}

	// No more calling time.Sleep! Create a ticker that will trigger up to 30 times a second
	// for a more consistent frame rate and animation speed. Obviously more useful in demos that
	// actually have movement. Still not as good as calculating delta time for consistent framerate
	// independent animation - but this is WAY more convenient!
	ticker := time.NewTicker(time.Second / 30)

	// New main loop. Broke events and drawing into seperate functions - it was getting unwieldy
	for !quit {
		HandleEvents()
		Draw()
		<-ticker.C // wait up to 1/30th of a second

	}
	// Self explainatory
	Shutdown()
}
{{< / highlight >}}

This lesson uses the SDL_tff library so a new import has been added.

> import "github.com/veandco/go-sdl2/ttf"

Much like SDL2's sdl.Init, to use SDL_tff you must first initialize it.

> ttf.Init()

Opens the true type font file and sets the size of the font type.

> ttf.OpenFont("Roboto-Regular.ttf", 40)

These 3 functions all render text to a surface. When programming in C/C++ SDL provides numerous different ways to input text and turn it into a surface. With Go there is only the [**UTF-8**](https://en.wikipedia.org/wiki/UTF-8) functions. Go is all about the UTF-8 - hardly surprising since the co-creators of UTF-8 ([**Rob Pike**](https://en.wikipedia.org/wiki/Rob_Pike) & [**Ken Thompson**](https://en.wikipedia.org/wiki/Ken_Thompson)) make up two of the three original creators of Go.

> font.RenderUTF8Solid

> font.RenderUTF8Shaded

> font.RenderUTF8Blended


 Once the newly created surface with text is created the program turns it into a hardware accelerated texture.

> renderer.CreateTextureFromSurface(solidSurface)

Once the text has been turned into a texture it is just like [**lesson 3**](http://markkeeley.us/2016/go-sdl2-lesson-3/) and dealing with images. All that's left is the clean up.

> solidSurface.Free()

Release the surface memory since it is no longer needed. It would be nice to be able to skip the surface creation and go straight to a texture but that is not an option with the SDL_tff library.

> font.Close()

When we no longer need the font *ttf.Font it is released from memory in the program.

> solidTexture.Destroy()

Like any texture, once we are done using the texture with the text it needs to be properly released.

>  ttf.Quit()

Time to properly shut down the SDL_tff library.

The glorious multicolored text program

So about those 3 types of text renderings (solid/shaded/blended).... Typically it goes:

**Sold**: The best performance but lowest quality, best to use on text that frequently changes such as fps counters

**Shaded**: Gives nicer anti-aliased text but is slower than Solid and has a box around it

**Blended**: Best quality, slowest performance.

**July 31, 2018 Update:** Updated code to deal with breaking changes introduced to Go-SDL2 by [version 0.3](https://github.com/veandco/go-sdl2/releases).
