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

While I was working on lesson 4 I noticed that the **[SDL2 bindings](https://github.com/veandco/go-sdl2)** had been updated since the last time I checked, and the update added a new **[example](https://github.com/veandco/go-sdl2/blob/master/examples/text/text.go)** on font rendering. Fortunately for me, the example doesn't deal with hardware acceleration and only handles 1 of the 3 types of fonts so I'm not trashing this lesson. 

![Go SDL2 Lesson 4](/media/lesson04.png)

<!--more-->

It is worth remembering that the default for "go get" when pointed at a package is to add any files the local repository is missing and leave everything else alone. Use "go get -u" to also update local files. [**Lesson 4 source code and font**](https://github.com/MarkKeeley/Go-SDL2-Lessons/tree/master/Lesson04). The code is a bit different from the previous lessons, stuffing everything into a main() was getting to be a mess.

> import "github.com/veandco/go-sdl2/sdl_ttf"

This lesson uses the SDL_tff library so a new import has been added.

> ttf.Init()

Much like SDL2's sdl.Init, to use SDL_tff you must first initialize it.

> ttf.OpenFont("Roboto-Regular.ttf", 40)

Opens the true type font file and sets the size of the font type.

> font.RenderUTF8_Solid

> font.RenderUTF8_Shaded

> font.RenderUTF8_Blended


These 3 functions all render text to a surface. When programming in C/C++ SDL provides numerous different ways to input text and turn it into a surface. With Go there is only the [**UTF-8**](https://en.wikipedia.org/wiki/UTF-8) functions. Go is ALL about the UTF-8 - hardly surprising since the co-creators of UTF-8 ([**Rob Pike**](https://en.wikipedia.org/wiki/Rob_Pike) & [**Ken Thompson**](https://en.wikipedia.org/wiki/Ken_Thompson)) make up two of the three original creators of Go. Once the newly created surface with text is created the program turns it into a hardware accelerated texture.

> renderer.CreateTextureFromSurface(solidSurface)

Once the text has been turned into a texture it is just like [**lesson 3**](http://markckeeley.com/2016/06/go-sdl2-lesson-3/) and dealing with images. All that's left is the clean up.

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

However I have to question some of this. Many of the performance concerns seem to hark back to the days of SDL1.x without the hardware acceleration focus. Looking at the SDL_tff website it seems as though very little was changed when SDL jumped to 2.x and it's hardware acceleration focus. I can certainly see the performance issues when dealing solely with surfaces, but since I am just turning the surfaces into textures the performance issues seem far less relevant. Things to further investigate - some day.


* * *


[**Lesson 4 source code**](https://github.com/MarkKeeley/Go-SDL2-Lessons/tree/master/Lesson04)
