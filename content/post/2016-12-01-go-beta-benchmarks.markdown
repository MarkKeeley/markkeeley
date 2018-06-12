+++
Author = "Mark Keeley"
comments = "false"
date = "2016-12-01T19:56:04-06:00"
description = "Using Go beta 1.8"
draft = false
slug = ""
tags = ["Go / Golang", "SDL2", "C++", "Benchmarks"]
title = "More Go SDL2 Benchmarks"
topics = ["Programming"]
type = "post"

+++

With the announced release of the Go compiler version 1.8 beta I thought it time to revisit the Go + SDL2 benchmarks. Particularly since [in the release notes](https://beta.golang.org/doc/go1.8#gc) it mentions

> "The overhead of calls from Go into C has been reduced by about half."

There's also [this Github issue](https://github.com/golang/go/issues/9704) showing that Go version 1.4 was where CGo overhead became nasty - a 3x performance decrease. The link also suggests that with 1.8 it is finally being addressed, so let's take a look. Time is measured in milliseconds (ms).

**An average of benchmarks from my desktop computer (less time is better)**

| # Rectangles  | Go beta 1.8   | Go 1.7      |
| ------------- | ------------- | ----------- |
| 10            | 0.4030985ms   | 0.4002203ms |
| 100           | 0.4177637ms   | 0.4175224ms |
| 1,000         | 1.1490982ms   | 1.6002346ms |
| 10,000        | 9.9492775ms   | 12.9985173ms|
| 100,000       | 82.2884138ms  | 121.952314ms|

------------------------------
**An average of benchmarks from my laptop (less time is better)**

| # Rectangles  | Go beta 1.8   | Go 1.7      |
| ------------- | ------------- | ----------- |
| 10            | 0.2882692ms   | 0.2972377ms |
| 100           | 0.3013944ms   | 0.3102127ms |
| 1,000         | 0.6096483ms   | 1.2503166ms |
| 10,000        | 5.5626756ms   | 8.7506485ms |
| 100,000       | 54.9406094ms  | 82.7094087ms|

<!--more-->

-------------------------------

Just as before, my desktop is an old Intel Core2 Quad CPU Q9300 @ 2.50GHz with an AMD 4670 graphics card.

```
***********
Go 1.8 beta
***********
=10 boxes=
405.534µs
405.343µs
394.46µs
402.32µs
405.914µs
401.789µs
403.375µs
408.294µs
397.931µs
406.025µs

=100 boxes=
411.406µs
417.978µs
415.455µs
415.485µs
416.749µs
418.648µs
421.826µs
415.416µs
421.449µs
423.225µs

=1000 boxes=
1.166739ms
1.139887ms
1.148647ms
1.15924ms
1.146983ms
1.137122ms
1.148518ms
1.153419ms
1.141753ms
1.148674ms

=10000 boxes=
9.614775ms
9.897048ms
8.356915ms
11.392998ms
10.392426ms
8.409605ms
8.443812ms
14.372582ms
8.871394ms
9.74122ms

=100000 boxes=
82.317284ms
82.35999ms
82.362017ms
82.419417ms
82.390328ms
82.306703ms
82.226408ms
82.187245ms
82.116696ms
82.19805ms
```

The laptop used in these benchmarks is an Intel Core i5-4200U CPU @ 1.60GHz and has an integrated 4400 graphics card.

```
**********
Go 1.8 beta
**********
=10 boxes=
309.879µs
367.879µs
216.124µs
295.467µs
268.832µs
284.095µs
279.479µs
282.671µs
291.414µs
286.852µs

=100 boxes=
301.139µs
296.806µs
317.762µs
291.937µs
303.484µs
300.208µs
298.546µs
302.161µs
302.435µs
299.466µs

=1000 boxes=
604.908µs
592.622µs
625.08µs
597.209µs
605.569µs
648.889µs
588.973µs
589.311µs
651.354µs
592.568µs

=10000 boxes=
5.077872ms
6.40837ms
5.059974ms
5.019638ms
6.254908ms
6.085182ms
5.115433ms
6.605422ms
5.029988ms
4.969969ms

=100000 boxes=
56.943097ms
53.877997ms
54.447624ms
53.006996ms
53.529224ms
55.795715ms
55.366334ms
55.812515ms
56.184784ms
54.441808ms
```

Keep in mind, this isn't the final release of the compiler, so things are subject to change. The official release of Go 1.8 is scheduled for February 2017.
