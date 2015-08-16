# Contributor: FAWN <add@email.cz>
# Maintainer: FAWN <add@email.cz>
pkgname=kdemod3-knetworkmanager
pkgver=3.5.12
pkgrel=2
license=('GPL')
arch=('i686' 'x86_64')
options=(libtool)
pkgdesc="A NetworkManager front-end for KDEmod based on Trinity 3.5.12"
url="http://en.opensuse.org/Projects/KNetworkManager"
depends=('networkmanager>=0.8' 'networkmanager<=0.8.3-0.20110113' 'kdemod3-kdelibs' 'kdemod3-dbus-qt3')
makedepends=('pkgconfig' 'libnl1')
source=(http://mirror.ets.kth.se/trinity/releases/3.5.12/applications/knetworkmanager-${pkgver}.tar.gz knetworkmanager.conf.patch)
md5sums=('481d588d3ad8f5e038a954fa969dc4a3' 'a9d35768468b82e4313c13e7f2dbadb3')

build() {
	cd $srcdir/applications/knetworkmanager
	cp -Rp /usr/share/aclocal/libtool.m4 admin/libtool.m4.in
	cp -Rp /usr/share/libtool/config/ltmain.sh admin/ltmain.sh

	#LIBNM-GLIB name patch
	sed -ie 's/libnm_glib/libnm-glib/g' knetworkmanager-0.8/configure.in.in

	#UI patch by Elvaka
	sed -ie 's/Form1/SecurityForm/g' knetworkmanager-0.8/src/connection_setting_wireless_security_auth.ui
	
	#knetworkmanager.conf patch
	patch knetworkmanager-0.8/knetworkmanager.conf < $srcdir/knetworkmanager.conf.patch

	make -f admin/Makefile.common || return 1

	export PATH=/opt/qt/bin:/opt/kde3/bin:$PATH
	export LD_LIBRARY_PATH=/opt/kde3/lib:/opt/kde3/lib/kde3:$LD_LIBRARY_PATH
	export PKG_CONFIG_PATH=:/opt/kde3/lib/pkgconfig:/opt/qt/lib/pkgconfig:$PKG_CONFIG_PATH

	./configure --prefix=/opt/kde3 --sysconfdir=/etc --libdir=/opt/kde3/lib \
	--with-qt-dir=/opt/qt --with-qt-includes=/opt/qt/include \
	--with-distro=arch --disable-rpath --localstatedir=/var \
	--with-qt-libraries=/opt/qt/lib --disable-debug \
	--without-arts --without-dialup --enable-final || return 1

	make || return 1

	make DESTDIR=$pkgdir/ install || return 1
}