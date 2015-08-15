# Maintainer   : Kaushal M <kshlmster at gmail dot com>
# Contributors : Tony Lambiris <tony at libpcap dot net>
#                Henning Mueller <henning at orgzim dot net>
pkgname=glusterfs-git
_gitname="glusterfs"
pkgver=v3.7dev.9.g5b8de97
pkgrel=1
pkgdesc="A cluster file-system capable of scaling to several peta-bytes (git master branch version)."
arch=('i686' 'x86_64')
url="http://www.gluster.org/"
license=(GPL2 LGPL3)
depends=('fuse' 'libxml2' 'python2' 'libaio' 'lvm2' 'openssl' 'libibverbs' 'readline' 'ncurses')
makedepends=('cmockery2')
provides=('glusterfs')
conflicts=('glusterfs' 'glusterfs-beta')
#Using github as it is faster. The github git repo is synced with gluster repo
#_gitroot="git://git.gluster.com/glusterfs.git"
source=(git://github.com/gluster/glusterfs.git)

sha256sums=('SKIP')

pkgver() {
        cd $_gitname
        git describe --always | sed 's|-|.|g'
}

build() {
  cd "$_gitname"

  ./autogen.sh
  ./configure \
    --prefix=/usr \
    --sysconfdir=/etc \
    --localstatedir=/var \
    --mandir=/usr/share/man/ \
    --libexecdir=/usr/lib/$pkgname \
    --sbindir=/usr/bin \
    --with-mountutildir=/usr/bin \
    --disable-syslog \
    PYTHON=python2
  make
}

package() {
  cd "$_gitname"
  make DESTDIR="$pkgdir" install

  install -D -m 755 README \
    $pkgdir/usr/share/doc/glusterfs/
  install -D -m 755 INSTALL \
    $pkgdir/usr/share/doc/glusterfs/
  install -D -m 755 COPYING-* \
    $pkgdir/usr/share/doc/glusterfs/

  install -D -m 644 extras/systemd/glusterd.service \
    $pkgdir/usr/lib/systemd/system/glusterd.service
}
