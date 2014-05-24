---
date: '2010-09-06 17:07:13'
layout: post
slug: chapod-calvin-hobbes-apod
status: publish
title: Chapod (Calvin &amp; Hobbes Apod)
wordpress_id: '308'
categories:
- Homebrew
- Programming
---

[![](http://fsk141.com/uploads/2010/09/chapod-800x500.jpg)](http://fsk141.com/uploads/2010/09/chapod.jpg)

**Idea:** [http://www.reddit.com/r/pics/comments/d9zfg/i_used_to_change_my_desktop_background_every_day](http://www.reddit.com/r/pics/comments/d9zfg/i_used_to_change_my_desktop_background_every_day)

**APOD:** [http://apod.nasa.gov/apod/](http://apod.nasa.gov/apod/)

**Implementation:** [https://github.com/fsk141/scripts/blob/master/chapod](https://github.com/fsk141/scripts/blob/master/chapod)

**Download:** [https://raw.github.com/fsk141/scripts/master/chapod](https://raw.github.com/fsk141/scripts/master/chapod) (wget, or right click & save as)

I made this a while ago, and thought some people would like to know that it's working awesome.

**Setup:**

Change the RESOLUTION variable to fit your screen size

**Usage: (Works on Mac & Linux)**

{% highlight bash %}

chapod ## without any options will download everything & set your desktop bg

chapod  ## will set to any apod you want!

eg: chapod http://apod.nasa.gov/apod/ap110628.html

{% endhighlight %}

don't forget to chmod +x if thing's aren't working out well.

If you would like it to auto-run every day:

{% highlight bash %} crontab -e # and add the following:
 5 12 * * *	$HOME/.scripts/scripts/chapod  {% endhighlight %}

**Requirements:**

- Imagemagick

- Mac/Linux

- Curl

- feh (if on linux)

**Program below: **

{% highlight bash %}
#!/bin/bash

#################################################
# Calvin & Hobbes APOD #
# Version 1.2 (stupid convert) #
#################################################

#///////////////////////////////////////////////#
# Author: Jonny Gerold <jonny@fsk141.com> #
#///////////////////////////////////////////////#

#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 2 of the License, or
# (at your option) any later version.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program. If not, see <http://www.gnu.org/licenses/>.
#

# Storage Directory
SDIR="$HOME/.chapod"

# Screen Resolution
RESOLUTION="1280x800"

# Raw Calvin & Hobbes foreground
RAWCH="http://dl.dropbox.com/u/52078/chblank.png"

# Base URL for apod
BURL="http://apod.nasa.gov/apod/"

# OS Check
OS=`uname -s`

# Where is convert
if [[ "$OS" == "Darwin" ]]; then
if [[ -x /usr/local/bin/brew ]]; then
IM="/usr/local/bin/convert"
## prolly should put in an else for macports
fi
else
IM="/usr/bin/convert"
fi

# If a url is specified use that, otherwise use apod
if [[ -n "$1" ]]; then

# Entered URL
EURL="$1"
IMAGEPATH=`curl $EURL 2>/dev/null | grep 'href="image' | cut -d\" -f2`
APOD="$BURL$IMAGEPATH"
else
IMAGEPATH=`curl $BURL 2>/dev/null | grep 'href="image' | cut -d\" -f2`
APOD="$BURL$IMAGEPATH"
fi

# Make SDIR
if [[ ! -d $SDIR ]]; then
mkdir $SDIR
fi

# Get the raw Calvin & Hobbes image
if [[ ! -e $SDIR/chblank.png ]]; then
curl $RAWCH -o $SDIR/chblank.png 2>/dev/null
fi

# Get the apod
if [[ -e $SDIR/apod.jpg ]]; then
rm $SDIR/apod.jpg
fi
curl $APOD -o $SDIR/apod.jpg 2>/dev/null

# Resize raw C&H image & apod
for image in apod.jpg chblank.png
do $IM $SDIR/$image -background transparent -resize $RESOLUTION^ -gravity south -extent $RESOLUTION $SDIR/tmp.$image
done

# Lets join them together
$IM $SDIR/{tmp.apod.jpg,tmp.chblank.png} -mosaic $SDIR/chapod.jpg

rm $SDIR/tmp.*

# Now lets set the background
if [[ $OS == "Darwin" ]]; then
osascript -e "tell application \"System Events\" to set picture of every desktop to \"$SDIR/$CHA\""
else
feh --bg-scale $SDIR/chapod.jpg &
fi
{% endhighlight %}
