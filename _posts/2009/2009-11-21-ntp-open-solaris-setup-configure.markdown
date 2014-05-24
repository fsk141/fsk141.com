---
date: '2009-11-21 12:31:57'
layout: post
slug: ntp-open-solaris-setup-configure
status: publish
title: "NTP Open Solaris (Setup &amp; Configure)"
wordpress_id: '172'
categories:
- Opensolaris
tags:
- ntp
- Opensolaris
- time
---

NTP is awesome, it's been around forever, and it's one of the first things I setup on any new machine. This is an especially important service for a NAS device, since sync times are crucial, and there are many important services that are time dependent (snapshots for example).

**1) Set the Time**

{% highlight bash %}
ntpdate 0.pool.ntp.org
{% endhighlight %}

--- Output ---

{% highlight bash %}
root@TrayNAS:/etc/inet# ntpdate 0.pool.ntp.org
21 Nov 12:21:19 ntpdate[1296]: adjust time server 66.96.96.29 offset -0.000521 sec
{% endhighlight %}

**2) Configure**

{% highlight bash %}
cp /etc/inet/ntp.server /etc/inet/ntp.conf
echo "server 0.pool.ntp.orgnserver 1.pool.ntp.orgnserver 2.pool.ntp.org" >> /etc/inet/ntp.conf
svcadm enable ntp
svcs ntp
{% endhighlight %}

--- Output ---

{% highlight bash %}
root@TrayNAS:/etc/inet# svcs ntp
STATE          STIME    FMRI
online         12:12:57 svc:/network/ntp:default
{% endhighlight %}

Woot, time is setup. Enjoy synchronized time...

_**Resources:**_



	
  * [http://www.itwiki.se/OpenSolaris/ntpd](http://www.itwiki.se/OpenSolaris/ntpd)


