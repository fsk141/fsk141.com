---
date: '2011-11-05 14:25:05'
layout: post
slug: punch-a-useful-feature-into-todo-txt
status: publish
title: '`Punch` a Useful Feature Into todo.txt '
wordpress_id: '922'
categories:
- Linux
- Software
---

[![](http://fsk141.com/uploads/2011/11/todopunch.png)](http://fsk141.com/uploads/2011/11/todopunch.png)

After using [todo.txt](http://todotxt.com) for years, I've realized that a crucial feature has been missing. I've been doing tasks, completing them; and then wondering curiously how much time I spent on something after the fact. Well after a quick googling; I found [punch-time-manager](https://code.google.com/p/punch-time-tracking), and was rewarded with a punch card style time tracking utility. It has a simple punch in/out feature with a reporting function similar to todo.txt. After using punch-time-management for the past couple weeks, I love it; and hope you'll come to love it too.



	
  1. Setup todo.txt &amp; punch-time-tracking
{% highlight bash %}
# Archlinux && yaourt (automagic)
yaourt -S todotxt punch-time-tracking

# Manual way
wget http://cloud.github.com/downloads/ginatrapani/todo.txt-cli/todo.txt_cli-2.8.tar.gz
wget https://punch-time-tracking.googlecode.com/files/punch-time-tracking-1.3.zip

tar xf todo.txt_cli-2.8.tar.gz
unzip punch-time-tracking-1.3.zip

# read the readme for todo.txt && setup

# You can add the executables (Punch.py & todo.sh) into a PATH dir or add to .bashrc || .zshrc
# This is what I have in my .zshrc
alias todo='/usr/bin/todo.sh -n -d ~/Dropbox/Public/todo.txt/todo.cfg'
{% endhighlight %}

	
  2. Modify Punch.py with todo.cfg location
{% highlight bash %}
# edit /usr/bin/punch (or Punch.py if you manually setup)

# locate the following && add your config location...
files = [ "todo.cfg", ".todo.cfg" ]

## add your specific location if it's different from the predefined defaults
files = [ "todo.cfg", ".todo.cfg", "Dropbox/Public/todo.txt/todo.cfg" ]
{% endhighlight %}

	
  3. After setup, add a task to your todo &amp; punch it:
{% highlight bash %}
$ punch in 21
Start timer on: todo/punch post

$ punch what
Active task: todo/punch post (0 minutes)

$ punch out
Stop timer on: todo/punch post

$ punch report
2011-10-28 (1 hours 43 minutes):
	do some domain moving... (12 minutes)
	mutt (1 hours 31 minutes)
	zfs (0 minutes)
2011-10-29 (0 minutes):
	Transdroid (0 minutes)
2011-11-05 (0 minutes):
	todo/punch post (0 minutes)
{% endhighlight %}


