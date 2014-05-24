---
date: '2009-01-26 00:00:00'
layout: post
slug: container-enter-failed-openvz
status: publish
title: Container enter failed [OpenVZ]
wordpress_id: '38'
categories:
- Linux
tags:
- OpenVZ
---

The VE will start, but you are unable to 'vzctl enter VE#' right?

You get something like:

    
    container enter failed(?)


To get past this you need to manually create the pty/tty devices:

    
    vzctl exec  101 /sbin/MAKEDEV tty
    vzctl exec 101 /sbin/MAKEDEV pty
    vzctl enter 101


Where 101 = VE# in question...
