---
date: '2008-12-10 00:00:00'
layout: post
slug: how-to-create-zfs-stripe-pool
status: publish
title: How to create zfs stripe (pool) [NAS]
wordpress_id: '47'
categories:
- ZFS
tags:
- Linux
- NAS
- NFS
- Open Solaris
---

I recently (Last night to be exact) I installed Open Solaris as my NAS/Backup Server. Why you may ask? Well I love [Arch Linux](http://www.archlinux.org) yet see the *need* to start using [ZFS](http://en.wikipedia.org/wiki/ZFS). ZFS is freakin-fan-tastic. It's puts the S in simple, and allows you to have a filesystem that does much more than a common file system. Such as NFS, SMB, and Compression (did I mention that this is all built in :) I'm going to go through the simple process to setup a zfs striped pool, and setup a few datasets, and apple compression.

OpenSolaris tools I'll cover:
[format](http://docs.sun.com/app/docs/doc/816-5166/format-1m)
[zfs(1M)](http://docs.sun.com/app/docs/doc/816-5166/zfs-1m)
[zpool(1M)](http://docs.sun.com/app/docs/doc/816-5166/zpool-1m)

Lets start out by finding the disks that we would like to add to the pool:

    
    root@Nom:~# format < /dev/null


Which will look like this:

    
    root@Nom:~# format < /dev/null
    Searching for disks...done
    
    AVAILABLE DISK SELECTIONS:
           0. c3d0 
              /pci@0,0/pci-ide@1f,1/ide@0/cmdk@0,0
           1. c4d1 
              /pci@0,0/pci-ide@1f,2/ide@0/cmdk@1,0
           2. c5d0 
              /pci@0,0/pci-ide@1f,2/ide@1/cmdk@0,0
    Specify disk (enter its number):


This shows the three disks that are present in my system. I'll break it down a little. The number you see at the beginning is the number as per the format command. Then the information in the <>'s displays a disk ID, size of the disk, and some other little tidbits for the people that care.

What you're looking for is "c4d1" & "c5d0" which are the two Western Digital 1TB disks that I'm going to make my pool with.

To create the pool use the zpool command:

    
    root@Nom:~# zpool create nom c4d1 c5d0


That's it, you've now created your first zfs pool. Just to sum up what I just did, I formatted the disks, set the mountpoints, mounted the device, and now have an active zfs pool.

If you would like to see the zpools that you currently have do the following command:

    
    root@Nom:/nom# zpool list
    NAME    SIZE   USED  AVAIL    CAP  HEALTH  ALTROOT
    nom    1.81T  82.5K  1.81T     0%  ONLINE  -
    rpool   149G  3.36G   146G     2%  ONLINE  -


I could go ahead and add a NFS share and Compression yet, why not stay organized :) I would rather create individual file systems to store the different data that I have.

To show what I mean I'll show you what a zfs file system is:

    
    root@Nom:/nom# zfs list
    NAME                       USED  AVAIL  REFER  MOUNTPOINT
    nom                       70.5K  1.78T    18K  /nom
    rpool                     4.36G   142G    72K  /rpool
    rpool/ROOT                2.37G   142G    18K  legacy
    rpool/ROOT/opensolaris    2.37G   142G  2.24G  /
    rpool/dump                1019M   142G  1019M  -
    rpool/export                59K   142G    19K  /export
    rpool/export/home           40K   142G    19K  /export/home
    rpool/export/home/fsk141    21K   142G    21K  /export/home/fsk141
    rpool/swap                1019M   143G    16K  -


This command shows pools/file systems. If you look you can see my two pools (nom, rpool) and the filesystems underneath the pools (ROOT, dump, export, swap)

I would like to create subsets like the above (File Systems)

    
    root@Nom:/nom# zfs create nom/backup


So now if I do a zfs list I get the following:

    
    root@Nom:/nom# zfs list
    NAME                       USED  AVAIL  REFER  MOUNTPOINT
    nom                       97.5K  1.78T    18K  /nom
    nom/backup                  18K  1.78T    18K  /nom/backup
    rpool                     4.36G   142G    72K  /rpool
    rpool/ROOT                2.37G   142G    18K  legacy
    rpool/ROOT/opensolaris    2.37G   142G  2.24G  /
    rpool/dump                1019M   142G  1019M  -
    rpool/export                59K   142G    19K  /export
    rpool/export/home           40K   142G    19K  /export/home
    rpool/export/home/fsk141    21K   142G    21K  /export/home/fsk141
    rpool/swap


Since I'm going to be backing up to this file system I would like to turn on a couple little things... To get a listing of what zfs set can set then just type 'zfs set'

    
    root@Nom:/nom# zfs set compression=on nom/backup
    root@Nom:/nom# zfs set sharenfs=rw nom/backup


I've just setup automagical compression, along with a read/write nfs share for '/nom/backup' Now all I need to do is setup nfs on my client machine to connect to the nfs server.

Links:
[http://www.sun.com/bigadmin/features/articles/zfs_overview.jsp](http://www.sun.com/bigadmin/features/articles/zfs_overview.jsp)
[http://blogs.sun.com/timthomas/entry/creating_zfs_file_systems_from](http://blogs.sun.com/timthomas/entry/creating_zfs_file_systems_from)
