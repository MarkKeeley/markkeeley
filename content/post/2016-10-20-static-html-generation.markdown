+++
Author = "Mark Keeley"
topics = ["Programming"]
comments = "false"
date = "2016-10-20T01:04:17-05:00"
description = "Why not just use WordPress?"
draft = false
slug = ""
tags = ["WordPress", "HTML"]
title = "Static HTML Generation"
type = "post"

+++

The last time I spent serious time working with web pages/HTML Internet Explorer 5.5 and 6 roamed the earth. Thanks to the lack of Cascading Style Sheet support, if you wanted to do layouts the only options were tables and frames. I became very good at nesting tables within tables within tables.

Thankfully things have changed since then. Microsoft browsers are no longer giants with 90% (or more) usage stats but sit at much less than 10% usage (even when combining Internet Explorer and Edge stats together.) CSS is THE way to format websites, allowing for a clean separation of content and appearance. HTML5 allows for better semantics and improves the situation for those with disabilities. JavaScript... is still an awful mess of a language. Okay, so some things were bound to remain the same.<!--more--> 

When I looked to get back into the web page world I started using [WordPress](https://wordpress.org/). Having used it, it comes as no surprise to me that WordPress has been the most used Content Management System for years. By some counts, WordPress powers up to 30% of sites on the web. Enormous amounts of time and effort were put into making the software so user friendly. Anyone who has used a word processor like Wordpad or Microsoft Word will feel right at home. 

For all its user friendliness, WordPress (and the other PHP based competitors) have some serious downsides. My biggest complaint must always be security. Hacked WordPress sites have been a reoccurring issue over the years. Due to the technology used to make it, it must always have more security risks. Yes, having WordPress auto update (if installed/configured correctly) helps, but when there is a security issue you and thousands of others may have already been hit before the problem is discovered, a fix is created, and your site updates to the latest version. Few WordPress sites run vanilla, the large plug-in ecosystem is also a security problem. Plug-ins are reviewed far less often (if ever) and even if a plug-in was good when you installed it, if it ever became compromised you would have a security hole without even knowing it. 

Stock WordPress has no meaningful defenses against brute force attacks. Maybe it is better at some web hosting places, but by default WordPress only has enforced strong passwords to defend itself. Automated systems constantly scanning the internet for sites that run WordPress, then trying brute force attacks to log into the Administrator account. Sure, they are unlikely to succeed in the short term, and probably not in the medium term either, but they are out there constantly attacking. 

It seems I wasn't alone in thinking there had to be better options. Static websites have made a comeback. By writing content in [markdown](https://infogalactic.com/info/Markdown) you have full separation of content and HTML. There is no database to deal with, so no having to worry about SQL backups. There's also no PHP processing to do, just send the static files - which makes performance FAST. 

Best of all, the security is great. There's nothing to log into so brute force attacks are irrelevant. No having to worry about plug-in security issues. And no constant updates due to the latest security problem.

So what's the downside? It's not quite as user friendly as WordPress. WordPress uses a What You See is What You Get interface while static HTML generators revolve around editing text files in markdown format. In addition, most static generators require at least some proficiency in command line usage, which limits the number of people that can use them. 

There's a surprisingly large number of static HTML generators to choose from. [Jekyll](http://jekyllrb.com/) seems to be the most prominent. I've been using [Hugo](http://gohugo.io/) due to my familiarity with the Go programming language. It has the advantage over Jekyll of being a Go program and so it is just a simple binary. At the time of writing this site is made with Hugo and uses a Hugo theme I created.
