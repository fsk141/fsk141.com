---
date: '2010-10-08 14:02:55'
layout: post
slug: tutorial-openvz-debian-zenoss-monitor-your-world
status: publish
title: '[Tutorial] OpenVZ - Debian - Zenoss (monitor your world)'
wordpress_id: '441'
categories:
- Linux
- Tutorials
tags:
- debian
- Linux
- OpenVZ
- zenoss
---

[![openvz-debian-zenoss](http://farm5.static.flickr.com/4105/5063551922_209885c7ce_o.png)](http://www.flickr.com/photos/68444690@N00/5063551922/)

Do you have an OpenVZ HN (Host Node) & have no idea what's happening with your nodes? Well get out of the dark ages & add a [Zenoss](http://community.zenoss.org) installation to monitor your VE's (Virtual Environments). Why did I choose debian when zenoss supports a bunch of distros? Well there are repos for debian/ubuntu (which will auto-setup for the most part), and I'm not a fan of ubuntu. Thus we'll use debian. The whole setup only takes a few minutes, so lets jump right in...

**1)** _Make a new VE with Debian 5:_

{% highlight bash %}
# On your HN run:

# Download the debian 5 template
sudo wget -c http://download.openvz.org/template/precreated/debian-5.0-x86_64.tar.gz -o /vz/template/cache/debian-5.0-x86_64.tar.gz

# Create debian VE
sudo vzctl create 1 --ostemplate debian-5.0-x86_64

# Hooray your VE is setup, now move onto configuration >>
{% endhighlight %}

**2)** _Set VE options:_

{% highlight bash %}
# Configure debian VE (naibed-zen is debian backwards with a lil zen added)
# You can enter this all on one line, I just spread it out to make it easier to read
sudo vzctl set 1 
--hostname naibed-zen 
--ipadd <ipaddress> 
--searchdomain <yoursearchdomain> 
--nameserver "<your nameserver/nameservers (separated by spaces)> 
--vmguarpages $((256 * 4096)) 
--privvmpages $((256 * 6144)) 
--swappages $((256 * 1024)) 
--meminfo none 
--onboot yes 
--save

#--vmguarpages $((256 * 4096)) # guaranteed memory (4GB)
#--privvmpages $((256 * 6144)) # burst memory (6GB)
#--swappages $((256 * 1024)) # swap memory
#--meminfo none # I was having memory listing issues, and this fixed it
{% endhighlight %}

This will configure the empty shell of a VE that we setup & make is more usable:

**VEID:** (Virtual Environment ID) == 1 in our case, but you can set it to whatever you want
**Hostname:** naibed-zen
**IP Address:** (eg. 192.168.1.2)
**Search Domain** (/etc/resolv.conf): (eg. google.com *this is optional)
**Nameserver(s)** (/etc/resolv.conf): (eg. 192.168.1.1)
**Guaranteed Memory:** (change the last number, 4096 == 4GB)
**Burst Memory:** (change the last number, 6144 == 6GB)
**Turn on at boot:** yes
**Don't forget to save:** if you don't --save then your HN won't remember the settings when rebooted

If I were you I would enter the VE (sudo vzctl enter 1) && test to make sure networking is working... ping google or something, if not, then make sure your HN is setup correctly & that your dns servers are correct

**3)** _Setup Zenoss:_

Now that we have our VE all ready to go, lets setup zenoss...

{% highlight bash %}
# Enter the VE
sudo vzctl enter 1
echo "deb http://dev.zenoss.org/deb main stable" >> /etc/apt/sources.list
apt-get update
apt-get install zenoss-stack
/etc/init.d/zenoss-stack start
{% endhighlight %}

It should look something like this:

{% highlight bash %}
$ sudo vzctl enter 2
entered into CT 2
naibed:/# echo "deb http://dev.zenoss.org/deb main stable" >> /etc/apt/sources.list
naibed:/# apt-get update
... updating pkg-database ...

naibed:/# apt-cache search zenoss-stack # just verifying that the repo is correct
zenoss-stack - Zenoss Stack with all requirements.

naibed:/# apt-get install zenoss-stack
Reading package lists... Done
Building dependency tree... Done
The following NEW packages will be installed:
  zenoss-stack
0 upgraded, 1 newly installed, 0 to remove and 14 not upgraded.
Need to get 110MB of archives.
After this operation, 386MB of additional disk space will be used.
WARNING: The following packages cannot be authenticated!
  zenoss-stack
Install these packages without verification [y/N]? Y
Get:1 http://dev.zenoss.org main/stable zenoss-stack 3.0.2-0 [110MB]
Fetched 110MB in 11s (9756kB/s)
Selecting previously deselected package zenoss-stack.
(Reading database ... 23039 files and directories currently installed.)
Unpacking zenoss-stack (from .../zenoss-stack_3.0.2-0_amd64.deb) ...
Setting up zenoss-stack (3.0.2-0) ...

naibed:/# /etc/init.d/zenoss-stack start
nohup: redirecting stderr to stdout
Starting mysqld.bin daemon with databases from /usr/local/zenoss/mysql/data
/usr/local/zenoss/mysql/scripts/ctl.sh : mysql  started at port 3307
Daemon: zeoctl .
daemon process started, pid=2050
Daemon: zopectl .
daemon process started, pid=2061
Daemon: zenhub starting...
Daemon: zenjobs starting...
Daemon: zenping starting...
Daemon: zensyslog starting...
Daemon: zenstatus starting...
Daemon: zenactions starting...
Daemon: zentrap starting...
Daemon: zenmodeler starting...
Daemon: zenperfsnmp starting...
Daemon: zencommand starting...
Daemon: zenprocess starting...
Daemon: zenwin starting...
Daemon: zeneventlog starting...
naibed:/#

# You're done, move onto testing & pat yourself on the back
{% endhighlight %}

**4)** _Test & Enjoy:_

To test just go to a web browser & enter the IP that you choose for the machine followed by **:8080** (eg. 192.168.1.2:8080) If everything went as expected you should be greeted with a zenoss setup page
[![Success](http://farm5.static.flickr.com/4090/5063595990_07aa576991.jpg)](http://www.flickr.com/photos/68444690@N00/5063595990/)
