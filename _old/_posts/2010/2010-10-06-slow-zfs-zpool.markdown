---
date: '2010-10-06 18:45:17'
layout: post
slug: slow-zfs-zpool
status: publish
title: Slow ZFS zpool?
wordpress_id: '414'
categories:
- ZFS
tags:
- openindiana
- Opensolaris
- slow
- smartctl
- smartmontools
- tler
- wdidle
- wdtler
- zfs
---

[![IMAG0122](http://farm5.static.flickr.com/4103/5058827436_54ff6bc68a.jpg)](http://www.flickr.com/photos/68444690@N00/5058827436/)

Is a slow zfs pool giving you the blue's? Well it sure was for me, and it took me forever to figure out the problem. I scoured the internet for many solutions, many of which didn't work, and it wasn't until I used my brain did I figure out what was happening ;) Hopefully this post can reduce your frustrations, and help you resolve your slow zfs pool easily.
I checked so many things, and in the end it was something very simple. But lets start with all the things that I tried & didn't work. These are common problems with zfs when you use cheap consumer hardware. I'm using Western Digital Green Drives, and this is the price that I have to pay.

**Wrong Direction:**



	
  * **TLER - **First I thought that [Time-Limited Error Recovery (TLER)](http://en.wikipedia.org/wiki/Time-Limited_Error_Recovery) was dramatically slowing the drives down. I had heard of this happening & went off to google to find some solutions. The wikipedia page << explains that WDTLER.EXE can help, so I tried that on my warrior boot usb (I'll post this later), and it didn't work for me, it just hung. So I started up another thing on my boot usb ([PartedMagic](http://partedmagic.com)) & went the trusty ol' Open Source route with smartmontools. You need to download from svn & compile manually to get the new features. Then run the following:


{% highlight bash %}
smartctl -l scterc /dev/sda # Query the drive for TLER status
smartctl -l scterc,70,70 /dev/sda # This will set TLER to 7 seconds (default)
{% endhighlight %}

{% highlight bash %}
# You get this if your drive can't do TLER (bummer)
Warning: device does not support SCT Error Recovery Control command

# This is what you will get if your drive has TLER enabled (hooray)
SCT Error Recovery Control:
           Read:     70 (7.0 seconds)
          Write:     70 (7.0 seconds)
{% endhighlight %}

	
  * **Then I thought "You have 100% blocking":**


{% highlight bash %}
# iostat -xnz
<pre>
<pre>                    extended device statistics
    r/s    w/s   kr/s   kw/s wait actv wsvc_t asvc_t  %w  %b device
    0.0   87.0    0.0 2878.1  0.0  0.0    0.0    0.4   0 100 c4t0d0
    0.0   83.0    0.0 2878.1  0.0  0.1    0.2    0.7   1  50 c4t1d0
    1.0    0.0   28.0    0.0  0.0  0.0    0.0    5.4   0   1 c4t2d0
{% endhighlight %}

See disk c4t0d0 is at 100% blocking :( Anywho here is a fix for that:

{% highlight bash %}
su - # enter password
echo zfs_vdev_max_pending/W0t1 | mdb -kw
echo "set zfs:zfs_vdev_max_pending=1" >> /etc/system
{% endhighlight %}

This is normally the case of ZFS using incorrect scheduling & sending your cheap sata disks too many tasks & overloading them. Thankfully I didn't have blocking, but I made the patch anyways since I'm using WD Green drives.

	
  * **"Your drives are idling too much? (clicking sound)"**


Fire up smartctl again & run the following:

{% highlight bash %}

smartctl -a /dev/rdisk1 | grep ID ; smartctl -a /dev/rdisk1 | grep Start

# This is what I got on my macbook pro - 320GB caviar black drive (it's my secondary)

[jgerold@jgerold-13mbp.oc.cox.net ~]$ smartctl -a /dev/rdisk1 | grep ID ; smartctl -a /dev/rdisk1 | grep Start
ID# ATTRIBUTE_NAME          FLAG     VALUE WORST THRESH TYPE      UPDATED  WHEN_FAILED RAW_VALUE
 4 Start_Stop_Count        0x0032   099   099   000    Old_age   Always       -       1770
{% endhighlight %}

Should think about maybe replacing this soon?

Not necessarily, upon looking at the PDF from Western Digital I have 600,000 spin up/down to look forward to on this disk. I'm not sure if RAW_VALUE equates to actual spin up/down & that smartctl is just being funky?

But if you have a lower value, then I wouldn't worry; yet if your count is high & ever growing then I would recommend you boot into a windows instance (use bartpe of some sort), and grab a copy of WDIDLE.EXE ([http://webdiary.com/i/?p=515](http://webdiary.com/i/?p=515)) and turn off idleing wdiddle /d or something like that? I'm not sure I haven't tried it.

**Right Direction (This is what fixed it for me):**


I removed a horrible disk from my pool!
{% highlight bash %}zpool offline ambry c0d4{% endhighlight %}

I wish I had the readout for my 'iostat -mX' but my forth drive had a 3285ms timeout and was making the whole pool slow down to a crawl because it was failing (but not dead). I was noticing full disk usage with I would do an ls on a small dir, or copy data, or anything that required disk use. The crazy thing is that it took about 3 days for the array to recover once I removed the bad disk from the pool. Me being ever curious would run the following to monitor how fast the drives were:

{% highlight bash %}while true; iostat -mX /dev/{ct01,ct02,ct03}; sleep 1; done{% endhighlight %}

I joyfully watched my disks go from about .5 MB/s to now over 120MB/s (per disk)

**Afterthoughts:**

I wish I would have ran an iostat before trying all the things that I did :( But everything looks better in hindsight. I'm just glad that this is fixed and I was able to make a secondary backup, just in case anything else goes wrong before I get my new server up.

**Sources:**
- [http://en.wikipedia.org/wiki/Time-Limited_Error_Recovery](http://en.wikipedia.org/wiki/Time-Limited_Error_Recovery)
- [http://letsgetdugg.com/2009/10/21/zfs-slow-performance-fix](http://letsgetdugg.com/2009/10/21/zfs-slow-performance-fix)
- [http://www.csc.liv.ac.uk/~greg/projects/erc](http://www.csc.liv.ac.uk/~greg/projects/erc)
- [http://webdiary.com/i/?p=515](http://webdiary.com/i/?p=515)
