---
date: '2010-09-17 10:35:10'
layout: post
slug: upgrade-opensolaris-to-openindiana
status: publish
title: Upgrade OpenSolaris to OpenIndiana
wordpress_id: '327'
categories:
- Opensolaris
tags:
- openindiana
- Opensolaris
- pkg
---

OpenIndiana was just released a few days ago (I just figured out today) > Anywho who wants to be up to the most bleeding up to dateness? Well I certainly do, so here's how:

{% highlight bash %}

Last login: Wed Sep 15 23:15:07 2010 from jgerold-13mbp.o
Sun Microsystems Inc.   SunOS 5.11      snv_129 November 2008

# Add the new repository

<strong>pfexec pkg set-publisher --non-sticky opensolaris.org
pfexec pkg set-publisher -O http://pkg.openindiana.org/dev openindiana.org
pfexec pkg set-publisher -P openindiana.org</strong>

# Unset the previous publisher

$ <strong>pkg publisher</strong> # List the current Publishers

PUBLISHER                             TYPE     STATUS   URI
openindiana.org          (preferred)  origin   online   http://pkg.openindiana.org/dev/
opensolaris.org          (non-sticky) origin   online   http://pkg.opensolaris.org/dev/

# Remove the old publisher
<strong>pfexec pkg unset-publisher opensolaris.org</strong>

# Checking to make sure everything went smoothly

$ <strong>pkg publisher</strong>
PUBLISHER                             TYPE     STATUS   URI
openindiana.org          (preferred)  origin   online   http://pkg.openindiana.org/dev/

# Run your update, and enjoy your new repository:

$ <strong>pfexec pkg image-update</strong>

{% endhighlight %} 
