---
date: '2008-12-23 00:00:00'
layout: post
slug: one-time-grub-entry
status: publish
title: One time GRUB entry
wordpress_id: '42'
categories:
- Linux
---

So you have a remote server that you need to test a new kernel on, and you dont have a remote kvm/console. What to do???

Use a nice little grub haxie of course.

    
    echo "savedefault --default=1 --once" | grub --batch


The number 1 being the grub entry that you would like to try out. This command will boot the selected entry, in this case 1, for one boot cycle, and then return to the default kernel.

    
    timeout   5
    default   0
    color light-blue/black light-cyan/blue
    
    title  Arch Linux
    root   (hd0,0)
    kernel /boot/vmlinuz26 root=/dev/disk/by-uuid/3e2569bd-3c65-408b-bafa-bab760741faf ro
    initrd /boot/kernel26.img
    
    title  Arch Linux Fallback
    root   (hd0,0)
    kernel /boot/vmlinuz26 root=/dev/disk/by-uuid/3e2569bd-3c65-408b-bafa-bab760741faf ro
    initrd /boot/kernel26-fallback.img


Here is my current menu.lst. So if I ran the command above, it would boot "Arch Linux Fallback" once, and then return to the default kernel. If you have a power management device then you can completely avoid failure.

Resources:
[GRUB: Boot another OS once](http://sidvind.com/wiki/GRUB:_Boot_another_OS_once)
