---
date: '2009-10-15 02:23:01'
layout: post
slug: flickr-in-linux
status: publish
title: Flickr with Linux
wordpress_id: '134'
categories:
- Linux
tags:
- best flickr apps linux
- flickr
- flickr applications
- flickr linux
- Linux
- linux flickr
---

![flickr](http://l.yimg.com/g/images/en-us/flickr-yahoo-logo.png)

Flickr is the best. All it takes is $24 a year, and you can back up all the images you want, and you get a nifty web interface too. The wonderful thing about flickr is that they provide an API so that a bunch of talented programmers can make interfaces that work well with flickr. Many people have taken advantage of the API, and have made some excellent programs. The problem being is that it's difficult to find great programs. Ones that aren't glitch prone, out-dated, or just plain useless. I've gone on a gigantus search for the creme-de-le-creme, and here are my results.

Lets start out with the _**Flickr Web Uploader**_:

Access the uploader here: [http://www.flickr.com/photos/upload](http://www.flickr.com/photos/upload)

To upload photos it's a simple 2 - 3 step process depending if you would like to add a description/tags to your uploaded photos.

**Step 1 (Select photos): **

My image capture program kinda crapped out, but you get the idea. Click the large black box, and multi-select the files you would like to upload.

[![Flickr_Upload](http://farm3.static.flickr.com/2428/4013735212_911da539f1.jpg)](http://www.flickr.com/photos/68444690@N00/4013735212/)

**Step 2 (Verify files & upload):**

It's pleasant that there is a verification page so you can add/remove files at your leisure.

[![Flickr_Upload_2](http://farm4.static.flickr.com/3496/4012968853_391bdf004d.jpg)](http://www.flickr.com/photos/68444690@N00/4012968853/)

**Step 3 Finished (Add more info by clicking "add a description"):
**

[![Flickr_Upload_3](http://farm3.static.flickr.com/2518/4013736984_3a092c04bb.jpg)](http://www.flickr.com/photos/68444690@N00/4013736984/)

Pros:
*	It's very simple, and works as advertised.
*	Multi-Select is a bonus
*	Has nice looking effects
Cons:
*	Bogs down web browser substantially when uploading
*	Couple minor visual glitches on my system

**Summary:**

The Flickr web uploader features a hassle free approach to uploading your photos, and requires no setup, or install of anything. I use the web uploader all the time, and just hate that it pretty much prevents me from browsing the web when I'm uploading files. The uploaded definitely cripples my web browsing experience, and that's annoying...

------

Lets move onto something more dedicated... [Desktop-Flickr-Organizer](http://code.google.com/p/dfo/) (aka dfo)

If you are an Arch Linux user, then go and grab my [PKGBUILD](http://aur.archlinux.org/packages.php?ID=12069) in the [AUR](http://aur.archlinux.org/) here: [http://aur.archlinux.org/packages.php?ID=12069](http://aur.archlinux.org/packages.php?ID=12069)

If not, then look into your local package repositories for dfo.

After you install go ahead and click Flickr > Connect

[![Flickr-Connect](http://farm3.static.flickr.com/2500/4013766386_df9584f437_o.jpg)](http://www.flickr.com/photos/68444690@N00/4013766386/)

This will go and grab all your photos, and import them into dfo. Once everything is imported, your sets will show up. At this point it's a snap to add new sets, upload photos, and download photos. It's just a matter of clicking the right buttons. And there aren't that many buttons, so it is difficult to screw up.

I would upload some pictures, but the program is uber buggy, and all I can do right now is build my image database. Hopefully I can do other things when that's done.

Pros:
*	Clean UI
*	Speedily Grabs Pictures

Cons:
*	Uber buggy
*	No longer Maintained (or at least it seems that way)


**Summary:**

DFO would be a great app if it wasn't one of the buggier programs in my selection. The developer of [flickrfs](http://http://manishrjain.googlepages.com/flickrfs) built & maintains dfo, and it seems like both projects had a wonderful start, and then they both dropped off the deep end. No longer to be updated, or work without epic bugs.

------

Another wonderful program called _**flickrtouchr**_ written by [Colm MacCárthaigh](http://www.stdlib.net/~colmmacc/) is dedicated to downloading your flickr photos/sets/everything. His version was made to sync with an ipod touch. Yet I found a version that downloads the whole images, and was pretty close to what I needed out of the box. I found it [here](http://hivelogic.com/articles/backing-up-flickr/) on [Dan Benjamin's](http://hivelogic.com/about) site and modified a couple things so that it works better :)

You can download my version via github here: [http://github.com/fsk141/fsk141-flickrtouchr](http://github.com/fsk141/fsk141-flickrtouchr)

    
    [fsk141@Scribbly ~/fsk141-flickrtouchr]$ ls
    README  bak  flickrtouchr.py
    [fsk141@Scribbly ~/fsk141-flickrtouchr]$ ./flickrtouchr.py bak
    ./flickrtouchr.py:27: DeprecationWarning: the md5 module is deprecated; use hashlib instead
    import md5
    Download Mode (1=Collection, 2=Set, 3=All): 3
    Flickr_Upload ... in set ... 2009-Flickr_Linux
    Flickr_Upload_2 ... in set ... 2009-Flickr_Linux
    Flickr_Upload_3 ... in set ... 2009-Flickr_Linux
    Flickr-Connect ... in set ... 2009-Flickr_Linux
    ...


Pros:
*	Simple CLI
*	Fast
*	No frills awesomeness
Cons:
*	Not perfect code, but pretty darn close (could use improvements)


**Summary:**

I love this program for downloading off flickr. It's one of the simplest programs to use. Other than the couple of modification I made, it suits my needs very well for downloading.

I'm still on the lookout for programs that aren't as buggy as dfo, and do what I need. I'm planning on trying flickcurl, frupple, and frogr. I'll write up any interesting features that I find when I find them.

------

On a side note, I think http://www.seoishard.com/seo-tool/flickr+manager+2.8 << is a scam. I believe that it's entering hidden links in my website when I add flickr images. I'm going to look for little bugs in the code. It epically blows, cause I know a lot of people prolly use this plugin because it "works." Well not only does it work, but it's scamming all over the internet. eh
