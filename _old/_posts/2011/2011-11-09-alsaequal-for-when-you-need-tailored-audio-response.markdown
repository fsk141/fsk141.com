---
date: '2011-11-09 18:44:12'
layout: post
slug: alsaequal-for-when-you-need-tailored-audio-response
status: publish
title: Alsaequal for When you Need Tailored Audio Response
wordpress_id: '931'
categories:
- Linux
- Software
tags:
- alsa
- alsaequal
- alsamixer
- audio
- equalizer
- linux equalizer
- mixer
- mpd
- sound
---

[![](http://fsk141.com/uploads/2011/11/alsaequal.png)](http://fsk141.com/uploads/2011/11/alsaequal.png)

Music producers have a tremendously hard process of making an album; then optimizing it to sound good on other peoples sound equipment. Whether it be a 10k setup, or a dinky pair of earbuds you picked up for pennies at a swap meet. The problem lies in the endless possibilities of end user. When you have a precision setup that has been configured, and adjusted properly, you should be happily listening to just about anything you pump out of the system. Yet when you have a lesser setup; you need to compensate for losses (whether it be low end, mid, or highs)

I'm an avid collector of FLAC songs; and take a very anal approach to the music that touches my ears. I would rather take up more space on my hard drive, then lose quality. I also use decent earbuds, that are attached to a high definition output. Still, even with a good input; I get degradation along the line. I listen to a large spectrum of music, and while some music sounds great, some doesn't. I might be enjoying some classical, then some melodic metal jumps in, and sounds like it was recorded in a carpet lined room that absorbed all the healthy mids... I was unhappily content with no easy solution being found for alsa, until recently!

My current settings are pictured above, and are the "perfect equalizer" settings for my dynamically changing musical tastes.

If you would like to also enjoy alsaequal, please follow the following steps:



	
  1. **Install alasequal (yaourt -S alsaequal)**

	
  2. **Create .asoundrc with the following (this will allow multiple audio output \[eg mpd &amp; firefox] via dmix)**
{% highlight bash %}
ctl.equal {
        type equal;
}

pcm.plugequal {
        type equal;
        # Modify the line below if you don't
        # want to use sound card 0.
        slave.pcm "plug:dmix";
}

###

# Comment out the 'default' version for 'optional' equal, visa versa

#pcm.equal {
pcm.!default {

###
        type plug;
        slave.pcm plugequal;
}
{% endhighlight %}

	
  3. **Configure mpd (if you use it) -- this is my sound output device in /etc/mpd.conf:**
{% highlight bash %}
audio_output {
	type		"alsa"
	name		"My ALSA EQ"
	device		"plug:plugequal"
	mixer_type      "software"	# optional
	mixer_device	"default"	# optional
	mixer_control	"PCM"		# optional
	mixer_index	"0"		# optional
	use_mmap	"yes"
}
{% endhighlight %}

	
  4. **Set equalizations:**
{% highlight bash %}
alsamixer -D equal
# or
alsamixergui -D equal
{% endhighlight %}

	
  5. **Enjoy alsa &amp; all the sounds coming out of your computer!**


**Resources:**



	
  * [http://doc.slitaz.org/en:guides:alsaequal](http://doc.slitaz.org/en:guides:alsaequal)



	
  * [http://www.thedigitalmachine.net/alsaequal.html](http://www.thedigitalmachine.net/alsaequal.html)


