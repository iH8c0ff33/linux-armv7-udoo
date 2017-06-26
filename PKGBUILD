buildarch=4

pkgbase=linux-armv7-udoo
pkgname=("${pkgbase}")
_pkgurlname=v3.14.56-1
_srcname=linux_kernel-3.14.56-1
_kernelname=${pkgbase#linux}
_desc="ARMv7 UDOO QUAD/DUAL"
pkgver=3.14.56
pkgrel=1
arch=('armv7h')
url="http://www.kernel.org/"
license=('GPL2')
makedepends=('xmlto' 'docbook-xsl' 'kmod' 'inetutils' 'bc' 'git' 'uboot-tools' 'vboot-utils' 'dtc')
options=('!strip')
source=("https://github.com/iH8c0ff33/linux_kernel/archive/${_pkgurlname}.tar.gz"
        ".config")
md5sums=("78f60b29291d709fe64b528221602d45"
         "91bc26375d611d82e54b5068b15dfa51")

prepare() {
    cd "${srcdir}/${_srcname}"

    # add pkgrel to extraversion
    sed -ri "s|^(EXTRAVERSION =)(.*)|\1 \2-${pkgrel}|" Makefile

    cat "${srcdir}/.config" > ./.config
}

build() {
    cd "${srcdir}/${_srcname}"

    make ${MAKEFLAGS} zImage modules dtbs
}

_package() {
    pkgdesc="The Linux Kernel and modules - ${_desc}"
    depends=('coreutils' 'linux-firmware' 'kmod')
    optdepends('crda: set porper wireless channels for your country')
    provides=('kernel26' "linux=${pkgver}")
    conflicts=('linux')
    replaces=('linux-mvebu' 'linux-udoo' 'linux-sun4i' 'linux-sun5i' 'linux-sun7i' 'linux-usbarmory' 'linux-wandboard' 'linux-clearfog')
    install=${pkgname}.install

    cd "${srcdir}/${_srcname}"

    KARCH=arm

    _kernver="$(make kernelrelease)"
    _basekernel=${_kernver%%-*}
    _basekernel=${_basekernel%.*}

    mkdir -p "${pkgdir}"/{lib/modules,lib/firmware}
    make ${MAKEFLAGS} INSTALL_MOD_PATH="${pkgdir}" modules_install firmware_install
    make ${MAKEFLAGS} dtbs
    cp arch/$KARCH/dts/*.dtb "${pkgdir}/boot/dtbs"
    cp arch/$KARCH/boot/zImage "${pkgdir}/boot/zImage"

    # set correct depmod command for install
    sed \
        -e  "s/KERNEL_NAME=.*/KERNEL_NAME=${_kernelname}/g" \
        -e  "s/KERNEL_VERSION=.*/KERNEL_VERSION=${_kernver}/g" \
        -i "${startdir}/${pkgname}.install"

     # remove build and source links
    rm -f "${pkgdir}"/lib/modules/${_kernver}/{source,build}
    # remove the firmware
    rm -rf "${pkgdir}/lib/firmware"
    # make room for external modules
    ln -s "../extramodules-${_basekernel}${_kernelname:--ARCH}" "${pkgdir}/lib/modules/${_kernver}/extramodules"
    # add real version for building modules and running depmod from post_install/upgrade
    mkdir -p "${pkgdir}/lib/modules/extramodules-${_basekernel}${_kernelname:--ARCH}"
    echo "${_kernver}" > "${pkgdir}/lib/modules/extramodules-${_basekernel}${_kernelname:--ARCH}/version"

    # Now we call depmod...
    depmod -b "$pkgdir" -F System.map "$_kernver"

    # move module tree /lib -> /usr/lib
    mkdir -p "${pkgdir}/usr"
    mv "$pkgdir/lib" "$pkgdir/usr"
}