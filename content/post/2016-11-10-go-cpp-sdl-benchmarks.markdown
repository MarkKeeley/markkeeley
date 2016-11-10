+++
Author = "Mark Keeley"
comments = "false"
date = "2016-11-10T13:49:44-06:00"
description = "Go vs C++ SDL2 Performance"
draft = false
tags = ["Go / Golang", "SDL2", "C++", "Benchmarks"]
title = "Go SDL2 Benchmarks"
topics = ["Programming"]
type = "post"
slug = ""

+++

I've been messing with [Go](http://golang.org) and the [SDL2](https://github.com/veandco/go-sdl2) binding off and on for years now, but I never wrote down any benchmark tests that I did. I'm regretting that now, it would have been nice to track improved performance of Go over the years. Unfortunately I have no recorded data to compare it, but it is striking to see how well the 1.7 Go compiler handles the garbage collector. In the past Go performance was limited by the garbage collector periodically jumping in with the "stop the world" phase. Sure, there are methods to reduce impact (limiting object creation/deletion, object pools) but no matter what you would get hammered when the garbage collector froze the program. Recent versions of Go have made the garbage collector phase so much less punishing. 

I have the benchmark details farther down in the post, but first two short summaries. The tests measure how long (in milliseconds) it takes for the programs to render X number of rectangles on the screen using SDL2. Keep in mind that for a program to run at 60 Frames Per Second it must do so in approx. 16 milliseconds. At 30 Frames Per Second it must be approx. 33 milliseconds. Since I don't have any previous benchmarks to compare Go 1.7 to I thought it would be interesting to compare its performance to a similar program written in C++.

**An average of benchmarks from my desktop computer (less time is better)**

| # Rectangles  | C++           | Go          |
| ------------- | ------------- | ----------- |
| 10            | 0.3963521ms   | 0.4002203ms |
| 100           | 0.4165391ms   | 0.4175224ms |
| 1,000         | 0.5797817ms   | 1.6002346ms |
| 10,000        | 3.554465ms    | 12.9985173ms|
| 100,000       | 25.00806ms    | 121.952314ms|

------------------------------
**An average of benchmarks from my laptop (less time is better)**

| # Rectangles  | C++           | Go          |
| ------------- | ------------- | ----------- |
| 10            | 0.2921746ms   | 0.2972377ms |
| 100           | 0.3251477ms   | 0.3102127ms |
| 1,000         | 0.3811346ms   | 1.2503166ms |
| 10,000        | 1.950219ms    | 8.7506485ms |
| 100,000       | 15.1074ms     | 82.7094087ms|

<!--more-->

------------------------

Looking at the results I can't help but think that the garbage collector has become a very small performance hindrance, but the overhead from making CGO calls adds up very fast. CGo has long been known as relatively slow and these benchmarks show how that starts to kick in at 1,000 rectangles.

![Picture of benchmark program](/media/sdl2benchmarktest.png "Benchmark program with 100 rectangles")

First off, [the source files for the tests](/media/sdltests.zip). Both the Go & C++ programs are set to test with 100 boxes. To change the number of boxes you will need to alter the for loops (for C++ in main.cpp and for Go in application.go.)

With Go (version 1.7.1) I used the standard compiler commands:
```
go build *.go
```
With C++ (gcc version 6.2.0 20161005 (Ubuntu 6.2.0-5ubuntu12):
```
g++ main.cpp -std=c++14 -lSDL2 -O2 -o filename
```

Okay, time for the not averaged results. My desktop is an old Intel Core2 Quad CPU Q9300 @ 2.50GHz with an AMD 4670 graphics card.


```
********************************
Desktop with C++
********************************
==============10 boxes ========
0.333354 ms
0.400406 ms
0.406296 ms
0.405378 ms
0.450913 ms
0.374105 ms
0.380829 ms
0.402206 ms
0.406057 ms
0.403977 ms

==============100 boxes ==========
0.415323 ms
0.419094 ms
0.425321 ms
0.416306 ms
0.418322 ms
0.410273 ms
0.414865 ms
0.417496 ms
0.415185 ms
0.413206 ms

==============1000 Boxes =========
0.545799 ms
0.578346 ms
0.582804 ms
0.582687 ms
0.585887 ms
0.580466 ms
0.583244 ms
0.58577 ms
0.584332 ms
0.588482 ms

================10000 Boxes ========
2.23993 ms
2.25013 ms
4.44348 ms
2.40282 ms
2.35446 ms
2.54453 ms
6.54055 ms
3.29681 ms
6.96199 ms
2.50995 ms

===============100000 Boxes =========
22.6267 ms
22.3682 ms
25.3389 ms
22.8278 ms
22.3182 ms
23.2125 ms
42.1277 ms
22.7506 ms
21.0538 ms
25.4562 ms

*****************************
Desktop with Go
****************************
===============10 Boxes ======
388.382µs
401.809µs
396.355µs
404.153µs
398.289µs
399.626µs
403.744µs
409.995µs
398.756µs
401.094µs

===============100 Boxes ======

419.371µs
415.344µs
419.862µs
412.585µs
419.488µs
415.733µs
415.314µs
420.301µs
418.432µs
418.794µs

================1000 Boxes =======
1.316431ms
1.332868ms
1.306373ms
1.310674ms
1.644223ms
1.564882ms
1.587554ms
2.829726ms
1.568733ms
1.540882ms

=================10000 Boxes =====
13.183428ms
12.692799ms
11.889217ms
11.977253ms
16.467113ms
13.593718ms
11.531742ms
14.715364ms
12.319642ms
11.614897ms

================100000 Boxes ===
121.672637ms
122.483229ms
122.093538ms
122.583879ms
122.136179ms
122.348391ms
121.62343ms
121.396209ms
121.754013ms
121.431635ms
```

The laptop used in these benchmarks is an Intel Core i5-4200U CPU @ 1.60GHz and has an integrated 4400 graphics card.

```
***************************
Laptop with C++
**************************
==========10 Boxes =======
0.304366 ms
0.304299 ms
0.306677 ms
0.295419 ms
0.298447 ms
0.293363 ms
0.285045 ms
0.281533 ms
0.281204 ms
0.271393 ms

==========100 Boxes =======
0.431869 ms
0.204399 ms
0.34622 ms
0.312939 ms
0.297589 ms
0.231969 ms
0.397206 ms
0.36182 ms
0.327425 ms
0.340041 ms

===========1000 Boxes =====
0.381604 ms
0.377685 ms
0.368124 ms
0.378868 ms
0.353749 ms
0.375721 ms
0.373941 ms
0.386789 ms
0.451024 ms
0.363841 ms

============10000 Boxes ====
2.368 ms
2.20093 ms
1.70634 ms
1.49063 ms
1.58963 ms
1.60123 ms
1.53591 ms
2.46688 ms
2.00849 ms
2.53418 ms

============100000 Boxes ==
14.4924 ms
15.1953 ms
15.6873 ms
14.9915 ms
14.9055 ms
15.334 ms
15.6173 ms
15.4903 ms
15.2485 ms
14.1119 ms

****************************
Laptop with Go
***************************
============10 Boxes ====
302.778µs
272.478µs
321.091µs
306.152µs
300.264µs
296.503µs
301.094µs
300.467µs
392.969µs
178.581µs

============100 Boxes ====
290.288µs
323.781µs
300.636µs
322.913µs
292.544µs
320.227µs
306.196µs
353.156µs
295.038µs
297.348µs

==============1000 Boxes =====
1.13933ms
1.340332ms
2.261644ms
1.344164ms
1.042639ms
962.64µs
941.348µs
936.445µs
973.012µs
1.561612ms

===============10000 Boxes ===
8.987828ms
9.127491ms
7.615318ms
9.039982ms
8.624624ms
9.198335ms
7.63453ms
9.189426ms
8.665179ms
9.423772ms

===============100000 Boxes ===
85.207424ms
82.710589ms
82.130984ms
82.187846ms
82.314294ms
82.029169ms
81.144697ms
84.567176ms
82.038978ms
82.76293ms
```

In case you are wondering, some of the Go results are listed in microseconds (µs) instead of milliseconds (ms). Just move the decimal place to the left 3 places to convert microseconds into milliseconds.
```
290.288µs equals 0.290288ms
```

