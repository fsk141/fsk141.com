---
date: '2011-11-02 21:34:48'
layout: post
slug: simply-check-if-you-have-new-gmails
status: publish
title: Simply Check if you Have new Gmails
wordpress_id: '895'
categories:
- Homebrew
- Linux
- Software
---

[![](http://fsk141.com/uploads/2011/11/gmailstatus.png)](http://fsk141.com/uploads/2011/11/gmailstatus.png)

After setting up mutt; I figured I'd go the extra mile; and add a notification system so that I know when I have some new emails. Well come to find out; it was easy as pie. And I learned a few new little things in the process.
First of all; gmail has a nifty url that serves a feed for your emails:

[https://mail.google.com/mail/feed/atom](https://mail.google.com/mail/feed/atom)

The feed will tell you about new emails, unread emails, and stuff like that. But what makes the feed so awesome, is that it' s well labeled, and easy to parse. So for my script I just need to call the url, pass a username and password, and enjoy the output:

Download Script: [https://raw.github.com/fsk141/scripts/master/checkgmail](https://raw.github.com/fsk141/scripts/master/checkgmail)

Or view it below:

{% highlight bash %}

#!/bin/sh

## Simple Check Gmail ##
## Jonny Gerold  ##

# User & Password should look like the following:
## set imap_user = me@domain.tld
## set imap_pass = my_pass
### You can hardset USER & PASS here if you would like; I just
### didn't want my user/pass all over the place in conf files

USER=$(grep 'set imap_user' ~/.mutt/muttrc | awk '{ print $4 }')
PASS=$(grep 'set imap_pass' ~/.mutt/muttrc | awk '{ print $4 }')

# Check mail status...
function check () {
curl -s -u $USER:$PASS https://mail.google.com/mail/feed/atom
}

# Find number of messages unread
function fullcount () {
check |  grep '<fullcount>' | sed -e 's/<fullcount>//' -e 's/<\/fullcount>//'
}

# Print result
function print () {
if [[ -n $(fullcount) ]] && [[ $(fullcount) == '1' ]]; then
echo "You have $(fullcount) new email!"
else
echo "You have $(fullcount) new emails!"
fi
}

# notify-send that message!
#notify-send "$(print)"

# output number of new emails
fullcount

# static print 'You have $(fullcount) new email(s)
#print

{% endhighlight %}

How to make the script work for you?



	
  1. Add it to your crontab
{% highlight bash %}10 * * * * * ~/checkgmail{% endhighlight %}

	
  2. If you use statnot; add it to your ~/.statusline
{% highlight bash %}
# I have the script set to just output the number of new emails
function nEmail () {
if [[ $(~/.scripts/checkgmail) > 0 ]]; then
echo ">> $(~/.scripts/checkgmail) NEW EMAIL | "
fi
}
# my actual output looks something like this:
echo "$(nEmail)$(mpd)V: $volume | B: $(bat) | $datetime";
{% endhighlight %}

	
  3. Add it to conky
{% highlight bash %}exec ~/checkgmail{% endhighlight %}



