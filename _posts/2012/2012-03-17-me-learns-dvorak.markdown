---
date: '2012-03-17 00:53:13'
layout: post
slug: me-learns-dvorak
status: publish
title: "/me learns dvorak"
wordpress_id: '1056'
categories:
- Linux
tags:
- dvorak
- typing
description: "Switching to dvorak was an interesting adventure. I got a little bored and decided to learn dvorak this week. I can type at a slow as molasses speed of about 11 WPM :( I'm painfully typing this whilst I justify learning dvorak not only to preoccupy my time, but also to become more efficient at typing."
---

[![AOEU](https://farm4.staticflickr.com/3599/3347936935_9283eb4126.jpg)](http://www.flickr.com/photos/fsk141/3347936935/)

I got a little bored and decided to learn dvorak this week. I can type at a slow as molasses speed of about 11 WPM :( I'm painfully typing this whilst I justify learning dvorak not only to preoccupy my time, but also to become more efficient at typing.

How does this relate to linux you might ask? Well I have a nice tie in to help you switch keyboard layouts fast and easily. Also tell you about a great typing tutorial program.


# setxkbmap


setxkbmap is a simple yet very useful program. It's used to change keyboard layouts, and works like this:

	setxkbmap us # set us layout
	setxkbmap dvorak# set dvorak layout

I also like having a shortcut alias in my ~/.zshrc

	alias us='setxkbmap us'
	alias dvorak='setxkbmap dvorak'

To view all of the layouts that you have take a look at `/usr/share/kbd/keymaps/i386/<layout>/*.gz` ; where layout like dvorak:

	├── dvorak
	│   ├── ANSI-dvorak.map.gz
	│   ├── dvorak-fr.map.gz
	│   ├── dvorak-l.map.gz
	│   ├── dvorak.map.gz
	│   ├── dvorak-r.map.gz
	│   ├── dvorak-ru.map.gz
	│   └── no-dvorak.map.gz



# dvorak7min



This program is an excellent typing tutorial for those of you looking to learn dvorak. Simply start up the program, and pick a lesson:

[![](http://fsk141.com/uploads/2012/03/2012-03-17-004651_624x392_scrot.png)](http://fsk141.com/uploads/2012/03/2012-03-17-004651_624x392_scrot.png)

The interface has a toggleable osd keyboard that can be shown to encourage touch typing.

[![](http://fsk141.com/uploads/2012/03/2012-03-17-004411_623x390_scrot.png)](http://fsk141.com/uploads/2012/03/2012-03-17-004411_623x390_scrot.png)
