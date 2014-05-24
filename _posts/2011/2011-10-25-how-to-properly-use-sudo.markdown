---
date: '2011-10-25 22:46:22'
layout: post
slug: how-to-properly-use-sudo
status: publish
title: sudo (brief history -- &amp; installation) -- Part one of multipart series
wordpress_id: '880'
categories:
- Linux
- Tutorials
tags:
- Arch Linux
- Linux
- sudo
- todd c. miller
---

![](http://imgs.xkcd.com/comics/sandwich.png)

There was a time in all of our lives when we needed to login to a root account to install packages, or for the smarter amongst linux noobs were the people that used su - ;) I don't actually recall when I started using sudo (**S**uper **U**ser **D**o -- Pronounced soo-doo), but I know I've been using it long enough to forget when I started using it :0 The powers of sudo with multi-user environments are endless. Being able to restrict programs to categorical execution rights is fantastic. Especially when you only want to allow a user to view logs; and do nothing else with the system (dev team); or allow someone to add users so you don't have to, yet not worry about them dicking anything else up.

This tutorial is short, sweet, and simple, and based primarily on using sudo with scripts in a single user environment. Yet I'll go into a short detail on how to expand for multi-user machines (it's fairly straightforward)

**Lets start with a little intro to sudo:**


> Sudo was first conceived and implemented by Bob Coggeshall and Cliff Spencer around 1980 at the Department of Computer Science at SUNY/Buffalo. It ran on a VAX-11/750 running 4.1BSD. An updated version, credited to Phil Betchel, Cliff Spencer, Gretchen Phillips, John LoVerso and Don Gworek, was posted to the net.sources Usenet newsgroup in December of 1985.

-- [http://www.sudo.ws/sudo/history.html](http://www.sudo.ws/sudo/history.html)


Sudo has been around for a long time, maintained by a lot of different people, and taken gradual changes until Todd C. Miller took hold of the project. A constant stream of updates is provided by Quest Software, and their sponsorship of sudo by paying Todd to manage sudo.

**Sudo Setup:**

Before starting anything, lets start by setting sudo up.

{% highlight bash %}

pacman -S sudo

## this will add your username to group wheel

## We'll use this group assignment later

gpasswd -a <your_username> wheel

{% endhighlight %}

**Sudo Configuration:**
Now that everything's all setup, we need to dive into /etc/sudoers (carefully), and edit some things.
**DO NOT EDIT /etc/sudoers DIRECTLY...**

{% highlight bash %}
su -
visudo
{% endhighlight %}

Instead of showing you the default /etc/sudoers file, I'll instead show you what you could easily replace it with:

{% highlight bash %}
#
# /etc/sudoers -- visudo
#

Cmnd_Alias    SUSPEND = /usr/sbin/pm-suspend
Cmnd_Alias    INTERNET = /usr/bin/netcfg, /sbin/ifconfig, /usr/sbin/iwconfig, /usr/sbin/iwlist, /usr/sbin/dhcpcd
Cmnd_Alias    SAVEPOWER = /home/fsk141/.scripts/autopower

root ALL=(ALL) ALL

%wheel ALL=(ALL) ALL, NOPASSWD: SUSPEND, NOPASSWD: INTERNET, NOPASSWD: SAVEPOWER
{% endhighlight %}

Lets break this down into simple little bits. I'm using Cmnd_Alias' for a simple purpose, and I have a very stripped down visudo. I have all of my necessary programs that live in my misc scripts in Cmnd_Alias' and then call them from the wheel group selector (%wheel)

The simplest solution would to have just two lines in your /etc/sudoers file:

{% highlight bash %}

#root can login &amp; has all permissions

root ALL=(ALL) All

#Users of group wheel can login &amp; has all permissions

%wheel ALL=(ALL) ALL

{% endhighlight %}

This will allow you to say 'sudo su -', from your standard prompt, and be dropped into a nice root prompt. Or easily run 'sudo rc.d start nginx' to start your webserver without having to login to root first...

------

This is just a starter in a multi-part series for sudo, I plan on writing a more complicated write up for multi-user applications, and multi-system applications (restricting users to certain apps, certain servers, etc) I also have an interview lined up with Todd C. Miller, and hope to get some insightful comments out of him.
