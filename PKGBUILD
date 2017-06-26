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
source=("https://github.com/iH8c0ff33/linux_kernel/archive/${_pkgurlname}.tar.gz")
md5sums=("78f60b29291d709fe64b528221602d45")

prepare() {
    # add pkgrel to extraversion
    sed -ri "s|^(EXTRAVERSION =)(.*)|\1 \2-${pkgrel}|" "${srcdir}/${_srcname}/Makefile"
    echo "$(pwd)"
    echo "${pkgdir}"
    echo "${srcdir}/${_srcname}"
}