---
date: '2011-10-07 17:40:01'
layout: post
slug: fix-pty-allocation-request-failed-on-channel-0
status: publish
title: '[fix] PTY allocation request failed on channel 0 (Arch Linux VPS)'
wordpress_id: '775'
categories:
- Linux
tags:
- Arch Linux
- fix
- vps
---

Another problem with the new glibc & old kernel pair, anywho; this is an easy fix, just remember to do it before you logout, or it's a little painful (unless you have root privs)



	
  1. Start with adding the correct entries to /etc/fstab:
{% highlight bash %}
#
# /etc/fstab: static file system information
#
# <file system>    <dir>    <type>    <options>    <dump>    <pass>
devpts                 /dev/pts      devpts    defaults            0      0
shm                    /dev/shm      tmpfs     nodev,nosuid        0      0
{% endhighlight %}

	
  2. Then add the following to /etc/rc.local
{% highlight bash %}
rm -rf /dev/ptmx
mknod /dev/ptmx c 5 2
chmod 666 /dev/ptmx
umount /dev/pts
rm -rf /dev/pts
mkdir /dev/pts
mount /dev/pts
{% endhighlight %}

	
  3. Reboot, and cross your fingers for success; hopefully everything will be okay :)


WAIT, I rebooted, and can't access my vps, how do I fix this?

Well a couple solutions include you logging into a serial console and running the commands.

Or if you have root access availiable you can login with:

{% highlight bash %}

ssh yourserver.com '/bin/bash -i' #Run all the same procedures as above, reboot

{% endhighlight %}

Otherwise you're kinda screwed, and just have to reinstall your VPS, and fix the issue before you reboot...
