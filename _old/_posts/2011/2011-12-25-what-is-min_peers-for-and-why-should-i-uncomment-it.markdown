---
date: '2011-12-25 00:27:59'
layout: post
slug: what-is-min_peers-for-and-why-should-i-uncomment-it
status: publish
title: '[rTorrent] What is min_peers for, and why Should I Uncomment it?'
wordpress_id: '1078'
categories:
- Linux
- Software
description: "After using rTorrent, and always having min_peers commented out in my .rtorrent.rc. I figured that I would try and figure out what it does, and why I should uncomment it. Simply put, it's one of the simplest uncommentable things in your config that drastically improves performance."
---

[![](http://fsk141.com/uploads/2011/12/2011-12-24-161910_1280x800_scrot-750x468.png)](http://fsk141.com/uploads/2011/12/2011-12-24-161910_1280x800_scrot.png)

After using rTorrent, and always having min_peers commented out in my .rtorrent.rc. I figured that I would try and figure out what it does, and why I should uncomment it. Simply put, it's one of the simplest uncommentable things in your config that drastically improves performance.

Here is a snippet from my current config, and a generalization of what min/max peers does.

{% highlight bash %}
min_peers = 50 ; look for more peers if limit doesn't reach 50
max_peers = 500 ; if there are 500 peers, don't allow any more

# Same as above for seeding completed torrents (-1 = same as downloading)
min_peers_seed = 10
max_peers_seed = 100
{% endhighlight %}


**max_peers** should make sense (if your client has more than the alloted peers, don't let anyone else connect)

**min_peers** is the exact opposite, if you have a min_peers count set, rTorrent will keep branching out to find more peers until the condition is met.



> "* When the download has less than min_peers connections, it will after 30 seconds request more from the trackers. This will only happen maximum 5 times and only if more peers have been connected to in the mean time. It supports multiple trackers by trying the other groups, though not other trackers within a group, if the first does not provide more peers."   
--libtorrent mailing list



When you connect to a bt-tracker, your client will automatically initiate connections with happy peers, and once you start downloading, it will check for new peers every once in a while. But if you are connected to a few good peers, and you don't have min_peers set, rTorrent expects you're happy, and just continues with those. If you're leeching &amp; need to download at a better rate, or are seeding and want your torrent to be seeded to as many peers as possible, then it's to your advantage to set min_peers to a higher number.

The higher you set the number (min_peers), the more inclined you are to share/seed your torrents.

The lower the number (min_peers), the less inclined you are to share your torrents with others.

It's awesome that the architects of rTorrent allow an overall (min/max_peers) and a seeding specific (min/max_peers_seed).

Now you can happily set min_peers and get more happy customers connecting to your torrents, Enjoy!



Resources:



	
  1. [http://rakshasa.no/pipermail/libtorrent-devel/2005-August/000305.html](http://rakshasa.no/pipermail/libtorrent-devel/2005-August/000305.html)


