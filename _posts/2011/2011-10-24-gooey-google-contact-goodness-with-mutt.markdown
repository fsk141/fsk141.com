---
date: '2011-10-24 23:23:16'
layout: post
slug: gooey-google-contact-goodness-with-mutt
status: publish
title: Gooey Google Contact Goodness (with mutt)
wordpress_id: '866'
categories:
- Linux
tags:
- email
- goobook
- google contacts
- Linux
- mutt
---

[![](http://fsk141.com/uploads/2011/10/goobook_entries.png)](http://fsk141.com/uploads/2011/10/goobook_entries.png)

After just being acquainted with mutt ([My fear of mutt, and why it was all for NULL](http://fsk141.com/my-fear-of-mutt-and-why-it-was-all-for-null)) I forgot that I hate remembering things. I have too many things to remember already by using linux; why would I want to start having to remembering people's names and email addresses on top of all that unix/linux sysadmin knowledge?

Well instead of hacking together some halfassed solution by exporting a csv of my google contacts, and parsing, and gobblygook; I did a quick search and found that there's a program called goobook ([Homepage](http://pypi.python.org/pypi/goobook/1.4alpha4)) Also, and a little down the page is a neato description on how to use it with mutt.

**After installing goobook (yaourt -S goobook-git) I configured it by making a new file ~/.netrc**

{% highlight bash %}
machine google.com
login me@gmail.com
password my_password_here
{% endhighlight %}

**I then tested it out to make sure it's working:**

{% highlight bash %}
# Enjoy all your contacts being dumped to stdout
goobook dump_contacts
{% endhighlight %}

As long as something outputted then you should be golden, otherwise enjoy troubleshooting; there's not that much you could have done wrong up until this point :P

**Now that you have goobook working, lets integrate it into mutt:**

{% highlight bash %}
# Add the following to your ~/.mutt/muttrc

# Goobook query (google contacts)
set query_command="goobook query '%s'"
bind editor \t complete-query ## tab completion for contacts :)

# Add contacts to google
macro index,pager a "goobook add" "add the sender address to Google contacts"

# Reload goobook db
macro index,pager gr "!goobook reload\n" "Goobook reload"
{% endhighlight %}

**Now all you have to do is reload mutt, and enjoy your spoils:**



	
  * To search just start an email; and hit your tab key to query the contact database

	
  * To add a new contact for an email you're reading/have selected just press the letter a

	
  * To reload the goobook database (say you added a contact with your phone, or online); just hit gr in sequence; and enjoy a fresh and up to date goobook database.


On a side note, I seem to be migrating deeper into cli apps, and ditching the gui counterparts. Even though I have a killer computer with a bunch of ram;  I figure the cpu cycles can be better spent on something else. And at best case scenario, it might save some of my precious battery life. Expect some more posts I guess about all the new cli things that I've setup.
