---
date: '2014-04-28 18:00:00'
layout: post
slug: run-a-full-bitcoin-node
status: publish
title: "Run A Full Bitcoin Node"
categories:
- Bitcoin
tags:
description: ""
---

<a href="/images/2014/04-28-all-in-with-bitcoin/bitcoin.png">{% thumbnail images/2014/04-28-all-in-with-bitcoin/bitcoin.png 600x600> %}</a>

# Introduction

Started by installing a new debian vps, could only install debian 6, so I upgraded it to 7.5

[[[
deb http://mirrors.kernel.org/debian/ wheezy main
deb-src http://mirrors.kernel.org/debian/ wheezy main

deb http://security.debian.org/ wheezy/updates main
deb-src http://security.debian.org/ wheezy/updates main
]]]

[[[
apt-get update
apt-get upgrade
]]]

... wait patiently fer everything to finish

... set the time
[[[
rm /etc/localtime
# look in /usr/share/zoneinfo for timezone info
ln -s /usr/share/zoneinfo/America/Los_Angeles /etc/localtime
]]]

... set the hostname
[[[
vim /etc/hostname
vim /etc/hosts
	(((
	127.0.0.1 localhost.localdomain localhost
	# Auto-generated hostname. Please do not remove this comment.
	199.167.192.178 coinity.fsk141.com  coinity
	)))

... clean up all the shit
[[[
ps ax # remove processes
apt-get remove apache2 sendmail apache2-doc apache2-mpm-prefork apache2-utils apache2.2-bin apache2.2-common bind9 bind9-host bind9utils samba samba-common sendmail-base sendmail-bin sendmail-cf sendmail-doc
]]]

...secure the box
[[[
adduser <username> # adduser fsk141
(on local pc) ssh-keygen [if you haven't created a key before]
(olpc) ssh-copy-id <servername or ip> # ssh-copy-id coinity.fsk141.com
visudo (already had a group to add to (sudo group)
	(((
	%sudo	ALL=(ALL:ALL) ALL
	)))
gpasswd -a <username> <groupname> # gpasswd -a fsk141 sudo
vim /etc/ssh/sshd_config 
	(((
	PasswordAuthentication no
	PermitRootLogin no
	)))

service ssh restart # leave a terminal open just in case you mess up
test login # ssh coinity.fsk141.com
]]]

... install bitcoind, setup configuration, download bootstrap.dat
[[[
# this is kinda a retarted way to go about it
vim /etc/apt/sources.list
	(((
	#Repo to install bitcoind; remove after done installing!
	deb http://http.us.debian.org/debian/ unstable main contrib non-free
	deb-src http://http.us.debian.org/debian/ unstable main contrib non-free 
	)))
apt-get update
apt-get install bitcoind

vim /etc/apt/sources.list
	((( comment out the lines for bitcoind )))
]]]

... install rtorrent to download bootstrap.dat

[[[
apt-get install rtorrent
wget https://bitcoin.org/bin/blockchain/bootstrap.dat.torrent
rtorrent bootstrap.dat.torrent
]]]

... wait for that shiz to finish (should prolly start this at the beginning... heh)

[[[
rm bootstrap.dat.torrent
mkdir ~/.bitcoin
mv bootstrap.dat ~/.bitcoin
bitcoind # add an rpc user / pass if asked

	(((
	fsk141@coinity:~$ bitcoind
	Error: To use the "-server" option, you must set a rpcpassword in the configuration file:
	/home/fsk141/.bitcoin/bitcoin.conf
	It is recommended you use the following random password:
	rpcuser=bitcoinrpc
	rpcpassword=5YGymiG8HGt32m8eUK4t3mjbacZ6aUrVAfMTNHXfvPZy
	(you do not need to remember this password)
	The username and password MUST NOT be the same.
	If the file does not exist, create it with owner-readable-only file permissions.
	It is also recommended to set alertnotify so you are notified of problems;
	for example: alertnotify=echo %s | mail -s "Bitcoin Alert" admin@foo.com
	)))

vim ~/.bitcoin/bitcoin.conf

	(((
	rpcuser=bitcoinrpc
	rpcpassword=5YGymiG8HGt32m8eUK4t3mjbacZ6aUrVAfMTNHXfvPZy
	)))

chown 600 ~/.bitcoin/bitcoin.conf
]]]

... have bitcoind start @ reboot
[[[
crontab -e
	(((
	@reboot while true; do /usr/bin/bitcoind; done

	)))
]]]

... wait for everything to finish, check here:
https://getaddr.bitnodes.io

...set rdns if you're cool

... setup vnstat if you're really cool

[[[
apt-get install vnstat
vim /etc/vnstat.conf # edit default interface (venet0) for me
service vnstat start
update-rc.d vnstat enable
]]]

... install nginx to go above and beyond
[[[
apt-get install nginx
fsk141@coinity:~$ sudo service nginx start
sudo update-rc.d nginx enable
]]]

create an automagical script to setup bitcoind w/ nginx maybe...
-Setup a vps
-Secure a vps
-Setup bitcoind
-vnstat monitor
Add a little flare with nginx & a hosted page to donate


# Conclusion
