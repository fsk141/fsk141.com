---
date: '2009-11-22 02:48:12'
layout: post
slug: zfs-dedup-opensolaris-repos-beadm
status: publish
title: ZFS Dedup | OpenSolaris repos | beadm
wordpress_id: '200'
categories:
- ZFS
tags:
- beadm
- dedup
- NAS
- pkg repos
---

DEDUP IS AMAZING. Can't wait to try it out, but I'm waiting for my system to upgrade from the dev repository, err, I might just hold off cause it's taking forever. Maybe I'll play with it in a VM, instead of my Home NAS :*
Anywho please read up on it here: [http://blogs.sun.com/bonwick/entry/zfs_dedup](http://blogs.sun.com/bonwick/entry/zfs_dedup)

It's an amazing technology, and I'm super happy that it's finally out.

------

Just in case you want to upgrade 111b to 127, or just have the most up to date packages try the following:

{% highlight bash %}
pkg set-publisher -O http://pkg.opensolaris.org/dev "opensolaris.dev"
pkg set-publisher -P opensolaris.dev
{% endhighlight %}

Now when you look at your repos, dev is default or 'preferred'

{% highlight bash %}
fsk141@TrayNAS:/mnt$ pkg publisher
PUBLISHER                             TYPE     STATUS   URI
opensolaris.dev          (preferred)  origin   online   http://pkg.opensolaris.org/dev/
opensolaris.org                       origin   online   http://pkg.opensolaris.org/release/
{% endhighlight %}

------

As I work with OpenSolaris I am continually amazed, and pissed off at the same time. Where some things are insanely simple and straightforward. Other things are tedious, and hard because of my Linux background. Another thing that I am letdown is the lack of community like Arch Linux. There is a very large OpenSolaris community, but not in the sense of packages, and a lot is left to you (as in manually compile)
------

One of the difficult concepts to grasp is the boot environment concept. It's kinda like a snapshot of sorts for your boot environment. Since OpenSolaris isn't a rolling distro like Arch Linux, there are different revisions. Well instead of forcing some difficult reinstall you can simply 'pkg image-update' and upon your next reboot you have your new environment. You also have the ability to easily revert.

{% highlight bash %}
fsk141@TrayNAS:/mnt$ beadm list
BE             Active Mountpoint Space   Policy Created
--             ------ ---------- -----   ------ -------
TrayNAS_Nov_21 -      -          304.15M static 2009-11-21 15:53
opensolaris    -      -          7.57M   static 2009-10-16 08:19
opensolaris-1  NR     /          3.62G   static 2009-11-22 01:44

fsk141@TrayNAS:/mnt$ beadm destroy TrayNAS_Nov_21
Are you sure you want to destroy TrayNAS_Nov_21? This action cannot be undone(y/[n]): y
{% endhighlight %}

beadm is a great tool to modify your BE, and I love the simplicity in the BE scheme.
