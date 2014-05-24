---
date: '2010-09-21 15:08:50'
layout: post
slug: arch-linux-on-imac-arch-only
status: publish
title: Arch Linux on iMac [Arch Only]
wordpress_id: '330'
categories:
- Linux
- Mac
tags:
- Archlinux
- ati
- Linux
- linux on mac
- linux without os x
- Mac
- rEFIt
---

So I just got an iMac for work, and decided to put linux on it. I could have stuck with Mac, but meh, I need linux! It makes me all warm and fuzzy inside. I wrote down a log of what I did to get everything working, enjoy.

[![New-Desk](http://farm5.static.flickr.com/4111/5054457761_6cc63cddd5.jpg)](http://www.flickr.com/photos/68444690@N00/5054457761/)

1) Boot up Mac & install rEFIt ([http://refit.sourceforge.net](http://refit.sourceforge.net)) && reboot twice (second reboot should show rEFIt menu.

2) Boot Archlinux CD ( I choose the x86_64 netinstall image) & append "nomodeset" *without quotes* to the end of the boot line

3) Convert disk from GUID to MBR
- **login (root)**
- **dhcpcd eth0**
- **pacman -Sy && pacman -Su**
- **pacman -S gdisk**
- \[root@archiso ~]# fdisk -l | grep /dev #(this will give you your disk)

Disk /dev/sda: 500.1 GB, 500107862016 bytes << this is the disk I want (I only have one disk in the system)

- \[root@archiso ~]# gdisk /dev/sda

GPT fdisk (gdisk) version 0.6.10

Partition table scan:
MBR: MBR only
BSD: not present
APM: not present
GPT: present

Found valid MBR and GPT. Which do you want to use?
1 - MBR
2 - GPT
3 - Create blank GPT

Your answer:
1

---

1  - MBR (This really doesn't matter since we are going to wipe GPT, but select it anyway)

---

Command (? for help): x

Expert command (? for help): z
About to wipe out GPT on /dev/sdc. Proceed? (Y/N): y
GPT data structures destroyed! You may now partition the disk using fdisk or
other utilities.
Blank out MBR? (Y/N): y

---

This will wipe out GPT & MBR for a brand new HDD, now you are ready to continue with your arch install as if it was a beige box.

---
4) Continue With Install (/arch/setup)
- Select Source (kernel.org)
- Set clock
- Prepare Hard Drive(s)
- You can autoprepare if you wish, this is what I did on my 500GB drive:
- Manually Partition
100GB /
398GB /home
2GB   /swap
- Manually configure block devices
- UUID is king
- /dev/sda1 - xfs - /
- /dev/sda2 - xfs - /home
- /dev/sda3 = swap
- Ignore /boot missing error
- Select Packages
- asterisk base & base-devel
- openssh
- sudo
- Configure (change hostname & stuff)
- Install bootloader
- Configure menu.lst & add "nomodeset"  to the end of the kernel lines
- Something like this: `kernel /boot/vmlinuz26 root=/dev/disk/by-uuid/15734824-7189-44e5-ac82-81b4215144f6 ro nomodeset`

- Reboot

5) Insert MAC OS X disk (hold alt & boot)
- Utilities > Terminal:
- *bless --device /dev/disk0 --setBoot --legacy --verbose* (where /dev/disk0 is where I installed grub \[I've heard there can be issues with installing grub on the root of the disk, but I've had no problems so far])

6) boot & do some configging:
- pacman -S xorg
- lspci | grep VGA (http://wiki.archlinux.org/index.php/MacBook#Xorg)
- I have ATI drivers:
- I chose catalyst drivers over xf86-video-ati/radeonhd since they worked well, and were a breeze to install through yaourt:
- *yaourt -S catalyst* (or you can manually do everything [start here](http://aur.archlinux.org/packages.php?K=catalyst)
- I got hung up with my ~/.xinitrc & it was causing problems, yet after removing it I was able to start twm with no issues

Here is my xorg.conf just in case anyone is interested:

{% highlight bash %}
Section "ServerLayout"
 Identifier     "Xorg Configured"
 Screen      0  "aticonfig-Screen[0]-0" 0 0
 InputDevice    "Keyboard0" "CoreKeyboard"
 InputDevice    "USB Mouse" "CorePointer"
EndSection

Section "Files"

# Additional fonts: Locale, Gimp, TTF...
#    FontPath     "/usr/share/lib/X11/fonts/latin2/75dpi"
#    FontPath     "/usr/share/lib/X11/fonts/latin2/100dpi"
# True type and type1 fonts are also handled via xftlib, see /etc/X11/XftConfig!
 ModulePath   "/usr/lib/xorg/modules"
 FontPath     "/usr/share/fonts/misc:unscaled"
 FontPath     "/usr/share/fonts/misc"
 FontPath     "/usr/share/fonts/75dpi:unscaled"
 FontPath     "/usr/share/fonts/75dpi"
 FontPath     "/usr/share/fonts/100dpi:unscaled"
 FontPath     "/usr/share/fonts/100dpi"
 FontPath     "/usr/share/fonts/PEX"
 FontPath     "/usr/share/fonts/cyrillic"
 FontPath     "/usr/share/fonts/Type1"
 FontPath     "/usr/share/fonts/ttf/western"
 FontPath     "/usr/share/fonts/ttf/decoratives"
 FontPath     "/usr/share/fonts/truetype"
 FontPath     "/usr/share/fonts/truetype/openoffice"
 FontPath     "/usr/share/fonts/truetype/ttf-bitstream-vera"
 FontPath     "/usr/share/fonts/latex-ttf-fonts"
 FontPath     "/usr/share/fonts/defoma/CID"
 FontPath     "/usr/share/fonts/defoma/TrueType"
EndSection

Section "Module"
 Load  "ddc"  # ddc probing of monitor
 Load  "dbe"
 Load  "dri"
 Load  "extmod"
 Load  "glx"
 Load  "bitmap" # bitmap-fonts
 Load  "type1"
 Load  "freetype"
 Load  "record"
EndSection

Section "ServerFlags"
 Option        "AllowMouseOpenFail" "true"
 Option        "AutoAddDevices" "False"
EndSection

Section "InputDevice"
 Identifier  "Keyboard0"
 Driver      "keyboard"
 Option        "CoreKeyboard"
 Option        "XkbRules" "xorg"
 Option        "XkbModel" "pc105"
 Option        "XkbLayout" "us"
 Option        "XkbVariant" ""
EndSection

Section "InputDevice"
 Identifier  "USB Mouse"
 Driver      "mouse"
 Option        "Device" "/dev/input/mice"
 Option        "SendCoreEvents" "true"
 Option        "Protocol" "IMPS/2"
 Option        "ZAxisMapping" "4 5"
 Option        "Buttons" "5"
EndSection

Section "Monitor"
 Identifier   "aticonfig-Monitor[0]-0"
 Option        "VendorName" "ATI Proprietary Driver"
 Option        "ModelName" "Generic Autodetecting Monitor"
 Option        "DPMS" "true"
EndSection

Section "Device"
 Identifier  "aticonfig-Device[0]-0"
 Driver      "fglrx"
 BusID       "PCI:1:0:0"
EndSection

Section "Screen"
 Identifier "aticonfig-Screen[0]-0"
 Device     "aticonfig-Device[0]-0"
 Monitor    "aticonfig-Monitor[0]-0"
 DefaultDepth     24
 SubSection "Display"
 Viewport   0 0
 Depth     24
 Modes    "1920x1200"
 EndSubSection
EndSection

Section "DRI"
 Mode         0666
EndSection
{% endhighlight %}

After I got X installed I went on to install dwm (my window manager of choice), and synergy so that I don't have to use another keyboard/mouse. This is a minimalistic rundown of what I had to do to get things working, and well things work. Please let me know if you're having problems, since I hit a few hitches along the way, most were very easy to solve, otherwise I would have added them in.

------

Sources:
- [http://www.rodsbooks.com/gdisk/wipegpt.html](http://www.rodsbooks.com/gdisk/wipegpt.html)
- [http://mac.linux.be/content/single-boot-linux-without-delay](http://mac.linux.be/content/single-boot-linux-without-delay)
- [http://wiki.archlinux.org/index.php/ATI](http://wiki.archlinux.org/index.php/ATI)
- [http://wiki.archlinux.org/index.php/MacBook](http://wiki.archlinux.org/index.php/MacBook)
