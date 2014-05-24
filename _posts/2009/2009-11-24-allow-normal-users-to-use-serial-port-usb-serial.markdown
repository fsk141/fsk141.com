---
date: '2009-11-24 15:40:13'
layout: post
slug: allow-normal-users-to-use-serial-port-usb-serial
status: publish
title: Allow Normal Users to Use Serial Port (USB Serial)
wordpress_id: '220'
categories:
- Linux
tags:
- arduino
- serial
- ttyS0
- uucp
---

OMG: [http://www.sparkfun.com/commerce/news.php?id=305](http://www.sparkfun.com/commerce/news.php?id=305) << Can't wait for Free Day, it's going to be epic.
Anywho, that's gotten me excited, and I pulled out my Arduino, to continue reading 'Getting Started With Arduino' I'm working along, and went to start up the 'Arduino 0017', and my serial port didn't work. Well I tried it via sudo, and it worked. So at first I added sudo to the '/usr/bin/arduino' executable. Yet this was very hackish, so I went looking for the solution.

{% highlight bash %}

gpasswd -a fsk141 uucp

{% endhighlight %}

Where 'fsk141' == your username... This will add you to the uucp group, where you can enjoy serial access. Woot!

Now I'm looking at how to ditch the Arduino IDE, and compile manually. It has something to do with a makefile, but I haven't gotten it to work yet... Updates to follow one I get CLI upload to the Arduino

_**Resources:**_



	
  * [http://wiki.archlinux.org/index.php/Arduino](http://wiki.archlinux.org/index.php/Arduino)


