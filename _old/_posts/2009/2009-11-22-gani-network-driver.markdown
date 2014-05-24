---
date: '2009-11-22 02:57:36'
layout: post
slug: gani-network-driver
status: publish
title: Gani Network Driver
wordpress_id: '206'
categories:
- Networking
---

Earlier today I came across a giant snag with my new NAS. After about 3GB of network throughput (under high load), the network stack would quit, and meh...
So I went to my trusty friend Google (how did people ever live without this), and ended up finding out that Masayuki Murayama's 'gani' driver works great, and have had my network at heavy load for the past 6 or so hours with no problems what-so-ever... 

You can get all his drivers here: [http://homepage2.nifty.com/mrym3/taiyodo/eng/](http://homepage2.nifty.com/mrym3/taiyodo/eng/)

This is a valuable link, I have used his drivers before for my first board in this ever stressful NAS setup. Anywho the install was very straightforward (make, make install, reboot), and everything is working wonderfully ever since.
