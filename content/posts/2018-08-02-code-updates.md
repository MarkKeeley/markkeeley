+++

Author = "Mark Keeley"
type = "posts"
title = "We await the coming of Vgo!"
description = "The Scourge of Carpathia, The Sorrow of Moldavia, Vigo the Carpathian, Vigo the Cruel, Vigo the Torturer, Vigo the Despised, Vigo the Unholy"
draft = false
tags = ["Go / Golang", "SDL2"]
topics = []
comments = "false"
slug = ""
date = 2018-08-02T01:14:05-05:00

+++
_"On a mountain of skulls, in the castle of pain, I sat on a throne of blood! What was will be! What is will be no more! Now is the season of EVIL!"_ - Vigo
![Vigo the Carpathian](/media/vigo.jpg)

Okay, so I'm a fan of OG [Ghostbusters](https://www.imdb.com/title/tt0087332/?ref_=fn_al_tt_2) - even [Ghostbusters II](https://www.imdb.com/title/tt0097428/?ref_=fn_al_tt_4) and I do like this [Etsy Canvas Print](https://www.etsy.com/listing/214553292/vigo-the-carpathian-canvas-print/) - but I don't like it enough to pay $160. At the time of writing this it shows there's only 1 remaining so you may want to act fast  if you do want it.

Sadly, this post isn't actually about [Vigo](http://ghostbusters.wikia.com/wiki/Vigo) (the Ghostbusters antagonist) but about [Vgo](https://github.com/golang/go/wiki/vgo) (Versioned Go modules.) A long standing weakness of the Go programming language has been managing dependencies. Using "go get" would grab the latest version of a library to import, which isn't always what you would want. Libraries you used would introduce breaking changes (intentionally or otherwise) and suddenly your program wouldn't work on other computers (or yours if you updated.) There were a few attempts to fix this like 'dep' and a far too late effort to get Go library writers to use git tags as a band aid solution. Ultimately, the answer sadly came down to making copies of all your dependencies in private code repository (like Google) or vendoring copies of dependencies in a folder with your own code. 

With the upcoming release of [Go 1.11](https://tip.golang.org/doc/go1.11) this problem might finally have a solution. Go 1.11 will feature an experimental versioning system called VGo. Version 1.11 isn't out yet but you can download the Beta 2 version to help test it out. 

So with Vgo in the news I decided to take a look at my [Go-SDL2 code](http://localhost:1313/tags/sdl2/) for the first time in probably a year. And much to my dismay, the code didn't work. [Even Go-SDL2](https://github.com/veandco/go-sdl2/), the wonderfully stable, boring, thin Go wrapper around SDL2, had code breaking changes. These weren't even changes to fix bugs, they were mostly just name changes for the sake of style. What's worse is reading the [release page](https://github.com/veandco/go-sdl2/releases) it seems even more code breaking for the sake of style are planned. When even Go-SDL2 is breaking code, VGo cannot arrive soon enough.

I've updated all the Go-SDL2 code on this site and Github to work with the current version of Go-SDL2. I've even used this to test out [Hugo's](http://gohugo.io/) improved code syntax highlighter. Earlier versions of Hugo used [Pygments](http://pygments.org/) to highlight code and it was SLOW. Even with the limited number of posts that had Go code, pygments slowed things down. Fortunately newer versions of Hugo use [Chroma](https://gohugo.io/content-management/syntax-highlighting/), which seems every bit as performance driven as Hugo.

<!--more-->
