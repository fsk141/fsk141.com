---
date: '2009-02-03 00:00:00'
layout: post
slug: opensolaris-bugs-new-things-i-learned
status: publish
title: OpenSolaris bugs/new things I learned
wordpress_id: '37'
categories:
- Opensolaris
tags:
- Open Solaris
---

root@FSK-Backup:~# beadm
    Traceback (most recent call last):
      File "/usr/sbin/beadm", line 41, in ?
        from osol_install.beadm.BootEnvironment import *
    ImportError: No module named osol_install.beadm.BootEnvironment


To fix this run the following:

    
    cp /usr/lib/python2.4/vendor-packages/osol_install/beadm/__init__.py /usr/lib/python2.4/vendor-packages/osol_install/__init__.py


New comands I learned:

    
    pkg search 'string'
    beadm list
    zfs list -t snapshot
    svcs
    svcadm


I've been using OpenSolaris more and more while I tweak my NAS server, and I'm quite happy with how it's setup. I wish the package manager was a little more. I'm an avid user of pacman (Arch Linux) and it's the king of all package managers, so there is a lot to be missed in pkg.
