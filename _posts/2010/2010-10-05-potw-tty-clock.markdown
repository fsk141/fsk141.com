---
date: '2010-10-05 20:19:23'
layout: post
slug: potw-tty-clock
status: publish
title: '[PotW] tty-clock'
wordpress_id: '399'
categories:
- Software
tags:
- cli
- clock
- console
- terminal
- tty-clock
---

[![tty-clock_small](http://farm5.static.flickr.com/4112/5056275792_53c5a1e325.jpg)](http://www.flickr.com/photos/68444690@N00/5056275792/)


**Program:** tty-clock

**Site Link:** [http://github.com/xorg62/tty-clock](http://github.com/xorg62/tty-clock)

**Download:**



	
  1. 'git clone http://github.com/xorg62/tty-clock.git'

	
  2. download the [branch master](http://github.com/xorg62/tty-clock/archives/master) tarball/zip


**Installation:**



	
  1. use your package manager (yaourt -S tty-clock for archlinux)

	
  2. download ^^ & compile from source >>


**Compile:**

{% highlight bash %}
make
sudo make install
tty-clock
{% endhighlight %}

**Use it!:**

{% highlight bash %}
usage : tty-clock [-sctrvih] [-C [0-7]] [-f format]
<pre>    -s            Show seconds
    -c            Set the clock at the center of the terminal
    -C [0-7]      Set the clock color
    -t            Set the hour in 12h format
    -r            Do rebound the clock
    -f format     Set the date format
    -v            Show tty-clock version
    -i            Show some info about tty-clock
    -h            Show this page
{% endhighlight %}

**Screenshots:**

**[![tty-clock](http://farm5.static.flickr.com/4083/5055588265_bd9b8471de.jpg)](http://www.flickr.com/photos/68444690@N00/5055588265/)
**
