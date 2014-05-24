---
date: '2010-10-09 12:33:30'
layout: post
slug: tutorial-how-to-unencrypt-kindle-books
status: publish
title: '[Tutorial] How to unencrypt kindle books'
wordpress_id: '451'
categories:
- Kindle
- Tutorials
tags:
- drm
- kindle
- remove drm
- unencrypt
---

[![IMAG0082](http://farm5.static.flickr.com/4103/5065126211_d5366759d7.jpg)](http://www.flickr.com/photos/68444690@N00/5065126211/)

So you want a brand new kindle book & you can't find it on your favorite torrent/rapidshare/ftp? ;) Well that's too bad, but there is a giantious amazon store that sell books... But I have two kindles, and when I buy a book, I want to be able to share it without restrictions between my kindles. The [old version](http://nyquil.org/archives/1128-Converting-Kindle-Books-a-painful-process-that-works-for-reading-Kindle-books-without-a-Kindle.html) of the unencrypt hack was a little complicated and required you to find your PID, enter it in a program, and then a program would spit out a decrypted book for you. Well the new Kindle4Pc method is fantastic. It's easy, and gives you what you're looking for with the least effort necessary.

First off download the file that I packaged up with all the pieces that you need:

[unencrypt.zip](http://dl.dropbox.com/u/52078/unencrypt.zip)
Included in the zip file is:



	
  1. Python 2.6.6 (version that will work)

	
  2. KindleForPC-installer(beta) \[you NEED this version of kindle4PC for this hack to work]

	
  3. unswindle.pyw (initiator for the whole process)

	
  4. mobidedrm.py (magic file)


The process is painfully simple:

	
  1. Install python

	
  2. Install Kindle4PC if you don't have it installed & download your book(s) to be undrm'd

	
  3. Uninstall Kindle4PC

	
  4. DISCONNECT FROM THE INTERNET (if you don't then when you install Kindle4PC(beta) it will upgrade & you DONT want the newest version)

	
  5. Install Kindle4PC(beta) from the zip file

	
  6. Execute unswindle.pyw (either by double clicking or dragging to cmd prompt)

	
  7. This will startup Kindle4PC & all you need to do now is open the book that you would like to decrypt

	
  8. Now exit Kindle4PC and wait for the magic to happen

	
  9. Magic will show up in the form of a "save as" window. Just select where you would like to save the decrypted .mobi && enjoy.

	
  10. To finish everything up just drop it on your kindle with all your other books, and happy reading


