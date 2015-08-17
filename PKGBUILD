#!/bin/bash

# Maintainer: Kurt J. Bosch <kjb-temp-2009 at alpenjodel.de>
# Contributor: Sergej Pupykin <pupykin.s+arch@gmail.com>
# Contributor: Kaos < gianlucaatlas at gmail dot com >

_pkgname=uswsusp
pkgname=${_pkgname}-fbsplash
pkgver=1.0
pkgrel=7
pkgdesc="Userspace software suspend aka suspend-utils - with Fbsplash support"
arch=('i686' 'x86_64')
url="http://suspend.sourceforge.net"
license=('GPL')
depends=('libgcrypt' 'libx86' 'lzo2'
	'pciutils>=2.2.4' 'zlib'
	'fbsplash'
	'mkinitcpio'
)
optdepends=(
	'fbsplash-themes-arch-banner: Show icons for daemons and pseudo-services'
	'fbsplash-extras: Show icons for daemons and pseudo-services'
	'pm-utils: Show icons for daemons and pseudo-services'
)
provides=("${_pkgname}=${pkgver}")
conflicts=("${_pkgname}")
backup=('etc/suspend.conf')
install=INSTALL
changelog=CHANGELOG
source=(
	"http://downloads.sourceforge.net/project/suspend/suspend/suspend-${pkgver}/suspend-utils-${pkgver}.tar.bz2"
	'suspend-0.9_pre0-errno.patch'
	'uresume-hook'
	'uresume-install'
	'suspend.conf.patch'
	's2ram-chvt63.patch'
	's2disk-no-chvt1.patch'
	's2disk-splash-prepare-fail-msg-only.patch'
	's2disk-keep-vt16.patch'
	'fbsplash-no-pre-snapshoot-progress.patch'
	'fbsplash-no-fade-effects.patch'
	'resume-disable-print-libgcrypt-version.patch'
)

build() {
	cd "${srcdir}"/suspend-utils-${pkgver}

	# Fix build error - patch from http://bugs.gentoo.org/339759
	patch -Np0 -i ../suspend-0.9_pre0-errno.patch

	# Make the config file a bit more nice
	patch -Np2 -i ../suspend.conf.patch

	# Show a black screen instead of ugly console
	patch -Np2 -i ../s2ram-chvt63.patch
	patch -Np2 -i ../s2disk-no-chvt1.patch

	# Don't show message "Looking for splash system..."
	patch -Np2 -i ../s2disk-splash-prepare-fail-msg-only.patch

	# If already on default Fbsplash VT, don't change away from it
	patch -Np2 -i ../s2disk-keep-vt16.patch

	# Fix initial suspend progress bar shown again after resume (0% = 100% resume in reverse)
	patch -Np2 -i ../fbsplash-no-pre-snapshoot-progress.patch

	# Disable fadein/fadeout effects (kernel cmdline isn't parsed)
	patch -Np2 -i ../fbsplash-no-fade-effects.patch

	# Don't show message "resume: libgcrypt version: VERSION"
	patch -Np2 -i ../resume-disable-print-libgcrypt-version.patch

	./configure \
		--prefix=/usr \
		--enable-compress \
		--enable-encrypt \
		--enable-threads \
		--disable-resume-static \
		--disable-static \
		--sysconfdir=/etc \
		--enable-fbsplash
	make
}

package() {
	cd "${srcdir}"/suspend-utils-${pkgver}

	make DESTDIR="${pkgdir}" install
	rmdir "${pkgdir}"/dev

	install -D -m 644 "${srcdir}"/uresume-hook    "${pkgdir}"/usr/lib/initcpio/hooks/uresume
	install -D -m 644 "${srcdir}"/uresume-install "${pkgdir}"/usr/lib/initcpio/install/uresume
}

md5sums=('02f7d4b679bad1bb294a0efe48ce5934'
         '511f2309fccc4c6ec411104d1bcd371e'
         'fe43dbb7c80d48425f8afa03ae7cc317'
         'bfac28c03f84d56d341ed24e318dfb6e'
         '459b51ca2c39f4fba3d567127dc93165'
         '95111769efe97103f1ab5cfe9a408545'
         '4927a0bfa0056918ab58c47e8f837ef9'
         'c367b273e0d9430e44d86e092eff23f6'
         '8db88e41c1a8a34cf36d59498f19bdcc'
         '917d079ea7cd9a02d19361e71b0b3ac7'
         'a479dc5f19d1ee9c8315f3e63217a65a'
         '87adbe05e7281f3a571cdcae7e0378fc')
