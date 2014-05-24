---
date: '2008-12-01 00:00:00'
layout: post
slug: guile-history
status: publish
title: Guile History
wordpress_id: '50'
categories:
- Programming
tags:
- lisp
---

I got to liking guile right off the bat as a lisp working environment. Yet it bugged me that you couldn't use the up and down arrows to see your previous entries. Well here is the fix:

    
    (use-modules (ice-9 readline)) (activate-readline)


If you would like it to automagically be applied on startup then edit ~/.guile:
Add the following line:

    
    (use-modules (ice-9 readline)) (activate-readline)
