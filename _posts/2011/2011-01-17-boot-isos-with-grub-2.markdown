---
date: '2011-01-17 13:01:41'
layout: post
slug: boot-isos-with-grub-2
status: publish
title: Boot iso's with grub 2
wordpress_id: '600'
categories:
- Linux
tags:
- fat32
- grub
- grub2
- iso
- parted magic
- pmagic
---

Okay; to prefix this post I just want to say this is AWESOME! I've wanted & tried to find a way to do this for years, and guess I didn't try hard enough; or google very well? I found this the other day, and thought I would share it with the internets...
Firstly install grub2 on your medium you would like to use. Since I'm using this featurette for booting iso's off of my portable toolkit I'll show you how to install grub2 on your usb drive.

**1) Install grub2 (if you don't already have it), dosfstools, cfdisk (or just use fdisk or gparted for those gui peoples)**



	
  * This should do the trick if you are on Arch Linux


{% highlight bash %}pacman -S dosfstools grub2{% endhighlight %}

	
  * Check your repos, or manually compile && make sure you don't do any nasty grub1 conflicts


**2) Format disk with one FAT32 partition**

I used fdisk & had no problems

{% highlight bash %}sudo fdisk /dev/sdd{% endhighlight %}



	
  * (sdd == the disk I would like to format) && make one partition that's bootable & FAT32 flagged



	
  * Delete all the partitions first & then create a new one with defaults && DONT FORGET TO SET THE BOOT FLAG TO ON ( a )


{% highlight bash %}
[~]$ sudo fdisk /dev/sdd

Command (m for help): d
Selected partition 1

Command (m for help): d
No partition is defined yet!

Command (m for help): n
Command action
 e   extended
 p   primary partition (1-4)
p
Partition number (1-4, default 1): 1
First sector (2048-126959615, default 2048):
Using default value 2048
Last sector, +sectors or +size{K,M,G} (2048-126959615, default 126959615):
Using default value 126959615

Command (m for help): a
Partition number (1-4): 1

Command (m for help): t
Selected partition 1
Hex code (type L to list codes): b
Changed system type of partition 1 to b (W95 FAT32)

Command (m for help): w
The partition table has been altered!

Calling ioctl() to re-read partition table.
Syncing disks.
{% endhighlight %}

	
  * d == delete partitions (delete until there aren't any left)

	
  * n == create new partition

	
    * p == primary

	
    * 1 == partition number

	
    * First sector (just hit enter; it's fine how it is)

	
    * Last sector (just hit enter to use entire disk)




	
  * a == set partition 1 to bootable (this is important)

	
  * w == write all the changes to disk & quit

	
  * t == set partition type && b == FAT32


**3) Add a filesystem**

{% highlight bash %}sudo mkfs.vfat -F 32 /dev/sdd1{% endhighlight %}



	
  * -F 32 == FAT32 && /dev/sdd1 == the partition you created with cfdisk


**4) Install grub2**

{% highlight bash %}

sudo mkdir /mnt/flash

sudo mount /dev/sdd1 /mnt/flash

sudo grub-install --force --no-floppy --root-directory=/mnt/flash /dev/sdd

{% endhighlight %}



	
  * make a folder to mount your disk into && mount it (need this to install grub boot files)

	
  * --force isn't necessary, but helps for any un-necessary glitches that don't matter

	
  * --no-floppy explicitely prevents floppy emulation (you don't need it)

	
  * --root-directory is where you mounted your disk (if you put it somewhere other than /mnt/flash)

	
  * /dev/sdd is the device node of your device


Once you hit enter you should be greeted with a sweet little message of success:

{% highlight bash %}

Installation finished. No error reported.

{% endhighlight %}

**5) Install a test iso to try out booting from iso**

Just an FYI; not all linux iso's have iso compatibility compiled in, so it's a hit/miss if this works. You just have to try it, and google around until you find something that works.

{% highlight bash %}

cd /mnt/flash

mkdir isos

cd isos

wget http://exo.enarel.eu/mirror/partedmagic/pmagic-5.9.iso

sudo vim /mnt/flash/boot/grub/grub.cfg

{% endhighlight %}



	
  * Now all we need to do is add our parted magic iso features into grub.cfg


{% highlight bash %}

set timeout=10
set default=0

menuentry "Parted Magic" {
 set isofile="/isos/pmagic-5.9.iso"

 loopback loop $isofile
 linux (loop)/pmagic/bzImage iso_filename=$isofile edd=off noapic load_ramdisk=1 prompt_ramdisk=0 rwnomce sleep=10 loglevel=0
 initrd (loop)/pmagic/initramfs
}

{% endhighlight %}

**6) ENJOY!**
