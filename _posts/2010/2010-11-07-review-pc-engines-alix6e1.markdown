---
date: '2010-11-07 16:17:40'
layout: post
slug: review-pc-engines-alix6e1
status: publish
title: '[Review] PC-Engines alix6e1'
wordpress_id: '547'
categories:
- Hardware
- Networking
- Reviews
tags:
- alix6e1
- amd geode
- openwrt
- pc engines
- router
---

[![set_logo](http://farm2.static.flickr.com/1134/5155732266_444fe1461a.jpg)](http://www.flickr.com/photos/68444690@N00/5155732266/)








**Product:**


alix6e1






**Manufacturer:**


PC Engines






**Price:**


$85.00 USD






**Product Link:**


[http://www.pcengines.ch/alix6e1.htm](http://www.pcengines.ch/alix6e1.htm)






**Manual:**


[http://www.pcengines.ch/pdf/alix2.pdf](http://www.pcengines.ch/pdf/alix2.pdf)




**Summary:**

I can't speak for everyone, but I'm very picky when it comes to hardware. I will normally research a new mainboard for quite some time, until I find just the right one. Then it comes, and I'm let down by some chipset issue, socket placement issue, or other nitpicks. These little issues end up swaying my "perfect" board into something sub-par by the time I go to use it in my projects. I think that too many manufacturers today try to please everyone & end up adding too much flac to their boards, until the hardcore enthuiasts are stuck with a bunch of crap that they don't want/need. (Onboard video, extra pci-express slots, hdmi, onboard wifi???)

I've had the pleasure of using Alix boards in a few of my projects so far, and haven't had the slightest regret to spending a little extra money to get something that is perfect. Pascal (main dude \[and I think only dude] at PC engines) is a genius. He holds his place in a niche market, and does an amazing job at designing these boards. Just taking a visual look at the board you can see that everything is very well thought out. I dislike boards with large chunks of PCB that are left virtually untouched; due to poor overall design. The alix6e1 is no exception to all of the Alix boards in that they are very concentrated, and the components are splendidly placed. What makes the alix6e1 spectacular is that in the same form-factor of PC Engines other boards, so much is placed to fit in the board. You get a Wan/Lan port (2x 10/100), miniPCI card, miniPCI Express card, usb, cf card slot, 500 Mhz cpu, ram, and all the extra bits to make it work. This along with a great BIOS built from the ground up, makes this board difficult to pass up, especially since they are only $85.00 USD at the moment.

**Specs:**



	
  * CPU: 500 MHz AMD Geode LX800

	
  * DRAM: 256 MB DDR DRAM

	
  * Storage: CompactFlash socket

	
  * Power: DC jack or passive POE, min. 7V to max. 20V

	
  * Three front panel LEDs, pushbutton

	
  * Expansion: 1 miniPCI slot, 1 miniPCI Express slot (USB only), LPC bus

	
  * Connectivity: 2 Ethernet channels (Via VT6105M 10/100)

	
  * I/O: DB9 serial port, dual USB port

	
  * Board size: 6 x 6" (152.4 x 152.4 mm)

	
  * Firmware: tinyBIOS


**Hardware:**



	
  * **Case (case1c1blku):**

[![case_front-tilt](http://farm5.static.flickr.com/4153/5155829664_0110027d08.jpg)](http://www.flickr.com/photos/68444690@N00/5155829664/)

The case is absolutely splendid. Mind you it has a few "features" that some people might find unfavorable for their situation, yet everything is just perfect for me... I'd like to start with the most awesome part of this case; it's utmost care to simplicity. I have dealt with a LOT of router cases in my time here on earth, and all of them seem to be overcomplicated (mostly plastic) pieces of crap. The case1c1blku is a no-hinge clam-shell design.

[![case_open](http://farm5.static.flickr.com/4091/5155832816_155a83a909.jpg)](http://www.flickr.com/photos/68444690@N00/5155832816/)

It's takes 4 screws to mount the board, and another 4 to close the case up tight. The mounting anchor points are integrated in the case (soldered in), and seem very sturdy. I would have no regrets to throwing this router around in my bag as a mobile router due to the case ruggedness.

[![case_mount](http://farm5.static.flickr.com/4061/5155224905_a5958850fd_m.jpg)](http://www.flickr.com/photos/68444690@N00/5155224905/)

The punch-outs on the back for the i/o ports are cleanly punched, and there are no barbs left behind (prolly laser cut).

[![case_back](http://farm2.static.flickr.com/1098/5155830544_3f337c33b4.jpg)](http://www.flickr.com/photos/68444690@N00/5155830544/)

To assemble:

0) Add CF card now, unless you don't plan on using one

1) Remove the com port anchor screws

2) Tilt board into case with i/o ports in their holes

3) Bend the front a little till the board snugly fits in place

4) Screw down & enjoy

One of the "features" that some people might dislike is the non-removable CF card. I see this as a security feature, just in case you don't want someone live-tampering with your device (if in a secure environment). Another "feature" of this is that the CF card doesn't have the ability to move around (it's stuck next to the front of the case). So if you're especially rough with your alix6e1 you don't have to worry about the card falling out.

[![case_cf-card](http://farm2.static.flickr.com/1374/5155223003_aa51114524.jpg)](http://www.flickr.com/photos/68444690@N00/5155223003/)
	
  * **Layout:**

The overall use of space on this board is astounding. The CPU is butts up next to the memory, which allows for shorter traces. The Ethernet controllers are squished underneath the dead space for the miniPCI card. Even more space is saved with the heavy use of SMC's (surface mounted components). Even the tolerances between components seem extreme (ceramic resistors next to clock chip (to the right of sim card slot)). It's hard to find mistakes on this board, since everything is so well though out. The only pressing issue that I could find was a missing centimeter between the miniPCI & miniPCI Express card. If the miniPCI Express card was pushed to the left a little bit, or re-positioned; a normal sized pci card would fit more easily. I've been informed that this is planned for the next generation of this board, so at least it's been noted and will be fixed.

[![layout_top](http://farm2.static.flickr.com/1310/5156219166_28b66af491.jpg)](http://www.flickr.com/photos/68444690@N00/5156219166/)

The bottom is just as pretty as the top. Many resistors, and a few smc's are on the bottom, which makes me happy. It shows that a 2 layer design was implemented well. And based on how everything is neatly jammed into the board, the bottom compliments the top very well. It's great to see a nice fat ground strip flowing throughout the core of the board (not a lot of backwards tracing to find a ground). And one last thing that I almost forgot to mention is a wonderful silkscreen that coincides with schematics on the site. If you are the tinkering type, and something happens to go horribly wrong; the ability to dive in, and find the problem yourself is simplified with technical documentation. The fact that Pascal releases his schematics is fantastic, and allows you to work on your hardware long after your warranty is up.

[![layout_bottom](http://farm5.static.flickr.com/4086/5155604797_4ff53dc705.jpg)](http://www.flickr.com/photos/68444690@N00/5155604797/)

The back of the board (the side with all the goodies) is fairly well thought out. I love that there are 2 usb ports, but a lot of dongles aren't compatible with stacking that close. It would be great if there were two single usb ports (such as on either side of the wan port) But far enough so that if you have wide usb dongles it won't interfere. I have a large collection of usb flash drives, and a bunch of them cut it close to interfering with the Ethernet port in the current configuration. Another awesome thing to have would be and integrated PSU (or an optional module). Where all you plugin is a short 2 prong cable. That would be a dream :)

[![layout_ports](http://farm5.static.flickr.com/4021/5156215672_188f7344b1.jpg)](http://www.flickr.com/photos/68444690@N00/5156215672/)
	
  * **Input/Output:**

Lets start with the basics: 10/100 Ethernet, Serial Port, miniPCI & miniPCI Express & sim card slot. Two Ethernet ports are more than enough for a 10/100 router. One WAN (who has 100Mb internet anyway ;) && one LAN (going to hook it up to a Gb switch anyway). The great thing about the 10/100 ports is that each has it's own Via VT6105M chips. Instead of using a cludge multiplex hack, the time and effort was put in the use two individual chips. I'm glad that there is an external serial port (I don't know how you could get away without having one), and it's spaced far enough from the other i/o ports that makes it easy to plug in a larger serial adapter and still have room for an Ethernet jack. The biggest mistake of some boards is that serial jacks are put to close to other component jacks which disables you plugging in chunky cables. The miniPCI slot bummed me out. It's just a little too close (about .5mm) to the miniPCI Express card slot which disables you from using full size miniPCI cards. You **can** still use full size miniPCI cards, but they will bow a little bit from hitting the top of the miniPCI Express connector. I tried to get a good picture, but couldn't get a good angle, sorry. I never like to bend boards, or force anything, but it is a very minor bend. In the long run if you ran a full sized miniPCI card; you wouldn't have any issues, it's just not perfect. The miniPCI Express slot is a minor letdown; in that it only supports usb miniPCI Express cards. (kinda hit/miss if your card is usb or native minipci express). You would have to look very closely at the documentation of the card you are planning to purchase to make sure that it's a usb card & not native. And most of the time there isn't sufficient documentation :( I didn't have a sim card to test the sim card slot with (since I use verizon (cdma)), but I can't see it being hard to recognize/setup.

Lets finish off I/O with usb, power, cf card, and optional ports... I kinda wish that the usb were oriented a little differently to accommodate more usb options. In the current configuration you are allowed two thin usb devices. It would be nice if the usb ports were oriented differently to allow more variation with usb dongles. The cf card slot is perfectly placed & it's horribly placed (depending how you look at things) For one it's secure. To access the card you have to open the case, pull up the mainboard & only then can you remove/replace the cf card. On the other hand if your a hacker that wants to constantly change/hack & break your router OS & swap out cf cards; you'll have no luck with the current configuration. The one last dream of this board would be to have the included optional components well **not optional**. The internal mini-ide port would be killer for a little extra storage, and internal usb would be spectacular for a nice little flash drive that doesn't stick out the back of the device. I know they are optional for a reason, but I wish the reason was NULL & they were included ;)
	
  * **Power Consumption:**

I'm using a 12V 1A PSU brick & am consuming on average 6watts. I could probably optimize OpenWRT to save a half or maybe even a full watt, but for now I'm fine with 6 watts.
	
  * **Benchmarks:**

I've been working on true benchmarks for a while now, and am going to outline everything in another post with OpenWRT due to so much entanglement with the OS. The quick benchmark is what you would expect from a 10/100 Ethernet connection (not any faster or slower) && the processor is quick & performs VERY well for the application. Everything is robust, yet power efficient & cool. No fans are needed whatsoever even with full load; since the CPU & accompanying chips are cool to touch & don't overheat.
**Software:**



	
  * **BIOS:**

The BIOS is streamlined, and non-cluttered. It's a perfect fit to the overall design of the board. It was built from the ground up for the alix board series. A quick attachment via usb serial converter && a `screen /dev/cu.usbserial 38400` (default baud rate) gave me the goods I was looking for.

A little run down on what's shown on bootup:

1) BIOS version (this is the newest for the board)

2) Base Memory

3) Extended Memory (aka. usable memory)

All the rest is initiated by hitting the letter 's' on your keyboard (to startup the config menu)


- CF card mode modification

- MFGPT workaround (timer hack)

- late PCI init (fix for lousy pci cards)

- enable/disable serial console

- PXE (pixie boot is so 1337)

- xmodem (upload binary \[usually used to flash bios])

-- More information can be found in the manual ([http://www.pcengines.ch/pdf/alix2.pdf](http://www.pcengines.ch/pdf/alix2.pdf))

{% highlight bash %}&lt;br /&gt;
PC Engines ALIX.2 v0.99h&lt;br /&gt;
640 KB Base Memory&lt;br /&gt;
261120 KB Extended Memory&lt;/p&gt;
&lt;p&gt;01F0 Master 848A SAMSUNG CF/ATA&lt;br /&gt;
Phys C/H/S 4065/16/63 Log C/H/S 1016/64/63&lt;/p&gt;
&lt;p&gt;BIOS setup:&lt;/p&gt;
&lt;p&gt;(9) 9600 baud (2) 19200 baud *3* 38400 baud (5) 57600 baud (1) 115200 baud&lt;br /&gt;
*C* CHS mode (L) LBA mode (W) HDD wait (V) HDD slave (U) UDMA enable&lt;br /&gt;
(M) MFGPT workaround&lt;br /&gt;
(P) late PCI init&lt;br /&gt;
*R* Serial console enable&lt;br /&gt;
(E) PXE boot enable&lt;br /&gt;
(X) Xmodem upload&lt;br /&gt;
(Q) Quit&lt;br /&gt;
{% endhighlight %}
	
  * **Choice of Router OS:**

I choose [OpenWrt](http://openwrt.org) for now. I've been using dd-wrt on my wrt-350n for the longest time, and would like a change of scenery. Especially since there are a few really stupid quirks to dd-wrt that bug me. (Like why the hell isn't IPv6 support compiled in :() The installation went flawlessly. I just `dd if=openwrt-x86-generic-combined-ext2.img of =/dev/disk3` (something like that on my mac), and started up the board, and bingo, everything was ready to go. The BIOS had no issues with my CF card, and OpenWrt has amazing support for the alix boards (x86 version of OpenWrt). I've been running it for a while, and it's been chugging along with no issues. I plan to have an OpenWrt tutorial in the future, so I won't go into much detail about OpenWrt, but expect more in the future.
**Summary:**

Other than a couple little quirks (usb only miniPCI Express ; miniPCI -> miniPCI Express gap & cf card spacing) this board is fantastic. It draws very little power, is powerful, and runs cool & quiet (well silent). The case is a simple clamshell that is easy to assemble, and feels very sturdy. The BIOS rocks with a perfect amount of features. The overall potential for this board is amazing (diy mifi device); I'm sad that I won't be taking full advantage of the mini-pci express slot; since I use my Droid Incredible for a mobile hotspot. I guess the one and only big letdown for me is limited support for mini pci-express slot. Since it only allows usb cards (a couple gobi devices, novatel, and some others) you are forced to purchase a mini-pci card for wireless. It would be wonderful to be able to choose between mini-pci && mini pci-express in the future since mini pci-express cards are more widely available & cheaper. I would & do recommend this board to any diy hobbyist that want's a great board, for cheap, with plenty of features to chew on.
