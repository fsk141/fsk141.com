---
date: '2009-01-13 00:00:00'
layout: post
slug: manual-nfs-pains
status: publish
title: Manual NFS pains
wordpress_id: '40'
categories:
- Opensolaris
tags:
- Linux
- NFS
---

I've been privileged; in that I haven't had to deal with the complexities of NFS. Instead ZFS has managed all my NFS stuffs. Recently I've been testing NFS on OpenVZ containers, and have had to learn some of the oddities to NFS3/4 and came across this problem:

    
    [root@cos47i686 mnt]# mount -t nfs4 box1:/demo /mnt/box1
    Warning: rpc.idmapd appears not to be running.
             All uids will be mapped to the nobody uid.
    mount: fs type nfs4 not supported by kernel
    [root@cos47i686 mnt]# /etc/init.d/rpcidmapd start
    Starting RPC idmapd: Error: RPC MTAB does not exist.


The solution is this:

    
    mount -t rpc_pipefs sunrpc /var/lib/nfs/rpc_pipefs/


Then you just need to start 'rpcidmapd' and mount your nfs share.

Resources:
[Re: NFS4 startup error - RPC MTAB not found ??](http://linux.derkeiler.com/Mailing-Lists/Fedora/2004-06/4558.html)
