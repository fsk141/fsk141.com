---
date: '2009-10-07 03:03:15'
layout: post
slug: teach-mpd-a-lesson-about-stealing-your-cpu-cycles
status: publish
title: Teach mpd a lesson about stealing your CPU cycles
wordpress_id: '110'
categories:
- Linux
---

MPD by default is setup to use whatever it can output to (audio wise), even though such sources might be very inefficient. I was noticing that mpd was using upwards of 30% of my dual core CPU, and it got me to googling for an answer. Well after reading up on mpd configuration tweaks: [http://mpd.wikia.com/wiki/Tuning](http://mpd.wikia.com/wiki/Tuning) I got my configuration file to use alsa, and now I'm seeing 1-3% cpu usage from mpd most the time it's off the radar completely.

I've posted my whole mpd.conf below, yet the information I modified was the 'audio_output bit.


    
    
    # See the mpd.conf man page for a more detailed description of each parameter.
    
    music_directory         "~/music"
    #playlist_directory "/var/lib/mpd/playlists"
    db_file "~/.mpd/mpd.db"
    log_file "~/.mpd/mpd.log"
    pid_file "~/.mpd/mpd.pid"
    state_file "~/.mpd/mpdstate"
    #
    user "fsk141"
    #
    audio_output {
    type            "alsa"
    name            "My ALSA Device"
    #       auto_resampler "no"
    #       format          "44100:16:2"    # optional
    mixer_device    "default"       # optional
    mixer_control   "Master"        # optional
    #       mixer_index     "0"             # optional
    options         "dev=dmixer"
    device          "plug:dmix"
    
    }
    
