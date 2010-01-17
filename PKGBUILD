# $Id$
# Maintainer: Andrea Scarpino <andrea@archlinux.org>
# Contributor: Douglas Soares de Andrade <douglas@archlinux.org>
# Contributor: riai <riai@bigfoot.com> Ben <ben@benmazer.net>

pkgname=pyqt
pkgver=4.7.0
_pkgver=4.7
pkgrel=1
pkgdesc="A set of Python bindings for the Qt toolkit"
arch=('i686' 'x86_64')
url="http://riverbankcomputing.co.uk/software/pyqt/intro"
depends=('sip>=4.10' 'qscintilla>=2.4.1' 'dbus-python' 'openssl')
license=('GPL')
provides=('pyqt4')
replaces=('pyqt4')
conflicts=('pyqt4')
source=(http://riverbankcomputing.com/static/Downloads/PyQt4/PyQt-x11-gpl-${_pkgver}.tar.gz)
md5sums=('4882179ab4da128aaa8520f146afb955')
options=('force')

build() {
  cd ${srcdir}/PyQt-x11-gpl-${_pkgver}
  
  python configure.py --confirm-license \
    -b /usr/bin \
    -d /usr/lib/python2.6/site-packages \
    -v /usr/share/sip || return 1

  # Thanks Gerardo for the rpath fix
  find -name 'Makefile' | xargs sed -i 's|-Wl,-rpath,/usr/lib||g;s|-Wl,-rpath,.* ||g'

  make || return 1

  # INSTALL_ROOT is needed for the QtDesigner module, the other Makefiles use DESTDIR
  make DESTDIR=${pkgdir} INSTALL_ROOT=${pkgdir} install || return 1
}
