# Maintainer: Axel Moinet (BuLLeKeUp) <bullekeup@gmail.com>
# Contributor: Allan McRae <allan@archlinux.org>

# toolchain build order: linux-api-headers->glibc->binutils->gcc->binutils->glibc

# build from head of release branch as bug fix releases are rare

pkgname=binutils-pep
pkgver=2.25.1
pkgrel=1
_commit=2bd25930
pkgdesc="A set of programs to assemble and manipulate binary and object files"
arch=('i686' 'x86_64')
url="http://www.gnu.org/software/binutils/"
license=('GPL')
groups=('base-devel')
depends=('glibc>=2.22' 'zlib')
makedepends=('git')
checkdepends=('dejagnu' 'bc')
conflicts=('binutils-multilib', 'binutils')
replaces=('binutils-multilib')
provides=("binutils=$pkgver-$pkgrel")
options=('staticlibs' '!distcc' '!ccache')
install=binutils.install
source=(git://sourceware.org/git/binutils-gdb.git#commit=${_commit}
        binutils-e9c1bdad.patch)
md5sums=('SKIP'
         'eb3aceaab8ed26e06d505f82beb30f8f')

prepare() {
  cd ${srcdir}/binutils-gdb

  # https://sourceware.org/bugzilla/show_bug.cgi?id=16992
  patch -p1 -i ${srcdir}/binutils-e9c1bdad.patch

  # hack! - libiberty configure tests for header files using "$CPP $CPPFLAGS"
  sed -i "/ac_cpp=/s/\$CPPFLAGS/\$CPPFLAGS -O2/" libiberty/configure

  mkdir ${srcdir}/binutils-build
}

build() {
  cd ${srcdir}/binutils-build

  ${srcdir}/binutils-gdb/configure --prefix=/usr \
    --with-lib-path=/usr/lib:/usr/local/lib \
    --with-bugurl=https://bugs.archlinux.org/ \
    --enable-threads --enable-shared --with-pic \
    --enable-ld=default --enable-gold --enable-plugins \
    --enable-deterministic-archives \
    --disable-werror --enable-targets=x86_64-pep --disable-gdb

  # check the host environment and makes sure all the necessary tools are available
  make configure-host

  make tooldir=/usr
}

check() {
  cd ${srcdir}/binutils-build
  
  # unset LDFLAGS as testsuite makes assumptions about which ones are active
  # ignore failures in gold testsuite...
  make -k LDFLAGS="" check || true
}

package() {
  cd ${srcdir}/binutils-build
  make prefix=${pkgdir}/usr tooldir=${pkgdir}/usr install

  # Remove unwanted files
  rm ${pkgdir}/usr/share/man/man1/{dlltool,nlmconv,windres,windmc}*

  # No shared linking to these files outside binutils
  rm ${pkgdir}/usr/lib/lib{bfd,opcodes}.so
}
