---
date: '2009-10-06 03:14:41'
layout: post
slug: image-organization-in-n-out-nas
status: publish
title: Image Organization | In-N-Out NAS
wordpress_id: '98'
categories:
- Hardware
tags:
- images
- in-n-out nas
- NAS
- picasa
- scripts
---

It's painful, but I've been meaning to get my images in good order for a long time now. Well I made a little script to rename all the JPG & AVI extensions to lower case, and remove the IMG_0 crap.

    
    
    #!/bin/bash
    
    mmv "*.JPG" "#1.jpg"
    mmv "IMG_0*" "#1"
    
    mmv "*.AVI" "#1.avi"
    mmv "MVI_0*" "#1"
    
    chmod 755 *
    


mmv is a powerful tool, and I'm using a very simple implementation of it. I've gotten all my images in order at the moment, and right now I'm pushing everything into Picasa 3.5 to get all the faces recognized. This is an excellent selling point of Picasa 3.5. It works pretty well, and my overall experience with Picasa has been excellent. Well I'm off to sleep for now, but an update is soon to come with my overall experience of Picasa.

------
The In-N-Out NAS is at a major standstill at the moment. I didn't think a 32 bit system would be a problem when I purchased the VIA board. Yet I forgot about the "performance benefit" with ZFS & 64 bit. Well come to find out, 32 bit is restricted to 1TB or less. Anything higher is rejected with a nice little 'f you for using 32 bit' message in '/var/adm/messages' In the mean time I'm trying to get a new motherboard, prolly core duo mini-itx intel board. The overall experience of this homebrew NAS has been an unsavory experience, yet it looks great, and once everything is worked out, I should have a wonderful NAS setup.
