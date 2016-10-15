+++
Author = "Mark Keeley"
topics = ["Programming"]
comments = "false"
date = "2016-09-16T08:12:18-05:00"
description = "Rectangle Collision Detection"
draft = false
slug = "go-sdl2-lesson5"
tags = ["Go / Golang", "SDL2"]
title = "Go SDL2 Lesson 5"
type = "post"

+++

Today is all about collision detection using SDL rectangles. This method does have problems dealing with very rounded images, but it has the benefit of being fast and easy to check if there has been a collision. In this example two filled in boxes (blue and green) are checked to see if they intersect/collide. When they do intersect, a third box (outlined and red) is drawn showing the location of where they collide.

![Go SDL2 Lesson 5](/media/lesson5gosdl2.png)

<!--more-->

The latest version of SDL (2.0.4) added a convenient [SDL_PointInRect](https://wiki.libsdl.org/SDL_PointInRect) function to check if a point collides with a rectangle, but at the moment, the Go SDL2 bindings do not support this. This would be handy for something like checking if the mouse is in a certain area (since in SDL mouse position is held in a point), but it is simple enough to create a mouse rectangle and update the mouse position to it.

As usual, the code is kept simple and heavily commented. Have fun.

* * * 

[**Lesson 5 source code**](https://github.com/MarkKeeley/Go-SDL2-Lessons/blob/master/Lesson05/lesson05.go)

