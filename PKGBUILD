# Maintainer: Masahiro Sakai <masahiro.sakai@gmail.com>

_realname=mkl-dnn
_mklversion=2019.0.5.20190502
_mklpackage=mklml_win_${_mklversion}
pkgbase=mingw-w64-${_realname}
pkgname="${MINGW_PACKAGE_PREFIX}-${_realname}"
pkgver=0.19
pkgrel=1
pkgdesc="MKL-DNN - Intel(R) Math Kernel Library for Deep Neural Networks (mingw-w64)"
arch=('x86_64')
url='https://github.com/intel/mkl-dnn'
license=('Apache License 2.0')
depends=("${MINGW_PACKAGE_PREFIX}-gcc-libs")
makedepends=("${MINGW_PACKAGE_PREFIX}-gcc" "${MINGW_PACKAGE_PREFIX}-cmake")
options=('staticlibs' '!strip')
install=${_realname}.install
source=(${_realname}-${pkgver}.tar.gz::https://github.com/intel/mkl-dnn/archive/v${pkgver}.tar.gz
        ${_mklpackage}.zip::https://github.com/intel/mkl-dnn/releases/download/v${pkgver}/${_mklpackage}.zip)
sha256sums=('ba39da6adb263df05c4ca2a120295641fc97be75b588922e4274cb628dbe1dcd'
            '535857b17643d7f7546b58fc621244e7cfcc4fff2aa2ebd3fc5b4e126bfc36cf')

prepare() {
  cd "${srcdir}/${_realname}-${pkgver}"
  [[ -d external ]] && rm -rf external
  mkdir -p external
  cp -a ${srcdir}/${_mklpackage} external/
}

build() {
  [[ -d "${srcdir}/build-${MINGW_CHOST}" ]] && rm -rf "${srcdir}/build-${MINGW_CHOST}"
  mkdir "${srcdir}/build-${MINGW_CHOST}"
  cd "${srcdir}/build-${MINGW_CHOST}"
  MSYS2_ARG_CONV_EXCL="-DCMAKE_INSTALL_PREFIX=" \
  ${MINGW_PREFIX}/bin/cmake.exe \
    -G "MSYS Makefiles" \
    -DCMAKE_INSTALL_PREFIX=${MINGW_PREFIX} \
    -DARCH_OPT_FLAGS="" \
    "${srcdir}/${_realname}-${pkgver}"
  make -j1
}

package() {
  cd "${srcdir}/build-${MINGW_CHOST}"
  make DESTDIR="${pkgdir}" install
  install -Dm644 -t "${pkgdir}${MINGW_PREFIX}/share/doc/mklml" "${srcdir}/${_realname}-${pkgver}/external/${_mklpackage}/license.txt"
  # Manually strip libmkldnn.dll so that mklml.dll and libiomp5md.dll is left intact.
  ${MINGW_PREFIX}/bin/strip --strip-unneeded "${pkgdir}${MINGW_PREFIX}/bin/libmkldnn.dll"
}
