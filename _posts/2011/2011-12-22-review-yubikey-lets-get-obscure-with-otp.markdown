---
date: '2011-12-22 17:43:02'
layout: post
slug: review-yubikey-lets-get-obscure-with-otp
status: publish
title: '[Review] Yubikey - Let''s get obscure with otp'
wordpress_id: '1030'
categories:
- Linux
- Reviews
tags:
- 2 phase auth
- one time password
- otp
- wordpress yubikey
- yubico
- yubikey
description: "I was happy to receive my [Yubikey](https://www.yubico.com/personal-use) by [Yubico](https://www.yubico.com) the other day; and have been happily including it into my computer life. For those of you that aren't familiar with Yubico, or OTP (one time password) devices; it's basically a stateless, timeless keyfob that generates a text string based off an AES encrypted key. To verify your identity, all you need to do is authenticate with the Yubico server (they have the private AES key that will verify your public key) You can use your yubikey for a multitude of two phase authentication choices, from logging into your wordpress to your ssh servers."
---

[![](http://fsk141.com/uploads/2011/12/package_inside-750x617.jpg)](http://fsk141.com/uploads/2011/12/package_inside.jpg)

I was happy to receive my [Yubikey](https://www.yubico.com/personal-use) by [Yubico](https://www.yubico.com) the other day; and have been happily including it into my computer life. For those of you that aren't familiar with Yubico, or OTP (one time password) devices; it's basically a stateless, timeless keyfob that generates a text string based off an AES encrypted key. To verify your identity, all you need to do is authenticate with the Yubico server (they have the private AES key that will verify your public key) You can use your yubikey for a multitude of two phase authentication choices, from logging into your wordpress to your ssh servers.

**Before we dive into the uses, let's gawk at the beautiful simplicity of the device, and it's accompanying software.**

The key comes packaged in a neat little plastic sleeve:

[![](http://fsk141.com/uploads/2011/12/package_front-750x827.jpg)](http://fsk141.com/uploads/2011/12/package_front.jpg)

There are two flavors of yubikey, black &amp; white. I choose the beautiful black. It's about as large as two quarters put next to one another.

[![](http://fsk141.com/uploads/2011/12/key_size-750x636.jpg)](http://fsk141.com/uploads/2011/12/key_size.jpg)

The top (shown above) has an activator pad; in which you press to receive your otp; or if you hold for longer than 2 seconds; you'll receive a text string of your choosing (YKPersonalization software required)

The bottom of the device has a simple identifier (Powered by Yubico), and small qrcode near the inserting end of the key.

[![](http://fsk141.com/uploads/2011/12/key_front-750x591.jpg)](http://fsk141.com/uploads/2011/12/key_front.jpg)

Upon plugging in the device I loved how it matched my laptop.

[![](http://fsk141.com/uploads/2011/12/key_laptop-750x580.jpg)](http://fsk141.com/uploads/2011/12/key_laptop.jpg)

One thing that I thought of soon after plugging it in is how wonderful it would be if it were just a tiny nub sticking out of the usb with the contact surface almost flush with the usb port (I can dream :) But the usb flash drive form factor is nice, recognizable, and very thin. Overall the design is very neat. The device is sturdy, yet very thin and light at the same time.

The companion software for the Yubikey is FANTASTIC. For arch linux I was wonderfully excited to simply run `yaourt -S yubikey-personalization-gui` &amp; executing sudo YKPersonalization

[![](http://fsk141.com/uploads/2011/12/yubikey_software-750x593.png)](http://fsk141.com/uploads/2011/12/yubikey_software.png)

This wonderful gui is simple to use, and provides an enormous functionality to your Yubikey.

Out of the box, the Yubikey is ready to go. There are two configurable slots available, and slot 1 is configured to use the Yubico OTP. Once plugged into the software you can reprogram slot 2 to have a static password; secret message, or anything you so desire. You can reprogram slot 1 if you would like, but it will prohibit use with Yubico OTP, until reconfigured &amp; reauthenticated with the Yubico servers.

The first thing I tried once getting my Yubikey was to navigate to [http://demo.yubico.com/php-yubico/one_factor.php](http://demo.yubico.com/php-yubico/one_factor.php) and test out my device. Once inserted into your computer (Linux, Mac, Windows) the key is automatically recognized as a simple keyboard, and works as if you were just typing. You can test out otp, password/otp, otp/username/password on the demo server. It's a wonderful place to start playing with your device.

After spending about 5 minutes playing around with the demo server, installing the software, and adding funny messages to my slot 2 of the device, and hitting the keypad a bunch of times to see a bunch of seemingly random character sets fly by; I decided to dive into actually making the Yubikey work with my day-to-day life. I started by adding &amp; configuring this wordpress site to use otp. It's a simple 3 step process.



	
  1. Sign up for an api key ([https://upgrade.yubico.com/getapikey](https://upgrade.yubico.com/getapikey))

	
  2. Download &amp; install the wordpress plugin ([https://wordpress.org/extend/plugins/yubikey-plugin](https://wordpress.org/extend/plugins/yubikey-plugin))

	
  3. Wait about 5 minutes (for api key to propagate), and login with your newly added otp field on your wordpress login page.


Other than just add otp to wordpress I read up on using it with gmail (not quite sure how to do it with linux just yet, but I plan to write a script that will allow me to use my Yubikey with 2 phase auth for gmail/google apps. I have a lot more to do with my yubikey, and love having a 2'nd phase of authentication to add some needed complexity to my life. I feel more secure, and enjoy the metallic tactile interface to access my websites &amp; servers. I have a lot more that I need to write about for this wonderful device, just think of this as a teaser review.

**Some great reads about the Yubikey &amp; implementations:**

[https://www.yubico.com/documentation](https://www.yubico.com/documentation)

[https://www.yubico.com/openid-server](https://www.yubico.com/openid-server)

[https://wiki.archlinux.org/index.php/Yubikey](https://wiki.archlinux.org/index.php/Yubikey)
