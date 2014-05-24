---
date: '2011-12-31 00:44:28'
layout: post
slug: rtorrent-the-complete-guide
status: publish
title: rTorrent -- The complete guide
wordpress_id: '878'
categories:
- Linux
- Software
tags:
- .rtorrent.rc
- how to use rtorrent like a pro
- libtorrent
- master rtorrent
- rtorrent
- rtorrent configuration
- rtorrent setup
- rtorrent user guide
description: "Get ready for an adventure! This is one of the most comprehensive articles (or mini-tomes) I've written, covering rTorrent. I try to touch everything from just starting out, to some advanced tidbits to make torrenting easy, breazy and beautifully simple. I plan to make this into a distributable pdf or other portable format soon, so check back, I'll post the links near the top of the post. Please comment if you find any errors, have questions, or would like me to expand on anything. I would hope there aren't any glaring errors/typos, but at around 3000 words, this covers a lot, and I wouldn't be surprised if I oops'ed somewhere(s)."
---

[![](http://fsk141.com/uploads/2011/12/2011-12-24-161350_1280x800_scrot-750x468.png)](http://fsk141.com/uploads/2011/12/2011-12-24-161350_1280x800_scrot.png)

<!--
History
Installation
Configuration (.rtorrent.rc)
First Startup
Keybindings
Advanced Setup
Resources Unused (comparison with transmission)
-->

## Intro

Get ready for an adventure! This is one of the most comprehensive articles (or mini-tomes) I've written, covering rTorrent. I try to touch everything from just starting out, to some advanced tidbits to make torrenting easy, breazy and beautifully simple. I plan to make this into a distributable pdf or other portable format soon, so check back, I'll post the links near the top of the post. Please comment if you find any errors, have questions, or would like me to expand on anything. I would hope there aren't any glaring errors/typos, but at around 3000 words, this covers a lot, and I wouldn't be surprised if I oops'ed somewhere(s).

## History

Lets start with a wee bit of history... The project was started up around 2004 (I know it was previous, but I can't find an exact date).

I base this off of Jari Sundell's (Rakshasa) \[creator of lib/rTorrent] note on the newly created mailing list testing its functionality:

{% highlight bash %}
Hello, anyone there?

Rakshasa

*meowrrr*
>> http://rakshasa.no/pipermail/libtorrent-devel/2004-July/000000.html
{% endhighlight %}

At this time **(Julyish - 2004) libtorrent (0.5.5)/rTorrent (0.1.5)** was "working," but still had a long way to go from where it is today.

It's subsequent **0.6/0.2** branch had a ton of bug fixes, an addition of a throttle'ing system, and separation of openssl due to ill-licensing.

**0.7/0.3** added in udp support, torrent preallocation, min_peers, code optimization, and the almighty fast resume.

**0.8/0.4** added new throttle implementation, global limits, proxy support, interface improvements, load, load_run, untied options (killer features), scheduling, session locking. So many wonderful things were added!

**0.9/0.5** was chock full of bugfixes / minor improvements, but also ipv6 support (this was in 2006 -- eary adoption)

**0.10/0.6 - current (0.12/0.8)** had so many wonderful bug fixes & additions, it's not even worth going over them since they were all so useful to today's end result of fantastic.





## Installation


{% highlight bash %}
# you can compile from source (but I wouldn't suggest it
## you can grab the stock rTorrent (Arch Linux)
pacman -S rtorrent

## or you can get rtorrent-extended (much better, supports colors & all kinds of awesome)
yaourt -S rtorrent-extended
{% endhighlight %}



## Configuration (.rtorrent.rc)




> Lets start by taking a look at my configuration:
-- I'll reference bits of this throughout the article --
-- You can simply copypasta this into your ~/.rtorrent.rc & edit to your liking --
[https://github.com/fsk141/dotfiles/blob/master/Hot/.rtorrent.rc](https://github.com/fsk141/dotfiles/blob/master/Hot/.rtorrent.rc)

{% highlight bash %}
#
# ~/.rtorrentrc
#

# Global upload and download rate in KiB. "0" for unlimited.
download_rate = 1000
upload_rate = 50

min_peers = 50 ; look for more peers if limit doesn't reach 50
max_peers = 500 ; if there are 500 peers, don't allow any more

# Same as above but for seeding completed torrents (-1 = same as downloading)
min_peers_seed = 10
max_peers_seed = 100

# Maximum number of simultanious uploads per torrent.
max_uploads = 25

port_range = 6881-6999 ; ports to use for listening

# Start opening ports at a random position within the port range.
port_random = yes 

check_hash = yes ; check hash on finished torrents

# encryption settings
encryption = allow_incoming,enable_retry,prefer_plaintext

use_udp_trackers = yes ; setup client to use udp (stateless) trackers

# DHT clientless tracker
dht = auto
dht_port = 6881
peer_exchange = yes

# Session tmp file (relative dir is good, absolute is bad)
session = ./Downloads/.session

# Default directory to save the downloaded torrents.
directory = ./Downloads/

# Close torrents when diskspace is low.
schedule = low_diskspace,5,60,close_low_diskspace=2048M

# Watch a directory for new torrents, and stop those that have been deleted (^d)
schedule = watch_directory,5,5,"load_start=./Downloads/watch/*.torrent"
schedule = watch_directory_2,5,5,"load_start=~/Downloads/watch/music/*.torrent,d.set_directory=~/Downloads/music/"
schedule = watch_directory_3,5,5,"load_start=~/Downloads/watch/television/*.torrent,d.set_directory=~/Downloads/television/"
schedule = watch_directory_4,5,5,"load_start=~/Downloads/watch/books/*.torrent,d.set_directory=~/Downloads/books/"
schedule = untied_directory,5,5,stop_untied= ; stop torrents once removed from the client

# Colors # only in rtorrent extended
## Colors: 0 = black 1 = red 2 = green 3 = yellow 4 = blue 5 = magenta 6 = cyan 7 = white

done_fg_color = 6
#done_bg_color
active_fg_color = 7
#active_bg_color
{% endhighlight %}

Configuring rTorrent, and any torrent client, can make or break your experience with torrenting. It's very important to set throttleing, port forwarding, and peer settings specific to your network. I would start by navigating to [http://speedtest.net](http://speedtest.net) and running a test to figure out your network speeds.

[![](http://fsk141.com/uploads/2011/12/speedtest.png)](http://fsk141.com/uploads/2011/12/speedtest.png)

> 
> ### Throttleing
> 
> 
After sizing up your network connection, you need to do a little math (or googleing). Speedtest.net gives you your networking potential in network terms (Mbps -- Mega bits per second), yet you want to know KBps (Kilo Bytes per second)

------
Quick lesson on bits vs Bytes:
* There are 8 bits in a byte
-- **networking (per second):** bit/ps (1) > kilobit/ps (1000) > megabit/ps (1000000) > gigabit > terabit
-- **filesystems:** bit (1) > Byte (8) > kiloByte (1024) > megaByte (1048576)> gigaByte > teraByte
* Converting bits to Bytes:
-- 100kbps (100 kilo bits per second) = 100/8 (bits to Bytes) = 12.5 KBps (12.5 kilo bytes per second)
-- 1Mbps (1 Mega bit per second) = 1\*1024/8 (converting Mbit > KByte)
------

Easiest way to convert is to just use google calculator (16.35Mbps in KBps):
[![](http://fsk141.com/uploads/2011/12/google-750x164.png)](http://fsk141.com/uploads/2011/12/google.png)

Now that you have your rate of bits in Bytes; we need to do a little more math.

My general rule of thumb is to allocate a maximum of 75% network potential to torrenting, and to leave the other 25% for general web traffic (web browsing, youtubing, netflix). So if I have 2092KBps download bandwith I would like to give rTorrent about 1569 (2092*.75). And for upload potential 267.84 (357.12*.75).

I scale back since there's a lot of netflix going on, and set my max download rate to 1000KBps and my upload rate to 50KBps.

{% highlight bash %}
# Global upload and download rate in KiB. "0" for unlimited.
download_rate = 1000
upload_rate = 50
{% endhighlight %}

Other that setting a proper throttleing speed, it's also necessary to set proper min/max_peer settings. According to the bittorrent spec, you should never have more than 5-10 torrents open at one time (unless you have a multi-Mbit connection). The reasoning behind this is that it hurts the network to have torrent clients taking up peer slots, and not being able to download/upload anything due to all your bandwith being used up. Setting min/max_peers allows rTorrent to scale based on your personal network

> 
> ### min_peers/max_peers
> 
> 
There is probably a formula to associate min/max_peers with your network, but I just took a guess, and it works very well for me, and should scale well for big/small networks

If you want to know more about min/max_peers please check out one of my other posts:
[http://fsk141.com/what-is-min_peers-for-and-why-should-i-uncomment-it](http://fsk141.com/what-is-min_peers-for-and-why-should-i-uncomment-it)

{% highlight bash %}
min_peers = 50 ; look for more peers if limit doesn't reach 50
max_peers = 500 ; if there are 500 peers, don't allow any more

# Same as above but for seeding completed torrents (-1 = same as downloading)
min_peers_seed = 10
max_peers_seed = 100
{% endhighlight %}

> 
> ###  -- Sidenote --
> 
> 
Basic format of .rtorrent.rc config variables are

{% highlight bash %}config_name = value{% endhighlight %}

You can add comments with **#** or inline with **;**

{% highlight bash %}
#comment
config_name = value

config_name = value ; inline comment (haven't seen this documented, but it works)
{% endhighlight %}


> 
> ### Max uploads
> 
> 
I have my rate set a bit high at 25 max uploads; but I have a big network to support it. I would suggest setting this to around 8-10 for lesser networks. Like I touched on earlier, it's not helpful to have a bunch of open connections if your bandwith can't support them.
{% highlight bash %}
# Maximum number of simultanious uploads per torrent.
max_uploads = 25
{% endhighlight %}


> 
> ### Port forwarding
> 
> 
It's unholy to start any torrent client without opening ports. Most clients have gotten smarter with udp, and dht (trackerless) where you can get away downloading files, and uploading a little. Yet the preferred clients are the ones that have open ports, and are connectable by others.

Google around for port forwarding, or just login to your router (192.168.1.1), and look for port forwarding, virtual servers, or something similar. I opened a port range 6881-6999 (default torrent port range) on my router and allow rTorrent to access the entire port space randomly.
{% highlight bash %}
port_range = 6881-6999 ; ports to use for listening

# Start opening ports at a random position within the port range.
port_random = yes 
{% endhighlight %}


> 
> ### The rest
> 
> 
I commented my .rtorrent.rc pretty well, so you can understand what things do. Things like session_dir & directory are important for basic functionality to work:

{% highlight bash %}
# Session tmp file (relative dir is good, absolute is bad)
session = ./Downloads/.session

# Default directory to save the downloaded torrents.
directory = ./Downloads/
{% endhighlight %}

Everything else in my configuration file is to make things simplier, but are optional, a little complicated, and I will go over them below in the Advanced Setup section.

## First Startup

### Initiate Countdown

Now that you have a smashing .rtorrent.rc you're ready to fire up rTorrent & get torrenting.

Oh, wait! How about a super tip!

Instead of running rTorrent in the foreground, I suggest initiating a screen session previous to starting rTorrent. This is just in case you close your terminal.

Simple screen primer (`man screen` to get more info):
{% highlight bash %}
screen #starts a new screen session
rtorrent #starts rTorrent in new screen session
------
^a d # disconnects from screen session
screen -x # reconnects to screen session
{% endhighlight %}

So now that you have a new screen session, and started up rTorrent, lets take a look around, and see what we have. You can arrow around, and try to figure things out, but I would highly suggest running a `man rtorrent`

### Annotated rTorrent

Let's start with a blank slate so we can go over the basics:
[![](http://fsk141.com/uploads/2011/12/annotated_blank-750x468.png)](http://fsk141.com/uploads/2011/12/annotated_blank.png)

This should make sense to even the dumbest among us. The annotated version of the rTorrent gui shows everything you need to know. The important things are on the bottom left, and everything else is secondary to that ;)

### Load Your First Torrent

If I were you, I would grab a torrent and load it up so that you can follow along with the rest of the tutorial. The easiest way to do this is to download a torrent < How about [arch_core-dual](http://www.archlinux.org/iso/2011.08.19/archlinux-2011.08.19-core-dual.iso.torrent) >

After you have the .torrent file, head into rTorrent. 
{% highlight bash %}
click <enter>

# you'll receive a load> prompt
## enter the path to the .torrent file
### for me it's in:
### ~/Downloads/archlinux-2011.08.19-core-dual.iso.torrent

load> ~/Downloads/archlinux-2011.08.19-core-dual.iso.torrent

#### tab completion works:
#### ~/Downloads/arch<tab> would do the trick!
{% endhighlight %}

This will add the torrent into rTorrent in a stopped state. If you would like the torrent to start automatically, then use  instead of  to load your torrent.
{% highlight bash %}
# hover over the torrent (arrow keys)

^s # ctrl-s -- to start the torrent

## unless you used <backspace> in which case it should auto start
{% endhighlight %}

Now that we have something started, you should see your bottom status bar light up. Numbers should fly by... Telling you the speed of download/upload, connected peers, and everything you need to know.

### Get Torrent Details

At this time start poking your keyboard >
{% highlight bash %}
# hover over a torrent

<right arrow>

## you'll now be in torrent view mode
## hover over the right fields to view contents

<up arrow> & <down arrow> #to navigate sub fields
<right arrow> & <left arrow> to view properties

### Choices
#### Peer List
#### Info
#### File List
#### Tracker List
#### Chunks Seen
#### Transfer List
{% endhighlight %}

### Some Other Things

I would try hitting your number keys (from 1 to 0). This will give you your different views (active, seeding, inactive, etc.)

asd zxc ASD ZXC (play around with these keys for modifying throttle on the fly.

{% highlight bash %}
^s (start)
^d (stop) -- hit twice to remove
^r (hash check)
{% endhighlight %}

## Keybindings

How about a full rundown on all the special keys for rTorrent. I know it's a little tedious to look at tables of information, but try your hand at most of the keybindings that are outlined below, and it'll become second nature in a matter of minutes.

### Lets Start With The Basics
  
<table>
<tbody>
<tr>
<td>backspace</td>
<td>Add torrent using an URL or file path. Use tab to view directory content and do auto-complete. Also, wildcards can be used. For example: ~/torrent/*</td>
</tr>
<tr>
<td>return</td>
<td>Same as backspace, except the torrent remains inactive. (Use ^s to activate)</td>
</tr>
<tr>
<td>^o</td>
<td>Set new download directory for selected torrent. Only works if torrent has not yet been activated.</td>
</tr>
<tr>
<td>^s</td>
<td>Start download. Runs hash first unless already done.</td>
</tr>
<tr>
<td>^d</td>
<td>Stop an active download, or remove a stopped download.</td>
</tr>
<tr>
<td>^k</td>
<td>Stop and close the files of an active download.</td>
</tr>
<tr>
<td>^r</td>
<td>Initiate hash check of torrent. Without starting to download/upload.</td>
</tr>
</tbody>
</table>




### Throttle Control


This is for global throttleing, and changes can be viewed in the bottom left.
<table>
<tbody>
<tr>
<td>a/s/d</td>
<td>Increase the upload throttle by 1/5/50 KB.</td>
</tr>
<tr>
<td>z/x/c</td>
<td>Decrease the upload throttle by 1/5/50 KB.</td>
</tr>
<tr>
<td>A/S/D</td>
<td>Increase the download throttle by 1/5/50 KB.</td>
</tr>
<tr>
<td>Z/X/C</td>
<td>Decrease the download throttle by 1/5/50 KB.</td>
</tr>
</tbody>
</table>



### Main View Navigation


[![](http://fsk141.com/uploads/2011/12/2011-12-26-001502_1280x800_scrot-750x468.png)](http://fsk141.com/uploads/2011/12/2011-12-26-001502_1280x800_scrot.png)


<table>
<tbody>
<tr>
<td>^q</td>
<td>Initiate shutdown, press again to force the shutdown and skip sending the stop signal to trackers.</td>
</tr>
<tr>
<td>up arrow/down arrow</td>
<td>Select item.</td>
</tr>
<tr>
<td>left</td>
<td>Go back to the previous screen.</td>
</tr>
</tbody>
</table>


### Download View

<table>
<tbody>
<tr>
<td>right</td>
<td>Switch to selected view</td>
</tr>
<tr>
<td>left</td>
<td>Switch to view selection or back to main view</td>
</tr>
<tr>
<td>1/2</td>
<td>Adjust max uploads.</td>
</tr>
<tr>
<td>3/4</td>
<td>Adjust min peers.</td>
</tr>
<tr>
<td>5/6</td>
<td>Adjust max peers.</td>
</tr>
<tr>
<td>p</td>
<td>Display peer list</td>
</tr>
<tr>
<td>o</td>
<td>Display torrent info</td>
</tr>
<tr>
<td>i</td>
<td>Display file list</td>
</tr>
<tr>
<td>u</td>
<td>Display tracker list</td>
</tr>
<tr>
<td>t/T</td>
<td>Initiate tracker request. Use capital T to force the request, ignoring the "min interval" set by the tracker.</td>
</tr>
</tbody>
</table>


### Peer List

[![](http://fsk141.com/uploads/2011/12/peer_list-750x468.png)](http://fsk141.com/uploads/2011/12/peer_list.png)


<table>
<tbody>
<tr>
<td>left</td>
<td>Switch to view selection</td>
</tr>
<tr>
<td>right</td>
<td>Show peer details</td>
</tr>
<tr>
<td>\*</td>
<td>Snub peer (stop uploading to this peer)</td>
</tr>
<tr>
<td>k</td>
<td>Kick peer (disconnect from peer)</td>
</tr>
<tr>
<td>B</td>
<td>Ban peer (No unbanning is possible.) 0.8.4+</td>
</tr>
</tbody>
</table>



### File List


[![](http://fsk141.com/uploads/2011/12/file_list-750x468.png)](http://fsk141.com/uploads/2011/12/file_list.png)


<table>
<tbody>
<tr>
<td>left</td>
<td>Switch to view selection</td>
</tr>
<tr>
<td>right</td>
<td>Show file details</td>
</tr>
<tr>
<td>space</td>
<td>Change the file priority; applies recursively when done on a directory</td>
</tr>
<tr>
<td>\*</td>
<td>Change the priority of all files</td>
</tr>
<tr>
<td>/</td>
<td>Collapse directories. While collapsed, press right to expand the selected directory.</td>
</tr>
</tbody>
</table>



### Tracker List


[![](http://fsk141.com/uploads/2011/12/tracker_list-750x468.png)](http://fsk141.com/uploads/2011/12/tracker_list.png)


<table>
<tbody>
<tr>
<td>left</td>
<td>Switch to view selection</td>
</tr>
<tr>
<td>\*</td>
<td>Enable/disable tracker</td>
</tr>
<tr>
<td>space</td>
<td>Rotate trackers in a group</td>
</tr>
</tbody>
</table>


## Advanced Setup

dive into watch directories & some more powerful things (scgi & external clients)

I hope that you've learned at lest one tidbit of awesomeness by now. If you have, great, if not, hopefully this section will tickle your fancy.

### Scheduling

Starting out with rTorrent is rough. You're given almost a blank slate, a config file with some complex things that are foreign (coming from a nice gui client that does everything for you). But once you learn the power behind having a solid .rtorrent.rc, you realize why you've switched, and why you'll never go back. Scheduling is one such feature that is unbelievably powerful.

We'll start with the simplest of schedule scripts. It's purpose is to make sure you don't overfill your system with bits & bytes.



#### Excerpt from rtorrent man page on scheduling


>> schedule = id,start,interval,command
    Call  command  every  interval  seconds, starting from start. An interval of zero calls the task once, while a start of zero calls it immediately. Currently command is forwarded to the option handler.  start and interval may optionally use a time format, dd:hh:mm:ss. F.ex to start a task every day at 18:00, use 18:00:00,24:00:00.



The following task will prevent rTorrent from downloading too much, and entirely filling your hard drive up with data.
{% highlight bash %}
# Close torrents when diskspace is low.
##
## schedule (initiate a new schedule script)
## low_diskspace (scheduler name)
## 5,60 (start 5 seconds after rtorrent starts; run every 60 seconds)
schedule = low_diskspace,5,60,close_low_diskspace=2048M
{% endhighlight %}

My most favorite of configurations are the following. It simplifies my flow of downloading, and makes it a breeze to add an infinite number of torrents all at once (your computer/network permitting). The following schedulers are for watch directories. A watch directory watches for new \*.torrent files, and loads them into rtorrent. You can simply add a watch folder, and you will have a bunch of torrents start up in your default download_dir. Or you can add a watch folder with a d.set_directory set to the folder in which you would like the torrent stored (golden awesome rainbow sauce)

It looks something like this:
{% highlight bash %}
# download to default directory (download_dir)
## start after 5; run every 5 seconds
## load all \*.torrent files in ./Downloads/watch
schedule = watch_directory,5,5,"load_start=./Downloads/watch/*.torrent"

# download to ~/Downloads/music/
## schedule (initiate a new schedule script)
## watch_directory_2 (names watch_directory script)
### you must have a different name for every watch_directory
## 5,5 (start after 5; run every 5 seconds)
## load_start=~/Downloads/watch/music/*.torrent (load all \*.torrent files in ~/Downloads/watch/music)
## d.set_directory=~/Downloads/music/ (download files to ~/Downloads/music)

schedule = watch_directory_2,5,5,"load_start=~/Downloads/watch/music/*.torrent,d.set_directory=~/Downloads/music/"

# and here are all my watch_directory scripts together
# Watch a directory for new torrents, and stop those that have been deleted (^d)

schedule = watch_directory,5,5,"load_start=./Downloads/watch/*.torrent"
schedule = watch_directory_2,5,5,"load_start=~/Downloads/watch/music/*.torrent,d.set_directory=~/Downloads/music/"
schedule = watch_directory_3,5,5,"load_start=~/Downloads/watch/television/*.torrent,d.set_directory=~/Downloads/television/"
schedule = watch_directory_4,5,5,"load_start=~/Downloads/watch/books/*.torrent,d.set_directory=~/Downloads/books/"
schedule = untied_directory,5,5,stop_untied= ; stop torrents once removed from the client
{% endhighlight %}



#### Stop torrents at certain ratio

Say you're generous, but not too generous. You would like to seed back the torrents you have downloaded, yet you don't want to give away your bandwith indefinitely. Then this is the solution for you:

{% highlight bash %}
# stop_on_ratio = min_ratio,min_upload,max_ratio
## schedule (initiate a new schedule script)
## ratio (uses ratio scheduler)
## 60,60 (start after 60, run every 60 seconds)
## stop_on_ratio=200,50M,300 (200% [min_ratio], 50M [min_upload], 300% [max_ratio])

schedule = ratio,60,60,"stop_on_ratio=200,50M,300"
{% endhighlight %}



#### Setting scheduled throttle times



So you want ALL of your bandwith, uninterrupted, during the whole day. Most likely you want to heavily throttle durring the day, and open up the floodgates at night. I've outlined a simple scheduler to have unlimited upload/download from 22:00 - 06:00, and throttle during everything else:

{% highlight bash %}
#set daily download/upload rates
#
# Apply 25KB/s down & 5KB/s up at 06:00

schedule = throttle_1,06:00:00,24:00:00,download_rate=25
schedule = throttle_2,06:00:00,24:00:00,upload_rate=5

# Apply unlimited upload/download at 22:00
schedule = throttle_3,22:00:00,24:00:00,download_rate=0
schedule = throttle_4,22:00:00,24:00:00,upload_rate=0

{% endhighlight %}


#### Using another config file (other than ~/.rtorrent.rc)


Once upon a time I wanted to run rTorrent as root, I wanted to run torrenting over port 443 (https) or 80 (http) so that I could torrent in a blocked network. I needed to execute rTorrent with root since all ports under 1000 aren't allowed to be allocated to regular users. Instead of setting everything up for the root user, just use sudo and specify the configuration file manually:

{% highlight bash %}
rtorrent -n -o import=~/my_other_rtorrent.rc
{% endhighlight %}

I have more things to add to this section, but I need a better way to organize them. You'll just have to wait for future updates.


## Resources Unused (comparison with transmission)

[![](http://fsk141.com/uploads/2011/12/resources-750x468.png)](http://fsk141.com/uploads/2011/12/resources.png)

This was quite shocking to say the least. I didn't really think there would be a literal ton of difference between rTorrent and Transmission. I've always known rTorrent to be the best contender in minimalist, low resource torrent clients. Yet Transmission is also touted as lightweight. Yet as you can see above rTorrent never really swayed from 1-3% cpu usage, whereas Transmission spawned a gazillion processes and had about 25-50% cpu usage.

[![](http://fsk141.com/uploads/2011/12/transmission-750x468.png)](http://fsk141.com/uploads/2011/12/transmission.png)

I should really need to go into more detail, since well everyone knows that rTorrent is the almost resourceless client. It has almost no overhead, uses minimal resources, is programmed well, with performance in mind, and has a large community of users behind to support it's overall success.


**Resources:**
  1. `man rtorrent`
  2. [http://libtorrent.rakshasa.no/wiki/RTorrentUserGuide](http://libtorrent.rakshasa.no/wiki/RTorrentUserGuide)
  3. [http://libtorrent.rakshasa.no/wiki/RTorrentCommonTasks](http://libtorrent.rakshasa.no/wiki/RTorrentCommonTasks)
  4. [https://wiki.archlinux.org/index.php/RTorrent](https://wiki.archlinux.org/index.php/RTorrent)
