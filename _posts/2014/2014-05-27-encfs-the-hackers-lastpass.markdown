---
date: '2014-05-26 21:46:00'
layout: post
slug: encfs-the-hackers-lastpass
title: "Encfs - The Hackers Lastpass"
btcaddy: 1BaUpLKReSfr5s3My1QHVcpBGM1CgTrXT9
categories:
- Linux
tags:
- encfs
- encryption
- fuse
- filesystems
- lastpass
description: "The necessity to store private information is crutial. Whether it be passwords, qr codes to backup your 2FA (I lost a phone once, big pain in the ass), bitcoin private keys, or just information you would like to hide from prying eyes. While there are amazing services available (lastpass for example), the control is in the enterprise, and less up to you. With encfs you can easily encrypt data, move it around, and sync it with whatever services (dropbox, copy.com, your own server) your heart desires. You can even have encfs directories within encfs directories for tiered levels of awesome (aka crazy... :)"
---

{% thumbnail images/2014/05-26-encfs-the-hackers-lastpass/encfs.png 600x600> %}

# Introduction
The necessity to store private information is crutial. Whether it be passwords, qr codes to backup your 2FA (I lost a phone once, big pain in the ass), bitcoin private keys, or just information you would like to hide from prying eyes. While there are amazing services available (lastpass for example), the control is in the enterprise, and less up to you. With encfs you can easily encrypt data, move it around, and sync it with whatever services (dropbox, copy.com, your own server) your heart desires. You can even have encfs directories within encfs directories for tiered levels of awesome (aka crazy... :)

# What is encfs?
The premise of encfs is simple, and while it has it's caveat's, it's a perfect level of protection for most. 
<br /><br />

Benefits to encfs are it's size, and no necessity to preallocate space. When starting out encfs is only a couple byte's &amp; will grow with whatever data you throw at your unencrypted directory. Encfs is also synced on a file-by-file basis (so if you're using a service like dropbox, it will know what individual files to update). This reduces the need to backup the entire encrypted partition every time you need to backup, just send the updates (automagically with a service, or rsync -ru). Encfs can also be added on top of whatever filesystem you want (nfs, zfs, ext4, samba, dropbox, etc), it's run through a fuse module, and believe me, it's magic (well not magic, but simply magic).
<br /><br />

Drawbacks to encfs include anything pertaining to metadata. While you can't run [file](http://man.cx/file) to figure out what the data is, you can view the number of files / folders, permissions, filesize, and an approximate of filename size. But for all intensive purposes these don't matter to most. Especially when you're using encfs as a secondary layer of protection over say [dmcrypt](https://code.google.com/p/cryptsetup/wiki/DMCrypt)

# Installation

I'm going to go over a basic installation (taking into account that you're prolly not compiling this; if you are you should know what's up).

{% highlight bash %}
pacman -S encfs ## arch linux
apt-get install encfs ## debian / ubuntu
{% endhighlight %}

At this point you are provided with a few lovely utilities (encfs, encfsctl, encfssh). I'll be going over the usefulness of encfs & encfsctl. By the way there are a few gui's to help the end user even further, but I'll let you make the judgement call on that (it's easy enough on the cli).

# Diving Right in
So now that we know a little about what encfs is, lets start using it. You might be suprised at how amazingly simple this is to use.
<br /><br />

I store my encrypted directory on copy.com (so it's constantly backed up), and store my unencrypted directory as a dotfile ".Unencrypted" for example. The purpose being that I have the directory that matters backed up, and the other quasi hidden with the guise of a dotfile (hidden in \*nix world)
<br /><br />

Let's start by creating our directory structure. How encfs works is that you have an encrypted folder &amp; and unencrypted one. You mount the encrypted folder to the unencrypted one &amp; store all your data there. If encfs doesn't exist yet, then it'll query you to generate a new encryption scheme &amp; ask for a password.
<br /><br />

I've gone ahead and created a directory structure for demonstration purposes. (~/encfs/{Private,Unprivate})

{% highlight bash %}
mkdir ~/encfs/{Private,Unprivate}
{% endhighlight %}

Now to initialize the creation of your encrypted paradise:

	encfs <path to encrypted dir> <path to unencrypted dir>

{% highlight bash %}
[fsk141 ~/encfs]$ encfs ~/encfs/Private ~/encfs/Unprivate 
Creating new encrypted volume.
Please choose from one of the following options:
 enter "x" for expert configuration mode,
 enter "p" for pre-configured paranoia mode,
 anything else, or an empty line will select standard mode.
?>
{% endhighlight %}

At this point you have the option to select defaults (**s**) (great for most), or (**p**) paranoid (for a bit more protection), or even better go expert & select the max for everything (kinda slow).
<br /><br />

To take a quick tangent, here's the description of what's what from the man page:
{% highlight bash %}
       Standard mode uses the following settings:
           Cipher: AES
           Key Size: 192 bits
           PBKDF2 with 1/2 second runtime, 160 bit salt
           Filesystem Block Size: 1024 bytes
           Filename Encoding: Block encoding with IV chaining
           Unique initialization vector file headers

       Paranoia mode uses the following settings:
           Cipher: AES
           Key Size: 256 bits
           PBKDF2 with 3 second runtime, 160 bit salt
           Filesystem Block Size: 1024 bytes
           Filename Encoding: Block encoding with IV chaining
           Unique initialization vector file headers
           Message Authentication Code block headers
           External IV Chaining
{% endhighlight %}

Back to setting up encfs, I selected (**s**), since that will suffice for me,

{% highlight bash %}
[fsk141 ~/encfs]$ encfs ~/encfs/Private ~/encfs/Unprivate 
Creating new encrypted volume.
Please choose from one of the following options:
 enter "x" for expert configuration mode,
 enter "p" for pre-configured paranoia mode,
 anything else, or an empty line will select standard mode.
?> s

Standard configuration selected.

Configuration finished.  The filesystem to be created has
the following properties:
Filesystem cipher: "ssl/aes", version 3:0:2
Filename encoding: "nameio/block", version 3:0:1
Key Size: 192 bits
Block Size: 1024 bytes
Each file contains 8 byte header with unique IV data.
Filenames encoded using IV chaining mode.
File holes passed through to ciphertext.

Now you will need to enter a password for your filesystem.
You will need to remember this password, as there is absolutely
no recovery mechanism.  However, the password can be changed
later using encfsctl.

New Encfs Password: 
Verify Encfs Password: 
{% endhighlight %}

After selecting standard mode &amp; pressing enter, you'll be told what's what &amp; asked to enter a password. Enter your password (you can change this later), and that's it! At this point you now have "~/encfs/Private" which stores your encrypted data that lives in "~/encfs/Unprivate". And encfs has also taken the liberty in mounting ~/encfs/Private -> ~/encfs/Unprivate for you.

{% highlight bash %}
[fsk141 ~]$ df -h | grep Unprivate
encfs                      212G  192G  8.8G  96% /home/fsk141/encfs/Unprivate
{% endhighlight %}

# But What Happens After I reboot
Upon rebooting, the unencrypted partition will no longer be mounted. If you would like to manually unmount, or test to make sure you did everything correctly use fusermount.

{% highlight bash %}
[fsk141 ~/encfs]$ fusermount -u ~/encfs/Unprivate 
{% endhighlight %}

Now lets remount everything (as if we rebooted)

{% highlight bash %}
[fsk141 ~/encfs]$ encfs ~/encfs/Private ~/encfs/Unprivate
EncFS Password: 
[fsk141 ~/encfs]$ 
{% endhighlight %}

If everything went well, then you'll get a prompt back, and your encfs partition is mounted properly.

# Using Your Shiny new encfs Container
So if I wanted to store some files, this is what it would look like:

{% highlight bash %}
[fsk141 ~/encfs]$ touch Unprivate/{here,are,some,files}

[fsk141 ~/encfs]$ ls Unprivate 
are  files  here  some

[fsk141 ~/encfs]$ ls Private 
1Pm2o0r1uYgZ1iKS7SCrnQQf  BW5E85i6VJHclb3D2RCf9B0b  MxZrqiVg7t13camXVQqE9a4k  ROm97CNXS9XRFiHk3jKRKGfx
{% endhighlight %}

The beauty lies in the ability to store more than just blobs of text. You can store anything (images, documents, music, bitcoin private keys, etc.)

# Conclusion
Having the ability to privately and securely store data is crutial. There should be no excuse for not storing and backing up your important data. The problem lies when the data is private (passwords &amp; the like), and not wanting to throw them up on a service like dropbox due to the nature of the information. Encfs allows you to forgo the worry of storing this data in even the most insecure of places. Knowledge is power, go forth &amp; encrypt all the datas!
<br /><br />

**References:**

1. [http://www.arg0.net/encfs](http://www.arg0.net/encfs)

2. [http://www.arg0.net/encfsintro](http://www.arg0.net/encfsintro)

3. [https://defuse.ca/audits/encfs.htm](https://defuse.ca/audits/encfs.htm)
