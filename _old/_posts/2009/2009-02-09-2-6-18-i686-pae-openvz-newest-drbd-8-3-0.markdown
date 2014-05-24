---
date: '2009-02-09 00:00:00'
layout: post
slug: 2-6-18-i686-pae-openvz-newest-drbd-8-3-0
status: publish
title: 2.6.18 {i686 - PAE} [OpenVZ *newest* | DRBD 8.3.0
wordpress_id: '36'
categories:
- Linux
- Programming
tags:
- CentOS
- kernel
- Linux
---

How to compile 2.6.18 with newest DRBD.

It's kinda a joke that OpenVZ just released a new RHEL5 kernel and it has DRBD 8.2.6 (meh) So I'm doing a short write up on the commands to semi-automatically build a 2.6.18 kernel with OpenVZ & DRBD 8.3.0. I'm choosing to do i686, but if you want to change it to x86_64 or remove PAE then just edit .config & download the correct OpenVZ patch.

    
    mkdir -p /usr/src/kernels/2.6.18_vz_drbd_ibcs/src
    cd /usr/src/kernels/2.6.18_vz_drbd_ibcs/src
    wget http://www.kernel.org/pub/linux/kernel/v2.6/linux-2.6.18.tar.gz
    wget http://oss.linbit.com/drbd/8.3/drbd-8.3.0.tar.gz
    wget http://voxel.dl.sourceforge.net/sourceforge/linux-abi/ibcs-3_7.tgz
    wget http://download.openvz.org/kernel/branches/rhel5-2.6.18/028stab060.2/patches/patch-92.1.18.el5.028stab060.2-combined.gz
    wget http://download.openvz.org/kernel/branches/rhel5-2.6.18/028stab060.2/configs/kernel-2.6.18-i686-PAE.config.ovz
    
    tar xzf linux-2.6.18.tar.gz -C ..
    tar xzf drbd-8.3.0.tar.gz -C ..
    mkdir ../ibcs
    tar xzf ibcs-3_7.tgz -C ../ibcs
    cp patch-92.1.18.el5.028stab060.2-combined.gz ..
    cd ..
    
    gunzip -c patch-92.1.18.el5.028stab060.2-combined.gz | patch -p0 patch-92.1.18.el5.028stab060.2-combined.gz
    
    cd drbd-8.3.0
    make KDIR=/usr/src/kernels/2.6.18_vz_drbd_ibcs/linux-2.6.18 kernel-patch
    cp patch-linux-2.6.18-drbd-8.3.0 ..
    cd ..
    patch -p0 < patch-linux-2.6.18-drbd-8.3.0
    
    cd linux-2.6.18
    cp ../src/kernel-2.6.18-i686-PAE.config.ovz ./.config
    make menuconfig


After you run: make menuconfig, this is the one important thing that you need to change...

    
    Device Drivers > Block devices >     DRBD Distributed Replicated Block Device support


Then build the kernel:

    
    time make -j7 rpm


Now install, and enjoy ;)

Make the initrd image:

    
    mkinitrd /boot/initrd-2.6.18-PAE.img 2.6.18-prep


Add this to /boot/grub/menu.lst

    
    title CentOS (2.6.18 OPENVZ|DRBD)
            root(hd0,0)
            kernel /vmlinuz-2.6.18-prep ro root=LABEL=/
            initrd /initrd-2.6.18-PAE.img
