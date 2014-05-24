---
date: '2012-05-17 23:42:02'
layout: post
slug: the-playful-side-of-ls
status: publish
title: "The playful side of ls"
wordpress_id: '1282'
categories:
- Linux
tags:
- dir
- directory listing
- ls
description: "I use ls way to many times to count (2215 times in my 10k line ~/.histfile). And while I use this amazing application daily, I didn't know the power lying in the depths of ls. A short visit to the manfile showed me the beautiful side behind ls. The little things that matter, but didn't use because it was faster to whip up a quick awk script instead of reading the manpage."
thumb: "http://fsk141.com/uploads/2012/05/man_ls.png"
---

[![](http://fsk141.com/uploads/2012/05/man_ls.png)](http://fsk141.com/uploads/2012/05/man_ls.png)


# Introduction

I use ls way to many times to count (2215 times in my 10k line ~/.histfile). And while I use this amazing application daily, I didn't know the power lying in the depths of ls. A short visit to the manfile showed me the beautiful side behind ls. The little things that matter, but didn't use because it was faster to whip up a quick awk script instead of reading the manpage.


## ls && ls -la


-- **First things first, lets show the side of ls that most of us use most:**

[![](http://fsk141.com/uploads/2012/05/ls.png)](http://fsk141.com/uploads/2012/05/ls.png)

[![](http://fsk141.com/uploads/2012/05/ls-la.png)](http://fsk141.com/uploads/2012/05/ls-la.png)

I would say that ls and ls -la are the most commonly used variations of ls. One shows a simple directory structure, while the other shows executable permissions, ownership, filesize, modification date; all the necessary useful bits that you need to see.

Lets start with some visuals. If you didn't notice, my listings are in color (I have ls --color as an alias to ls). But say I wanted to have directories stand out a little more, or symlinks:

[![](http://fsk141.com/uploads/2012/05/ls-F.png)](http://fsk141.com/uploads/2012/05/ls-F.png)

According to the manpage there are many indicators `(one of */=>@|)`, yet I'm unsure what the other indicators are? If I knew I would have included a few more examples.

Other than displaying little symbols to specify what the file is, there are a couple formatting options:


## ls -m


-- **commas (and a similar example with tr):**
[![](http://fsk141.com/uploads/2012/05/ls-m_tr.png)](http://fsk141.com/uploads/2012/05/ls-m_tr.png)


## ls -1


-- **single-column (and example with sed -- since sed is a stream editor, no options will output results on a new line):**
[![](http://fsk141.com/uploads/2012/05/ls-1_sed1.png)](http://fsk141.com/uploads/2012/05/ls-1_sed1.png)


## ls --group-directories-first


-- **list folders first & combo list folders first with single-column layout:**
[![](http://fsk141.com/uploads/2012/05/ls-group-1.png)](http://fsk141.com/uploads/2012/05/ls-group-1.png)


## ls -I


-- **hide items while listing (--hide is ignored with -a|-A , yet -I|--ignore isn,t):**
[![](http://fsk141.com/uploads/2012/05/ls-hide-I.png)](http://fsk141.com/uploads/2012/05/ls-hide-I.png)


## ls -lh


-- **display with file sizes (will not size folders, only files):**
[![](http://fsk141.com/uploads/2012/05/ls-lh.png)](http://fsk141.com/uploads/2012/05/ls-lh.png)


## ls -og


-- **don't show owner/user | both**
[![](http://fsk141.com/uploads/2012/05/ls-og.png)](http://fsk141.com/uploads/2012/05/ls-og.png)


## ls -q


-- **wrap names in quotes (great for scripts that need to follow directories)**
[![](http://fsk141.com/uploads/2012/05/ls-q.png)](http://fsk141.com/uploads/2012/05/ls-q.png)


## ls -R


-- **list recursively (prints prettier than ls ***
[![](http://fsk141.com/uploads/2012/05/ls-R-750x230.png)](http://fsk141.com/uploads/2012/05/ls-R.png)


## ls -t


-- **sort by time (and formatting how time is shown (--time-style=+)**
[![](http://fsk141.com/uploads/2012/05/ls-t.png)](http://fsk141.com/uploads/2012/05/ls-t.png)


## ls -v


-- **sort by version (this is my favorite, no need for sort -V; this will sort by version syntax**
[![](http://fsk141.com/uploads/2012/05/ls-v.png)](http://fsk141.com/uploads/2012/05/ls-v.png)


## ls -X


-- **sort by extension (alpha order -- great for grouping extensions)**
[![](http://fsk141.com/uploads/2012/05/ls-X.png)](http://fsk141.com/uploads/2012/05/ls-X.png)


# Conclusion:


I hope that something new sprouted into your brain pertaining to ls. It seems that the program was written and rewritten to provide a full featured directory listing utility without the need to break out any stream editors like sed or awk. In fact a lot of the simple flags that encompass what is ls I would emulate with piping commands together just because I didn't think that ls had useful options. Well now I know the power of ls, and learned that I should check out the flags pertaining to even the simplest of programs to find gems of programming prowess.
