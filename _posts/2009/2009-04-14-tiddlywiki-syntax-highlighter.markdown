---
date: '2009-04-14 10:40:13'
layout: post
slug: tiddlywiki-syntax-highlighter
status: publish
title: Tiddlywiki Syntax Highlighter
wordpress_id: '16'
categories:
- Software
tags:
- tiddlywiki
---

Ohhh! Awww!
![sh](http://syntaxhighlighter.googlecode.com/files/Overview01.png)
The [tiddlywiki](http://www.tiddlywiki.com) implementation of [syntaxhighlighter](http://code.google.com/p/syntaxhighlighter) is crucial part of my note taking experience. I do a lot of note taking pertaining to code, and this makes it nice and pretty. I had some trouble finding the correct link to the plugin. The link on [tiddlywiki.org/wiki/Syntax_Highlighting](http://tiddlywiki.org/wiki/Syntax_Highlighting) was broken, and since it was in japanese, I had difficulty searching the site for the section I needed. Anywho I stumbled across the [correct link](http://www.coolcode.cn/?action=show&id=310) and fixed it in the tiddlywiki post on syntax highlighting. Well then why am I making this post? Well first off to give a little better install instruction, and a link to the plugin for easy of use ++ link back to original site.

How to install
----------------

**Simple Way:**

Navigate to your TiddlyWiki & click the backstage link at the upper right, followed by the import button. Paste the URL of my Notes into the URL box: "http://dl.getdropbox.com/u/52078/Notes/Notes.html" followed by the open button.

[![](http://farm4.static.flickr.com/3413/3441519793_7561c8646e.jpg)](http://www.flickr.com/photos/68444690@N00/3441519793)

You will be presented with a long list of tiddlers. You can proceed to scroll down to the bottom and check 'StyleSheet', 'StyleSheetSyntaxHighlighter' and 'SyntaxHighlighterPlugin' followed by the import button.

[![](http://farm4.static.flickr.com/3364/3441519823_eeb002df61.jpg)](http://www.flickr.com/photos/68444690@N00/3441519823)

That should be it, enjoy.

**Manual Way:**

Download 'syntaxhighlighterplugin.zip' from:

[http://www.coolcode.cn/?action=show&id=310](http://www.coolcode.cn/?action=show&id=310) (look for the link near the top of the page)

or my mirror:

[http://dl.getdropbox.com/u/52078/syntaxhighlighterplugin.zip](http://dl.getdropbox.com/u/52078/syntaxhighlighterplugin.zip)

and extract...

Click the New Tiddler link to the left, and Name the tiddler 'StyleSheetSyntaxHighlighter' Navigate to your unzipped archive and cut and paste the contents of 'SyntaxHighlighter.css' into your new tiddler.

Navigate to the Shadow tiddler 'StyleSheet' and add \[\[StyleSheetSyntaxHighlighter]]

[![](http://farm4.static.flickr.com/3646/3441569431_e09d4b4100.jpg)](http://www.flickr.com/photos/68444690@N00/3441569431)

Add another 'New Tiddler' named 'SyntaxHighlighterPlugin' and paste the contents of 'shPlugin.js' into this tiddler, then tag the tiddler with 'systemConfig'

Finish everything up by copying 'clipboard.swf' to the directory of your TiddlyWiki...

Enjoy ;)
