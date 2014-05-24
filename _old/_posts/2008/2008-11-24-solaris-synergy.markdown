---
date: '2008-11-24 00:00:00'
layout: post
slug: solaris-synergy
status: publish
title: OpenSolaris | Synergy
wordpress_id: '53'
categories:
- Software
tags:
- Linux
- Open Solaris
---

Recently I've started using OpenSolaris 2008.11 for work and play. It's kinda a pain to get used to cause I'm so used to the linux way of doing things.

Such as:
Basic list all with ps:

    
    Linux  |  OpenSolaris
    ---------------------
    ps ax  |  ps -Af


Services in OpenSolaris:

    
    Linux     |  OpenSolaris
    ------------------------
    service   |  svcs -a (list all services running)
    init.d    |  svcadm (to start, stop, and restart)
    rc.d      |


------

/etc/synergy.conf

    
    section: screens
            Osol:
            Macarch:
    end
    
    section: links
            Osol:
                    left = Macarch
            Macarch:
                    right = Osol
    end


Then all I have to do is run 'synergys' on the host computer (the one with the keyboard & mouse), and synergyc 10.200.2.10 (the ip address of the host)
