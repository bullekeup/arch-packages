# $Id: PKGBUILD 138787 2015-08-26 14:57:27Z arodseth $
# Maintainer: Jelle van der Waa <jelle@vdwaa.nl>
# Contributor: Douglas Soares de Andrade <dsandrade@gmail.com>
# Contributor: William Rea <sillywilly@gmail.com>
# Contributor: Axel Moinet <bullekeup@gmail.com>

pkgname=pygoocanvas
pkgver=0.14.1
pkgrel=8
pkgdesc='GooCanvas Python bindings'
arch=('x86_64' 'i686')
url='https://wiki.gnome.org/Projects/PyGoocanvas'
license=('LGPL')
depends=('python2' 'goocanvas1' 'pygtk')
makedepends=('pkgconfig' 'git' 'gtk-doc')
options=('docs')
source=("git://github.com/GNOME/pygoocanvas.git#tag=${pkgname^^}_${pkgver//./_}"
        "pygoocanvas.patch")
md5sums=('SKIP'
         '6eef440ba652eed920a546bd51fbc141')

export PATH=/home/workspace/tools/pybin:$PATH
export PYTHON=python2

prepare() {
  cd "$pkgname"

  git apply "${srcdir}/pygoocanvas.patch"
  ./autogen.sh CFLAGS='-I /usr/include/python2.7' --prefix=/usr --disable-docs
}

build() {
  cd "$pkgname"

  ./configure CFLAGS='-I /usr/include/python2.7' --prefix=/usr --disable-docs
  make
}

package() {
  make -C "$pkgname" DESTDIR="$pkgdir" install
}

# getver: raw.githubusercontent.com/GNOME/pygoocanvas/master/NEWS
# vim:set ts=2 sw=2 et:
