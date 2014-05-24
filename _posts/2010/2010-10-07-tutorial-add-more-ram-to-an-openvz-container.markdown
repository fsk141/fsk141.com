---
date: '2010-10-07 15:45:35'
layout: post
slug: tutorial-add-more-ram-to-an-openvz-container
status: publish
title: '[Tutorial] Add more RAM to an OpenVZ container'
wordpress_id: '423'
categories:
- Linux
- Tutorials
tags:
- OpenVZ
- pagesize
- ram
---

![extra ram](http://imgur.com/MtiLa.jpg)

It seems like a simple idea, but adding ram to an OpenVZ container is a little bit tricky.

Lets say we would like to have 512MB dedicated & 1024MB Burst

In OpenVZ l33t Sp3ak it would look something like this:

{% highlight bash %}
vzctl set <vpsid> --vmguarpages 131072 --save # 512MB Dedicated (Guaranteed)

vzctl set <vpsid> --privvmpages 262144 --save # 1024 Burst (Granted)
{% endhighlight %}

For the longest time it was a pain in the ass to grasp this concept, and I would use someone else's config file & just copy those properties when I knew they allocated 512MB ram, or the like. Anywho I don't know why the OpenVZ wiki can't be simpler, or their 'vzctl' program for that matter. Why can't you just specify the KB/MB/GB value that you want??? Well they don't make it easy, and here's "The Easy Way"

{% highlight bash %}
vzctl set <vpsid> --vmguarpages $((256 * 512)) --save #  512MB Dedicated (Guaranteed)
// 512MB * <whatever_MB_you_want>

vzctl set <vpsid> --privvmpages $((256 * 1024)) --save # 1024 Burst (Granted)
// 1024MB * <whatever_MB_you_want>
{% endhighlight %}

Q:
> Why & how does this sorcery work?


A:
> Lets pick apart the commands declaration (`--vmguarpages 131072)`


`131072 (number of pages)*4096(how big a page is)/1024 (knock up to KB)/1024 (knock up to MB)`

{% highlight bash %}((131072*4096)/1024)/1024{% endhighlight %}


> Oh, ok great, that really explains things? (you might say)? Well lemme go a little deeper & give you a little example to execute on your Host Node (HN)


{% highlight c %}
// Find the pagesize of your machine (x86/x86_64 should be 4096)

#include <unistd.h>
#include <stdio.h>

int main(int argc, char* argv[])
{
long sz = sysconf(_SC_PAGESIZE);
printf("Memory pagesize on this box : %i Bytesn", sz);
return 0;
}

// compile & execute
// copy to a file (pages.cpp) g++ -o pages pages.cpp ; chmod +x ./pages ; ./pages
// you "should" get 4096 Bytes (unless you're on ia64 hardware)
{% endhighlight %}

Now that you've verified your page size, lets move onto how 512MB computes to 131072 pages:

4096 Bytes = 1 page = 4 Kilobytes
Since there are 1024 KB in a MB lets calculate for pages which is 1024/4 (where 1024 is a MB we are calculating for one page (4 part groups of 1024) == 256

**What is 256?** 256 is the number of pages in a MB, so if we multiply 256 *  (number of MB's RAM we want) that will give us the number of pages to correctly give OpenVZ the amount of pages it needs.

{% highlight bash %}
// some examples
1GB (1024 * 256) = 262144
vzctl set <vpsid> --vmguarpages 262144 --save // if you pre-calculate
vzctl set <vpsid> --vmguarpages $((256 * 1024)) --save // this just calculates with bash

16GB (18432 * 256) = 4718592
vzctl set <vpsid> --vmguarpages 4718592 --save
vzctl set <vpsid> --vmguarpages $((256 * 18432)) --save
{% endhighlight %}

Well I hope this helped someone like me that couldn't for the life of me understand what the hell they were talking about on the OpenVZ wiki. It wasn't until I hit a tiny tidbit of happiness from the linux.com article (where they did 256 * 256), then it took me a little longer reading the Setting_UBC_parameters to figure out page size, and after I knew pages were 4KB, I thought it would be nice to outline my findings. Enjoy :)

**Sources:**

[http://wiki.openvz.org/Setting_UBC_parameters](http://wiki.openvz.org/Setting_UBC_parameters)
[http://wiki.openvz.org/UBC_primary_parameters](http://wiki.openvz.org/UBC_primary_parameters)
[http://wiki.openvz.org/UBC_secondary_parameters](http://wiki.openvz.org/UBC_secondary_parameters)
[http://www.linux.com/archive/articles/114214](http://www.linux.com/archive/articles/114214)
