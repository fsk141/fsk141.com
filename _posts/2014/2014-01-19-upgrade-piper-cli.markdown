---
date: '2014-01-18 19:05:00'
layout: post
slug: upgrade-piper-cli
status: publish
title: "Update Your Piper via CLI"
categories:
- Linux
tags:
- Piper
- Bitcoin
- Hardware Wallet
description: "The piper is a low cost, all in one, beautiful piece of equipment. It provides a cost effective solution for secure offline wallet generation. Comprised of a raspberry pi, thermal printer, and a single button for operation. It's incredibly easy to operate, and functions entirely independently (just requires power). It also can even be plugged into a monitor, keyboard, &amp; mouse to provide a graphical configuration interface / desktop environment. The other day I received an [email](http://us7.campaign-archive1.com/?u=5600dcb59fc896fa75b7b3d6c&id=f23b4f9732&e=b6d25600af) stating that a new update was available. While I was excited for some of the changes (such as continuous printing), I was too lazy to plugin a monitor, keyboard, and mouse to initiate the upgrade. Instead I plugged it into my network, spent a minute looking at the upgrade script, and ran it from the command line."
---

{% thumbnail images/2014/01-19-upgrade-piper-cli/piper-upgraded.jpg 600x600> %}

# Introduction
The piper is a low cost, all in one, beautiful piece of equipment. It provides a cost effective solution for secure offline wallet generation. Comprised of a raspberry pi, thermal printer, and a single button for operation. It's incredibly easy to operate, and functions entirely independently (just requires power). It also can even be plugged into a monitor, keyboard, &amp; mouse to provide a graphical configuration interface / desktop environment. The other day I received an [email](http://us7.campaign-archive1.com/?u=5600dcb59fc896fa75b7b3d6c&id=f23b4f9732&e=b6d25600af) stating that a new update was available. While I was excited for some of the changes (such as continuous printing), I was too lazy to plugin a monitor, keyboard, and mouse to initiate the upgrade. Instead I plugged it into my network, spent a minute looking at the upgrade script, and ran it from the command line.

# Download &amp; Investigation
I started out by downloading the update ( [https://piperwallet.com/updates/update2.zip](https://piperwallet.com/updates/update2.zip) ) to my local machine and checked out the contents. Upon unzip'ing, I found it contains an Instructions.rtf & an update.tar file. The Instructions.rtf was filled with a detailed description on how to plug in the piper to a monitor, keyboard, and mouse; proceeding to untar the update.tar &amp; run it by "double clicking." Like I mentioned I'm way too lazy to do all of that :), so I plugged it into a free port on my router, plugged it in, and waited for it to showup on my dhcp table.

While it was booting away I studied the contents of update2.zip a little more in depth. I began by untar'ing update.tar and was greeted with the following:

{% highlight bash %}
├── updater
│   ├── copyKeys.py
│   ├── data
│   ├── data1.sh
│   ├── data2.sh
│   ├── dist-packages
│   ├── updater
│   └── updater-src
│       └── updater.c
{% endhighlight %}

I quickly checked out updater-src & was greeted with an incredibly simple C file that runs the following:
{% highlight bash %}
#include <stdio.h>
int main(){
	system("/bin/bash /home/pi/Desktop/updater/data1.sh");
}
{% endhighlight %}

Checked out data1.sh which simply calls data2.sh &amp; sends a couple xmessage notifications to inform the user of the progress. I double checked data2.sh &amp; found out it's all simple command line arguments to backup the old &amp; replace with the new. 

After examining the files at hand and becoming content with the expected results, I checked back on my router to see if anything had popped up.

## Bingo!
{% thumbnail images/2014/01-19-upgrade-piper-cli/dhcp-readout.png 600x600> %}

I tried to ssh with a few default credentials, and failed. Went looking for the default raspberry login (it's what piper is built on top of); and found out the default user:pass combo is pi:raspberry ; tried this &amp; failed. Then tried pi:piper &amp; was greeted with a console prompt.

That was easy! I proceeded to transfer update.tar, run data1.sh, reboot, &amp; enjoy my newly updated piper (ver1.05)

{% highlight bash %}
#(on local machine) -- send update.tar to piper [user: pi ; password: piper] ; replace 10.1.1.115 with your ip
scp update.tar pi@10.1.1.115:
ssh pi@10.1.1.115
mv update.tar ~/Desktop
cd ~/Desktop
tar xf update.tar
cd updater
./data1.sh
#wait till complete :)
sudo reboot
{% endhighlight %}

# Conclusion
After the reboot, you should be greeted with a happy stub of paper spit out the top of the piper. It'll notify you of the successful update, and provide you a version number (like the first image in this post). Enjoy your newly updated piper &amp; keep on printing on :)
