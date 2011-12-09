# $Id$
# Maintainer: Andrea Scarpino <andrea@archlinux.org>

pkgname=python-qt
pkgver=4.8.2
pkgrel=1
pkgdesc="A set of Python bindings for the Qt toolkit"
arch=('i686' 'x86_64')
url="http://riverbankcomputing.co.uk/software/pyqt/intro"
license=('GPL')
depends=('python-sip' 'qt')
makedepends=('phonon' 'qt-assistant-compat')
optdepends=('phonon: enable audio and video in PyQt applications'
	'qscintilla: QScintilla API'
	'qt-assistant-compat: add PyQt online help in Qt Assistant')
conflicts=('python2-qt')
source=("http://riverbankcomputing.co.uk/static/Downloads/PyQt4/PyQt-x11-gpl-${pkgver}.tar.gz"
        'fix-stackedwidget-bug.patch')
md5sums=('142a32f126f205a2bd77f6a9910f5333'
        '42cfd44a8ec063cce3e328ddb9892565')

build() {
  cd ${srcdir}/PyQt-x11-gpl-${pkgver}

  # Already fixed upstream
  patch -Np1 -i ${srcdir}/fix-stackedwidget-bug.patch

  python configure.py \
    --confirm-license \
    -v /usr/share/sip \
    --qsci-api

  # Thanks Gerardo for the rpath fix
  find -name 'Makefile' | xargs sed -i 's|-Wl,-rpath,/usr/lib||g;s|-Wl,-rpath,.* ||g'

  make
}

package(){
  cd ${srcdir}/PyQt-x11-gpl-${pkgver}
  # INSTALL_ROOT is needed for the QtDesigner module, the other Makefiles use DESTDIR
  make DESTDIR="${pkgdir}" INSTALL_ROOT="${pkgdir}" install
}
