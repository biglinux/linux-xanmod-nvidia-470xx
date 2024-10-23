# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Contributor: Thomas Baechler <thomas@archlinux.org>

_linuxprefix=linux-xanmod

pkgname="${_linuxprefix}-nvidia-470xx"
pkgdesc="NVIDIA drivers for linux"
pkgver=470.256.02
pkgrel=61151
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
        '0003-Fix-conftest-to-use-nv_drm_gem_vmap-which-has-the-se.patch'
        'kernel-6.10.patch')
sha256sums=('fe8f58732055dacc4af0c4bb2371022d6e116e9f9594d7d3bea71f5a8a29e2b1'
            'eafd8a3c9740f34c8a0ccd0942d05318be94889eeb64ff66c54c8d8524ff5fd0'
            'aad55ebe45fca932ebeea5071bde489d3533bcccb3fe16995c8e70929b62e01a'
            '2339209c742bf58e5aa1e5c369e925f0c78eeb74537288183b683882ebf78809'
            '94c3429db54954d8529ec135770d7dfda08185287565c38368f25f6d76ce3399')

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
