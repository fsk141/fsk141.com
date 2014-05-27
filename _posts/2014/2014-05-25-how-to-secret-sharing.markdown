---
date: '2014-05-25 16:34:00'
layout: post
slug: how-to-secret-sharing
title: "[Tutorial] Secret Sharing"
btcaddy: "1Hzx83FJcbA3r5T5zPad8SR7ggxsw9KKQX"
categories:
- Tutorials
tags:
- ssss
- Secret Sharing
- Shamir's Secret Sharing Scheme
- PassGuardian.com
- Bitcoin

description: "Secret sharing, or the ultimate way to distribute your secrets. It involves taking a secret message, splitting it into pieces, and repiece back together to output the original secret. Be it to people, randomly stuck to the underside of chairs in your favorite coffee joint, or plastered across your city. The possibilities are endless, and left up to your most creative distribution schemes. Secret sharing is becoming more relevant with the necessity to store bitcoin private keys. Along with that you can store any short (128 char) message (link to a website, gpg blob, passwords)"
---

{% thumbnail images/2014/05-25-how-to-secret-sharing/ssss.png 600x600> %}

# Introduction
Secret sharing, or the ultimate way to distribute your secrets. It involves taking a secret message, splitting it into pieces, and repiece back together to output the original secret. Be it to people, randomly stuck to the underside of chairs in your favorite coffee joint, or plastered across your city. The possibilities are endless, and left up to your most creative distribution schemes. Secret sharing is becoming more relevant with the necessity to store bitcoin private keys. Along with that you can store any short (128 char) message (link to a website, gpg blob, passwords)

# What is Secret Sharing
Secret sharing aka secret splitting is a method of splitting up a message into multiple different pieces; of which the pieces can be reassembled to rejoin the message for reading. 

Essentially turning something like this:

	1-1bcc9f7314438081494f129bbebc4727452e48d448053e5369c7e93dd0b872c2bfabb7797464ef37b1853d6f558573f28b3051
	4-17aba270ab53e6c33a048e12174ec3b09961fed5b0e077084da49d8875d219bb870fb1e484505eb66f4a146270763beeffa690

<br />

Into this:

	5JVZAHBEQjue5d1riTq7UswE9ya52tGccEZBy99WRjvYjmd8CF8 ## bitcoin private key I'm using as an example

# Install a Helper Program
If you're on linux there is a simply wonderful program called [ssss](http://point-at-infinity.org/ssss/), which does all the heavy lifting for you.

	yaourt -S ssss #on arch
	apt-get install ssss #debian/ubuntu


# Utilize ssss-split & ssss-combine to Make Magic Happen
Once you have ssss installed you will be given two helper programs (ssss-combine & ssss-split) that allow you to encrypt / decrypt your text blob.

## ssss-split
ssss-split allows you to take a message (128 ASCII chars, or a hex blob), and split it into any number of shares, and specify how many shares are required to put the secret back together.

{% highlight bash %}
[fsk141 ~]$ ssss-split 
Split secrets using Shamirs Secret Sharing Scheme.

ssss-split -t threshold -n shares [-w token] [-s level] [-x] [-q] [-Q] [-D] [-v]

OPTIONS
       -t threshold
              Specify the number of shares necessary to reconstruct the secret.

       -n shares
              Specify the number of shares to be generated.

       -w token
              Text token to name shares in order to avoid confusion in case one utilizes secret sharing to protect several independent secrets. The generated  shares
              are prefixed by these tokens.

       -s level
              Enforce  the  schemes  security  level (in bits). This option implies an upper bound for the length of the shared secret (shorter secrets are padded).
              Only multiples of 8 in the range from 8 to 1024 are allowed. If this option is ommitted (or the value given is 0) the security level is chosen automat‐
              ically depending on the secret's length. The security level directly determines the length of the shares.

       -x     Hex mode: use hexadecimal digits in place of ASCII characters for I/O. This is useful if one wants to protect binary data, like block cipher keys.

       -q     Quiet mode: disable all unnecessary output. Useful in scripts.

       -Q     Extra quiet mode: like -q, but also suppress warnings.

       -D     Disable the diffusion layer added in version 0.2. This option is needed when shares are combined that where generated with ssss version 0.1.

       -v     Print version information.

{% endhighlight %}

Let's start by creating a message, splitting it up, and going over some of the options. 

I have a public/private key combo, of which I would like to backup, secure, and split up the private key:

	public:private
	1ssssjnGSxpByts9VbgE6yppLG2Z8J1nE:5JVZAHBEQjue5d1riTq7UswE9ya52tGccEZBy99WRjvYjmd8CF8

There are a couple ways you can go about encrypting your message, pipe content to ssss-split or interactively.

Piping output:

{% highlight bash %}
[fsk141 ~]$ echo "5JVZAHBEQjue5d1riTq7UswE9ya52tGccEZBy99WRjvYjmd8CF8" | ssss-split -n4 -t2
Generating shares using a (2,4) scheme with dynamic security level.
Enter the secret, at most 128 ASCII characters: Using a 408 bit security level.
1-20e0f2b33ae876f79e7fec03e60a2a9afe66c7715fa4cd61562137ffef0d1a49adfb9362cbd15868423843c28edbeb293760c3
2-69b6aff223e44e52c9e8642c68811fd1787a3a6130e5e0fe0a2b78d5330b7afc736802c55b1c1108e345273000697a4e201cc0
3-ae849b32d4e059ce049ae3c912f80ce8058e6e9115dafb8b3e2d423378f6a56f391972582b58d6288391fb6185f8f56cd2c8d8
4-fb1a157011fc3f1866c77473759775467443c041ee67bbc0b23fe6808b07bb97ce4f218a7a8683c9a1bfeed51d0c58800ee4d8
{% endhighlight %}

Or just start ssss-split & enter the information you desire at the prompt (typed data will not be echo'd to prompt):

{% highlight bash %}
[fsk141 ~]$ ssss-split -n4 -t2
Generating shares using a (2,4) scheme with dynamic security level.
Enter the secret, at most 128 ASCII characters: Using a 408 bit security level.
1-1bcc9f7314438081494f129bbebc4727452e48d448053e5369c7e93dd0b872c2bfabb7797464ef37b1853d6f558573f28b3051
2-1fee74727eb3a2bf6789991cd9edc4aa0eeb252b1fa6069a75e6c5514c61abea57c84af224777fb7043fda6bb6d44bf958bde4
3-e3f02d72a71c43557dcbe061fb22ba2ec857fe7e2d38eedd7e06217538291cf20fe91e74eb860fc897567897e81b5c0016396e
4-17aba270ab53e6c33a048e12174ec3b09961fed5b0e077084da49d8875d219bb870fb1e484505eb66f4a146270763beeffa690
{% endhighlight %}

If you notice, both sets have different shares, but include the same blob of text (5JVZAHBEQjue5d1riTq7UswE9ya52tGccEZBy99WRjvYjmd8CF8). This is due to the use of random entropy based on /dev/urandom.

I specified a simple example where (-n4) is the number of shares to create, and (-t2) is the amount of shares required to retrieve the secret. I could spread out these 4 secrets, and if any two are put together with ssss-combine, I will be able to view my secret text.

## ssss-combine
Now that we have a bunch of pieces, let's put some together and get our encrypted text.

{% highlight bash %}
[fsk141 ~]$ ssss-combine 
Combine shares using Shamir's Secret Sharing Scheme.

ssss-combine -t threshold [-x] [-q] [-Q] [-D] [-v]

OPTIONS
       -t threshold
              Specify the number of shares necessary to reconstruct the secret.

       -x     Hex mode: use hexadecimal digits in place of ASCII characters for I/O. This is useful if one wants to protect binary data, like block cipher keys.

       -q     Quiet mode: disable all unnecessary output. Useful in scripts.

       -Q     Extra quiet mode: like -q, but also suppress warnings.

       -D     Disable the diffusion layer added in version 0.2. This option is needed when shares are combined that where generated with ssss version 0.1.

       -v     Print version information.
{% endhighlight %}

Just like before you can pipe data, or input interactively... I put 2 of the 4 secrets into a text file, and sent them off to ssss-combine to get back my original private bitcoin key. :) Be mindful that you have to match your splitting and combining threshold to properly decrypt (otherwise you'll just get breakage)

{% highlight bash %}
[fsk141 ~]$ cat secrets                   
1-1bcc9f7314438081494f129bbebc4727452e48d448053e5369c7e93dd0b872c2bfabb7797464ef37b1853d6f558573f28b3051
4-17aba270ab53e6c33a048e12174ec3b09961fed5b0e077084da49d8875d219bb870fb1e484505eb66f4a146270763beeffa690

[fsk141 ~]$ cat secrets | ssss-combine -t2
Enter 2 shares separated by newlines:
Resulting secret: 5JVZAHBEQjue5d1riTq7UswE9ya52tGccEZBy99WRjvYjmd8CF8
Share [1/2]: Share [2/2]: 
{% endhighlight %}

# Conclusion

Congratulations you now know how to setup complicate sharing schemes, and protect your data that matters most. Go secure your private keys, throw a secret under your matress, a couple in some books, under a chair in your favorite public place, geocache one, hehe. Make sure to let me know your most creative places to hide your pieces in the comments.

<br />

**References:**

1) [http://en.wikipedia.org/wiki/Secret_sharing](http://en.wikipedia.org/wiki/Secret_sharing)

2) [http://point-at-infinity.org/ssss](http://point-at-infinity.org/ssss/)

3) [http://passguardian.com](http://passguardian.com/) -- browser interface for ssss (I would recommend downloading & running locally (from [here](https://github.com/amper5and/secrets.js/tree/gh-pages))
