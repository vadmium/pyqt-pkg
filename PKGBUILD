# $Id$
# Maintainer: Andrea Scarpino <andrea@archlinux.org>
# Contributor: Douglas Soares de Andrade <douglas@archlinux.org>
# Contributor: riai <riai@bigfoot.com> Ben <ben@benmazer.net>

pkgbase=pyqt
pkgname=('pyqt-common' 'pyqt' 'python2-pyqt')
pkgver=4.10
pkgrel=1
arch=('i686' 'x86_64')
url="http://riverbankcomputing.co.uk/software/pyqt/intro"
license=('GPL')
makedepends=('python-sip' 'python-dbus' 'python2-sip' 'phonon' 'mesa'
             'python2-opengl' 'qt-assistant-compat' 'qtwebkit' 'python2-dbus')
source=("http://downloads.sourceforge.net/${pkgbase}/PyQt-x11-gpl-${pkgver}.tar.gz")
md5sums=('b5953e96d0f82d322d0cba008163321e')

build() {
  cp -r PyQt-x11-gpl-${pkgver} Py2Qt-x11-gpl-${pkgver}

  cd PyQt-x11-gpl-${pkgver}
  python configure.py \
    --confirm-license \
    --qsci-api \
    -q /usr/bin/qmake-qt4

  # Thanks Gerardo for the rpath fix
  find -name 'Makefile' | xargs sed -i 's|-Wl,-rpath,/usr/lib||g;s|-Wl,-rpath,.* ||g'

  # Ugly workaround to fix build
  sed -i 's|/usr/include/qt4/phonon|/usr/include/phonon|' phonon/Makefile

  make

  ### Python2 version ###
  cd ../Py2Qt-x11-gpl-${pkgver}
  python2 configure.py \
    --confirm-license \
    -v /usr/share/sip \
    --qsci-api \
    -q /usr/bin/qmake-qt4

  # Thanks Gerardo for the rpath fix
  find -name 'Makefile' | xargs sed -i 's|-Wl,-rpath,/usr/lib||g;s|-Wl,-rpath,.* ||g'
  
  # Ugly workaround to fix build
  sed -i 's|/usr/include/qt4/phonon|/usr/include/phonon|' phonon/Makefile

  make
}

package_pyqt-common(){
  pkgdesc="Common PyQt files shared between pyqt and python2-pyqt"
  depends=('qt4')
  
  cd PyQt-x11-gpl-${pkgver}
  make -C pyrcc DESTDIR="${pkgdir}" install
  make -C pylupdate DESTDIR="${pkgdir}" install
  
  install -Dm644 PyQt4.api "${pkgdir}"/usr/share/qt4/qsci/api/python/PyQt4.api
}

package_pyqt(){
  pkgdesc="A set of Python 3.x bindings for the Qt toolkit"
  depends=('qtwebkit' 'python-sip' 'python-dbus' 'pyqt-common')
  optdepends=('phonon: enable audio and video in PyQt applications'
              'qscintilla: QScintilla API'
              'qt-assistant-compat: add PyQt online help in Qt Assistant')

  cd PyQt-x11-gpl-${pkgver}
  # INSTALL_ROOT is needed for the QtDesigner module, the other Makefiles use DESTDIR
  make DESTDIR="${pkgdir}" INSTALL_ROOT="${pkgdir}" install

  # Provided by pyqt-common
  rm "${pkgdir}"/usr/bin/{pylupdate4,pyrcc4}
  rm "${pkgdir}"/usr/share/qt4/qsci/api/python/PyQt4.api
}

package_python2-pyqt(){
  pkgdesc="A set of Python 2.x bindings for the Qt toolkit"
  depends=('qtwebkit' 'python2-sip' 'python2-dbus' 'pyqt-common')
  optdepends=('phonon: enable audio and video in PyQt applications'
              'python2-opengl: enable OpenGL 3D graphics in PyQt applications'
              'qscintilla: QScintilla API'
              'qt-assistant-compat: add PyQt online help in Qt Assistant')
  provides=('python2-qt')

  cd Py2Qt-x11-gpl-${pkgver}
  # INSTALL_ROOT is needed for the QtDesigner module, the other Makefiles use DESTDIR
  make DESTDIR="${pkgdir}" INSTALL_ROOT="${pkgdir}" install

  # Fix conflicts with pyqt
  mv "${pkgdir}"/usr/bin/{,python2-}pyuic4
  
  # Provided by pyqt
  rm "${pkgdir}"/usr/bin/{pylupdate4,pyrcc4}
  rm "${pkgdir}"/usr/lib/qt4/plugins/designer/libpythonplugin.so
  rm "${pkgdir}"/usr/share/qt4/qsci/api/python/PyQt4.api
}
