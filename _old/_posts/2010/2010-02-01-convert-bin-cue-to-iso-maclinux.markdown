---
date: '2010-02-01 17:58:06'
layout: post
slug: convert-bin-cue-to-iso-maclinux
status: publish
title: Convert .bin/.cue to .iso [Mac/Linux]
wordpress_id: '250'
categories:
- Linux
tags:
- bchunk
- bin.cue
- iso
- Linux
- Mac
- macports
- unix
---

I dislike .bin/.cue combinations for a few reasons. But the biggest is that Mac doesn't natively support mounting them. Good thing I have macports to install linux programs.

Firstly lets install bchunk (use your favorite package manager, or [macports](http://www.macports.org/) on the mac)

{% highlight bash %}
# Mac Installation
sudo port install bchunk
{% endhighlight %}

Then locate your .bin/.cue combinations and CONVERT!

{% highlight bash %}
jg_mbp:CBT NUGGETS CISCO CCNA CCENT EXAM-PACK 640-822 ICND1 jgerold$ bchunk agcnccci.{bin,cue} CCENT
binchunker for Unix, version 1.2.0 by Heikki Hannikainen <hessu@hes.iki.fi>
 Created with the kind help of Bob Marietta <marietrg@SLU.EDU>,
 partly based on his Pascal (Delphi) implementation.
 Support for MODE2/2352 ISO tracks thanks to input from
 Godmar Back <gback@cs.utah.edu>, Colas Nahaboo <Colas@Nahaboo.com>
 and Matthew Green <mrg@eterna.com.au>.
 Released under the GNU GPL, version 2 or later (at your option).

Reading the CUE file:

Track  1: MODE1/2352    01 00:00:00

Writing tracks:

 1: CCENT01.iso  338/338  MB  [********************] 100 %
jg_mbp:CBT NUGGETS CISCO CCNA CCENT EXAM-PACK 640-822 ICND1 jgerold$ ls
CCENT01.iso     agcnccci.bin    agcnccci.cue
# ^Yay a new .iso
{% endhighlight %}



bchunk makes .bin/.cue > .iso conversion EASY!
