---
date: '2009-02-17 00:00:00'
layout: post
slug: how-to-setup-ntp-on-opensolaris
status: publish
title: How to setup NTP on OpenSolaris
wordpress_id: '35'
categories:
- Opensolaris
tags:
- Open Solaris
---

NTP is one of the best utilities for any system. It automagically corrects your time according to the ntp time servers. Here's how to enable it on OpenSolaris:

I would recommend doing the following commands logged in via root (sudo su -):

    
    cp /etc/inet/ntp.client /etc/ntp.conf
    vi /etc/ntp.conf


Append the following to ntp.conf:

    
    server 0.pool.ntp.org
    server 1.pool.ntp.org
    server 2.pool.ntp.org


Then run the following commands:

    
    svcadm enable svc:/network/ntp:default
    svcadm restart svc:/network/ntp:default


The 'restart' might not be absolutely necessary, but it's done to check everything is in order.

If you would like to check that everything is up and running:

    
    svcs -a | grep ntp


It should give you something similar to the following:

    
    online         Feb_03   svc:/network/ntp:default


Enjoy your auto-updated time :)
