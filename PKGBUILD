# Maintainer: D347HL0RD <D347HL0RD@cachyos.org>

pkgname=cachyos-dnscrypt-proxy-git
_pkgname=dnscrypt-proxy
pkgver=2.1.2.r8.gca253923
pkgrel=1
pkgdesc="Wipe Snoopers Out Of Your Networks"
arch=(x86_64 x86_64_v3)
url="https://github.com/CachyOS/CachyOS-DNSCrypt-Proxy"
license=(ISC)
depends=(glibc)
makedepends=(git go)
optdepends=('python-urllib3: for generate-domains-blocklist')
provides=(dnscrypt-proxy)
conflicts=(dnscrypt-proxy cachyos-dnscrypt-proxy)
install=$_pkgname.install
source=(
	git+https://github.com/dnscrypt/$_pkgname.git
	$_pkgname.toml
	$_pkgname.{service,socket}
)
sha512sums=('SKIP'
            'f0a29d22fab01730f5a009064662d2f3abba673e1081e1c27d94b8cb4d202b0335b5344e067fadd53d9499ba12d0008c0d73f258472a7dfa0481aabd47a5dc57'
            '8e5d8b411ad6c3525bc0a8d060d6c0cbcaad55ab484b4780bde5b2d480629298eda8616599c1d2db605e19422f666bb5f27477b54707f55577a3b2ba3b9acd3d'
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
