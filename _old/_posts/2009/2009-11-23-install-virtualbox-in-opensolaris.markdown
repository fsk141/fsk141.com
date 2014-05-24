---
date: '2009-11-23 11:03:47'
layout: post
slug: install-virtualbox-in-opensolaris
status: publish
title: Install VirtualBox in OpenSolaris
wordpress_id: '212'
categories:
- Opensolaris
---

Virtualbox is awesome, and I think I might go with an all inclusive OpenSolaris server in the future. One that runs zfs, and all my NFS storage needs, but also has virtual machines for the other servers that I would like to run.

Here is how to install VirtualBox on Opensolaris:

Get the latest version from here: [http://www.virtualbox.org/wiki/Downloads](http://www.virtualbox.org/wiki/Downloads)

{% highlight bash %}
wget http://download.virtualbox.org/virtualbox/3.0.12/VirtualBox-3.0.12-54655-SunOS.tar.gz
tar -xzf VirtualBox-3.0.12-54655-SunOS.tar.gz
pkgadd -d VirtualBoxKern-3.0.12-54655.pkg</pre>
pkgadd -d VirtualBox-3.0.12-54655.pkg
{% endhighlight %} 
