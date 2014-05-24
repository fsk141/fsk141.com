---
date: '2008-12-11 00:00:00'
layout: post
slug: centos-2-6-24-i686pae-openvz-ovz006-5-ibcs-3-7-drbd-8-3-0-rc2
status: publish
title: CentOS 2.6.24 (i686PAE) | OpenVZ ovz006.5 | DRBD 8.3.0 | IBCS 3.7
wordpress_id: '46'
categories:
- Linux
tags:
- CentOS
- kernel
- Linux
- OpenVZ
---

The goal is to have an i686 kernel with 4GB+ memory support (PAE) with OpenVZ, IBCS, and DRBD.

Environment:
[Dell 2900](http://www.dell.com/content/products/productdetails.aspx/pedge_2900_3)
-8 Cores (1.60Ghz)
-24GB RAM

Operating System:
CentOS 5

Lets start by making ourselves a working directory:

    
    mkdir -p /usr/src/kernels/2.6.24_vz_drbd_ibcs/src
    cd /usr/src/kernels/2.6.24_vz_drbd_ibcs/src


Then we need to collect all the parts to build the patched kernel/modules:

    
    wget http://www.kernel.org/pub/linux/kernel/v2.6/linux-2.6.24.tar.gz
    wget http://oss.linbit.com/drbd/8.3/drbd-8.3.0.tar.gz
    wget http://voxel.dl.sourceforge.net/sourceforge/linux-abi/ibcs-3_7.tgz
    wget http://download.openvz.org/kernel/branches/2.6.24/2.6.24-ovz006.5/patches/patch-ovz006.5-combined.gz
    wget http://download.openvz.org/kernel/branches/2.6.24/2.6.24-ovz006.5/configs/kernel-2.6.24-i686-PAE.config.ovz


Now that we have all the pieces we need lets start extractin & patchin. BTW, I'll start all the command blocks with a pwd to show you where I am...:

    
    pwd
    /usr/src/kernels/2.6.24_vz_drbd_ibcs/src
    tar xzf linux-2.6.24.tar.gz -C ..
    tar xzf drbd-8.3.0.tar.gz -C ..
    mkdir ../ibcs
    tar xzf ibcs-3_7.tgz -C ../ibcs
    cp patch-ovz006.5-combined.gz ..
    cd ..


Ok stay calm, I just put all the source files in /src, and will build the kernel in 2.6.24_vz_drbd_ibcs.

    
    pwd
    /usr/src/kernels/2.6.24_vz_drbd_ibcs
    gunzip -c patch-ovz006.5-combined.gz | patch -p0 patch-ovz006.5-combined.gz


Lets continue by patching in drbd. You have to actually patch this in as opposed to building it as a module since that doesn't work.

    
    pwd
    /usr/src/kernels/2.6.24_vz_drbd_ibcs
    cd drbd-8.3.0
    make KDIR=/usr/src/kernels/2.6.24_vz_drbd_ibcs/linux-2.6.24 kernel-patch
    cp patch-linux-2.6.24-drbd-8.3.0 ..
    cd ..
    patch -p0 < patch-linux-2.6.24-drbd-8.3.0



    
    pwd
    /usr/src/kernels/2.6.24_vz_drbd_ibcs
    cd linux-2.6.24
    cp ../src/kernel-2.6.24-i686-PAE.config.ovz ./.config
    make menuconfig


After you're dropped into menuconfig you need to set drbd to(to compile drbd into the kernel)

    
    Device Drivers > Block devices >     DRBD Distributed Replicated Block Device support


At the moment you have a vanilla 2.6.24 kernel with OpenVZ ovz006.5 (patched) and DRBD 8.3.0 RC2 (patched). The only thing left is IBCS which we will install as a module after we have compiled and booted into the OS. As for now lets go ahead and compile, install, and boot.

    
    time make -j7 rpm


The build command 'time make -j7 rpm' will tell you how long it took to build at the end, the -j is how many jobs it can run at once (8 cores - 1 core for OS = 7 cores) and 'rpm' will do package it up for you in and store it at /usr/src/redhat/RPMS{\*}

If it compiles correctly you should get no errors, and see something like this:

    
    --output from compile--
    
    real	6m27.445s
    user	37m59.198s
    sys	6m18.564s


Then navigate to /usr/src/redhat/{athlon, i386, i486, i586, i686, noarch} depending on your archetecture. Mine was in i386.

    
    ls
    kernel-2.6.24-2.i386.rpm
    pwd
    /usr/src/redhat/RPMS/i386
    rpm -i kernel-2.6.24-2.i386.rpm


You should get something like the above...

Then you need to make the ramdisk:

    
    mkinitrd /boot/initrd-2.6.24.img 2.6.24


After that add an entry to grub to test your new kernel with OpenVZ and DRBD.

    
    title CentOS (2.6.24 VZ_DRBD_IBCS)
            root(hd0,0)
            kernel /vmlinuz-2.6.24 ro root=LABEL=/
            initrd /initrd-2.6.24.img


If everything goes as expected, then you should boot up. If it doesn't boot, or boots into the wrong kernel then you should check that you did everything correctly... Anywho, you should now be booted into your new kernel.

Lets continue by installing IBCS, and finishing up...

    
    pwd
    /usr/src/kernels/2.6.24_vz_drbd_ibcs/ibcs
    time make


If you received no errors, then continue edit /etc/rc.local to startup icbs on startup. Add the following to the end of your /etc/rc.local:

    
    /usr/src/kernels/2.6.24_vz_drbd_ibcs/ibcs/abi_ldr


You could move abi_ldr to /usr/bin if you would like, and just make sure to have the path above correspond where you decide to put abi_ldr.

You're Finished!

Resources:
[http://www.drbd.org/users-guide/s-build-from-source.html](http://www.drbd.org/users-guide/s-build-from-source.html)
[http://wiki.openvz.org/Download/kernel/2.6.24/2.6.24-ovz006.5](http://wiki.openvz.org/Download/kernel/2.6.24/2.6.24-ovz006.5)
[http://www.howtoforge.com/kernel_compilation_centos](http://www.howtoforge.com/kernel_compilation_centos)
