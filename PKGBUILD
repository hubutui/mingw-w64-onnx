# Maintainer: Hu Butui <hot123tea123@gmail.com>

_realname=onnx
pkgbase=mingw-w64-${_realname}
pkgname=("${MINGW_PACKAGE_PREFIX}-${_realname}")
pkgver=1.10.2
pkgrel=1
pkgdesc="Open standard for machine learning interoperability"
arch=('any')
mingw_arch=('mingw32' 'mingw64' 'ucrt64' 'clang64' 'clang32')
url="https://onnx.ai"
license=('Apache')
depends=(
  "${MINGW_PACKAGE_PREFIX}-gcc-libs"
  "${MINGW_PACKAGE_PREFIX}-protobuf"
)
makedepends=("${MINGW_PACKAGE_PREFIX}-cmake")
source=("${_realname}-${pkgver}.tar.gz::https://github.com/onnx/onnx/archive/refs/tags/v${pkgver}.tar.gz"
        "0001.fix-building.patch")
sha256sums=('520b3aa34272cc215e2eb41385f58adf01750d88858d4722563edca8410c5dc9'
            '93955c15b31773852f4aec97fd1d3254f66e6b48096dae4a9eefa58af0c3cdba')

prepare() {
  cd "${_realname}-${pkgver}"
  patch -p1 -i "${srcdir}/0001.fix-building.patch"
}

build() {
  echo "==> Build static version"
  MSYS2_ARG_CONV_EXCL="-DCMAKE_INSTALL_PREFIX=" \
  ${MINGW_PREFIX}/bin/cmake.exe \
    -B "${srcdir}/build-static-${MINGW_CHOST}" \
    -S ${_realname}-${pkgver} \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=${MINGW_PREFIX} \
    -DBUILD_SHARED_LIBS=OFF \
    -DONNX_USE_PROTOBUF_SHARED_LIBS=OFF \
    -G'MSYS Makefiles'
  ${MINGW_PREFIX}/bin/cmake.exe --build "${srcdir}/build-static-${MINGW_CHOST}"

  echo "==> Build shared version"
  MSYS2_ARG_CONV_EXCL="-DCMAKE_INSTALL_PREFIX=" \
  ${MINGW_PREFIX}/bin/cmake.exe \
    -B "${srcdir}/build-shared-${MINGW_CHOST}" \
    -S ${_realname}-${pkgver} \
    -DCMAKE_BUILD_TYPE=Release \
    -DCMAKE_INSTALL_PREFIX=${MINGW_PREFIX} \
    -DBUILD_SHARED_LIBS=ON \
    -DONNX_USE_PROTOBUF_SHARED_LIBS=ON \
    -G'MSYS Makefiles'
  ${MINGW_PREFIX}/bin/cmake.exe --build "${srcdir}/build-shared-${MINGW_CHOST}"
}

package() {
  DESTDIR="${pkgdir}" \
  ${MINGW_PREFIX}/bin/cmake.exe \
    --build "${srcdir}/build-static-${MINGW_CHOST}" \
    --target install

  DESTDIR="${pkgdir}" \
  ${MINGW_PREFIX}/bin/cmake.exe \
    --build "${srcdir}/build-shared-${MINGW_CHOST}" \
    --target install
}
