# Maintainer: BL4CKH47H4CK3R <109698175+BL4CKH47H4CK3R@users.noreply.github.com>

pkgname=cachyos-dnscrypt-proxy
_pkgname=dnscrypt-proxy
pkgver=2.1.2.r15.g4aa415de
pkgrel=1
pkgdesc="Wipe Snoopers Out Of Your Networks"
arch=(x86_64 x86_64_v3)
url="https://github.com/CachyOS/CachyOS-DNSCrypt-Proxy"
license=(ISC)
depends=(glibc)
makedepends=(git go)
optdepends=('python-urllib3: for generate-domains-blocklist')
provides=(dnscrypt-proxy)
conflicts=(dnscrypt-proxy)
install=$_pkgname.install
source=(
	git+https://github.com/dnscrypt/$_pkgname.git
	$_pkgname.toml
	$_pkgname.{service,socket}
)
sha512sums=('SKIP'
            '970e149e591efb20cb16dc2d8c1467ac8d96805dd06b0ba427ad46bbb0b679b3fe9f2e6a2b6b8a934f623c038b28c3397e7c6be54cb5fa5a692eb93e5897e9dd'
            'a62fe2b5c8e194931a1c3948b262b0a4ab766c1b649431aabe2ec9e527abd23346fd21d1fc0e25783b9137d278d49256e50f9ca8ed456c59e36b48414607bda2'
            '17175397a5a35692f300d6caff84eb236b21a6e41a870bca966c5576f0db2bc7556d6a214d2f7e985fe9e0be99ef6e0bb067f29cebd41c2ea374540d6f4bd990')

pkgver() {
	cd "$_pkgname"
	git describe --long | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

build() {
	cd $_pkgname/$_pkgname
	export CGO_CPPFLAGS="$CPPFLAGS"
	export CGO_CFLAGS="$CFLAGS"
	export CGO_CXXFLAGS="$CXXFLAGS"
	export CGO_LDFLAGS="$LDFLAGS"
	export GOFLAGS="-buildmode=pie -trimpath -ldflags=-linkmode=external -mod=readonly -modcacherw"
	go build
}

package() {
	local _config
	cd $_pkgname
	# executable
	install -Dm 755 $_pkgname/$_pkgname -t "$pkgdir/usr/bin/"
	# config files
	install -Dm 644 ../$_pkgname.toml "$pkgdir/etc/$_pkgname/$_pkgname.toml"
	for _config in {{allowed,blocked}-{ips,names},{cloaking,forwarding}-rules,captive-portals}.txt; do
		install -Dm 644 $_pkgname/example-$_config "$pkgdir/etc/$_pkgname/$_config"
	done
	# utils
	install -Dm 644 utils/generate-domains-blocklist/*.{conf,txt} -t "$pkgdir/usr/share/$_pkgname/utils/generate-domains-blocklist"
	install -Dm 755 utils/generate-domains-blocklist/generate-domains-blocklist.py "$pkgdir/usr/bin/generate-domains-blocklist"
	# systemd service/socket
	install -Dm 644 ../$_pkgname.{service,socket} -t "$pkgdir/usr/lib/systemd/system/"
	# license
	install -Dm 644 LICENSE -t "$pkgdir/usr/share/licenses/$_pkgname"
	# docs
	install -Dm 644 {ChangeLog,README.md} -t "$pkgdir/usr/share/doc/$_pkgname"
}
# vim:set ts=2 sw=2 et:
