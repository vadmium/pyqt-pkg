# $Id$
# Maintainer: Andrea Scarpino <andrea@archlinux.org>
# Contributor: Douglas Soares de Andrade <douglas@archlinux.org>
# Contributor: riai <riai@bigfoot.com> Ben <ben@benmazer.net>

pkgname=pyqt
pkgver=4.8.0
_pkgver=4.8
pkgrel=1
pkgdesc="A set of Python bindings for the Qt toolkit"
arch=('i686' 'x86_64')
url="http://riverbankcomputing.co.uk/software/pyqt/intro"
license=('GPL')
depends=('sip' 'qt' 'dbus-python')
makedepends=('phonon' 'python-opengl' 'qt-assistant-compat')
optdepends=('phonon: enable audio and video in PyQt applications'
	'python-opengl: enable OpenGL 3D graphics in PyQt applications'
	'qscintilla: QScintilla API'
	'qt-assistant-compat: add PyQt online help in Qt Assistant')
provides=('pyqt4')
replaces=('pyqt4')
conflicts=('pyqt4')
source=("http://riverbankcomputing.co.uk/static/Downloads/PyQt4/PyQt-x11-gpl-${_pkgver}.tar.gz")
md5sums=('003d195498eb42ca2daf7615fa01488d')

build() {
  cd ${srcdir}/PyQt-x11-gpl-${_pkgver}

  python2 configure.py \
    --confirm-license \
    -v /usr/share/sip \
    --qsci-api

  # Thanks Gerardo for the rpath fix
  find -name 'Makefile' | xargs sed -i 's|-Wl,-rpath,/usr/lib||g;s|-Wl,-rpath,.* ||g'

  make
}

package(){
  cd ${srcdir}/PyQt-x11-gpl-${_pkgver}
  # INSTALL_ROOT is needed for the QtDesigner module, the other Makefiles use DESTDIR
  make DESTDIR="${pkgdir}" INSTALL_ROOT="${pkgdir}" install
}
