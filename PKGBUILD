# Maintainer: Tom Wambold <tom5760@gmail.com>
pkgname=core-svn
pkgver=20130404
pkgrel=1
pkgdesc="Common Open Research Emulator"
arch=('i686' 'x86_64')
url="http://cs.itd.nrl.navy.mil/work/core/"
license=('BSD')
depends=('libev' 'ebtables' 'iproute2' 'python2' 'bridge-utils' 'tkimg'
         'xterm')
makedepends=('dia' 'help2man' 'imagemagick' 'curl')
provides=('core')
conflicts=('core')
backup=('etc/core/core.conf' 'etc/core/perflogserver.conf')
source=('core-emulator.service'
        'python2.patch'
        'core-gui-wish-version.patch')
md5sums=('a49c24c341b53a969ee93806b701915c'
         '1b22a0ad28499813c1dd35c36d507d8a'
         'd72eb6ca3bc9ed388532e310a8de5735')

_svntrunk="http://downloads.pf.itd.nrl.navy.mil/core/source/nightly_snapshots/core-svnsnap.tgz"

build() {
  cd "$srcdir"
  msg "Downloading nightly snapshot..."
  curl -O "$_svntrunk"
  tar xf core-svnsnap.tgz

  cd core

  msg "Starting build..."

  patch -p1 < ../python2.patch
  patch -p1 < ../core-gui-wish-version.patch

  ./bootstrap.sh
  ./configure PYTHON=/usr/bin/python2 --prefix=/usr
  make
}

package() {
  cd "$srcdir/core"
  make DESTDIR="$pkgdir/" install

  rm "$pkgdir/etc/init.d/core"
  rmdir "$pkgdir/etc/init.d"

  install -D "$srcdir/core-emulator.service" "$pkgdir/usr/lib/systemd/system/core-emulator.service"
}

# vim:set ts=2 sw=2 et:
