---
date: '2008-12-13 00:00:00'
layout: post
slug: remove-open-solaris-gui
status: publish
title: Remove Open Solaris GUI
categories:
- Opensolaris
tags:
- Open Solaris
- programming
---
Since I never plan to use Open Solaris as a desktop, and only as a server I grepped around for a way to remove Gnome/Xorg, and all the other little complexities that I didn't want. It's kinda a pain that the install CD doesn't allow you to choose not to install the GUI. Here's the code that I found to achieve the task of removing the GUI.

{% highlight bash %}
pkg uninstall -vr `pkg list | egrep '(aac|acc|atheros|audio|avahi|compiz|evolution|firefox|flac|gamin|gnome|ipp|ipw|iwi|iwk|musicbrainz|ogg|pkg-gui|print|thunderbird|tnetd|wlan|wlan|wpa|wpi|xcursor|xorg|xscreensaver)' | awk '{print $1}'`
{% endhighlight %} 
