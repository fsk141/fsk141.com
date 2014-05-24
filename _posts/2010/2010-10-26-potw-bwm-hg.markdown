---
date: '2010-10-26 22:08:55'
layout: post
slug: potw-bwm-hg
status: publish
title: '[PotW] bwm-hg'
wordpress_id: '524'
categories:
- Networking
- Software
tags:
- bandwith
- bwm-ng
- laptop
- NFS
- PotW
- throughput
- zfs
---

[![little-throughput](http://farm2.static.flickr.com/1311/5126378313_1a9ff96454.jpg)](http://www.flickr.com/photos/68444690@N00/5126378313/)

I was pressed to figure out how much throughput was going over an interface for my laptop; a few minutes later I found this gem! It has one purpose, and it does it exceptionally well! Well I lie, it has a few purposes, but it's main goal (and title) is to monitor bandwith \[*B*and *W*ith *M*onitor - *N*ext *G*eneration]. Why would I want to monitor my bandwidth? Well for this usage case I wanted a simple method to view what kind of nfs throughput I was getting on my laptop. Bingo, I spent a little over a millisecond installing bwm-ng &amp;&amp; was very happy to see exactly what I was looking for without having to do any special flags, or configuring the program, or anything. I quickly hit the *h* to get a few little tidbits of greatness; like how to change the probe timeout, and other magical things. I love this little program and use it on all my machines now for quick throughput monitoring on my network/disks (ya, it does disks too)

**Program:** bwm-ng

**Site Link:** [http://www.gropp.org/?id=projects&sub=bwm-ng](http://www.gropp.org/?id=projects&sub=bwm-ng)[](http://tmux.sourceforge.net)

**Download:**



	
  1. Download direct: [http://www.gropp.org/bwm-ng/bwm-ng-0.6.tar.gz](http://www.gropp.org/bwm-ng/bwm-ng-0.6.tar.gz)

	
  2. Use CVS & get the newest version: svn co https://svn.sourceforge.net/svnroot/bwmng/trunk bwm-ng


**Installation:**



	
  1. Use your package manager (pacman -S bwm-ng for archlinux)

	
  2. Mac (download [homebrew](http://github.com/mxcl/homebrew) &amp;&amp; **sudo  brew install tmux**)

	
  3. Compile manually >>


**Compile:**

{% highlight bash %}

./configure

make -j8 (make with 8 jobs at once [compile faster on a core duo])

sudo make install

{% endhighlight %}

**How to use:**

First off execute `bwm-ng`, you will be greeted with your active interfaces. Shown will be TX (Transmit), RX (Receive) & Total &amp;&amp; Total (of everything).

[![mac-ethernet](http://farm2.static.flickr.com/1063/5126980974_c5fe5103de.jpg)](http://www.flickr.com/photos/68444690@N00/5126980974/)

Right from the get go I hit the 'h' key for help, and quickly got acquainted with some powerful letters. 'd', 'u', 'n', 't' are my favs; they are the heart of the application. I started out by changing the default value shown to "auto" with 'd', then I changed the type of output to "average" with the letter 't'. I was tickled pink that in less than 5 minutes I had what I needed, with no hassle what-so-ever. Some other glorious things that this program does is allow you to select how you would like to output the data (ncurses, ncurses2 (colors & stuff), plain (text), and html).

All of these features also apply to monitoring disks as well. Just punch the 'n' key a few times until disks show up...

If I haven't expressed it already, I really like this program, and it's one of the ones that I'll be using regularly from now on (Until I find something better :)
**Keybindings:**

{% highlight bash %}

'h'  show this help
 'q'  exit
 '+'  increases timeout by 100ms
 '-'  decreases timeout by 100ms
 'd'  switch KB and auto assign Byte/KB/MB/GB
 'a'  cycle: show all interfaces, only those which are up,
only up and not hidden
 's'  sum hidden ifaces to total aswell or not
 'n'  cycle: input methods
'u'  cycle: bytes,bits,packets,errors
 't'  cycle: current rate, max, sum since start, average for last 30s

{% endhighlight %}

**--help:
**

{% highlight bash %}
bwm-ng --help
Bandwidth Monitor NG (bwm-ng) v0.6
Copyright (C) 2004-2007 Volker Gropp <bwmng@gropp.org>
USAGE: bwm-ng [OPTION] ... [CONFIGFILE]
displays current ethernet interfaces stats

Options:
 -t, --timeout <msec>    displays stats every <msec> (1msec = 1/1000sec)
 default: 500
 -d, --dynamic [value]   show values dynamicly (Byte KB or MB)
 -a, --allif [mode]      where mode is one of:
 0=show only up (and selected) interfaces
 1=show all up interfaces (default)
 2=show all and down interfaces
 -I, --interfaces <list> show only interfaces in <list> (comma seperated), or
 if list is prefaced with % show all but interfaces
 in list
 -S, --sumhidden [value] count hidden interfaces for total
 -A, --avglength <sec>   sets the span of average stats (Default 30s)
 -D, --daemon [value]    fork into background and daemonize
 -h, --help              displays this help
 -V, --version           print version info

Input:
 -i, --input <method>    input method, one of: getifaddrs sysctl netstat ioservice

Output:
 -o, --output <method>   output method, one of:
 plain, curses, curses2, csv, html
 -u, --unit <value>      unit to show. one of bytes, bits, packets, errors
 -T, --type <value>      type of stats. one of rate, max, sum, avg
 -C, --csvchar <char>    delimiter for csv
 -F, --outfile <file>    output file for csv and html (default stdout)
 -R, --htmlrefresh <num> meta refresh for html output
 -H, --htmlheader        show <html> and <meta> frame for html output
 -c, --count <num>       number of query/output for plain & csv
 -N, --ansiout           disable ansi codes for plain output
 (ie 1 for one single output)

{% endhighlight %} 
