---
date: '2009-11-23 21:16:41'
layout: post
slug: static-ip-opensolaris
status: publish
title: Static IP OpenSolaris
wordpress_id: '217'
categories:
- Opensolaris
tags:
- disable dhcp
- static ip
---

Setting a static IP in OpenSolaris is similar to Linux, but yet again; differences arise...

{% highlight bash %}
vi /etc/resolve.conf (make sure dns is correct [nameserver 10.0.0.1]
vi /etc/nwam/llp [change 'gani0 dhcp' to 'gani0 static 10.0.0.100' where gani0 is your interface, and 10.0.0.100 is the ip address you would like to assign]
# restart network
vi /etc/defaultrouter (add your router '10.0.0.1')
svcadm enable svc:/network/physical:default
svcadm restart svc:/network/physical:nwam
{% endhighlight %}

This is a quickie but a goodie...

**_Resources: _**

[http://briancline.org/read/file_server_3_tweaking_opensolaris](http://briancline.org/read/file_server_3_tweaking_opensolaris)
