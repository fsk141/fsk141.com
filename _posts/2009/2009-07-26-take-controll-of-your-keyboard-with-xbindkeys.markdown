---
date: '2009-07-26 21:47:20'
layout: post
slug: take-controll-of-your-keyboard-with-xbindkeys
status: publish
title: Take controll of your keyboard with xbindkeys
wordpress_id: '9'
categories:
- Linux
- Software
tags:
- Linux
- xbindkeys
---

For avid linux enthusiasts xbindkeys is the ideal program to master your keyboard and assign commands to key combinations. Desktops and laptops alike; xbindkeys helps you use those unmapped keys, and put them to good use. For example, when you install a fresh distro on your shiny new laptop... A lot of new laptops have a bunch of extra keys that don't always work out of the box. Some Desktop environments such as kde and gnome have gui configuration utilities to help you out. But if your like me and use [dwm](http://dwm.suckless.org) then gui tools aren't really for you.

To better understand Xbindkeys I'll plot up my laptop scenario... I have an IBM X61 that has a few keys that don't work quite how I would like them to work. These include my volume up & down, menu, page left & right.

[![](http://farm3.static.flickr.com/2669/3760101501_10417216e1.jpg)](http://www.flickr.com/photos/68444690@N00/3760101501)

[![](http://farm3.static.flickr.com/2528/3760902270_9aea90d87d.jpg)](http://www.flickr.com/photos/68444690@N00/3760902270)

There a couple ways that one can create an ~/.xbindkeysrc file. The manual and automatic way...

**Automatic: (Creates lot of comment flac :()**



* * *



1. Download xbindkeys & xbindkeys_config for your distro (included in most distros these days)
2.

    
    $touch ~/.xbindkeysrc


3. Click new at the bottom, add a name, get the key combo, and set the command.
4. Either hit File > Save, or Edit > View generated rc & copy to .xbindkeysrc
[![](http://farm3.static.flickr.com/2676/3760233491_6421669186.jpg)](http://www.flickr.com/photos/68444690@N00/3760233491)

**Manual: (My choice)**



* * *



1. Download xbindkeys
2. Fire up your editor of choice and make a new file in your /home/ directory named **.xbindkeysrc** (Don't forget the '.' at the beginning)
3. Fire up a terminal and execute 'xev'
[![](http://farm3.static.flickr.com/2438/3760192351_64d9a1786b.jpg)](http://www.flickr.com/photos/68444690@N00/3760192351)

------

xev is the only tool that I know that gives you all the information you need easily. Lets start with the Volume Down key, and add it to the ~/.xbindkeysrc file:

    
    KeyPress event, serial 27, synthetic NO, window 0xa00001,
    root 0x10b, subw 0xa00002, time 192787373, (40,35), root:(41,51),
    state 0x0, keycode 122 (keysym 0x1008ff11, XF86AudioLowerVolume), same_screen YES,
    XLookupString gives 0 bytes:
    XmbLookupString gives 0 bytes:
    XFilterEvent returns: False


This might look just like a jumble of letters and numbers, but it's easy to find what we need and adapt it to a simple template for the config.

    
    # Template
    
    # Key/Action Label
    "command"
    m: state (aka modifier key) + keycode # (in our case m: 0x0)
    keycode_name (optional in some cases, sometimes won't work without)


So if we add everything up we get something like the following:

    
    #~/.xbindkeysrc
    
    #Volume Down
    "amixer set PCM 1+"
    m:0x0 + c:122
    XF86AudioRaiseVolume


5. Finish by typing the other keys into xev, and add them to your ~/.xbindkeysrc

------

If you would like to take a look at my X61 xbindkeysrc please check it out in my git backup [On Github](http://github.com/fsk141/dotfiles/blob/5b1e77b48983bd510a17fdc9b7002373ca6163cc/MiniFSK/.xbindkeysrc)

Happy Launching...
