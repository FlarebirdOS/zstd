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
    'glibc'
    'libgcc'
    'libstdc++'
    'zlib'
    'xz'
    'lz4'
)
makedepends=(
    'cmake'
    'googletest'
    'ninja'
)
provides=('libzstd.so')
source=(https://github.com/facebook/zstd/releases/download/v${pkgver}/${pkgname}-${pkgver}.tar.gz
    https://github.com/facebook/zstd/commit/6af3842118ea5325480b403213b2a9fbed3d3d74.patch)
sha256sums=(eb33e51f49a15e023950cd7825ca74a4a2b43db8354825ac24fc1b7ee09e6fa3
    7c07be222d45718b81fb16f97e611adfeb24efa0712ca77fb1e08f4c67803ec3)

prepare() {
    cd ${pkgname}-${pkgver}
    # avoid error on tests without static libs, we use LD_LIBRARY_PATH
    sed '/build static library to build tests/d' -i build/cmake/CMakeLists.txt
    sed 's/libzstd_static/libzstd_shared/g' -i build/cmake/tests/CMakeLists.txt
    # fix manpages
    patch -Np1 -i ${srcdir}/6af3842118ea5325480b403213b2a9fbed3d3d74.patch
}

build() {
    cd ${pkgname}-${pkgver}

    local cmake_args=(
        -B flarebird-build
        -G Ninja
        -S build/cmake
        -D CMAKE_BUILD_TYPE=Release
        -D CMAKE_INSTALL_PREFIX=/usr
        -D CMAKE_INSTALL_LIBDIR=lib64
        -D BUILD_SHARED_LIBS=ON
        -D ZSTD_ZLIB_SUPPORT=ON
        -D ZSTD_LZMA_SUPPORT=ON
        -D ZSTD_LZ4_SUPPORT=ON
        -D ZSTD_BUILD_CONTRIB=ON
        -D ZSTD_BUILD_STATIC=OFF
        -D ZSTD_BUILD_TESTS=ON
        -D ZSTD_PROGRAMS_LINK_SHARED=ON
    )

    export CFLAGS+=' -ffat-lto-objects'
    export CXXFLAGS+=' -ffat-lto-objects'

    cmake "${cmake_args[@]}"

    cmake --build flarebird-build
}

package() {
    cd ${pkgname}-${pkgver}

    DESTDIR=${pkgdir} cmake --install flarebird-build
}
