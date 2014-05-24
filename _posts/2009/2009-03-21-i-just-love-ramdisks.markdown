---
date: '2009-03-21 00:00:00'
layout: post
slug: i-just-love-ramdisks
status: publish
title: I just love ramdisks
wordpress_id: '27'
categories:
- Linux
tags:
- Linux
---

Lately with all this kernel work I have taken full advantage of the wonderful ramdisk. Since the kernels are compressed and that takes a bunch of disk I/O's it's great to shave off minutes in uncompressing by using a ramdisk.

Here is a simple way to setup a ramdisk:

    
    
    sudo mkdir /mnt/ramdisk
    sudo mount -t tmpfs none /mnt/ramdisk
    



The possibilities are endless, and it helps a ton when you need a bunch of disk I/O, and have the memory to spare.
