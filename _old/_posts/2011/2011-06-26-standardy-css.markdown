---
date: '2011-06-26 01:46:57'
layout: post
slug: standardy-css
status: publish
title: Standardy CSS
wordpress_id: '712'
categories:
- Programming
---

In the general spirit of reset.css ([http://meyerweb.com/eric/tools/css/reset](http://meyerweb.com/eric/tools/css/reset)), and to help out with some styling for the [Fsk141 Framework](http://fsk141.com/fsk141-framework) I've pulled together what I call Standardy.css. What it is should help make a standard look across all browsers (well a starting point anyway) For everything that reset.css takes away, this puts it back in a cross browser compliant css method. I just whipped this up last night, and have started testing; everything seems to be alright thus far.
You can view standardy.css below; or download from here: [standardy.css](http://fsk141.com/uploads/2011/06/standardy.css)

{% highlight css %}
/* Standardy CSS (to use in conjunction with reset.css)
 *
 * License: none (public domain)
 *
 * Pulled together from around the internet &amp; optimized by Jonny Gerold &lt;jonny@fsk141.com&gt;
 *
 */

address,blockquote,body,dd,div,dl,dt,fieldset,form,frame,frameset,h1,h2,h3,h4,h5,h6,iframe,noframes,object,ol,p,ul,applet,center,dir,hr,menu,pre { display:block; }
li { display:list-item; }
head { display:none; }
table { display:table; }
tr { display:table-row; }
thead { display:table-header-group; }
tbody { display:table-row-group; }
tfoot { display:table-footer-group; }
col { display:table-column; }
colgroup { display:table-column-group; }
td,th { display:table-cell; }
caption { display:table-caption; text-align:center; }
th { font-weight:bolder; text-align:center; }
body { line-height:1.33; padding:8px; }
h1 { font-size:2em; margin:.67em 0; }
h2 { font-size:1.5em; margin:.83em 0; }
h3 { font-size:1.17em; margin:1em 0; }
h4,p,blockquote,ul,fieldset,form,ol,dl,dir,menu { margin:1.33em 0; }
h5 { font-size:.83em; line-height:1.17em; margin:1.67em 0; }
h6 { font-size:.67em; margin:2.33em 0; }
h1,h2,h3,h4,h5,h6,b,strong { font-weight:bolder; }
blockquote { margin-left:40px; margin-right:40px; }
i,cite,em,var,address { font-style:italic; }
pre,tt,code,kbd,samp { font-family:monospace; }
pre { white-space:pre; }
big { font-size:1.17em; }
small,sub,sup { font-size:.83em; }
sub { vertical-align:sub; }
sup { vertical-align:super; }
s,strike,del { text-decoration:line-through; }
hr { border:1px inset; }
ol,ul,dir,menu,dd { margin-left:40px; }
ol { list-style-type:decimal; }
ol ul,ul ol,ul ul,ol ol { margin-bottom:0; margin-top:0; }
u,ins { text-decoration:underline; }
center { text-align:center; }
br:before { content:"\A"; }
{% endhighlight %}
