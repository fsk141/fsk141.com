---
date: '2011-10-23 11:00:05'
layout: post
slug: tutorial-disable-ssh-password-login-enable-ssh-key-auth
status: publish
title: '[tutorial] Disable ssh password login; Enable ssh key auth'
wordpress_id: '805'
categories:
- Linux
- Tutorials
tags:
- key auth
- key authentication
- keyauth
- password
- security
- ssh
---

[![](http://fsk141.com/uploads/2011/10/2011-10-23-102506_235x164_scrot.png)](http://fsk141.com/uploads/2011/10/2011-10-23-102506_235x164_scrot.png)

So you have a Linux server accessible to the outside network? Oh, have you checked your auth logs lately? Bet not... Well go ahead and check; if you have a server publicly accessible your auth log should be full with potential login attempts. Hopefully all of them failed attempts; I would recommend going ahead and checking your Accepted logins, make sure it's not at 3 in the morning on a Saturday; or sometime you'd never login :)

Anywho; You could spend a whole lot of time and effort setting up a software or hardware mitigation solution. While this can be efficient in finding users that shouldn't have access to your servers, and can help you with overall blacklists, you can still get leaks through. Say someone happens to guess your password in less than 3 times and you limit is set to 3? What a bummer, server compromised. Anywho, this solution should prevent EVERYONE (except you of course) from accessing your server. Well via password auth anyways...


  1. **Lets start on the local machine that plans on connecting to the ssh server:**

{% highlight bash %}
###Result (this isn't my key; or hostname :):
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/fsk141/.ssh/id_rsa): .meh
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in .ssh/id_rsa.
Your public key has been saved in .ssh/id_rsa.pub.
The key fingerprint is:
a4:fb:92:4b:33:a5:f6:5b:b8:ce:f7:a1:78:73:4a:f0 fsk141@oHai
The key's randomart image is:
+--[ RSA 2048]----+
|                 |
|                 |
|        .        |
|       o         |
|      . S        |
|       + +       |
|      B.. E .    |
|     oo* =+...   |
|      .+O+o=.    |
+-----------------+
{% endhighlight %}


  2. **Now that we have our ssh public key available, lets copy it to the server:**
{% highlight bash %}
## if you have ssh-copy-id ##
ssh-copy-id myserver.com

## if you don't have ssh-copy-id ##
## you can run the ssh "commands" on you server
## I just made it so everything
## could be done from connecting localhost
ssh me@myserver.com 'if [ ! -d ~/.ssh ]; then mkdir ~/.ssh; fi'
scp .ssh/id_rsa.pub me@myserver.com:.ssh/authorized_keys
ssh me@myserver.com 'chmod 600 ~/.ssh/authorized_keys'
{% endhighlight %}




  3. **Now that we're all ready, test things out; make sure you can login w/o password auth**
{% highlight bash %}
ssh me@myserver.com
{% endhighlight %}



  4. **Onto disabling password auth** (**WARNING:** make sure you can login first, otherwise you'll lock yourself out!
  

Edit /etc/ssh/sshd_config &amp; add the following:
{% highlight bash %}
PermitRootLogin no ##(optional) enable this if you don't want root login (strongly advised)
PasswordAuthentication no
{% endhighlight %}



  5. **Restart sshd server**
{% highlight bash %}
sudo /etc/rc.d/sshd restart
{% endhighlight %}



  6. **Test everything on another terminal, before closing your session:**  
  

If everything works, hooray, enjoy a secure system.




