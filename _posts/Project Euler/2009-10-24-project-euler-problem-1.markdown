---
date: '2009-10-24 18:15:57'
layout: post
slug: project-euler-problem-1
status: publish
title: Project Euler Problem 1
wordpress_id: '148'
categories:
- Programming
tags:
- c++
- cpp
- gnu octave
- project euler
---

Just found a wonderful place to help me grow in programming, and learn some sweet math problems at the same time. That place is called Project Euler ([http://www.projecteuler.net](http://www.projecteuler.net)) named after [Leonhard Paul Euler](http://en.wikipedia.org/wiki/Leonhard_Euler). There is more information on the [about](http://projecteuler.net/index.php?section=about) page of the website.

After much struggle with GNU Octave I got the problem working, and also wrote a c++ version. I tried for about 2 hours to get the octave version working. mod() != % for modulo :(. This tripped me up, but everything works, and the solution program is below.

C++ Solution:

{% highlight c++ %}
// Project Euler - P1

#include <iostream>

using namespace std;

int main () {

int i=0;
int t=0;

for ( i=0; i < 1000; i++ ) {
	if ( i % 3 == 0 || i % 5 == 0 )
		t += i;
}

cout << t;

}

// 233168
{% endhighlight %}

Octave Solution:

{% highlight bash %}
# Project Euler - P1

i=0;
t=0;

for i=1:999
	if ( mod(i, 3) == 0 || mod(i, 5) == 0 )
		t += i
	endif
endfor

#233168
{% endhighlight %}

Octave is an awesome language to program in for math specific operations. It will take a little getting used to, but I believe it's fully compatible with matlab, so it will be a nice addition to my repertoire.
