---
date: '2009-09-09 00:46:46'
layout: post
slug: habari-on-lighttpd-setting-up-web-environment-new-laptop
status: publish
title: Habari on Lighttpd
wordpress_id: '693'
categories:
- Linux
tags:
- habari
- lighttpd
- rewrite rules
---

I've used apache for the longest time, and it has served me very well. I still use apache to host this site, and many other sites. Yet I've grown an itch to try something new, and have ventured to lighttpd. It's painless to setup, and the config file is one of the simplest around. Anywho after I switched over I tried to setup habari and hit a little snag. Habari uses a .htaccess file with apache to take advantage of mod_rewrite. Well lighttpd doesn't support .htaccess, or any other directory independent configs :). Anywho it's simple enough to setup a rewrite in /etc/lighttpd/lighttpd.conf. Yet if you set the rewrite for the localhost, you are left with the habari site, and then anything else you want to develop has to mangle together. I wanted a simple solution that would allow me to run habari, yet not in the root of my web directory. Also I wanted 'http://localhost' to display my webroot contents (who needs an index file?)

Step 1) Edit your /etc/hosts, and add another faux host besides localhost:

    
    #
    # /etc/hosts: static lookup table for host names
    #
    
    #      
    127.0.0.1       localhost.localdomain   localhost Minifsk
    127.0.0.1       fsk141.localdomain      localhost Minifsk
    # End of file


Step 2) Fire up your editor, and open /etc/lighttpd/lighttpd.conf

    
    #uncomment "mod_rewrite" module
    
    #add index.php to index file names
    index-file.names = ( "index.php", "index.html",
                                    "index.htm", "default.htm" )
    
    #add a vhost
    $HTTP["host"] =~ "^fsk141.localdomain$" {
      server.document-root = "/srv/http/Web/fsk141"
      url.rewrite-once =  (
            "^/(?!scripts/|3rdparty/|system/|doc/)(.*)$" =>  "/index.php"
      )
      server.errorlog = "/var/log/lighttpd/fsk141/error.log"
      accesslog.filename = "/var/log/lighttpd/fsk141/access.log"
    }
    
    #enable dir listing
    dir-listing.activate       = "enable"
    
    #uncomment fast-cgi stuf:
    fastcgi.server             = ( ".php" =>
                                   ( "localhost" =>
                                     (
                                       "socket" => "/var/run/lighttpd/php-fastcgi.socket",
                                       "bin-path" => "/usr/bin/php-cgi"
                                     )
                                   )
                                )


Save and Close!

Now all you have to do is go through the standard habari install steps. As per the directory you setup as your vhost. for the example above I must put my habari files in '/srv/http/Web/fsk141', and to start the install process I open 'http://fsk141.localdomain' Oh and btw the vhost includes the rewrite!!! Don't forget about the rewrite rules otherwise habari will bork...
