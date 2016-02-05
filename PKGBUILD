# Maintainer:	Ondřej Surý <ondrej@sury.
# Contributor:	Oleander Reis <oleander@oleander.cc>
# Contributor:	Otto Sabart <seberm[at]gmail[dot]com>

srcname=knot
pkgname=${srcname}
pkgver=2.1.0
pkgrel=4
pkgdesc='high-performance authoritative-only DNS server'
url='https://www.knot-dns.cz/'
arch=('i686' 'x86_64')
license=('GPL3')
install=install
depends=('liburcu>=0.5.4' 'gnutls>=3.0' 'zlib' 'lmdb' 'jansson')
makedepends=('autoconf>=2.65' 'libtool' 'flex>=2.5.3' 'bison>=2.3', 'pkg-config')
source=("https://secure.nic.cz/files/knot-dns/${srcname}-${pkgver}.tar.xz"
        'knot.service'
        'knot.tmpfiles'
	'nettle-3.2.patch')
sha256sums=('1f6ea98da000386bf86e015655a9ec974d361b62711caf06b55f3d9bb2aa85a9'
            'caa870a9c93c57c6311f9e8fb5685a9179bb9839a27a30cc1712c91df0d15090'
            '592ffb904b697b8c09ab95b3874ad00637333f1805ab2ab0ee50b4f484108ee2'
	    '568f8d0e4a535b23ac3c00c62957ab3ec99b64a69c6f5d5613cb4b8caedd45e6')

prepare() {
	cd "${srcdir}"/${pkgname}-${pkgver}
	patch -Np1 -i ../nettle-3.2.patch
}

build() {
	cd "${srcdir}/${srcname}-${pkgver}"

	./configure \
		--prefix /usr \
		--sbindir /usr/bin \
		--sysconfdir=/etc \
		--localstatedir=/var/lib \
		--libexecdir=/usr/lib/knot \
		--with-rundir=/run/knot \
		--with-storage=/var/lib/knot \
		--enable-recvmmsg=yes \
		--disable-silent-rules

	make
}

check() {
	cd "${srcdir}/${srcname}-${pkgver}"
	make check
}

package() {
	cd "${srcdir}/${srcname}-${pkgver}"

	make DESTDIR="${pkgdir}/" install
	install -Dm 644 "${srcdir}/knot.service" "${pkgdir}/usr/lib/systemd/system/knot.service"
	install -Dm 644 "${srcdir}/knot.tmpfiles" "${pkgdir}/usr/lib/tmpfiles.d/knot.conf"
}
