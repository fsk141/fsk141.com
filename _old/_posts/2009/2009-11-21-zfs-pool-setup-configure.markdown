---
date: '2009-11-21 16:04:11'
layout: post
slug: zfs-pool-setup-configure
status: publish
title: ZFS Pool (Setup &amp; Configure)
wordpress_id: '183'
categories:
- ZFS
---

ZFS is the best option for a NAS system. It makes for a super simple home NAS, and does away with a lot of config hassles. This is due to the fact that a lot of core services are built into zfs, or into the zfs stack rather. To setup any "extra" of zfs, all you need is a one line zfs command. `(zfs set sharenfs=on <pool> == instant nfs share)`

1) Create the Pool

We need to fetch the disk ID's (the number after the disk number)

{% highlight bash %}
format < /dev/null
{% endhighlight %}

So if we run that command I can pull out (c8d0 c8d1 c9d0 c9d1) as shown below:

{% highlight bash %}
root@TrayNAS:~# format < /dev/null

Searching for disks...done

AVAILABLE DISK SELECTIONS:
0. c7d0 &amp;lt;DEFAULT cyl 3904 alt 2 hd 128 sec 32&amp;gt;
/pci@0,0/pci-ide@1f,1/ide@0/cmdk@0,0
1. c8d0 &amp;lt;DEFAULT cyl 60797 alt 2 hd 255 sec 252&amp;gt;
/pci@0,0/pci-ide@1f,2/ide@0/cmdk@0,0
2. c8d1 &amp;lt;DEFAULT cyl 60798 alt 2 hd 255 sec 189&amp;gt;
/pci@0,0/pci-ide@1f,2/ide@0/cmdk@1,0
3. c9d0 &amp;lt;DEFAULT cyl 60798 alt 2 hd 255 sec 189&amp;gt;
/pci@0,0/pci-ide@1f,2/ide@1/cmdk@0,0
4. c9d1 &amp;lt;DEFAULT cyl 60798 alt 2 hd 255 sec 189&amp;gt;
/pci@0,0/pci-ide@1f,2/ide@1/cmdk@1,0
Specify disk (enter its number):
{% endhighlight %}

Take the extracted disk ID's (c8d0 c8d1 c9d0 c9d1) and apply them to the next command. Edit accordingly depending on what pool type you would like. (mirror, raidz1, raidz2)

{% highlight bash %}

zpool create -f ambry raidz1 c8d0 c8d1 c9d0 c9d1

{% endhighlight %}

I had to use -f because I have 3x 1.5TB drives, and one 2TB drive...

Now that we have the pool created, lets marvel at what we accomplished:

{% highlight bash %}

root@TrayNAS:~# zpool list ambry
NAME    SIZE   USED  AVAIL    CAP  HEALTH  ALTROOT
ambry  5.44T   137K  5.44T     0%  ONLINE  -
root@TrayNAS:~# zfs list ambry
NAME    USED  AVAIL  REFER  MOUNTPOINT
ambry  95.8K  4.00T  28.4K  /ambry

{% endhighlight %}

The difference in size is just one of those little quirks (zpool == raw disks, zfs == real space)

Another check just to make sure were all good, and using raidz1 (1 parity disk)

{% highlight bash %}
root@TrayNAS:~# zpool status ambry
  pool: ambry
 state: ONLINE
 scrub: none requested
config:

	NAME        STATE     READ WRITE CKSUM
	ambry       ONLINE       0     0     0
	  raidz1    ONLINE       0     0     0
	    c8d0    ONLINE       0     0     0
	    c8d1    ONLINE       0     0     0
	    c9d0    ONLINE       0     0     0
	    c9d1    ONLINE       0     0     0

errors: No known data errors
{% endhighlight %}

Everything looks good, lets setup some filesystems, sharing, compression, and copy over our data...

For the rest of the write up, I'll just output the commands I used to create my environment:

{% highlight bash %}
root@TrayNAS:~# zfs create ambry/Media
root@TrayNAS:~# zfs create ambry/Downloads
root@TrayNAS:~# zfs list | grep ambry
ambry                       176K  4.00T  31.4K  /ambry
ambry/Downloads            28.4K  4.00T  28.4K  /ambry/Downloads
ambry/Media                28.4K  4.00T  28.4K  /ambry/Media
root@TrayNAS:~# zfs get all ambry
NAME   PROPERTY              VALUE                  SOURCE
ambry  type                  filesystem             -
ambry  creation              Sat Nov 21 12:48 2009  -
ambry  used                  176K                   -
ambry  available             4.00T                  -
ambry  referenced            31.4K                  -
ambry  compressratio         1.00x                  -
ambry  mounted               yes                    -
ambry  quota                 none                   default
ambry  reservation           none                   default
ambry  recordsize            128K                   default
ambry  mountpoint            /ambry                 default
ambry  sharenfs              off                    default
ambry  checksum              on                     default
ambry  compression           off                    default
ambry  atime                 on                     default
ambry  devices               on                     default
ambry  exec                  on                     default
ambry  setuid                on                     default
ambry  readonly              off                    default
ambry  zoned                 off                    default
ambry  snapdir               hidden                 default
ambry  aclmode               groupmask              default
ambry  aclinherit            restricted             default
ambry  canmount              on                     default
ambry  shareiscsi            off                    default
ambry  xattr                 on                     default
ambry  copies                1                      default
ambry  version               3                      -
ambry  utf8only              off                    -
ambry  normalization         none                   -
ambry  casesensitivity       sensitive              -
ambry  vscan                 off                    default
ambry  nbmand                off                    default
ambry  sharesmb              off                    default
ambry  refquota              none                   default
ambry  refreservation        none                   default
ambry  primarycache          all                    default
ambry  secondarycache        all                    default
ambry  usedbysnapshots       0                      -
ambry  usedbydataset         31.4K                  -
ambry  usedbychildren        144K                   -
ambry  usedbyrefreservation  0                      -
{% endhighlight %}

I just created two filesystems to store my data, and then listed the zfs attributes available to me. I would like to take advantage of a few attributes, namely (compression, nfs, snapshots)

{% highlight bash %}
root@TrayNAS:~# zfs set sharenfs=rw,anon=0 ambry #allows root access from all hosts
root@TrayNAS:/# zfs set compression=on ambry/Media
root@TrayNAS:/# zfs set compression=on ambry/Downloads
^^^ I could set compression on for the whole ambry device, but want a little more fine grained control. ^^^
mkdir -p /old_ambry/Media
mkdir /old_ambry/Downloads
mount 10.0.0.100:/ambry/Media /old_ambry/Media
mount 10.0.0.100:/ambry/Downloads /old_ambry/Downloads
--- copied all my date over ---
{% endhighlight %}

I set compression on before moving data since it doesn't activate recursively. Also I could have used zfs cloning, yet I have no need for my previous snapshots, so it's not necessary...

More to come soon, but at this point you should be able to have a fully functioning NAS, with nfs. Enjoy
