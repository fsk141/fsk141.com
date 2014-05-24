---
date: '2011-04-18 04:42:14'
layout: post
slug: remove-kde-from-backtrack
status: publish
title: Remove KDE from Backtrack
wordpress_id: '616'
categories:
- Linux
---

[![Remove KDE](http://farm6.static.flickr.com/5105/5631126420_7c682f52c3.jpg)](http://www.flickr.com/photos/68444690@N00/5631126420/)

I just setup an awesome chroot on my arch box for bt4 (instructions to follow, this is just a quickie. Anywho, I'm getting rid of the flac. KDE is clearly the first flac to be pruned. I couldn't find anything on the internets explaining how to go about it, but this seems to work well; something I whipped up real quick...

    
    apt-get remove `dpkg --get-selections | grep kde | sed -e '/deinstall/d' | awk ' { print $1 } '`
