# Maintainer: Bernhard Landauer <bernhard@manjaro.org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>

# Arch credits:
# Maintainer : Thomas Baechler <thomas@archlinux.org>

_linuxprefix=linux-xanmod
_extramodules=$(find /usr/lib/modules -type d -iname 6.2.3*xanmod* | rev | cut -d "/" -f1 | rev)

pkgname=$_linuxprefix-nvidia-470xx
pkgdesc="NVIDIA drivers for linux"
pkgver=470.161.03
pkgrel=6231
arch=('x86_64')
url="http://www.nvidia.com/"
license=('custom')
groups=("$_linuxprefix-extramodules")
depends=("$_linuxprefix" "nvidia-utils=$pkgver")
makedepends=("$_linuxprefix-headers")
provides=("nvidia=$pkgver" 'NVIDIA-MODULE')
options=(!strip)
install=nvidia.install
_durl="https://us.download.nvidia.com/XFree86/Linux-x86"
source=("${_durl}_64/${pkgver}/NVIDIA-Linux-x86_64-${pkgver}-no-compat32.run"
        'kernel-6.2.patch')
sha256sums=('b318c15d519898fd60c64ef73fdd331045884a1d97d811a25fbced544b5be6ad'
            '3501f0bbd9603543da74873905448ff58f5095948a7375617bba74f122d575aa')

_pkg="NVIDIA-Linux-x86_64-${pkgver}-no-compat32"

prepare() {
    sh "${_pkg}.run" --extract-only

    cd "${_pkg}/kernel"
    patch -Np1 -i ${srcdir}/kernel-6.2.patch
}

build() {
    _kernver=$(find /usr/lib/modules -type d -iname 6.2.3*xanmod* | rev | cut -d "/" -f1 | rev)

    cd "${_pkg}"
    make -C kernel SYSSRC=/usr/lib/modules/"${_kernver}/build" module
}

package() {
    cd "${_pkg}"
    install -Dm644 kernel/*.ko -t "${pkgdir}/usr/lib/modules/${_extramodules}/"

    # compress each module individually
    find "${pkgdir}" -name '*.ko' -exec xz -T1 {} +

    install -Dm644 LICENSE -t "${pkgdir}/usr/share/licenses/${pkgname}/"
}
