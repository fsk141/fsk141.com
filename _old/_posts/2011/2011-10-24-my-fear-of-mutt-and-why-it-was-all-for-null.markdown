---
date: '2011-10-24 16:34:24'
layout: post
slug: my-fear-of-mutt-and-why-it-was-all-for-null
status: publish
title: My fear of mutt, and why it was all for NULL
wordpress_id: '822'
categories:
- Linux
tags:
- alpine
- cli
- email
- Linux
- mutt
---


mutt... For those of us that have been using linux for a long time, we've probably heard of mutt at least once or twice throughout our lives. And most of the things that go along with mutt are negatively centered around how much of a pain in the ass it is to configure. The awfully dreadful ~/.muttrc and all the confusion that is encompassed within. Well I fell woe to the same gossip of mutt for all the years, and have mostly stayed in my browser bubble; by using gmail in firefox, and being content.

After having some time to tinker with my current solution, I setup alpine. I was thrilled it took all of 2 minutes to setup, and have a working cli email solution. This wonderful setup experience was all that I liked about alpine. The Nano-esque feel and all the random buttons I had to press were painful. Coming from a vim background, using 'w' to search for something is unholy to the standard of '/' that I'm so familiar with. So after using alpine for a few minutes, I was fed up, and did a pacman -Rs re-alpine... I was content for a little while, until the next day I figured I'd try out mutt, and if it took me more than 20 minutes I would call it quits, and just live with firefox &amp; gmail.

To my surprise, mutt only took me about 5 minutes to fetch email, and about another 5 minutes to send mail, and about 15 to tweak to my liking. I was flabbergasted that I came away from the setup unscathed, and happy with what I achieved.

I started by pointing my browser at the ever helpful Arch Linux Wiki =>   [https://wiki.archlinux.org/index.php/Mutt](https://wiki.archlinux.org/index.php/Mutt) and gandering at the setup procedure.



	
  1. **I started by adding my login information and name to ~/.mutt/muttrc**

{% highlight bash %}
#
# ~/.mutt/muttrc -- Mutt configuration
#

# Name Info
set realname = "Jonny Gerold"
set from = "me@gmail.com"

# IMAP Settings
set imap_user=me@gmail.com
set imap_pass=my_password_here
set folder=imaps://imap.gmail.com
set imap_check_subscribed

# SMTP Settings
set smtp_url=smtps://$imap_user@smtp.gmail.com
set smtp_pass=my_password_here
{% endhighlight %}

	
  2. **I then proceeded to re-install mutt with mutt-trash which enabled trash folder support:**
{% highlight bash %}yaourt -Sy mutt-trash{% endhighlight %}

	
  3. **Then I found some sweet settings by searching the internet:**
{% highlight bash %}
# Folders
mailboxes     = "+INBOX"
set spoolfile = "+INBOX"
set postponed = "+[Gmail]/Drafts"

# Archive Messages (A)
macro index,pager A "unset trash\n " "Gmail archive message"

# Thread Sort
set sort=threads

# Need trash patch for this to work
set trash = "+[Gmail]/Trash"

# store message headers locally to speed things up
set header_cache = ~/.mutt/hcache

# allow mutt to open new imap connection automatically
unset imap_passive

# keep imap connection alive by polling intermittently (time in seconds)
set imap_keepalive = 300

# how often to check for new mail (time in seconds)
set mail_check = 120

# Auto display html
set mailcap_path 	= ~/.mutt/mailcap
auto_view text/html
{% endhighlight %}

	
  4. **I topped the sunday with some pretty colors:**
{% highlight bash %}cat /usr/share/doc/mutt/samples/colors.linux >> ~/.mutt/muttrc{% endhighlight %}

	
  5. **After that I cleaned everything up a little and was left with this (removed lots of color declarations to support transparency):**[https://raw.github.com/fsk141/dotfiles/master/Hot/.mutt/muttrc](https://raw.github.com/fsk141/dotfiles/master/Hot/.mutt/muttrc)

{% highlight bash %}
#
# ~/.mutt/muttrc -- Mutt configuration
#

# Name Info
set realname = "Jonny Gerold"
set from = "fsk141@gmail.com"

# IMAP Settings
set imap_user=fsk141@gmail.com
set imap_pass=mypass
set folder=imaps://imap.gmail.com
set imap_check_subscribed

# SMTP Settings
set smtp_url=smtps://$imap_user@smtp.gmail.com
set smtp_pass=mypass

# Folders
mailboxes     = "+INBOX"
set spoolfile = "+INBOX"
set postponed = "+[Gmail]/Drafts"

# Archive Messages (A)
macro index,pager A "unset trash\n " "Gmail archive message"

# Thread Sort
set sort=threads

# Need trash patch for this to work
set trash = "+[Gmail]/Trash"

# store message headers locally to speed things up
set header_cache = ~/.mutt/hcache

# allow mutt to open new imap connection automatically
unset imap_passive

# keep imap connection alive by polling intermittently (time in seconds)
set imap_keepalive = 300

# how often to check for new mail (time in seconds)
set mail_check = 120

# Auto display html
set mailcap_path 	= ~/.mutt/mailcap
auto_view text/html

# Colors

color error brightred white
color indicator brightyellow red
color status brightgreen blue
color search white black

{% endhighlight %}


{% highlight bash %}
# Contents of ~/.mutt/mailcap
text/html; w3m -I %{charset} -T text/html; copiousoutput; #make sure you have w3m installed
{% endhighlight %}



	
  1. This is what I'm left with:


The learning curve for mutt is very simple (well for me anyways); all the basic applicable commands are available at the top of the page, and anything else can easily be found by hitting '?' and just hitting spacebar a few times till you find what you need. The one custom thing that you should pay attention to is how to Archive / Delete messages. I added a little scriptlet so that if you are reading / selecting a message you can hit 'A' (capital A) &amp; your message will be marked to be archived. Otherwise hit 'd', and your message will be sent to trash (unless you don't have the trash patch, in which case you should comment out the set trash variable)

Another nifty inclusion in my muttrc is the ability to automatically parse html (so you don't get a bunch of garbled html, that makes the emails hard to read. This inclusion is with the set mailcap &amp; ~/.mutt/mailcap additions.

Other than that the configuration above is simple, commented, and straightforward. Nothing complicated, and if you just copy the config and go, it's less than 5 minutes to have a wonderfully beautiful email client setup, and ready to use.



**Update:** [http://fsk141.com/gooey-google-contact-goodness-with-mutt](http://fsk141.com/gooey-google-contact-goodness-with-mutt) << Add Google Contacts to mutt
