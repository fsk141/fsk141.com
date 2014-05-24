---
date: '2011-11-04 23:37:17'
layout: post
slug: boot-usb-drives-in-virtualbox
status: publish
title: Boot USB Drives in VirtualBox
wordpress_id: '913'
categories:
- Linux
tags:
- flash drive
- multipass
- USB
- virtualbox
---

[![](http://fsk141.com/uploads/2011/11/vboxusb.png)](http://fsk141.com/uploads/2011/11/vboxusb.png)

Just another one to add to the list of things that VirtualBox can't do out of the box. (Seems like it should be a little more full featured for being around so long.)
Anywho, VirtualBox can't load USB's out of the box, but with a little faux emulation you can boot a USB, and do some testing on your bootable USB sticks. It's elementary my dear watson, just execute the following, and be rewarded with a selectable USB device.

{% highlight bash %}

mkdir -p ~/.VirtualBox/HardDisks

## Replace /dev/sdb with your device (fdisk -l) if you aren't sure

VBoxManage internalcommands createrawvmdk -filename ~/.VirtualBox/HardDisks/usbdisk.vmdk -rawdisk /dev/sdb

{% endhighlight %}

That's it; not just start up a copy of VirtualBox &amp; make a new virtual machine; add your usbdisk.vmdk &amp; enjoy the pictorial below if you have any confusion:

[![](http://fsk141.com/uploads/2011/11/vbox1.png)](http://fsk141.com/uploads/2011/11/vbox1.png)[![](http://fsk141.com/uploads/2011/11/vbox2.png)](http://fsk141.com/uploads/2011/11/vbox2.png)[![](http://fsk141.com/uploads/2011/11/vbox3.png)](http://fsk141.com/uploads/2011/11/vbox3.png)[![](http://fsk141.com/uploads/2011/11/vbox4.png)](http://fsk141.com/uploads/2011/11/vbox4.png)
