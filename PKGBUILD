# Maintainer: brenix <brenix@gmail.com>
# Contributor: A.J. Korf <jacobkorf at gmail dot com>
# Contrubutor: Thomas Baechler <thomas@archlinux.org>

pkgname=nvidia-rifs
pkgver=302.17
_extramodules=extramodules-3.4-rifs
_kernver="$(cat /usr/lib/modules/${_extramodules}/version)"
pkgrel=2
_pkgdesc="NVIDIA drivers for linux-rifs."
pkgdesc="$_pkgdesc"
arch=('i686' 'x86_64')
url="http://www.nvidia.com/"
depends=('linux-rifs>=3.4' 'linux-rifs<3.5' "nvidia-utils=${pkgver}")
makedepends=('linux-rifs-headers>=3.4' 'linux-rifs-headers<3.5')
conflicts=('nvidia-96xx-all' 'nvidia-173xx-all' 'nvidia-rifs-stable' 'nvidia-beta-rifs')
license=('custom')
options=(!strip)
install=nvidia-rifs.install

if [ "$CARCH" = "i686" ]; then
	_arch='x86'
	_pkg="NVIDIA-Linux-${_arch}-${pkgver}"
	source=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")
	md5sums=('b7f908ea08218df08db06026215ec419')
elif [ "$CARCH" = "x86_64" ]; then
	_arch='x86_64'
	_pkg="NVIDIA-Linux-${_arch}-${pkgver}-no-compat32"
	source=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run")
	md5sums=('7e768412a16853078b04037a7cc9c8ac')
fi

build() {
	cd "${srcdir}"
	sh ${_pkg}.run --extract-only
	cd ${_pkg}/kernel
	make SYSSRC=/usr/lib/modules/"${_kernver}/build" module
}

package() {
	install -D -m644 "${srcdir}/${_pkg}/kernel/nvidia.ko" "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia.ko"
	install -d -m755 "${pkgdir}/usr/lib/modprobe.d"
	echo "blacklist nouveau" >> "${pkgdir}/usr/lib/modprobe.d/nvidia-rifs.conf"
	sed -i -e "s/EXTRAMODULES='.*'/EXTRAMODULES='${_extramodules}'/" "${startdir}/nvidia-rifs.install"
	gzip "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia.ko"
}
