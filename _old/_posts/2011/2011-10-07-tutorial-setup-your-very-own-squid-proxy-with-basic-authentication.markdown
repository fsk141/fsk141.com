---
date: '2011-10-07 18:20:48'
layout: post
slug: tutorial-setup-your-very-own-squid-proxy-with-basic-authentication
status: publish
title: '[tutorial] Setup your very own squid proxy [with basic authentication]'
wordpress_id: '781'
categories:
- Linux
- Networking
- Tutorials
tags:
- Arch Linux
- security
- squid
---

So, after being trained in the dark arts of pentesting, hacking, and other nefarious computer skills; I suppose I should secure my webtraffic when out and about.  I know there are many ways to do this (ssh being one:[ http://fsk141.com/simple-socks-5-proxy-ssh-tunnel](http://fsk141.com/simple-socks-5-proxy-ssh-tunnel), but I figure setting up a proxy is one of the easiest things to access &amp; use.



	
  1. Lets start by installing squid{% highlight bash %}
pacman -Sy squid #for Arch Linux -- use apt or whatever for other distros
{% endhighlight %}

	
  2. Then lets do a little configuration additions (make a new file /etc/squid/squid.conf):
{% highlight bash %}
# Plaintext Authentication Squid Setup
# /etc/squid/squid.conf
#
http_port 3129 #default port to connect with

auth_param basic program /usr/lib/squid/ncsa_auth /etc/squid/passwd # might need to change paths dependant on distro

#make /etc/squid/passwd with the following:
## htpasswd /etc/squid/passwd your_username_here # execute as root/sudo

auth_param basic children 5
auth_param basic realm Squid proxy-caching web server
auth_param basic credentialsttl 2 hours
auth_param basic casesensitive off

# acl allow rules
acl authenticated proxy_auth REQUIRED
http_access allow authenticated
{% endhighlight %}

	
  3. Save the file, and don't forget to create /etc/squid/passwd
{% highlight bash %}
htpasswd /etc/squid/passwd your_username_here # execute as root/sudo
{% endhighlight %}

	
  4. If everything is happy and giggly, then you can startup squid and have a ball browsing (sudo rc.d start squid)


