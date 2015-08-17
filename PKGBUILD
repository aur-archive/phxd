pkgname=phxd
pkgver=29
pkgrel=4
pkgdesc="A hotline server written in python."
url="https://bitbucket.org/jvoss"
arch=(any)
license=("unknown")
depends=(python2)
makedepends=(python2 mercurial)
conflicts=(python-hxd python2-hxd)
provides=(python-hxd python2-hxd)
replaces=(python-hxd python2-hxd)
install=phxd.install
source=("phxd.service")
md5sums=('addbc5cccaae7d8906422ec7638911c4')
_hgroot=$url
_hgrepo=phxd

build() {
cd $srcdir
  msg "Connecting to hg server..."
  if [[ -d "$_hgrepo/.hg" ]]; then
    msg "pull"
    ( cd $_hgrepo && hg pull -u )
  else
    msg "clone"
    hg clone "${_hgroot}/${_hgrepo}"
  fi
  cd "${_hgrepo}"

  msg "Mercurial checkout done or server timeout"
  msg "Starting build..."
  
  rm -rf "${srcdir}/${_hgrepo}-build"
  cp -r "${srcdir}/${_hgrepo}" "${srcdir}/${_hgrepo}-build"
  cd "${srcdir}/${_hgrepo}-build"
  
  sed -i "s:#!/usr/bin/env python:#!/usr/bin/python2:" phxd.py
}

package() {
  cd "$srcdir/${_hgrepo}-build"
  install -Dm755 "phxd.py" "$pkgdir/srv/phxd/phxd.py"
  install -Dm644 "$srcdir/phxd.service" "$pkgdir/usr/lib/systemd/system/phxd.service"
  cp -r "ssl" "$pkgdir/srv/phxd/"
  mkdir -p "$pkgdir/usr/bin"
  ln -s "/srv/phxd/phxd.py" "$pkgdir/usr/bin/phxd"
}
