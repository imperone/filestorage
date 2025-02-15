#!/bin/bash
# Maintained by imper <imperator999mcpe@gmail.com>

pkgname=filestorage
pkgver=13.1
pkgrel=1
pkgdesc='File storage service server for linux'
author="imperzer0"
arch=('any')
url=https://github.com/$author/$pkgname
license=('GPL3')
depends=('openssl' 'gcc' 'mariadb' 'mariadb-connector-cpp-git' 'lua')
makedepends=('cmake' 'git' 'gcc' 'make' 'mariadb-connector-cpp-git' 'lua')

_srcprefix="local:/"
_libfiles=("CMakeLists.txt" "main.cpp"
	"server.cpp" "server.h"
	"database.cpp" "database.h"
	"constants.hpp" "templates.hpp"
	"sha256.cpp" "sha256.hpp"
	"config_script.cpp" "config_script.h"
	"lua_config_libfunctions.cpp" "lua_config_libfunctions.h"
	"Gen_QR.cpp" "Gen_QR.h"
)

_rcfiles=(
	"resources/config_lib.lua"
	"resources/deleter.html"
	"resources/error.html"
	"resources/explorer.html"
	"resources/favicon.ico"
	"resources/filestorageconf.lua"
	"resources/index.html"
	"resources/invalid_credentials.html"
	"resources/uploader.html"
)

# shellcheck disable=SC2068
for _libfile in ${_libfiles[@]}; do
	source=(${source[@]} "$_srcprefix/$_libfile")
	sha256sums=(${sha256sums[@]} "SKIP")
done

# shellcheck disable=SC2068
for _rcfile in ${_rcfiles[@]}; do
	source=(${source[@]} "$_srcprefix/$_rcfile")
	sha256sums=(${sha256sums[@]} "SKIP")
done

external=(${external[@]} "https://raw.githubusercontent.com/cesanta/mongoose/d5993ba27ece4b406c230eca63b76dd5d2c28a2d/mongoose.c")
sha256sums=(${sha256sums[@]} "a0f97a894547cf98cbfc9b276fc924288658c772cdfe3e6a8d60df91d9cc27f7")

external=(${external[@]} "https://raw.githubusercontent.com/cesanta/mongoose/d5993ba27ece4b406c230eca63b76dd5d2c28a2d/mongoose.h")
sha256sums=(${sha256sums[@]} "a486f3d5baa0d31c576b24f909ede017331f5b104dc3eac945aceafe40f8fe67")

external=(${external[@]} "https://raw.githubusercontent.com/imperzer0/strscan/master/strscan.c")
sha256sums=(${sha256sums[@]} "b15cf49575f34072ded00866261106a277f9e29e77e8ee3278635cd2273f3939")

external=(${external[@]} "https://raw.githubusercontent.com/imperzer0/strscan/master/strscan.h")
sha256sums=(${sha256sums[@]} "f51d8e3e25b2bcef6387e981086ace1dc82584c3f63bb46a27a21483d533a1e7")

external=(${external[@]} "https://raw.githubusercontent.com/nayuki/QR-Code-generator/720f62bddb7226106071d4728c292cb1df519ceb/cpp/qrcodegen.cpp")
sha256sums=(${sha256sums[@]} "1f3b3fcdac6954c32cf583ccd02ec9b5901f756a38c461acedc70be4a77d3757")

external=(${external[@]} "https://raw.githubusercontent.com/nayuki/QR-Code-generator/720f62bddb7226106071d4728c292cb1df519ceb/cpp/qrcodegen.hpp")
sha256sums=(${sha256sums[@]} "b779c3b156cf7a57ce789d6fee4fc991ccc2913774d26c909d22bb8f26b2a793")

source=(${source[@]} ${external[@]})

setup_mariadb_cpp_connector_ubuntu() {
	echo " [INFO] Installing dependencies..."
	sudo apt-get install -y git cmake make gcc libssl-dev
	git clone https://github.com/MariaDB-Corporation/mariadb-connector-cpp.git
	echo " [INFO] Changing to build directory..."
	mkdir build && cd build || echo " [WARNING] Can't create or change to subdirectory 'build'!!!"
	echo " [INFO] CMaking..."
	cmake ../mariadb-connector-cpp/ -DCMAKE_BUILD_TYPE=RelWithDebInfo -DCONC_WITH_UNIT_TESTS=Off -DCMAKE_INSTALL_PREFIX=/usr/local -DWITH_SSL=OPENSSL
	echo " [INFO] Building..."
	cmake --build . --config RelWithDebInfo
	echo " [INFO] Installing..."
	sudo make install
	echo " [INFO] Returning from build..."
	cd ..
}

ubuntu_pkginstall() {
	sudo apt-get update && sudo apt-get upgrade -y
	echo " [INFO] Installing filestorage dependencies..."
	sudo apt-get install -y lua5.3 liblua5.3-dev libmariadb-dev
	setup_mariadb_cpp_connector_ubuntu
	echo " [INFO] Done."
}

notarch_prepare() {
	# shellcheck disable=SC2068
	for ex in ${external[@]}; do
		wget "$ex"
	done
}

_package_version=" ("$pkgver"-"$pkgrel")"

build() {
	ls -alshp
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
