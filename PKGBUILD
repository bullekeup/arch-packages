# $Id: PKGBUILD 269096 2016-06-07 12:10:37Z heftig $
# Maintainer: Jan Alexander Steffens (heftig) <jan.steffens@gmail.com>
# Contributor: Jaroslav Lichtblau <dragonlord@aur.archlinux.org>
# Contributor: Jeremy Cowgar <jeremy@cowgar.com>

pkgname=check
pkgver=0.11.0
pkgrel=1
pkgdesc="A unit testing framework for C"
url="https://libcheck.github.io/check/"
arch=(i686 x86_64)
license=(LGPL)
depends=(awk)
makedepends=(git)
source=("git+https://github.com/libcheck/check#tag=$pkgver")
md5sums=('SKIP')

prepare() {
  cd $pkgname
  autoreconf -fvi
}


build() {
  cd $pkgname
  ./configure --prefix=/usr --disable-static
  make
}

check() {
  cd $pkgname
  # Extremely long
  #make -k check
}

package() {
  cd $pkgname
  make DESTDIR="$pkgdir" install

  # get rid of the package's info directory
  rm "$pkgdir/usr/share/info/dir"

  # svn log file is too big
  rm "$pkgdir"/usr/share/doc/check/*ChangeLog*
}
