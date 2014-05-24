---
date: '2009-11-25 22:10:00'
layout: post
slug: cli-arduino-recover-from-epic-failure
status: publish
title: CLI Arduino | Recover from epic failure
wordpress_id: '223'
categories:
- Linux
tags:
- arduino
- cliard
- grub
- mount
---

I finally got the command line arduino interface working, and just need to think of a way to package it up... It's a program by Kimmo Kulovesi ([http://www.cs.helsinki.fi/u/kkuloves/](http://www.cs.helsinki.fi/u/kkuloves/)). The thing is that it works, but it seems like a big hack :). It's a giant shell script.

If your interested in a pre-packaged release then take a look at the two files below. It should be a pretty straightforward drop in. I put both files in ~/.scripts & "alias ardmake='~/.scripts/ardmake' I also extracted Arduino 0017 into ~/ . I plan to refine the program (basically make my own program), and package everything up into a nifty little package. More to come soon.

[http://github.com/fsk141/scripts/blob/master/ardmake](http://github.com/fsk141/scripts/blob/master/ardmake)

[http://github.com/fsk141/scripts/blob/master/dtr](http://github.com/fsk141/scripts/blob/master/dtr)

------

I was editing my /etc/rc.sysinit recently and epically borked it. Good thing I made a backup :) Anyway it's almost too easy to recover from epic non-boot failure.

1) Boot grub & hit 'e' (to edit)



	
  * Add S (or the full word Single) to the end of your kernel line

	
  * Hit enter then 'b' (to boot your edited entry


2) You will be dropped into single user mode (after you type your root password)

	
  * 'mount -o remount,rw /' (to mount / with read/write permissions)

	
  * fix what you broke, and reboot...


