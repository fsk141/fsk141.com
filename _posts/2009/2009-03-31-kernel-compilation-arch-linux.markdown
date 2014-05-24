---
date: '2009-03-31 00:00:00'
layout: post
slug: kernel-compilation-arch-linux
status: publish
title: Kernel Compilation &amp; Arch Linux
wordpress_id: '22'
categories:
- Linux
tags:
- Arch Linux
- kernel
---

[![](http://farm4.static.flickr.com/3462/3401383387_1764c8e018.jpg)](http://www.flickr.com/photos/68444690@N00/3401383387)

**Summary:**

A brief history of the infamous kernel. Followed by an in depth evaluation on the Arch Linux PKGBUILD of the stock kernel26. In that I'll delve into the new outline of the PKGBUILD along with ways to customize (.config options, patches, how to prune for performance)

**-- What is a kernel? --**

The kernel is the magical gateway that allows your hardware to communicate with applications. The kernel allows 'User space' to call system calls to the 'Kernel space.' Such as manipulating a file on a disk (open/close/read/write), or registering CPU commands. This being said the kernel is one of the lowest points of abstraction. It's the crutch in which the whole of your computer is held up. The kernel manages resources such as CPU, memory, and I/O (disks, peripherals, displays, etc.). The kernel also allows for synchronization and communication between processes. (Inter-Process Communication). It also allows such processes a method to access the aforementioned hardware resources.

The linux kernel is monolithic in nature, yet can be optimized with modules. Modules allow your kernel to be as small as you want, or easily expanded to include everything and the kitchen sink. It also allows you update modules; remove, update, reinsert, and be on your way again. More on modules to follow...

*-- kernel26-2.6.28 (PKGBUILD) --*

Please usher in the all powerful PKGBUILD! (/me welcomes)

Check out the the PKGBUILD in it's entirety here:

[http://repos.archlinux.org/wsvn/packages/kernel26/repos/core-x86_64/PKGBUILD](http:http://repos.archlinux.org/wsvn/packages/kernel26/repos/core-x86_64/PKGBUILD//)

And for all the other files hit them up on the svn:
[http://repos.archlinux.org/wsvn/packages/kernel26/repos/core-x86_64/](http://repos.archlinux.org/wsvn/packages/kernel26/repos/core-x86_64/http://)

For the article, the PKGBUILD will be broken into it's multiple pieces, and given a little description.

------
1) Common Head
------

    
      1 # $Id$
      2 # Maintainer: Tobias Powalowski 
      3 # Maintainer: Thomas Baechler
    
      4 pkgname=kernel26                # Build stock -ARCH kernel
      5 # pkgname=kernel26-custom       # Build kernel with a different name
      6 _kernelname=${pkgname#kernel26}
      7 _basekernel=2.6.28
      8 pkgver=${_basekernel}.7
      9 pkgrel=2
     10 _patchname="patch-${pkgver}-${pkgrel}-ARCH"
     11 pkgdesc="The Linux Kernel and modules"
     12 arch=(i686 x86_64)
     13 license=('GPL2')
     14 groups=('base')
     15 url="http://www.kernel.org"
     16 backup=(etc/mkinitcpio.d/${pkgname}.preset)
     17 depends=('coreutils' 'kernel26-firmware>=2.6.28' 'module-init-tools' 'mkinitcpio>=0.5.20')
     18 # pwc, ieee80211 and hostap-driver26 modules are included in kernel26 now
     19 # nforce package support was abandoned by nvidia, kernel modules should cover everything now.
     20 # kernel24 support is dropped since glibc24
     21 replaces=('kernel24' 'kernel24-scsi' 'kernel26-scsi'
     22           'alsa-driver' 'ieee80211' 'hostap-driver26'
     23           'pwc' 'nforce' 'squashfs' 'unionfs' 'ivtv'
     24           'zd1211' 'kvm-modules' 'iwlwifi' 'rt2x00-cvs'
     25           'gspcav1' 'atl2' 'wlan-ng26')
     26 install=kernel26.install
     27 source=(ftp://ftp.kernel.org/pub/linux/kernel/v2.6/linux-$_basekernel.tar.bz2
     28         ftp://ftp.archlinux.org/other/kernel26/${_patchname}.bz2
     29         # the main kernel config files
     30         config config.x86_64
     31         # standard config files for mkinitcpio ramdisk
     32         kernel26.preset)
     33 optdepends=('crda: to set the correct wireless channels of your country')
     34 md5sums=('d351e44709c9810b85e29b877f50968a'
     35          'e535d668afb04a590f03a47356794579'
     36          '3c47674afeae26616acd9dfc4060bd02'
     37          '1f0005febea457f47f26af1726dbf1b2'
     38          '25584700a0a679542929c4bed31433b6')


------

Lets break this down into it's relative pieces:
4) pkgname (ie. pacman -S kernel26)

6) \_kernelname (uname -r)

7) \_basekernel (well kernel w/o patches \[2.6.28])

8) pkgver (kernel path version \[2.6.28.7])

17) depends (coreutils, module-init-tools, and mkinitcpio are necessary. More on kernel26-firmware later)

26) install\* (install file is crucial for mkinitcpio to execute correctly after your kernel has been properly compiled)

Everything else should make sense in this part of the PKGBUILD. If you're unfamiliar with any of the other variables I would peruse around the arch wiki, and read up on PKGBUILDS

------
2) Make Chunk
------

The next section is where the magic happens. Lines 40-75 start out with a variable to choose the correct .config. Pretend you are your computer (metaphysically) and imagine what you would do if given the following commands. Realize that makepkg does a little magic in the background, yet most everything else is very straightforward.

43) We dive into the extracted kernel directory extracted automagically by makepkg.

46) This is a great place to put patches. More on patches in the next section of the article. After the patches have been applied (or discounted) we move onto configuring the kernel.

Tobias/Thomas are nice and provide a preset .config for x86 & x86_64 (how nice of them).

48-52) determine the machine type and output the correct .config accordingly.

53-55) Check to see if you have '_kernelname' set, and if you do it will change the variable in your .config to match your custom name. '2.6.28-ARCH' is the default for this field.

57-75) Finishes the make. It grabs the correct name of the kernel and pops it into a nifty variable for use later (_kernver). What follows it is a nice block of commented code. The line that follows through just states that whatever the kernel has as defaults will be taken, and the build will commence. Later in the article we will take a look at how to make our own config, and strip out some on the un-necessary things in the kernel. After the config is taken care of, makepkg procedes to build the bzImage (boot image), and the rest of the kernel (modules and stuff)

------

    
     40 build() {
     41   KARCH=x86
     42
     43   cd ${srcdir}/linux-$_basekernel
     44   # Add -ARCH patches
     45   # See http://projects.archlinux.org/git/?p=linux-2.6-ARCH.git;a=summary
     46   patch -Np1 -i ${srcdir}/${_patchname} || return 1
     47
     48   if [ "$CARCH" = "x86_64" ]; then
     49     cat ../config.x86_64 >./.config
     50   else
     51     cat ../config >./.config
     52   fi
     53   if [ "${_kernelname}" != "" ]; then
     54     sed -i "s|CONFIG_LOCALVERSION=.*|CONFIG_LOCALVERSION="${_kernelname}"|g" ./.config
     55   fi
     56   # get kernel version
     57   make prepare
     58   _kernver="$(make kernelrelease)"
     59   # load configuration
     60   # Configure the kernel. Replace the line below with one of your choice.
     61   #make menuconfig # CLI menu for configuration
     62   #make xconfig # X-based configuration
     63   #make oldconfig # using old config from previous kernel version
     64   # ... or manually edit .config
     65   ####################
     66   # stop here
     67   # this is useful to configure the kernel
     68   #msg "Stopping build"
     69   #return 1
     70   ####################
     71   yes "" | make config
     72   # build!
     73   make bzImage modules || return 1
     74   mkdir -p ${pkgdir}/{lib/modules,boot}
     75   make INSTALL_MOD_PATH=${pkgdir} modules_install || return 1


------
3) The rest
------

76-153) I will refrain from pasting this part of the PKGBUILD. A brief summary of what happens is this:

- Files are copied from working directory to installdir ($pkgdir)

- Miscellaneous files are copied from working dir to install dir.

161-179) This is where we continue:

------

    
    161   # install fallback mkinitcpio.conf file and preset file for kernel
    162   install -m644 -D ${srcdir}/kernel26.preset ${pkgdir}/etc/mkinitcpio.d/${pkgname}.preset || return 1
    163   # set correct depmod command for install
    164   sed 
    165     -e  "s/KERNEL_NAME=.*/KERNEL_NAME=${_kernelname}/g" 
    166     -e  "s/KERNEL_VERSION=.*/KERNEL_VERSION=${_kernver}/g" 
    167     -i $startdir/kernel26.install
    168   sed 
    169     -e "s|source .*|source /etc/mkinitcpio.d/kernel26${_kernelname}.kver|g" 
    170     -e "s|default_image=.*|default_image="/boot/${pkgname}.img"|g" 
    171     -e
    "s|fallback_image=.*|fallback_image="/boot/${pkgname}-fallback.img"|g" 
    172     -i ${pkgdir}/etc/mkinitcpio.d/${pkgname}.preset
    173
    174   echo -e "# DO NOT EDIT THIS FILEnALL_kver='${_kernver}'" > ${startdir}/pkg/etc/mkinitcpio.d/${pkgname}.kver
    175   # remove unneeded architectures
    176   rm -rf ${pkgdir}/usr/src/linux-${_kernver}/arch/{alpha,arm,arm26,avr32,blackfin,cris,frv,h8300,ia64,m32r,m68k,m68knommu,mips,mn10300,parisc,powerpc,ppc,s
    390,sh,sh64,sparc,sparc64,um,v850,xtensa}
    177   # remove the firmware
    178   rm -rf ${pkgdir}/lib/firmware
    179 }


------

162) Copies \*.preset file (mkinitcpio vars) into install dir. To backstep a little, just in case your unsure what I mean by installdir. Makepkg works with two directories: pkg & src. src is where the package is extracted and compiled. Durring the compile you can specify to compile to the pkg dir, or copy the files over after you have finished your compile (ie. binaries)

164-167) Posts correct variables into your \*.install file (uses a simple sed find and replace)

168-172) Pops correct vars into \*.preset

174) This is the one line not to screw around with. It creates a .kver file to let mkinitcpio what kernel it's compiling.

176) Removes erroneous directories

178) I would remove this like if you plan to compile your own kernel. Just leave the firmware in place...

------

**-- Patches --**

Definitely take a peek at:
[http://wiki.archlinux.org/index.php/Kernel_Patches_and_Patchsets](http://wiki.archlinux.org/index.php/Kernel_Patches_and_Patchsets)

This gives a great rundown on patches & patchsets ;)

Other than that, patches are for the tweakers at heart. If you would like to get a fraction more performance benefit then run with patches as opposed to attaching modules. Depending on what you want you might want to patch in the newest version of ext4. Or the latest version of OpenVZ. Or if you would really like to explore, dive into DRBD...

**-- Modules --**

Hooray, Modules! Modules are the easiest way to add functionality to a kernel. Not only do modules reduce the bloat of a once monolithic kernel. They also make it simple to upgrade seperate pieces of the kernel without restarting, or reloading a new kernel. There are certain limits, but they are far and wide, and when you run into one, it's simple to restart with a new kernel :)

------
/etc/rc.conf
------

    
    MODULES=(!pcspkr r8169 slhc snd-mixer-oss snd-pcm-oss snd-hwdep
    snd-page-alloc snd-pcm snd-timer snd snd-hda-intel soundcore nfs)


------

This is my modules line on my desktop. It's very simple. The pcsprk is disabled (!), and for the most part the rest of the things loaded are sound drivers.

That's not to say that I don't have more modules running. To check the currently running modules on your system run the 'lsmod' command. The output will look something similar to this:

------

    
    [fsk141@FSK-Main ~]$ lsmod | more
    Module                  Size  Used by
    battery                14600  0
    nls_cp437               8960  1
    vfat                   14464  1
    fat                    56760  1 vfat
    usb_storage           112832  1
    ipv6                  309952  32


------

Hum, battery on a desktop? How absurd, well let's make sure that it's not a cmos battery, or anything else important with 'modinfo':

------

    
    [fsk141@FSK-Main ~]$ modinfo battery
    filename:       /lib/modules/2.6.28-ARCH/kernel/drivers/acpi/battery.ko
    license:        GPL
    description:    ACPI Battery Driver
    author:         Alexey Starikovskiy 
    author:         Paul Diefenbaugh
    alias:          acpi*:PNP0C0A:*
    depends:
    vermagic:       2.6.28-ARCH SMP preempt mod_unload
    parm:           cache_time:cache time in milliseconds (uint)


------

Well, I don't need ACPI on my desktop, so lets remove it with 'modprobe -r battery' You need to run this as root or under sudo... By "removing it" it will remove it from the kernel. Likewise if you were to execute 'modprobe battery' it would insert the module.

**-- Compiling & Tweaking--**

To wrap things up lets go through a hypothetical scenario.

1) You edit your PKGBUILD and add the Viper Patchset (Best wishes to the late Vipercinus)

2) You replace 'yes "" | makeconfig' with 'make menuconfig' (Remove erroneous options, and modules) If you are unsure what something is, then pull up a trusty google page and look it up. For example if you use ext3 as your primary filesystem, then consider removing EXT2, EXT4, XFS, Reiser FS, etc. You might be suprised on how you can cut some time off boot by simply removing pieces from your kernel. (Leave the important stuff, otherwise you'll just end up with a useless pile of compiled junk)

3) 'makepkg' finishes, and you proceded to 'ls /boot/' and make sure that your mkinitcpio images were successfully created

4) Add a Grub entry:

------

    
    
    title  Arch Linux
    root   (hd0,0)
    kernel /boot/vmlinuz26-custom
    root=/dev/sda1 ro
    initrd /boot/kernel26-custom.img


------

5) Reboot and cross your fingers. If it reboots, and spits you back into a prompt then you're golden. Otherwise you will need to troubleshoot what you did wrong, or how you mucked things up.

------

**Conclusion:**

I wish you the best of luck with your kernel adventures, and Arch Linux in general. The process of building a kernel takes a lot of time to master, and is a very complicated affair. It's wonderful that the PKGBUILD is able to structure the process so nicely, and allow you create a package with a full kernel, and the instructions to run mkinitcpio, and create the correct boot images. I recommend trying out a couple different patchsets, and trying your hand at making a PKGBUILD of your own kernel. At the very least grab the default kernel, and compile it yourself to see what happens. I can recall the first time I compiled a program, and how confused yet joyous I was. I knew I did something, but I had no idea what that something amounted to. Now after compiling too many programs/kernels to count I look back, and recall the time I had learning. It was troublesome, and there weren't many resources that were able to help/step you through the whole process. I know that this isn't an all telling guide for kernel compiling. At the very least, I hope this peaks you interest to learn more.

Enjoy, Jonny Gerold (jonny@fsk141.com)
