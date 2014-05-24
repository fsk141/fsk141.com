---
date: '2010-09-06 22:41:21'
layout: post
slug: set-mac-os-x-background-wallpaper-via-cli
status: publish
title: Set Mac OS X background (wallpaper) via cli
wordpress_id: '321'
categories:
- Mac
tags:
- cli
- Mac
- OS X
- Wallpaper
---

This was a hard one to figure out. I first tried a method outlined here [http://thingsthatwork.net/index.php/2008/02/07/fun-with-os-x-defaults-and-launchd](http://thingsthatwork.net/index.php/2008/02/07/fun-with-os-x-defaults-and-launchd), and that didn't work to well & you had to kill the Dock every time you set a background.

Well after much digging through google I found out that you can use osascript to set your background. I wrote a little osascript, and low and behold a way to change my background via cli:

{% highlight bash %}

osascript -e "tell application "System Events" to set picture of every desktop to "<path>/image.jpg""

{% endhighlight %} 
