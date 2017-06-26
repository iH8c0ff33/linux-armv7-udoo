buildarch=4

pkgbase=linux-armv7-udoo
_srcname=v3.14.56
_kernelname=${pkgbase#linux}
_desc="ARMv7 UDOO QUAD/DUAL"
pkgver=3.14.56
pkgrel=1
arch=('armv7h')
url="http://www.kernel.org/"
license=('GPL2')
makedepends=('xmlto' 'docbook-xsl' 'kmod' 'inetutils' 'bc' 'git' 'uboot-tools' 'vboot-utils' 'dtc')
options=('!strip')
source=("https://github.com/UDOOboard/linux_kernel/archive/${_srcname}.tar.gz")
md5sums=("8a7488c529dadc1ae5ee0c5e4e6bd477")

prepare() {
    echo "$(pwd)"
    echo "${srcdir}/${_srcname}"
}