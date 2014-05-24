---
date: '2009-01-21 00:00:00'
layout: post
slug: simple-socks-5-proxy-ssh-tunnel
status: publish
title: Simple Socks 5 proxy [SSH tunnel]
wordpress_id: '39'
categories:
- Networking
---

Need a simple haxie to tunnel your computer traffic? Use an SSH tunnel; it's simple and painless.

What you need:
Server
-SSH

Client
-SSH
-Web Browser (Firefox + FoxyProxy \[What I use])
-Any other application that allows Socks 5 proxy

Procedure:

Open up a terminal and type the following:

    
    ~ ssh fsk141@fsk.tld -ND 1337


Description:
Use SSH to tunnel all of your traffic through port 1337

- ssh (Secure Shell)
- fsk141@fsk141.tld (user:host)
- -ND (look below for man page)
- 1337 (port number)

Man page entries for -N -D

    
           -D [bind_address:] port
                  Specifies a local ‘‘dynamic'' application-level port forwarding.  This works by allocating a socket to listen to port on the local side, optionally
                  bound  to  the  specified  bind_address.   Whenever a connection is made to this port, the connection is forwarded over the secure channel, and the
                  application protocol is then used to determine where to connect to from the remote machine.  Currently the SOCKS4 and  SOCKS5  protocols  are  sup?
                  ported, and ssh will act as a SOCKS server.  Only root can forward privileged ports.  Dynamic port forwardings can also be specified in the config?
                  uration file.
    
                  IPv6 addresses can be specified with an alternative syntax:
                   [bind_address/] port or by enclosing the address in square brackets.  Only the superuser can forward privileged ports.  By default, the local port
                  is  bound in accordance with the GatewayPorts setting.  However, an explicit bind_address may be used to bind the connection to a specific address.
                  The bind_address of ‘‘localhost'' indicates that the listening port be bound for local use only, while an empty address or ‘*' indicates  that  the
                  port should be available from all interfaces.
    
            -N     Do not execute a remote command.  This is useful for just forwarding ports (protocol version 2 only).


Now that you're connected you can jump over to firefox:

    
    Preferences > Advanced > Settings


[![](http://farm4.static.flickr.com/3529/3215016569_63bc44e0dd.jpg)](http://www.flickr.com/photos/68444690@N00/3215016569)

Just input the pertinent information (localhost:1337) and make sure socks5 is bubbled... That's it... It's a very simple process, and is very handy when you need to access a blocked site, or protect valuable information.
