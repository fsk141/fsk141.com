---
date: '2011-11-13 22:13:20'
layout: post
slug: muttdate-aka-mutt-update
status: publish
title: Muttdate (aka Mutt Update)
wordpress_id: '940'
categories:
- Linux
tags:
- .muttrc
- mutt
- mutt contacts
- update
- usage
---

After using mutt for a couple weeks, and tweaking the config here and there for random fixes or tweaks I've come across "the perfect muttrc" (well for me anyway). I've added a lot of tweaks from my last post, and removed the passwords entirely from the file. Along with header &amp; message caching. To make everything work hunky dory store the file in either ~/.muttrc; or where I prefer ~/.mutt/muttrc ; you also need to create cache dirs.

{% highlight bash %}mkdir -p ~/.mutt/cache/{bodies,headers}{% endhighlight %}

**Extra features I added:**



	
  * A -- Archive message

	
  * ga -- Goto All Mail

	
  * gi -- Goto Inbox

	
  * gs -- Goto Sent Mail

	
  * gd -- Goto Drafts




**Recommended Reading:** [http://www.mutt.org/doc/manual/manual-2.html](http://www.mutt.org/doc/manual/manual-2.html)


**Download here:** [https://raw.github.com/fsk141/dotfiles/master/Hot/.mutt/muttrc](https://raw.github.com/fsk141/dotfiles/master/Hot/.mutt/muttrc)

**View Here:**

{% highlight bash %}
#
# ~/.mutt/muttrc -- Mutt configuration
#

# Name Info
set realname = "Jonny Gerold"
set from = "fsk141@gmail.com"

# IMAP Settings
set imap_user = fsk141@gmail.com
set imap_pass = `grep 'Gmail' ~/Private/passwords.txt | awk '{print $2}'`
set folder = imaps://imap.gmail.com
set imap_check_subscribed

# SMTP Settings
set smtp_url=smtps://$imap_user@smtp.gmail.com
set smtp_pass = `grep 'Gmail' ~/Private/passwords.txt | awk '{print $2}'`

# keep imap connection alive by polling intermittently (time in seconds)
set imap_keepalive = 120

# set timeout (time in seconds)
set timeout = 120

# how often to check for new mail (time in seconds)
set mail_check = 60

# Folders
mailboxes     = "+INBOX"
set spoolfile = "+INBOX"
set postponed = "+[Gmail]/Drafts"
set record 	  = /dev/null

# Need trash patch for this to work
set trash = "+[Gmail]/Trash"

# store message headers locally to speed things up
set header_cache = ~/.mutt/cache/headers

# how about store messages too
set message_cachedir =~/.mutt/cache/bodies

# Mailcap (autoexecute program declarations)
set mailcap_path 	= ~/.mutt/mailcap

# Auto display html
auto_view text/html

# Archive Messages (A) and some other nifty commands
bind editor  noop #fix for spaces in names of folders
macro index,pager A "unset trash\n " "Gmail archive message"
macro index,pager gi "=INBOX" "Go to inbox"
macro index,pager ga "=[Gmail]/All Mail" "Go to all mail"
macro index,pager gs "=[Gmail]/Sent Mail" "Go to starred messages"
macro index,pager gd "=[Gmail]/Drafts" "Go to drafts"

# Goobook query (google contacts)
set query_command="goobook query '%s'"
bind editor \t complete-query

# Add contacts to google
macro index,pager a "goobook add" "add the sender address to Google contacts"

# Reload goobook db
macro index,pager gr "!goobook reload\n" "Goobook reload"

# Dont request to move messages
set move = no

# Auto include copy of original message when you reply
set include = yes

# Thread Sort (Top = newest messages)
set sort = threads
set sort_aux = 'reverse-last-date-received'

# Unset Markers (don't add + signs if message wraps)
unset markers

# allow mutt to open new imap connection automatically
unset imap_passive

# Colors (transparent background)
color error brightred white
color indicator brightyellow red
color status brightgreen blue
color search white black
{% endhighlight %}

**~/.mutt/mailcap**

{% highlight bash %}
text/html; links -html-numbered-links 1 -dump %s; copiousoutput;
image/*; feh -F %s;
application/pdf; evince %s;
{% endhighlight %}
**Other posts in my mutt adventure:**





	
  * **[http://fsk141.com/my-fear-of-mutt-and-why-it-was-all-for-null
](http://fsk141.com/my-fear-of-mutt-and-why-it-was-all-for-null )**

	
  * **[http://fsk141.com/gooey-google-contact-goodness-with-mutt
](http://fsk141.com/gooey-google-contact-goodness-with-mutt )**

	
  * **[http://fsk141.com/simply-check-if-you-have-new-gmails](http://fsk141.com/simply-check-if-you-have-new-gmails)
**


