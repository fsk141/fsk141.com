---
date: '2011-10-03 19:50:31'
layout: post
slug: fix-fatal-kernel-too-old-arch-linux-vps
status: publish
title: '[fix] FATAL: kernel too old (Arch Linux VPS)'
wordpress_id: '766'
categories:
- Linux
tags:
- alienvps
- Arch Linux
- buyvm
- glibc
- vps
---

Just got a shiny new vps with [AlienVPS](http://alienvps.com) after waiting way to long for [BuyVM](http://buyvm.net) to get in some stock. Anywho, I booted it up, installed Arch Linux, went to update, and BLAM! got a little broken VPS with the error: *FATAL: kernel too old*. Well I've come across this before, and it normally has to do with the Host Node being less than 2.6.18 kernel &amp; the VPS not being happy when you try to update glibc.

{% highlight bash %}
#old kernel support needs to be compiled into glibc
 --enable-kernel=2.6.18 --disable-profile
{% endhighlight %}

Anywho, the after trying to get the package &amp; update with a pacman -Su; Ignoring glibc &amp; toolchain in pacman.conf, and some other tomfoolery, I remembered that I can just add a nice friend of the VPS world, and get everything golden.

The Fix:



	
  1. Open up /etc/pacman.conf with a text editor (vi/vim/nano)

	
  2. Add the following **ABOVE CORE REPO!**:{% highlight bash %}
[glibc-vps]
Server = http://dev.archlinux.org/~ibiru/openvz/glibc-vps/$arch # where arch == i686 or x86_64
{% endhighlight %}

	
  3. Run pacman -Syu

	
  4. Do the following outlined here: [http://fsk141.com/fix-pty-allocation-request-failed-on-channel-0](http://fsk141.com/fix-pty-allocation-request-failed-on-channel-0)

	
  5. Sit back and enjoy your success...


Source (had to remember repo link): [https://wiki.archlinux.org/index.php/VPS_Repo](https://wiki.archlinux.org/index.php/VPS_Repo)
