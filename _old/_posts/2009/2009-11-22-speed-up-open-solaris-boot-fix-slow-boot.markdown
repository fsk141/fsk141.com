---
date: '2009-11-22 02:00:30'
layout: post
slug: speed-up-open-solaris-boot-fix-slow-boot
status: publish
title: Speed up Open Solaris Boot (Fix Slow Boot)
wordpress_id: '196'
categories:
- Opensolaris
---

It's so annoying to wait 5+ minutes for Open Solaris to boot. Come to find out it's because of the stupid graphical boot on startup. Damn gui'ness

**1) Fix Grub menu.lst**



	
  * Edit /rpool/boot/grub/menu.lst

	
  * Comment the following


{% highlight bash %}
splashimage
foreground
background
{% endhighlight %}

	
  * Remove 'console=graphics' from the 'kernel' line


You should end up with something like this:

{% highlight bash %}

splashimage /boot/grub/splash.xpm.gz
background 215ECA
timeout 30
default 2
#---------- ADDED BY BOOTADM - DO NOT EDIT ----------
title OpenSolaris 2009.06
findroot (pool_rpool,0,a)
bootfs rpool/ROOT/opensolaris
#splashimage /boot/solaris.xpm
#foreground d25f00
#background 115d93
#kernel$ /platform/i86pc/kernel/$ISADIR/unix -B $ZFS-BOOTFS,console=graphics
kernel$ /platform/i86pc/kernel/$ISADIR/unix -B $ZFS-BOOTFS
module$ /platform/i86pc/$ISADIR/boot_archive
#---------------------END BOOTADM--------------------
title TrayNAS_Nov_21
findroot (pool_rpool,0,a)
bootfs rpool/ROOT/TrayNAS_Nov_21
kernel$ /platform/i86pc/kernel/$ISADIR/unix -B $ZFS-BOOTFS
module$ /platform/i86pc/$ISADIR/boot_archive
#============ End of LIBBE entry =============
title opensolaris-1
findroot (pool_rpool,0,a)
bootfs rpool/ROOT/opensolaris-1
#splashimage /boot/solaris.xpm
#foreground d25f00
#background 115d93
#kernel$ /platform/i86pc/kernel/$ISADIR/unix -B $ZFS-BOOTFS,console=graphics
kernel$ /platform/i86pc/kernel/$ISADIR/unix -B $ZFS-BOOTFS
module$ /platform/i86pc/$ISADIR/boot_archive
#============ End of LIBBE entry =============

{% endhighlight %}

Resources:

	
  * [http://www.codestrom.com/wandering/2009/02/opensolaris-slow.html](http://www.codestrom.com/wandering/2009/02/opensolaris-slow.html)


