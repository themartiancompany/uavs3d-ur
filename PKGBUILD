# SPDX-License-Identifier: AGPL-3.0
#
# Maintainer: Truocolo <truocolo@aol.com>
# Maintainer: Pellegrino Prevete (tallero) <pellegrinoprevete@gmail.com>
# Maintainer: Daniel Bermond <dbermond@archlinux.org>

_os="$( \
  uname \
    -o)"
_proj="uavs3"
_pkg="${_proj}d"
pkgname="${_pkg}"
pkgver=1.1.r47.g1fd0491
_commit="1fd04917cff50fac72ae23e45f82ca6fd9130bd8"
pkgrel=1
_pkgdesc=(
  'An AVS3 decoder supporting'
  'AVS3-P2 baseline profile.'
)
arch=(
  'x86_64'
  'arm'
  'aarch64'
  'armv7l'
  'armv6l'
  'mips'
  'pentium4'
  'powerpc'
)
_http="https://github.com"
_ns="${_proj}"
url="${_http}/${_ns}/${_pkg}"
license=(
  'BSD'
)
depends=(
)
if [[ "${_os}" == "GNU/Linux" ]]; then
  depends+=(
    'glibc'
  )
elif [[ "${_os}" == "Android" ]]; then
  depends+=(
    'ndk-sysroot'
  )
fi
makedepends=(
  'git'
  'cmake'
)
provides=(
)
conflicts=(
)
source=(
  "git+${url}#commit=${_commit}"
  "010-${_pkg}-10bit.patch"
)
sha256sums=(
  'SKIP'
  '4cf38433b65937565935b922201e539eb0f5fd0ef32970b78b25f08c8e4519f1'
)

prepare() {
  [ -d "${_pkg}-10bit" ] && \
  rm \
    -r "${_pkg}-10bit"
  cp \
    -a \
    "${_pkg}" \
    "${_pkg}-10bit"
  patch \
    -d \
    "${_pkg}-10bit" \
    -Np1 \
    -i \
    "${srcdir}/010-${_pkg}-10bit.patch"
}

pkgver() {
  git \
    -C \
      "${_pkg}" \
    describe \
      --long \
      --tags | \
    sed \
      's/\([^-]*-g\)/r\1/;s/-/./g;s/^v//'
}

build() {
  local \
    _cmake_opts=()
  _cmake_opts=(
    -DCMAKE_BUILD_TYPE:STRING='None'
    -DCMAKE_INSTALL_PREFIX:PATH='/usr'
    -DCMAKE_SKIP_RPATH:BOOL='YES'
    -DBUILD_SHARED_LIBS:BOOL='ON'
  )
  cd \
    "${_pkg}"
  cmake \
    -B \
      build/linux \
    -S \
      . \
    "${_cmake_opts[@]}" \
    -DCOMPILE_10BIT='0'
  make \
    -C \
      build/linux
  cd \
    "${srcdir}/${_pkg}-10bit"
  cmake \
    -B \
      build/linux \
    -S \
      . \
    "${_cmake_opts[@]}" \
    -DCOMPILE_10BIT='1'
  make \
    -C \
      build/linux
}

package() {
  make \
    -C \
      "${_pkg}/build/linux" \
    DESTDIR="${pkgdir}" \
    install
  make \
    -C "${_pkg}-10bit/build/linux" \
    DESTDIR="${pkgdir}" \
    install
  install \
    -Dm755 \
    "${_pkg}/build/linux/${_proj}dec" \
    -t \
    "${pkgdir}/usr/bin"
  install \
    -Dm755 \
    "${_pkg}-10bit/build/linux/${_proj}dec" \
    "${pkgdir}/usr/bin/${_proj}dec-10bit"
  install \
    -Dm644 \
    "${_pkg}/COPYING" \
    "${pkgdir}/usr/share/licenses/${pkgname}/LICENSE"
}

