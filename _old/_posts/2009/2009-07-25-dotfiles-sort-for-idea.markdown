---
date: '2009-07-25 22:03:17'
layout: post
slug: dotfiles-sort-for-idea
status: publish
title: Dotfiles, sort, for, idea!
wordpress_id: '10'
categories:
- Linux
- Programming
tags:
- dotfiles
- flickr
- for
- snapfish
- sort
---

First, I would like to introduce a new link to the right (the hard links) labeled "Dotfiles" This will include all the dot configuration files for my misc computers.

------

I messed up a little bit with renaming some files and renamed some .avi files to .jpg. To sort the problem out I needed to figure out what the bigger files were to easily determine which were movies, and which were not. An easy way to achieve a size sort is this:

    
    du -h * ## basic size of files in a directory
    ---
    du -h * | sort -n  ## Size Ascending (Smallest to Largest)
    ---
    du -h * | sort -n -r ## Size Descending (Largest to Smallest)
    
    [fsk141@Minifsk ~/2009-Black_Mountain]$ du -h *
    ---
    1.9M	210.jpg
    6.2M	211.avi
    16M	212.avi
    15M	213.avi
    3.4M	214.jpg
    50M	215.avi
    14M	216.avi
    3.4M	217.jpg
    2.2M	218.jpg
    36M	219.avi
    5.3M	220.avi
    
    [fsk141@Minifsk ~/2009-Black_Mountain]$ du -h * | sort -n
    ---
    5.0M	029.jpg
    5.3M	220.avi
    5.7M	209.avi
    6.2M	211.avi
    13M	208.avi
    14M	216.avi
    15M	213.avi
    16M	212.avi
    36M	219.avi
    50M	215.avi


------

I've been using for statements recently, and got some help from: [http://www.freeos.com/guides/lsst/ch03sec06.html](http://www.freeos.com/guides/lsst/ch03sec06.html). I might write a little statement writeup (for, while, if, etc) But in the meantime I would like to share a couple that came in handy with some renames & fixes.

    
    for i in 1 2 3 4
    do mv $i.jpg $.avi
    done
    ---
    for ((  i = 0 ;  i < 20;  i++  ))
    do mv $i.JPG $i.jpg
    done


I love 'for' and 'if' statements, and need to get more into my "programming" mindset, and not waste so much time typing repetitive tasks.

------

IDEA!!!

So I love flickr, and backup all my images to my flickr account. (I need to make more public, but most are private) And I've printed pictures from snapfish in the past, but it's a little pain to upload to flickr and snapfish. So I'm thinking up a way to upload to both at the same time. I believe that both flickr and snapfish allow you to upload .zip files, yet I'm unsure if you can email them .zip archives and they will extract and everything for you? If that's possible then I will probably write up a script that zips up a directory and sends it to the respectable services. I'll post once I get something up.
