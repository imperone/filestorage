#!/bin/bash
# Maintained by imper <imperator999mcpe@gmail.com>

pkgname=filestorage
pkgver=1.0
pkgrel=1
pkgdesc='File storage service server for linux'
author="imperzer0"
arch=('x86_64')
url=https://github.com/$author/$pkgname
license=('GPL3')
depends=('openssl')
makedepends=('cmake' 'git')

_srcprefix="local:/"
_libfiles=("CMakeLists.txt" "main.cpp" "server.cpp" "server.h" "constants.hpp")

# shellcheck disable=SC2068
for _libfile in ${_libfiles[@]}; do
	source=(${source[@]} "$_srcprefix/$_libfile")
done

# shellcheck disable=SC2068
for _libfile in ${_libfiles[@]}; do
	sha256sums=(${sha256sums[@]} "SKIP")
done

source=(${source[@]} "https://raw.githubusercontent.com/cesanta/mongoose/d5993ba27ece4b406c230eca63b76dd5d2c28a2d/mongoose.c")
sha256sums=(${sha256sums[@]} "a0f97a894547cf98cbfc9b276fc924288658c772cdfe3e6a8d60df91d9cc27f7")

source=(${source[@]} "https://raw.githubusercontent.com/cesanta/mongoose/d5993ba27ece4b406c230eca63b76dd5d2c28a2d/mongoose.h")
sha256sums=(${sha256sums[@]} "a486f3d5baa0d31c576b24f909ede017331f5b104dc3eac945aceafe40f8fe67")

_package_version=" ("$pkgver"-"$pkgrel")"

build() {
	cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_C_COMPILER=gcc -DCMAKE_CXX_COMPILER=g++ \
		-DPACKAGE_VERSION="$_package_version" -DAPPNAME="$pkgname" .
	make -j 6
}

package() {
	install -Dm755 $pkgname "$pkgdir/usr/bin/$pkgname"
}

notarch_package() {
	cp -f $pkgname "$pkgdir/usr/bin/$pkgname"
	chmod 755 "$pkgdir/usr/bin/$pkgname"
}
