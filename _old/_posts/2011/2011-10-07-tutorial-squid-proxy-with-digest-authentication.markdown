---
date: '2011-10-07 18:37:17'
layout: post
slug: tutorial-squid-proxy-with-digest-authentication
status: publish
title: '[tutorial] Squid Proxy [with digest authentication]'
wordpress_id: '788'
categories:
- Linux
- Networking
- Tutorials
---

After posting a simple method of setting up squid proxy with basic authentication, I figured I'd post a little more secure method. The digest authentication procedure is simple, instead of transmitting your username/password in plaintext, you use an md5summed answer that protects your credentials. While some traffic could be sniffed (will address that with another post about ssl encrypting your squid proxy), your credentials will be safe. Anywho lets dive right in



	
  1. Install squid &amp; continue to step 2

	
  2. Configure a new /etc/squid/squid.conf
{% highlight bash %}
# Digest Squid Auth -- better method
# /etc/squid/squid.conf
#

http_port 3129

auth_param digest program /usr/lib/squid/digest_pw_auth -c /etc/squid/digest_passwd

# Make /etc/squid/digest_password this way:
## First get a script...
## wget http://dl.dropbox.com/u/52078/digest_passwd.sh
## Execute something similar to the following
### sh ./digest_passwd.sh your_username_here your_password_here 'Squid proxy-caching web server' > /etc/squid/digest_passwd # need to execute as root/sudo
## This will give you a happy digest_passwd file

auth_param digest children 5
auth_param digest realm Squid proxy-caching web server
auth_param digest nonce_garbage_interval 5 minutes
auth_param digest nonce_max_duration 30 minutes
auth_param digest nonce_max_count 50

acl authenticated proxy_auth REQUIRED
http_access allow authenticated
{% endhighlight %}

	
  3. Save the file, don't forget to create /etc/squid/digest_passwd
{% highlight bash %}# Make /etc/squid/digest_password this way:
## First get a script...
## wget http://dl.dropbox.com/u/52078/digest_passwd.sh (contents below)
------
#!/bin/sh

user=$1
pass=$2
realm=$3

if [ -z "$1" -o -z "$2" -o -z "$3" ] ; then
        echo "Usage: $0 user password 'realm'";
        exit 1
fi

ha1=$(echo -n "$user:$realm:$pass"|md5sum |cut -f1 -d' ')
echo "$user:$realm:$ha1"
------
## Execute something similar to the following
### sh ./digest_passwd.sh your_username_here your_password_here 'Squid proxy-caching web server' > /etc/squid/digest_passwd # need to execute as root/sudo
## This will give you a happy digest_passwd file
{% endhighlight %}

	
  4. Startup squid, and enjoy a slightly more protected experience...


