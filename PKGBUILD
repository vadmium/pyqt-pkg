# $Id$
# Maintainer: Andrea Scarpino <andrea@archlinux.org>
# Contributor: Douglas Soares de Andrade <douglas@archlinux.org>
# Contributor: riai <riai@bigfoot.com> Ben <ben@benmazer.net>

pkgname=pyqt
pkgver=4.7.0
_pkgver=4.7
pkgrel=3
pkgdesc="A set of Python bindings for the Qt toolkit"
arch=('i686' 'x86_64')
url="http://riverbankcomputing.co.uk/software/pyqt/intro"
license=('GPL')
depends=('sip' 'qscintilla' 'dbus-python' 'openssl')
makedepends=('phonon' 'python-opengl')
optdepends=('phonon' 'python-opengl')
provides=('pyqt4')
replaces=('pyqt4')
conflicts=('pyqt4')
source=("http://riverbankcomputing.com/static/Downloads/PyQt4/PyQt-x11-gpl-${_pkgver}.tar.gz"
	'fix-phonon-detection.patch')
md5sums=('4882179ab4da128aaa8520f146afb955'
         '364e2c96c6b2b1d98167b2c8b110d64c')

build() {
  cd ${srcdir}/PyQt-x11-gpl-${_pkgver}
  
  # already fixed upstream
  patch -Np1 -i ${srcdir}/fix-phonon-detection.patch || return 1

  python configure.py --confirm-license \
    -b /usr/bin \
    -d /usr/lib/python2.6/site-packages \
    -v /usr/share/sip || return 1

  # Thanks Gerardo for the rpath fix
  find -name 'Makefile' | xargs sed -i 's|-Wl,-rpath,/usr/lib||g;s|-Wl,-rpath,.* ||g'

  make || return 1
}

package(){
  cd ${srcdir}/PyQt-x11-gpl-${_pkgver}
  # INSTALL_ROOT is needed for the QtDesigner module, the other Makefiles use DESTDIR
  make DESTDIR=${pkgdir} INSTALL_ROOT=${pkgdir} install || return 1
}
