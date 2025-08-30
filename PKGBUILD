pkgname=zstd
pkgver=1.5.7
pkgrel=2
pkgdesc="Zstandard - Fast real-time compression algorithm"
arch=('x86_64')
url="https://facebook.github.io/zstd/"
license=(
    'BSD-3-Clause'
    'GPL-2.0-only'
)
depends=(
    'gcc-libs'
    'glibc'
    'lz4'
    'zlib'
    'xz'
)
source=(https://github.com/facebook/zstd/releases/download/v${pkgver}/${pkgname}-${pkgver}.tar.gz)
sha256sums=(eb33e51f49a15e023950cd7825ca74a4a2b43db8354825ac24fc1b7ee09e6fa3)

build() {
    cd ${pkgname}-${pkgver}

    export CFLAGS+=' -ffat-lto-objects'
    export CXXFLAGS+=' -ffat-lto-objects'

    make CC="${CHOST}-gcc" prefix=/usr libdir=/usr/lib64
}

package() {
    cd ${pkgname}-${pkgver}

    make DESTDIR=${pkgdir} prefix=/usr libdir=/usr/lib64 install
}
