---
date: '2011-11-14 01:20:17'
layout: post
slug: dwm-colors
status: publish
title: Dwm Colors
wordpress_id: '950'
categories:
- Linux
- Software
tags:
- config.h
- dwm
- dwm colors
- dwm.suckless.org
- nofmfgcolor
- normbgcolor
- normbordercolor
- selbgcolor
- selbordercolor
- selfgcolor
---

[![](http://fsk141.com/uploads/2011/11/firedwm-750x562.png)](http://fsk141.com/uploads/2011/11/firedwm.png)

Who says that dwm ([dwm.suckless.org](http://dwm.suckless.org)) has to be boring, default, and drab? I don't, and unless you're too lazy to edit a couple color lines, you shouldn't agree either. I've spent a lot of my life tweaking and playing with dwm. Not out of hassle, or unhappiness, but to make a great thing better, and more suited to my needs. Coloring dwm is one of the simplest, yet most satisfying things. I have a few color schemes that I made, but please feel free to provide me with your color schemes, and I would be happy to include them in the post (in future updates).

**01-Default:**
[![](http://fsk141.com/uploads/2011/11/0-Default-750x562.png)](http://fsk141.com/uploads/2011/11/0-Default.png)

{% highlight bash %}
0-Default
static const char normbordercolor[] = "#cccccc";
static const char normbgcolor[]     = "#cccccc";
static const char normfgcolor[]     = "#000000";
static const char selbordercolor[]  = "#0066ff";
static const char selbgcolor[]      = "#0066ff";
static const char selfgcolor[]      = "#ffffff";
{% endhighlight %}

**1-WoB (White on Black):**
[![](http://fsk141.com/uploads/2011/11/1-WoB-750x562.png)](http://fsk141.com/uploads/2011/11/1-WoB.png)

{% highlight bash %}
1-WoB
static const char normbordercolor[] = "#3E3E3E";
static const char normbgcolor[]     = "#000000";
static const char normfgcolor[]     = "#FFFFFF";
static const char selbordercolor[]  = "#000000";
static const char selbgcolor[]      = "#FFFFFF";
static const char selfgcolor[]      = "#000000";
{% endhighlight %}

**2-BoW (Black on White):**
[![](http://fsk141.com/uploads/2011/11/2-BoW-750x562.png)](http://fsk141.com/uploads/2011/11/2-BoW.png)

{% highlight bash %}
2-BoW
static const char normbordercolor[] = "#3E3E3E";
static const char normbgcolor[]     = "#FFFFFF";
static const char normfgcolor[]     = "#000000";
static const char selbordercolor[]  = "#FFFFFF";
static const char selbgcolor[]      = "#000000";
static const char selfgcolor[]      = "#FFFFFF";
{% endhighlight %}

**3-GoogleGrey (New Top Bar):**
[![](http://fsk141.com/uploads/2011/11/3-GoogleGrey-750x562.png)](http://fsk141.com/uploads/2011/11/3-GoogleGrey.png)

{% highlight bash %}
3-GoogleGrey
static const char normbordercolor[] = "#2D2D2D";
static const char normbgcolor[]     = "#2D2D2D";
static const char normfgcolor[]     = "#CCCCCC";
static const char selbordercolor[]  = "#CCCCCC";
static const char selbgcolor[]      = "#4C4C4C";
static const char selfgcolor[]      = "#CCCCCC";
{% endhighlight %}

**4-LightGrey:**
[![](http://fsk141.com/uploads/2011/11/4-LightGrey-750x562.png)](http://fsk141.com/uploads/2011/11/4-LightGrey.png)

{% highlight bash %}
4-LightGrey
static const char normbordercolor[] = "#3E3E3E";
static const char normbgcolor[]     = "#FFFFFF";
static const char normfgcolor[]     = "#000000";
static const char selbordercolor[]  = "#999999";
static const char selbgcolor[]      = "#999999";
static const char selfgcolor[]      = "#FFFFFF";
{% endhighlight %}

**5-SkyBlue:**
[![](http://fsk141.com/uploads/2011/11/5-SkyBlue-750x562.png)](http://fsk141.com/uploads/2011/11/5-SkyBlue.png)

{% highlight bash %}
5-SkyBlue
static const char normbordercolor[] = "#3E3E3E";
static const char normbgcolor[]     = "#B2D5EE";
static const char normfgcolor[]     = "#000000";
static const char selbordercolor[]  = "#B2D5EE";
static const char selbgcolor[]      = "#EAEFF2";
static const char selfgcolor[]      = "#000000";
{% endhighlight %}

**6-BlackBlue:**
[![](http://fsk141.com/uploads/2011/11/6-BlackBlue-750x562.png)](http://fsk141.com/uploads/2011/11/6-BlackBlue.png)

{% highlight bash %}
6-BlackBlue
static const char normbordercolor[] = "#2D2D2D";
static const char normbgcolor[]     = "#101010";
static const char normfgcolor[]     = "#868686";
static const char selbordercolor[]  = "#CCCCCC";
static const char selbgcolor[]      = "#224488";
static const char selfgcolor[]      = "#F1F3F8";
{% endhighlight %}

**7-ClassicTerm:**
[![](http://fsk141.com/uploads/2011/11/7-ClassicTerm-750x562.png)](http://fsk141.com/uploads/2011/11/7-ClassicTerm.png)

{% highlight bash %}
7-ClassicTerm
static const char normbordercolor[] = "#2D2D2D";
static const char normbgcolor[]     = "#000000";
static const char normfgcolor[]     = "#54F93A";
static const char selbordercolor[]  = "#CCCCCC";
static const char selbgcolor[]      = "#202020";
static const char selfgcolor[]      = "#54F93A";
{% endhighlight %}

**8-BlueHue:**
[![](http://fsk141.com/uploads/2011/11/8-BlueHue-750x562.png)](http://fsk141.com/uploads/2011/11/8-BlueHue.png)

{% highlight bash %}
8-BlueHue
static const char normbordercolor[] = "#2D2D2D";
static const char normbgcolor[]     = "#2B4E69";
static const char normfgcolor[]     = "#799AA5";
static const char selbordercolor[]  = "#CCCCCC";
static const char selbgcolor[]      = "#799AA5";
static const char selfgcolor[]      = "#FFFFFF";
{% endhighlight %}

**9-Win3.1:**
[![](http://fsk141.com/uploads/2011/11/9-Win3.1-750x562.png)](http://fsk141.com/uploads/2011/11/9-Win3.1.png)

{% highlight bash %}
9-Win3.1
static const char normbordercolor[] = "#2D2D2D";
static const char normbgcolor[]     = "#0000AA";
static const char normfgcolor[]     = "#F0F0F0";
static const char selbordercolor[]  = "#CCCCCC";
static const char selbgcolor[]      = "#009696";
static const char selfgcolor[]      = "#F0F0F0";
{% endhighlight %}

**10-Purple:**
[![](http://fsk141.com/uploads/2011/11/10-Purple-750x562.png)](http://fsk141.com/uploads/2011/11/10-Purple.png)

{% highlight bash %}
10-Purple
static const char normbordercolor[] = "#2D2D2D";
static const char normbgcolor[]     = "#662066";
static const char normfgcolor[]     = "#6A99C7";
static const char selbordercolor[]  = "#CCCCCC";
static const char selbgcolor[]      = "#7B277C";
static const char selfgcolor[]      = "#FFF8FF";
{% endhighlight %}

