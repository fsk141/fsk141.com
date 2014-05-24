---
date: '2014-01-17 14:05:00'
layout: post
slug: gsl-fix-archlinux
status: publish
title: "GSL Fix Arch Linux (Downgrade to 1.14)"
categories:
- Linux
tags:
- gsl
- compile
- jekyll
description: "Been using Jekyll for a while now on multiple blogs. Haven't updated fsk141.com in quite some time &amp; went to build the site (jekyll serve --lsi). I was greeted with the familiar 'build rb-gsl for 10x faster lsi generation'. Went to install gsl (gem install gsl), and got a big fat failed compilation. After trying a few patches for the gsl gem, compiling gsl manually, and a few other hacky tricks, I found it was as simple as downgrading gsl to 1.14 :)"
---

{% thumbnail images/2014/01-17-gsl-fix-archlinux/gem_gsl.png 600x600> %}

Been using Jekyll for a while now on multiple blogs. Haven't updated fsk141.com in quite some time &amp; went to build the site (jekyll serve --lsi). I was greeted with the familiar 'build rb-gsl for 10x faster lsi generation'. Went to install gsl (gem install gsl), and got a big fat failed compilation. After trying a few patches for the gsl gem, compiling gsl manually, and a few other hacky tricks, I found it was as simple as downgrading gsl to 1.14 :)

I simply downloaded the latest PKGBUILD and accompying gsl.install for gsl from archlinux.org &amp; modified it for 1.14 (changed version &amp; shasum)

To install gsl 1.14, make a directory (mkdir gsl1.14 && copy the following files below into said directory)

Finish installing your newly compiled gsl by running a

	makepkg && sudo pacman -U gsl-1.14-1-*.pkg.tar.xz

Follow the gsl downgrade with the ruby gem installation of rb-gsl with

	gem install gsl

&amp; Enjoy!

## PKGBUILD
{% highlight bash %}
# $Id$
# Maintainer: Ronald van Haren <ronald.archlinux.org>
# Contributor: Juergen Hoetzel <juergen.archlinux.org>
# Contributor: Tom Newsom <Jeepster@gmx.co.uk>

pkgname=gsl
pkgver=1.14
pkgrel=1
pkgdesc="The GNU Scientific Library (GSL) is a modern numerical library for C and C++ programmers"
url="http://www.gnu.org/software/gsl/gsl.html"
source=("http://ftp.gnu.org/gnu/gsl/$pkgname-$pkgver.tar.gz")
install=gsl.install
license=('GPL')
arch=('i686' 'x86_64')
depends=('glibc' 'bash')
sha1sums=('e1a600e4fe359692e6f0e28b7e12a96681efbe52')


build() {
    unset LDFLAGS

    cd "${srcdir}/${pkgname}-${pkgver}"
    ./configure --prefix=/usr
    make
}

check() {
    cd "${srcdir}/${pkgname}-${pkgver}"
    make check
}

package() {
    cd "${srcdir}/${pkgname}-${pkgver}"
    make DESTDIR="${pkgdir}" install
}
{% endhighlight %}

## gsl.install
{% highlight bash %}
infodir=/usr/share/info
  filelist=(gsl-ref.info.gz)

  post_install() {
  for file in ${filelist[@]}; do
    install-info $infodir/$file $infodir/dir 2> /dev/null
  done

  }

  post_upgrade() {
    post_install $1
  }

  pre_remove() {
  for file in ${filelist[@]}; do
    install-info --delete $infodir/$file $infodir/dir 2> /dev/null
  done

  }
{% endhighlight %}
