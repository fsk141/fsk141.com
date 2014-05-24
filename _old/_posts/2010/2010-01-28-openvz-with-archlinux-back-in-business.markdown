---
date: '2010-01-28 16:50:56'
layout: post
slug: openvz-with-archlinux-back-in-business
status: publish
title: OpenVZ with Archlinux (back in business)
wordpress_id: '242'
categories:
- Linux
tags:
- Arch Linux
- kernel
- Linux
- OpenVZ
---

{% highlight bash %}Linux smsitraining 2.6.26.8-OVZ26-gogol.1 #1 SMP Thu Jan 28 15:49:59 PST 2010 x86_64 Intel(R) Xeon(R) CPU E5520 @ 2.27GHz GenuineIntel GNU/Linux{% endhighlight %}

Success! I've been working all day on a working OpenVZ kernel on my dev machine. 2.6.18, 2.6.24, 2.6.18rhel5; all borked out on the machine (either mid compile, or crapped out at boot) Anywho I setup a PKGBUILD for 2.6.26 Gogol & made a few modifications, and BAM everything is golden.

I've updated git & uploaded the package to AUR: [http://aur.archlinux.org/packages.php?ID=34116](http://aur.archlinux.org/packages.php?ID=34116)

BTW, I just found out that an online friend of mine (Wonder) that is heavily involved in ArchLinux has created an Arch template for OpenVZ. Previous to this wonderful contribution OpenVZ has been without an Arch Linux template (that is updated), and it's been a real drag. You can find the template on the standard templates page:

[http://wiki.openvz.org/Download/template/precreated](http://wiki.openvz.org/Download/template/precreated)
