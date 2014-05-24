---
date: '2010-10-05 23:01:46'
layout: post
slug: a-little-find-magic
status: publish
title: A little find magic
wordpress_id: '410'
categories:
- Linux
tags:
- find
- irc
- mp3
- xargs
---

I was chillen in irc.freenode.net #archlinux, when an interesting question came up:


> 21:59 < treah> i need to find every mp3 file in all the sub directories of a folder and move them to the root directory of that folder that contains the sub directories


Well after a few jokes passed:

mv -r \*.mp3 ., and a few others I spent a few minutes and came up with this:

{% highlight bash %}
find . -name "*.mp3" | sed -e 's/.//./"/g' | sed -e 's/.mp3/.mp3"/g' > mp3list $$ mv `cat mp3list` .
{% endhighlight %}

A little cludge hack I know, but it "works" Then I optimized it a little more:

{% highlight bash %}
find . -name "*.mp3" -print0 -exec mv {} . ;
{% endhighlight %}

Then I just was having fun (and learned about xargs):

{% highlight bash %}
find . -name "*.mp3" -print0 | xargs -0 -I'{}' mv {} .
{% endhighlight %}



Thanks treah, it was my fun little learning experience of the night ;)
