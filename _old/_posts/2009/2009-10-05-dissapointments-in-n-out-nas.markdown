---
date: '2009-10-05 01:47:43'
layout: post
slug: dissapointments-in-n-out-nas
status: publish
title: Dissapointments | In-N-Out NAS
wordpress_id: '83'
categories:
- Hardware
---

I was very letdown with the latest [FreeNAS](http://www.freenas.org) RC1 & RC2. I was hoping the ZFS would be usable, and all the services (ssh, ftp, nfs) would happily work. Well that wasn't the case. I was planning on using FreeNAS for my latest project the In-N-Out NAS. More on that after my FreeNAS rant... So before all this started I tested out FreeNAS a few months ago to figure out if it was a viable alternative to my current OpenSolaris headless machine. Well it didn't quite cut it because FreeNAS is has an uber old version of ZFS, and they don't plan to upgrade until they migrate to 8.0 release of FreeBSD. Well I don't feel like waiting that long, and everything is pretty sketchy so I'll stay with the sure thing (OpenSolaris).

Onto more pressing issues. I have been working hard on a new NAS box for my network, and came up with a few designs, most of which were too complicated for me to build. So I came up with a simple box design made out of metal (pics once I finish). Well I've been busy, and haven't gotten the metal bent yet, so I spent a few hours working on a working NAS setup so that I can move my data onto the new hardware. I decided to build the system from an In-N-Out tray that I "borrowed," indefinitely. It turned out very awesome, yet I have ran into problem after problem with this little beast. First the Pico PSU that I purchased was borked, so I need to get another one of those. Then the standin PSU started overheating cause the fan wouldn't go on. I threw that out, and replaced it with yet another PSU. Eh, then I was having FreeNAS install issues, and finally once it was installed, it works horribly. Plus I think I might have a sata cable problem, but I'm unsure as of now. I kinda just replaced all my nice 6" cables with "reliable" 12" standard red ones.

My predicament at the moment is to go with a sure bet, and stick with OpenSolaris. Since ZFS reins supreme on Solaris, and it's a solid OS. I had a power surge a few months back, and it reset all my wonderful uptimes, but my current OpenSolaris setup has been up for around 80 days, with no problems whatsoever. I'm downloading a new disk image as I type, and will prolly install it before I rest for the night.

Anywho the software is always the most time consuming of any project, and I will expand on my final decision in the full writeup for the In-N-Out NAS.  Just to tease I've uploaded an image of a semi-finished In-N-Out NAS. I have a few more tweaks, and I have to get the Pico PSU all fixed up...

![in_n_out_nas_preview](http://fsk141.com/uploads/2009/10/in_n_out_nas_preview-300x233.jpg)
