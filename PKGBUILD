# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>

# Arch credits:
# Maintainer : Thomas Baechler <thomas@archlinux.org>

_linuxprefix=linux-xanmod

pkgname="${_linuxprefix}-nvidia-470xx"
pkgdesc="NVIDIA drivers for linux"
pkgver=470.239.06
pkgrel=69310
arch=('x86_64')
url="http://www.nvidia.com/"
license=('custom')
groups=("${_linuxprefix}-extramodules")
depends=("${_linuxprefix}" "nvidia-utils=${pkgver}")
makedepends=("${_linuxprefix}-headers")
provides=("nvidia=${pkgver}" 'NVIDIA-MODULE')
options=(!strip)
_durl="https://us.download.nvidia.com/XFree86/Linux-x86"
source=("${_durl}_64/${pkgver}/NVIDIA-Linux-x86_64-${pkgver}-no-compat32.run"
        '0001-Fix-conftest-to-ignore-implicit-function-declaration.patch'
        '0002-Fix-conftest-to-use-a-short-wchar_t.patch'
        '0003-Fix-conftest-to-use-nv_drm_gem_vmap-which-has-the-se.patch')
sha256sums=('4a4b2f1a0e5f4403dcc94b5df9970cb064fccee44e82282e6f03dbde6e4dfff0'
            'e9df0c3c563466f7d747a3bf45900ba7804b072c899cde016e8d557e1921d60e'
            '1181bef2128bd4d74b661164bed8a1eb19c917e68fedbcaf65a24a4c2638cb8f'
            '84f3e59f2730752c0c4ef8bd2488953ccad13eb91a7f045488754e3bd61a1a6e')

_pkg="NVIDIA-Linux-x86_64-${pkgver}-no-compat32"

prepare() {
    sh "${_pkg}.run" --extract-only

    cd "${_pkg}/kernel"
    local src
    for src in "${source[@]}"; do
        src="${src%%::*}"
        src="${src##*/}"
        [[ $src = *.patch ]] || continue
        msg2 "Applying patch: $src..."
        patch -Np1 < "../../$src"
    done
}

build() {
  _kernver="$(cat /usr/src/${_linuxprefix}/version)"
    _kernver="$(cat /usr/src/${_linuxprefix}/version)"

    cd "${_pkg}"
    make -C kernel SYSSRC=/usr/lib/modules/"${_kernver}/build" module
}

package() {
    _kernver="$(cat /usr/src/${_linuxprefix}/version)"

    cd "${_pkg}"
    install -Dm 644 kernel/*.ko -t "${pkgdir}/usr/lib/modules/${_kernver}/extramodules/"

    # compress each module individually
    find "${pkgdir}" -name '*.ko' -exec xz -T1 {} +

    install -Dm 644 LICENSE -t "${pkgdir}/usr/share/licenses/${pkgname}/"
}
